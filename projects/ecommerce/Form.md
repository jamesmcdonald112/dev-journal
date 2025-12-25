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



https://react.dev/learn/updating-objects-in-state

https://react.dev/learn/rendering-lists

https://react.dev/reference/react-dom/components/input



// `specRows` is added because React Hook Form cannot directly manage a Zod
// `z.record()` value. A record is an object with dynamic string keys, which
// RHF does not treat as a controllable field structure.
//
// To support dynamic “key/value” specification inputs (add/remove rows,
// validate each entry, track field state, etc.), the form exposes an array:
//
//   specRows: Array<{ key: string; value: string }>
//
// Users interact with this array in the UI. On submission, the array is
// transformed into the `specs` record expected by the base `productSchema`
// and by the database:
//
//   [{ key: "color", value: "red" }]
//   → { color: "red" }
//
// `productSchema` retains `specs` as `z.record(z.string())` for validation
// and persistence, while `productFormSchema` extends it with `specRows` to
// support a dynamic form interface. This is the standard pattern for managing
// key/value inputs in React Hook Form.



Here’s the **clean, professional, complete write-up** of how you arrived at the final architecture — exactly the kind of explanation a senior engineer would document.

  

Copy/paste this into your notes.

---

# **✅** 

# **Dynamic Specs Architecture — Full Explanation & Final Code**

  

## **Why this was needed**

  

The product model includes a specs field:

```
specs: z.record(z.string(), z.string()).default({})
```

A z.record() represents **an object with dynamic string keys**, e.g.:

```
{
  color: "blue",
  weight: "200g",
  material: "cotton"
}
```

React Hook Form **cannot** directly manage a record because:

- it has no fixed field names
    
- keys can be added/removed dynamically
    
- RHF requires predictable field paths like fieldA.0.key, fieldA.0.value
    

  

So you **cannot** do:

```
name="specs.color"
name="specs.weight"
```

because keys don’t exist until the user types them.

  

This is why dynamic key/value UIs require a **parallel array**, not a record.

---

# **✅** 

# **Solution: introduce specRows (UI representation)**

  

We add a UI-friendly array:

```
specRows: Array<{ key: string; value: string }>
```

Example:

```
[
  { key: "color", value: "blue" },
  { key: "weight", value: "200g" }
]
```

This structure works with React Hook Form:

- each row has a predictable index
    
- fields map cleanly:
    
    - specRows.0.key
        
    - specRows.0.value
        
    

  

On form submit, we convert:

```
specRows → specs
```

---

# **✅** 

# **Final form schema**

  

productFormSchema extends the _real_ product schema with UI-only fields:

```
import { z } from "zod";
import { productSchema } from "./product.schema";

// UI helper schema for React Hook Form
export const productFormSchema = productSchema.extend({
  specRows: z
    .array(
      z.object({
        key: z.string().min(1),
        value: z.string().min(1),
      })
    )
    .default([]),
});

export type ProductFormValues = z.infer<typeof productFormSchema>;
```

Notes:

- productSchema stays clean → used for DB & API validators
    
- productFormSchema = only for React Hook Form (client-side form)
    

---

# **✅** 

# **KeyValueRow Component (UI for one row)**

  

This component renders **one row** containing:

- key input
    
- value input
    
- remove button
    

```
import { Controller, type UseFormReturn } from "react-hook-form";
import type { ProductFormValues } from "@/app/features/products/schemas/product-form.schema";
import { Button } from "./ui/button";
import { Field, FieldError, FieldLabel } from "./ui/field";
import { Input } from "./ui/input";

interface KeyValueRowProps {
  form: UseFormReturn<ProductFormValues>;
  index: number;
  onRemove: () => void;
}

export default function KeyValueRow({
  form,
  index,
  onRemove,
}: KeyValueRowProps) {
  return (
    <div className="flex gap-4 items-end">
      {/* KEY */}
      <Controller
        name={`specRows.${index}.key`}
        control={form.control}
        render={({ field, fieldState }) => (
          <Field data-invalid={fieldState.invalid} className="flex-1">
            <FieldLabel htmlFor={`spec-key-${index}`}>Key</FieldLabel>
            <Input
              {...field}
              id={`spec-key-${index}`}
              aria-invalid={fieldState.invalid}
              placeholder="Colour"
              autoComplete="off"
            />
            {fieldState.invalid && <FieldError errors={[fieldState.error]} />}
          </Field>
        )}
      />

      {/* VALUE */}
      <Controller
        name={`specRows.${index}.value`}
        control={form.control}
        render={({ field, fieldState }) => (
          <Field data-invalid={fieldState.invalid} className="flex-1">
            <FieldLabel htmlFor={`spec-value-${index}`}>Value</FieldLabel>
            <Input
              {...field}
              id={`spec-value-${index}`}
              aria-invalid={fieldState.invalid}
              placeholder="Blue"
              autoComplete="off"
            />
            {fieldState.invalid && <FieldError errors={[fieldState.error]} />}
          </Field>
        )}
      />

      {/* REMOVE BUTTON */}
      <Button type="button" variant="destructive" size="sm" onClick={onRemove}>
        Remove
      </Button>
    </div>
  );
}
```

---

# **✅** 

# **How the whole system works**

  

### **1. User interacts with specRows**

  

Example in UI:

|**Key**|**Value**|
|---|---|
|“color”|“blue”|
|“size”|“XL”|

This populates:

```
specRows: [
  { key: "color", value: "blue" },
  { key: "size", value: "XL" }
]
```

### **2. On form submit → convert specRows → specs**

```
[
  { key: "color", value: "blue" }
  { key: "size", value: "XL" }
]

→

{
  color: "blue",
  size: "XL"
}
```

### **3. Then send specs to backend**

  

specRows stays on the frontend only.

---

# **📌** 

# **Summary (the note you wanted)**

  

> **React Hook Form cannot manage z.record() directly because records use dynamic keys.**

> **To support add/remove key/value fields, the form introduces a parallel UI-only array called specRows.**

> **Each row contains { key, value } so RHF can register predictable field paths.**

> **productFormSchema extends the real productSchema with this array.**

> **On submit, specRows is transformed into the real specs object expected by the API and database.**

> **This pattern is the standard, recommended approach for dynamic key/value inputs in React Hook Form.**

---

If you want, I can generate:

  

✅ the conversion function

✅ where to put it

✅ how to integrate KeyValueRow inside your form

✅ the entire /admin/product page including add/remove rows

  

Just tell me **“show the conversion”** or **“show full form integration.”**



## THE FIX BY CLAUDE
You had two schema files trying to do the same job:
  - productSchema - the main validation rules
  - productFormSchema - a copy that tried to fix type issues

  This created a mismatch: the form had optional fields (could be undefined), but components expected required fields (no undefined). TypeScript complained about this.

  The Solution

  Use ONE schema instead of two:

  1. Removed .default() from productSchema
    - .default() makes fields optional in TypeScript's eyes
    - Instead, React Hook Form's defaultValues handles the defaults
    - This keeps the schema clean and types strict
  2. Added specRows to productSchema
    - Moved it from the separate form schema into the main schema
    - Now everything is in one place
  3. Updated components to use productSchema directly
    - Deleted the unnecessary productFormSchema file
    - All components now import and use productSchema
    - All components get the exact same type - no mismatches

  Why It Works

  - Single source of truth - One schema, one type definition
  - No duplicated code - Don't maintain two schemas
  - Type safety - All components agree on what the form looks like
  - React Hook Form best practice - Defaults go in defaultValues, not the schema

  That's it! Much simpler and cleaner.





  1. Helper Methods for Toast Notifications
    - showSuccessToast() - displays success message when product is created
    - showErrorToast() - displays error message if something fails
  2. Data Transformation
    - convertSpecRowsToSpecs() - converts the specRows array into a specs object (e.g., [{key: "color", value: "red"}] → {color: "red"})
  3. API Integration
    - Updated onSubmit() to be async
    - Posts the product data to /api/products endpoint (which you already have)
    - Converts specRows to specs before sending
    - Handles errors gracefully with toast notifications
    - Resets the form after successful submission

  How It Works:

  When you submit the form:
  4. The form data is validated by Zod
  5. specRows array is converted to specs object
  6. Data is sent to /api/products via POST
  7. On success: green toast shows "Product created successfully"
  8. On error: red toast shows what went wrong
  9. Form resets automatically after success

  The implementation integrates seamlessly with your existing backend API and uses the helper methods to keep the toast logic separate and reusable.