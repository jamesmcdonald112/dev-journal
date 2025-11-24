Install:
```bash
npm install -D tsx
```

## **What TSX gives me**

- Run .ts files instantly
    
- No config
    
- Full ESM support (matches Astro’s environment)
    
- Perfect for quick debugging
    
- Cleaner and simpler than ts-node

Example file with console.log():
```ts
import { contactSchema } from "./contact.schema";

const result = contactSchema.safeParse({
  // put your test data here
  name: "",
  email: "not-an-email",
  message: "",
});

if (result.success) {
  console.log("✅ OK:", result.data);
} else {
  console.log("❌ FAILED");
  console.dir(result.error.format(), { depth: null });
}
```

Run files:
```bash
npx tsx debug/zod/contact-form-test.ts
```


## Optional Scripts
```json
"scripts": {
  "test:schema": "tsx debug/zod/contact-form-test.ts"
}
```

Use:
```bash
npm run test:schema
```