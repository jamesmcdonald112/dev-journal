---
title: Testing Environment Setup
type: case-study
status: draft
date: 2025-10-20
updated: 2025-10-20
summary:
tech_stack:
keywords:
---
# Testing Environment Setup

## What I Learned
Setting up a modern testing environment for a Next.js project using **Vitest**, **jsdom**, and **React Testing Library**.

Vitest provides fast, Vite-based unit testing, jsdom simulates a browser environment for component rendering, and React Testing Library helps verify UI behaviour in a user-focused way.
## Example / Code Snippet
```ts
// vitest.config.ts
import { defineConfig } from 'vitest/config'
import react from '@vitejs/plugin-react'
import tsconfigPaths from 'vite-tsconfig-paths'

export default defineConfig({
  plugins: [tsconfigPaths(), react()],
  test: {
    environment: 'jsdom',
  },
})
```

```tsx
// __tests__/page.test.tsx
import { render, screen } from '@testing-library/react'
import '@testing-library/jest-dom'
import Page from '../app/page'

test('renders heading', () => {
  render(<Page />)
  expect(screen.getByRole('heading', { name: /home/i })).toBeInTheDocument()
})
```
## Why It Matters
A proper test environment ensures that every UI component behaves correctly before deployment.
This setup mirrors real browser behaviour while staying lightweight and fast, enabling Test-Driven Development (TDD) across your Next.js projects.

## Related 
- [[learning-notes/Vitest]]
- [[learning-notes/Jsdom]]
- [[learning-notes/React Testing Library]]
- [[learning-notes/User-Event]]
- [[learning-notes/Jest-Dom]]

## References
- https://nextjs.org/docs/app/guides/testing/vitest
- https://vitest.dev/
- https://testing-library.com/docs/react-testing-library/intro/
- https://testing-library.com/docs/user-event/intro/
- https://testing-library.com/docs/ecosystem-jest-dom/
- https://github.com/jsdom/jsdom