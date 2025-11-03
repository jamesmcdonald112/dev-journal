


add custom style - https://tailwindcss.com/docs/adding-custom-styles

In your **globals.css** (or another imported stylesheet), you can write:
```css
@import "tailwindcss";

@theme {
  --color-primary: #2563eb;
  --color-secondary: #9333ea;
  --color-danger: #ef4444;
  --color-success: #22c55e;
  --color-warning: #f59e0b;
}
```


if yo uget an error in layout.tsx due to imporitng globals.css, do this

Add a declaration file to silence the TS warning (optional but clean):
```ts
// types/global.d.ts  (or css.d.ts)
declare module '*.css';
```