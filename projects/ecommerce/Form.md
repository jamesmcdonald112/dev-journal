https://ui.shadcn.com/docs/forms/react-hook-form

```bash
npx shadcn@latest init
```

You now have:

- a components.json config file
    
- a lib/utils.ts file (contains the cn() helper)
    
- updated app/globals.css with the full shadcn theme + Tailwind v4 tokens

### **Your project is now ready to use any shadcn components**

  ```bash
npx shadcn add input
npx shadcn add textarea
npx shadcn add button
npx shadcn add card
npx shadcn add form
npx shadcn add field
  ```


### **You have a reusable className utility**

cn() allows clean conditional classes.

It:

- takes any number of class strings
    
- removes anything that is false, null, undefined, or empty
    
- joins the remaining classes into one string

```tsx
<div className={cn("p-4", isActive && "bg-black text-white")}>
```

Exmple with multiple class names
```tsx
<div
  className={cn(
    "p-4 rounded-md",
    isError && "bg-red-500 text-white",
    isSelected ? "border-2 border-blue-500" : "border border-gray-300",
    customClassName,             // only added if truthy
  )}
>
  Content
</div>
```

### **You can build forms using shadcn’s official Form system**

  

It integrates perfectly with:

- react-hook-form
    
- zod
    
- @hookform/resolvers


# ** WHAT** 

# **@hookform/resolvers**

#  **DOES — PLAIN EXAMPLE**

  

Think of it like this:

- **Zod**: checks if the data is valid
    
- **React Hook Form (RHF)**: manages the form fields
    
- **@hookform/resolvers**: _translates_ Zod’s errors into a format RHF understands
    

  

Without the resolver, Zod speaks **French** and RHF speaks **English**.

The resolver is the **translator**.

---

# **1️⃣ Without Resolver → It Does NOT Work**

```ts
const schema = z.object({
  name: z.string().min(3),
})

const form = useForm({
  // ❌ no resolver
})
```

You submit invalid data.

  

Zod will produce errors like:

```json
{
  issues: [
    {
      path: ["name"],
      message: "String must contain at least 3 characters."
    }
  ]
}
```

React Hook Form **does not understand this**.

So RHF’s formState.errors stays empty → **no errors show up**.

---

# ** With Resolver → It Works**

```ts
const schema = z.object({
  name: z.string().min(3),
})

const form = useForm({
  resolver: zodResolver(schema),
})
```

Now when you submit invalid data, the resolver converts Zod’s error:

```
{
  issues: [...Zod errors...]
}
```

→ into RHF format:

```
{
  name: {
    type: "min",
    message: "String must contain at least 3 characters."
  }
}
```

RHF _understands this perfectly_.

---

# **3️⃣ Full Working Example**

```ts
import { useForm } from "react-hook-form"
import { zodResolver } from "@hookform/resolvers/zod"
import { z } from "zod"

const formSchema = z.object({
  name: z.string().min(3, "At least 3 characters required"),
})

export function DemoForm() {
  const form = useForm({
    resolver: zodResolver(formSchema),
  })

  return (
    <form onSubmit={form.handleSubmit(console.log)}>
      <input {...form.register("name")} />

      {form.formState.errors.name && (
        <p>{form.formState.errors.name.message}</p>
      )}

      <button type="submit">Submit</button>
    </form>
  )
}
```

### **Result:**

- User types **“ab”**
    
- Zod says: ❌ “Too short”
    
- Resolver translates → RHF error
    
- UI shows:
    

```
At least 3 characters required
```

---

# **4️⃣ Even Simpler Analogy**

  

Imagine this:

- **Zod** = inspector checking your data
    
- **React Hook Form** = manager controlling the form
    
- **@hookform/resolvers** = the phone line between them
    

  

Without the resolver:

❌ They cannot talk → errors never show

  

With the resolver:

✔ They talk → validation just works

---

# **Want an even shorter version?**

  

**Zod validates.**

**React Hook Form manages inputs.**

**@hookform/resolvers connects them.**

---

If you want, I can now show you how to set up your **Product Form** using:

- shadcn/ui
    
- react-hook-form
    
- zod
    
- resolver
    

  

Let me know and I’ll build the exact starter code.


---

docs - https://ui.shadcn.com/docs/forms/react-hook-form#input

the docs tell us to download these to things

```bash
npm install react-hook-form
```

Installs React Hook Form

```bash
npx shadcn@latest add field
```

```bash
npm install @hookform/resolvers zod
```
-  this is the bridge between the two

```bash
npm install sonner
```
- This for the toast pop up
  
  
  
Here is the **clean, simple explanation** of what happened, why it happened, and how the documentation links to it — without noise.

---

# **✅** 

# **1. WHY your code broke**

  

Your schema contains **transforming fields**:

- z.coerce.number()
    
- .default({})
    
- .default([])
    

  

These make the **input type** and **output type** different.

  

Example:

  

### **Input (what RHF collects from HTML form)**

```
price: string    // because HTML inputs always give strings
specs: undefined // because user didn’t provide anything
reviews: undefined
```

### **Output (what Zod returns after parsing + defaulting)**

```
price: number
specs: {}
reviews: []
```

So:

  

### **❗Input type ≠ Output type**

  

RHF sees an inconsistency → TypeScript complains.

---

# **✅** 

# **2. WHY you must provide input + output to useForm**

  

Directly from the resolver docs:

  

> useForm<Input, Context, Output>()

  

Source:

https://github.com/react-hook-form/resolvers?tab=readme-ov-file#typescript

  

They explicitly show:

```
useForm<z.input<typeof schema>, any, z.output<typeof schema>>({
  resolver: zodResolver(schema),
});
```

Meaning:

- **Input** = what RHF should expect BEFORE validation
    
- **Output** = what Zod returns AFTER validation
    

  

Because your schema transforms values, RHF cannot guess these types — so you must specify both.

---

# **✅** 

# **3. WHAT your fixed version does**

  

You wrote:

```ts
const form = useForm<
  z.input<typeof productSchema>,
  undefined,
  z.output<typeof productSchema>
>({
  resolver: zodResolver(productSchema),
  defaultValues: { ... }
});
```

- Undefined is used because the second generic is the resolver context type. It’s required by the useForm generic signature, but I don’t use any context in my validation. Biome complains about using any, so undefined is the correct explicit placeholder

Breaking it down:

  

### `**z.input<typeof productSchema>`**

  

TypeScript representation of “what form fields look like before Zod parses them”.

- price is string | number
    
- specs is possibly undefined
    
- reviews is possibly undefined
    

  

### `**z.output<typeof productSchema>`**

  

TypeScript representation of “guaranteed return type after Zod validates/parses”.

- price is number
    
- specs is {}
    
- reviews is []
    

  

RHF now understands:

- Input may be messy (strings, undefined, missing)
    
- Zod will clean it
    
- Output is safe, typed, transformed
    

  

So TypeScript stops complaining.

---

# **✅** 

# **4. WHY the shadcn example doesn’t need input/output**

  

Because their schema:

```
z.string().min(...)
z.string().min(...)
```

has:

  

### **✔ No coercion**

  

### **✔ No defaults**

  

### **✔ No type transformations**

  

So:

- input type = output type
    
- RHF can infer everything automatically
    
- No need for generics
    
- No errors
    

  

Your schema is more advanced — therefore the generics are required.

---

# **✅** 

# **5. Official docs that prove this**

  

### **A) Resolver docs:**

  

https://react-hook-form.com/docs/useform#resolver

  

> Resolver integrates external schema validators (Zod, Yup, etc.) and can transform values.

  

### **B) Resolver TypeScript docs:**

  

https://github.com/react-hook-form/resolvers?tab=readme-ov-file#typescript

  

They show EXACTLY your situation:

```ts
useForm<z.input<typeof schema>, any, z.output<typeof schema>>({
  resolver: zodResolver(schema),
});
```

---

# **✔ FINAL SUMMARY (simple)**

- Your schema transforms data (coercion + defaults).
    
- That makes **input type ≠ output type**.
    
- RHF needs both types because the resolver transforms values.
    
- You fixed it by supplying:
    

```ts
useForm<z.input<typeof schema>, any, z.output<typeof schema>>()
```

- This is **documented**, expected, correct**, and the right solution.
    

---

If you want, I can rewrite your README explanation in a **short clean paragraph** suitable for GitHub.