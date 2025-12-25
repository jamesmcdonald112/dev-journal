# **Why API Validation is essential**

  

Without validation, someone can send:

- missing fields
    
- wrong types
    
- too-short inputs
    
- security-risky strings
    
- unexpected JSON
    

  

If you assume data is valid → you get bugs or security vulnerabilities.

  

Zod fixes this.

---

# **🔥** 

# **How API Validation works**

  

You call the schema’s **safeParse()** method:

```ts
const parsed = contactSchema.safeParse(requestData);
```

Then check:

```ts
if (!parsed.success) {
  // validation failed
}
```

This ensures **your server never runs with invalid data**.

---

# **🧱** 

# **Real example (your project)**

  

### **File:** 

### **src/features/contact-form/contact.schema.ts**

```ts
import { z } from "zod";

export const contactSchema = z.object({
  name: z.string().min(2),
  email: z.string().email(),
  message: z.string().min(10),
});

export type ContactFormData = z.infer<typeof contactSchema>;
```

---

# **📮** 

# **API Validation in an Astro Server Action**

  

(File: src/features/contact-form/submit.ts)

```ts
import { contactSchema } from "./contact.schema";

export async function submit(formData: FormData) {
  const raw = Object.fromEntries(formData);

  const result = contactSchema.safeParse(raw);

  if (!result.success) {
    return {
      ok: false,
      errors: result.error.flatten().fieldErrors,
    };
  }

  // -----------------------------
  // ✔ VALID DATA
  // result.data is fully typed + safe
  // -----------------------------
  const data = result.data;

  return {
    ok: true,
    data,
  };
}
```

---

# **🧠** 

# **Why safeParse() is the only method you should use**

  

Zod.parse() throws exceptions → bad for server code.

  

safeParse() returns:

```ts
{ success: true, data: ... }
{ success: false, error: ... }
```

No exceptions. Predictable. Clean.

---

# **⚡ Typical flow in your Astro form**

1. User submits form
    
2. Astro server receives request
    
3. Zod validates
    
4. If errors → send fieldErrors back
    
5. If valid → perform email send, DB write, etc.
    

  

100% type-safe end-to-end.

---

# **🔐** 

# **Security benefits**

  

Zod eliminates:

- unsafe JSON
    
- missing fields
    
- type mismatches
    
- injection attempts like "<script>...</script>"
    
- backend errors from invalid values
    

  

Every request passes through a schema gate.

---

# **📌** 

# **Your Obsidian Note Should Say:**

  

## **Zod — API Validation**

  

### **Purpose**

  

Validate all incoming API data before using it.

  

### **Why**

- prevents invalid input
    
- prevents security issues
    
- avoids backend crashes
    
- ensures typed server-side data
    

  

### **Core method**

```
const parsed = schema.safeParse(data);
```

### **Pattern**

```
if (!parsed.success) return errorResponse
else use parsed.data safely
```

### **Real usage**

  

Used in:

- contact forms
    
- newsletter signup
    
- login/auth flows
    
- fetch-based APIs
    
- content collections
    
- any POST/PUT/PATCH request
    

  

### **Error format**

  

Use:

```
result.error.flatten().fieldErrors
```

Perfect shape for forms.

---
