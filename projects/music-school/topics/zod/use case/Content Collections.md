# **Why use Zod here? (The actual benefits)**

  

### **✔ Prevent broken pages**

  

If someone writes:

```
rating: "five"
```

Zod blocks the build with a clear error.

  

### **✔ Strong types everywhere**

  

Your components get:

- autocomplete
    
- correct field names
    
- correct types
    

  

No more guessing.

  

### **✔ Transform + preprocess**

  

Perfect for:

- prefixing asset paths
    
- converting strings to numbers
    
- validating image sizes
    
- reshaping coordinates
    

  

### **✔ Centralized schema**

  

All content for a section follows one structure.

---



# **Real Example — Testimonials Collection**

  

## **1. Create folder:**

```
src/content/testimonials/
```

## **2. Add entries:**

```
src/content/testimonials/sarah.md
src/content/testimonials/john.md
```

Example markdown:

```
---
name: "Sarah O’Brien"
instrument: "Piano"
rating: 5
quote: "James’s lessons are amazing."
---
```

## **3. Define the collection schema with Zod:**

  

**src/content/config.ts**

```ts
import { defineCollection, z } from "astro:content";

const testimonials = defineCollection({
  type: "content",
  schema: z.object({
    name: z.string(),
    instrument: z.string(),
    rating: z.number().min(1).max(5),
    quote: z.string().min(20),
  }),
});

export const collections = {
  testimonials,
};
```

## **4. Use it in a component:**

```ts
import { getCollection } from "astro:content";

const testimonials = await getCollection("testimonials");
```

Now you have:

```ts
testimonials: Array<{
  data: {
    name: string;
    instrument: string;
    rating: number;
    quote: string;
  }
}>
```

Fully typed. Fully validated. No mistakes possible.

---
