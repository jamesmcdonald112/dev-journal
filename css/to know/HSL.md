## **What HSL is (and why designers like it)**

  

**HSL = Hue, Saturation, Lightness**

  

Example:

```
--primary: hsl(220 90% 56%);
```

Designers like HSL because:

- **Hue** = the actual color (blue, red, green)
    
- **Saturation** = how intense it is
    
- **Lightness** = how light/dark it is
    
- You can **systematically generate shades** (lighter/darker) by changing one number
    
- Dark mode is easier (often just adjust lightness)
    

  

HSL is ideal for:

- Custom design systems
    
- Theme generation
    
- Brand-heavy products
    
- Teams with strong design input
    

---

## **What you’re using now (Tailwind palette aliases)**

  

Example:

```
--color-primary: var(--color-blue-600);
--color-primary-hover: var(--color-blue-700);
```

This means:

- Tailwind already solved color scales for you
    
- You’re **aliasing semantics** (primary, muted, destructive) to Tailwind colors
    
- You get consistency without thinking about color math
    

  

This is ideal when:

- You are **not a designer**
    
- You want speed and predictability
    
- You want to ship many sites
    
- You want fewer decisions per project
    

---

## **Why Tailwind is the right choice for you (right now)**

  

Your goals:

- Maintain ~100 sites
    
- Move fast
    
- Avoid design-system overhead
    
- Stay consistent
    

  

Tailwind wins because:

- Zero color math
    
- Built-in contrast-tested palettes
    
- Easy hover/focus states
    
- Easy refactors (change one alias)
    

  

You’re using tokens **semantically**, not visually — that’s correct.

---

## **When HSL becomes worth switching to**

  

Switch to HSL tokens **only if**:

- You need advanced theming (light/dark/custom themes)
    
- You want runtime color manipulation
    
- You’re building a reusable design system outside Tailwind
    
- A designer is actively shaping the palette
    

  

Until then: **don’t over-engineer**.

---

## **Rule of thumb (keep this)**

- **Tailwind palette tokens** → speed, pragmatism, scale
    
- **HSL tokens** → control, design systems, theming
    
