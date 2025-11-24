# **Zod → TypeScript Type Inference (Why it matters & how you use it)**

  

## **What it is (in one sentence):**

  

**Zod automatically generates TypeScript types from your schema so you never write the same type twice.**

  

That’s it.

  

You write the schema **once**, and Zod creates the TypeScript type **for free**.

---

# **🎯** 

# **The problem it solves**

  

Without Zod, you end up doing this:

```ts
// ❌ BAD — duplication
type ContactFormData = {
  name: string;
  email: string;
  message: string;
};

const contactSchema = {
  // you rewrite everything again in a validation library…
};
```

Two separate sources of truth = guaranteed bugs.

  

If you update one and forget the other → TypeScript lies to you.

---

# **✅** 

# **With Zod (the correct way)**

  

You define **only the schema**, and Zod infers the type:

```ts
import { z } from "zod";

export const contactSchema = z.object({
  name: z.string().min(2),
  email: z.string().email(),
  message: z.string().min(10),
});

export type ContactFormData = z.infer<typeof contactSchema>;
```

Now:

- The TypeScript type is **always correct**
    
- Your IDE gets perfect autocomplete
    
- Forms, server routes, and components stay **in sync**
    
- No duplicated type definitions ever
    

---

# **📌** 

# **What TS inference gives you (practical benefits)**

  

### **✔ Autocomplete in your IDE**

  

When you type:

```
formData.
```

You get **name, email, message**.

  

### **✔ Prevents bugs**

  

Change a field in the schema → everything updates.

  

### **✔ Safer forms**

  

Your server receives ContactFormData already typed.

  

### **✔ Safer components**

  

If you pass the wrong props, TS catches it instantly.

---

# **🔥** 

# **How you use it in your Astro project**

  

This is how you’ll use inference realistically:

  

### **1. Server Action (submit.ts)**

```ts
import type { ContactFormData } from "./contact.schema";

export async function handleSubmit(data: ContactFormData) {
  // TS knows all fields, all types
}
```

### **2. Form Component**

```ts
import type { ContactFormData } from "./contact.schema";

function validate(values: Partial<ContactFormData>) {
  // TS warns if you miss a field or mistype one
}
```

### **3. Content Collections**

```ts
type Testimonial = z.infer<typeof testimonialsSchema>;
```

Now your components know exactly:

- name: string
    
- rating: number
    
- quote: string
    
- etc.
    

---

