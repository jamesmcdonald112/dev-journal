## **Info Pack Delivery Service**

  

We created a **service layer** for the Info Pack feature to handle all side-effects in one place.

  

### **Purpose**

  

The service represents the **business action**:

**“Deliver an info pack submission.”**

  

It is responsible for:

- Coordinating what happens after a valid form submission
    
- Calling external systems (email, storage, logging)
    
- Keeping actions/routes thin and focused
    

  

### **Why a Service**

- Separates **business logic** from UI and routing
    
- Makes the feature easier to extend (email + Google Sheets later)
    
- Keeps external APIs (Resend, Sheets) isolated
    
- Matches **Bulletproof React** architecture
    

  

### **Location**

```
src/features/InfoPack/service/deliverInfoPack.ts
```

### **Role in the flow**

```
Form Action → deliverInfoPack (service) → APIs (email, storage)
```

The service does **not**:

- Render UI
    
- Validate input (handled by schema)
    
- Know about HTTP or forms
    

  

It only answers:

**“What should happen when an info pack is submitted?”**

```ts
export async function deliverInfoPack(input: {

fullName: string;

email: string;

phone: string;

studentAge?: number;

message?: string;

}): Promise<void> {

// TODO: Implement email sending (Resend, SMTP, etc.)

void input;

}
```


### **Note: Why** 

### **sendInfoPackHandler**

###  **exists (and what I got right/wrong)**

  

**What this file is for**

- sendInfoPack is the Astro Action wrapper (defineAction(...)).
    
- sendInfoPackHandler is the real logic, extracted into a **named function**.
    

  

**Why we created sendInfoPackHandler**

- Astro Actions are framework glue and awkward to unit test directly.
    
- A plain exported function is easy to test: you can call it with any input and assert what happens.
    

  

**What it does**

- **Honeypot check:** if input.website has a value → treat as bot → return { success: true, message: "OK" } and **do not** call the real delivery code.
    
- **Human path:** if honeypot is empty → call deliverInfoPack(input) (email / sheets etc).
    
- **Error path:** if delivery throws → Sentry.captureException(err) → throw an ActionError so the UI gets a controlled error.
    

  

**What I got right**

- Extracting the handler lets us test different inputs (honeypot empty vs filled) and cover branches automatically.
    
- Using a service (deliverInfoPack) makes it easy to test “called vs not called”.
    

  

**What I got slightly wrong**

- You don’t “return” an ActionError — you **throw** it.
    
- In tests, you usually **call** **sendInfoPackHandler** **directly** and **mock** **deliverInfoPack** **+** **Sentry** (you don’t mock the handler itself).
```ts
import { ActionError, defineAction } from "astro:actions";

import * as Sentry from "@sentry/astro";

import type { z } from "zod";

import { INFO_PACK_ERRORS } from "../config/error-messages";

import { infoPackSchema } from "../schemas/infoPack.schema";

import { deliverInfoPack } from "../service/deliverInfoPack";

  

type InfoPackInput = z.infer<typeof infoPackSchema>;

  

export async function sendInfoPackHandler(input: InfoPackInput) {

console.log("HANDLER RAN with input:", input);

  

// Honeypot: humans won’t fill `website`. If it has a value, treat as bot.

// Return success to avoid tipping off spam scripts (don’t send email, don’t log).

if (input.website?.trim()) {

console.log("HONEYPOT TRIGGERED");

return { success: true, message: "OK" };

}

  

try {

await deliverInfoPack(input);

return {

success: true,

message: "Info pack emailed successfully",

};

} catch (err: unknown) {

Sentry.captureException(err);

  

// If it's already an ActionError, rethrow to parent so it can be displayed in toast error message.

if (err instanceof ActionError) {

throw err;

}

  

throw new ActionError({

code: "INTERNAL_SERVER_ERROR",

message: INFO_PACK_ERRORS.action.internalServerError,

});

}

}

  

export const sendInfoPack = defineAction({

accept: "form",

input: infoPackSchema,

handler: sendInfoPackHandler,

});
```


Here are clean Obsidian notes for that test file. Copy/paste as-is.

---

# **InfoPack Zod Schema Tests (Vitest)**

  

**File:** test/features/InfoPack/schemas/infoPack.schema.test.ts

**Goal:** Lock down infoPackSchema behaviour so validation + transforms can’t silently regress.

  

## **Imports (what/why)**

- vitest (describe/test/expect) → unit test framework.
    
- zod type import → used only for typing test inputs.
    
- FORM_LIMITS → reuse real min/max limits (no hardcoded numbers in tests).
    
- infoPackSchema → the schema under test.
    

  

## **Input typing:** 

## `**z.input<typeof infoPackSchema>`**

```
type InfoPackInput = z.input<typeof infoPackSchema>;
```

- z.input = the _raw_ shape the schema accepts **before** transforms/coercion.
    
- Important because studentAge can arrive as "8" (string) from a form, even though output becomes number.
    

  

## **Helper factory:** 

## **makeValidInput(...)**

```
function makeValidInput(overrides: `Partial<InfoPackInput> `= {}): InfoPackInput {
  return { ...defaults, ...overrides };
}
```

Purpose:

- Creates a known-good baseline object.
    
- Each test overrides only the field it cares about.
    

  

Key details:

- `Partial<InfoPackInput>` means overrides can contain _only some_ fields.
    
- = {} means “if you pass nothing, default to empty object” (it does **not** auto-fill strings to ""; it’s just a default param value).
    

  

## **Pulling limits from config**

```
const { fullName: FULL_NAME_LIMITS, ... } = FORM_LIMITS;
```

Why:

- Tests stay correct if limits change (single source of truth).
    
- Variable names end with _LIMITS to be explicit.
    

---

# **Test groups (what each block guarantees)**

  

## **1) Happy path (full valid input)**

  

Asserts:

- .trim() works on fullName + message.
    
- .toLowerCase() works on email.
    
- website whitespace becomes "" (your honeypot transform behaviour).
    
- The final parsed object matches expected normalized output.
    

  

## **2)** 

## **fullName**

  

Tests:

- trims whitespace
    
- throws if length < min
    
- throws if length > max
    

  

Technique used:

```
"a".repeat(FULL_NAME_LIMITS.min - 1)
```

- Generates a string that’s **exactly 1 char too short/long**, so the failure is guaranteed.
    

  

## **3)** 

## **email**

  

Tests:

- trims + lowercases
    
- rejects invalid email
    
- rejects when longer than max
    

  

Max-length test uses a domain constant:

```
const domain = "@example.com";
const longEmail = `${"a".repeat(EMAIL_LIMITS.max - domain.length + 1)}${domain}`;
```

- Ensures the full string length exceeds max.
    

  

## **4)** 

## **phone**

  

Tests:

- accepts known-good phone string (whatever your regex allows)
    
- rejects invalid phone
    

  

Note:

- input.phone.trim() expectation verifies trimming is applied (if .trim() is in the schema).
    

  

## **5)** 

## **studentAge**

  

Tests:

- accepts valid number
    
- accepts numeric string input ("8") **because Zod coercion** (z.coerce.number())
    
- rejects decimals (8.5) and decimal strings ("8.5")
    
- rejects `< min, rejects >` max
    
- accepts missing studentAge (optional)
    

  

Why rawInput as unknown?

- Because the typed makeValidInput() returns studentAge: number.
    
- For the "8" case, you’re simulating what the browser actually sends.
    

  

## **6)** 

## **message**

  

Tests:

- missing message allowed (optional)
    
- whitespace-only message becomes "" after trim
    
- rejects message longer than max
    

  

## **7)** 

## **website**

##  **transform (honeypot field)**

  

Tests:

- missing → ""
    
- null → ""
    
- whitespace → ""
    
- non-empty → trimmed value remains
    

  

This guarantees:

- input.website is always a string after parse, so honeypot logic can safely do:
    

```
if (input.website.trim()) ...
```

  

---

# **Why this test file matters**

- Validates **both**: validation rules (min/max/regex/email) and normalization (trim/lowercase/coerce/transform).
    
- Makes spam protection safer: website is always normalized to a string, avoiding runtime surprises like null.
    

---

If you want, I can also write a short note for the _action handler_ tests (human path vs honeypot path vs error path) so your notes cover the whole flow end-to-end.


