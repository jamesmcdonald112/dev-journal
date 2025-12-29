
### **Vercel adapter + local runtime testing**

- Astro + @astrojs/vercel runs in **Vercel’s serverless environment** in production.
    
- npm run dev is Astro’s dev server (not identical to Vercel runtime).
    
- npx vercel dev runs a local dev server that emulates Vercel routing/runtime more closely **and can pull your env vars**.
    

  

**Recommended workflow:**

1. Build confidence locally: npm run dev
    
2. If it’s Vercel-specific (headers/rewrites/env/serverless): npx vercel dev
    
3. Final proof: deploy and test on the real domain
    

  

**Env vars locally:**

```
npx vercel env pull .env.local
```

This pulls your Vercel env vars into .env.local so local runs match production more closely.

---
