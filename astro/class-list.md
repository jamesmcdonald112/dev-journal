# class:list in Astro

`class:list` is an Astro directive that builds a `class` attribute from multiple values. It handles conditional classes cleanly without string concatenation.

## Syntax

```astro
<element class:list={[...values]} />
```

Accepts an array of:
- **Strings** — always included
- **Objects** — key is the class name, value is a boolean (included when `true`)
- **Arrays** — nested arrays, flattened
- **Falsy values** — `false`, `null`, `undefined` are ignored

## Real Example — TestimonialBlock.astro

```astro
<section class:list={["section testimonials", { "testimonials--dark": dark }]}>
```

- `"section testimonials"` — always applied
- `{ "testimonials--dark": dark }` — only applied when the `dark` prop is `true`

**Output when `dark={false}`:**
```html
<section class="section testimonials">
```

**Output when `dark={true}`:**
```html
<section class="section testimonials testimonials--dark">
```

## Why Use It

Without `class:list` you'd write:

```astro
<section class={`section testimonials${dark ? " testimonials--dark" : ""}`}>
```

That works but becomes hard to read with multiple conditions. `class:list` scales cleanly:

```astro
<section class:list={[
  "section",
  "testimonials",
  { "testimonials--dark": dark },
  { "testimonials--compact": compact },
  { "testimonials--featured": featured },
]}>
```

## BEM Pattern

The object syntax maps naturally to BEM modifiers — the block class is always in the string, modifiers are conditionally toggled via the object:

```astro
<div class:list={[
  "service-card",
  { "service-card--featured": featured },
  { "service-card--loading": loading },
]}>
```

## Compared to className in React

Same concept as React's `className` with a helper like `clsx` or `classnames`, but built into Astro natively — no package needed.
