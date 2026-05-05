# Dynamic HTML Tags in Astro

## The Pattern

```astro
const Tag = `h${level}` as "h1" | "h2" | "h3";
```

Then use it in the template like a regular component:

```astro
<Tag class="section-header__heading">Heading text</Tag>
```

## How It Works

A template literal builds the tag name string at runtime — `h1`, `h2`, or `h3` depending on the `level` prop. The `as "h1" | "h2" | "h3"` TypeScript assertion narrows the type from `string` to the union of valid values, which is required because JSX/Astro only accepts known tag names as element constructors.

## Why It's Useful

- **Semantic HTML** — the heading level is correct in the DOM (`<h1>`, `<h2>`, `<h3>`) rather than a styled `<div>`
- **Reusable component** — one `SectionHeader` component works on every page, with callers controlling the heading level via a prop
- **Accessible** — screen readers use heading levels to build the page outline; hardcoding `<h2>` everywhere would break that
- **Type safe** — TypeScript catches invalid values at compile time (`level={5}` would error)

## Real Example — SectionHeader.astro

```astro
---
interface Props {
  level?: 1 | 2 | 3;
  heading: string;
}

const { level = 2, heading } = Astro.props;
const Tag = `h${level}` as "h1" | "h2" | "h3";
---

<header class="section-header">
  <Tag class="section-header__heading">{heading}</Tag>
</header>
```

The services index page uses `level={1}` because it's the main page heading. All other sections default to `level={2}`.

## When to Use This Pattern

Use it any time a component renders a heading and the correct level depends on context:
- Section headers used on both standalone pages (need `h1`) and within pages (need `h2`)
- Card titles that might be `h2`, `h3`, or `h4` depending on where the card appears
- Any component where heading hierarchy matters for accessibility

## Variation — Any Element

The same pattern works for any HTML element:

```astro
const El = tag as "div" | "section" | "article";
```

Useful for semantic wrappers where the element type is a prop (e.g. a `<section>` on content pages, a `<div>` inside another `<section>`).
