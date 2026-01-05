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