---
title: Creating Pages in Next.js 14+
type: setup-guide
status: draft
date: 2025-10-22
updated: 2025-10-22
summary:
tech_stack:
keywords:
---
# Creating Pages in Next.js 14+

## What I Learned
To create a new page in **Next.js 14+**, it must live inside the /app directory as a **folder** containing a page.tsx file.

This follows the **App Router** conventions (introduced in Next.js 13), replacing the older /pages router used in previous versions.

## Example / Code Snippet
```
src/
  app/
    page.tsx              → renders `/`
    notes/
      page.tsx            → renders `/notes`
    layout.tsx            → wraps all pages
```

## Why It Matters
This structure ensures correct routing and layout inheritance across your app.
If you place pages under /pages or deviate from this pattern, Next.js will ignore them — leading to missing routes or unexpected errors.

## Related 
- [[Next.js Routing]]
- [[App Router]]

## References
- - [[https://nextjs.org/docs/app/getting-started/layouts-and-pages#the-app-directory]]
