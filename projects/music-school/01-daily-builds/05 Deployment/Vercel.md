```bash
npm install @astrojs/vercel
```

```ts
import vercel from "@astrojs/vercel";

export default defineConfig({
  output: "server",
  adapter: vercel(),
});
```

**Purpose:**

This installs the **Vercel server adapter** so Astro can run with output: "server" on Vercel. Without it, deploys fail with “NoAdapterInstalled”.

