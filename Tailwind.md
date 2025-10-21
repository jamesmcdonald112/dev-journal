


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