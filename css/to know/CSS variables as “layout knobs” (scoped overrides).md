You define a utility/composition rule that uses a variable with a fallback:

```css
.cluster { gap: var(--gap, 1rem); }
.flow > * + * { margin-block-start: var(--flow-space, 1rem); }
```

Then you override the variable **only in the context that needs it**.

  

### **Option A — override in CSS (recommended for repeated sections)**

```css
.call-to-action {
  --gap: 5rem;             /* overrides cluster/stack gap */
  --flow-space: 1.5rem;    /* overrides flow rhythm */
  background: pink;
  padding: 2rem;
  border-radius: 1rem;
}
```

Usage:

```html
<section class="call-to-action flow cluster">
  ...
</section>
```

### **Option B — override inline with** 

### **style=""**

###  **(use sparingly)**

  

Good for one-off tweaks or CMS-driven values.

```html
<section class="flow" style="--flow-space: 1.5rem">
  ...
</section>
```

Also works with gap:

```html
<div class="cluster" style="--gap: 2rem">
  ...
</div>
```

### **Rules of thumb**

- **Use CSS class overrides** when you’ll repeat the pattern (cleaner, reusable).
    
- **Use** **style=""** only for true one-offs or dynamic values.
    
- Variables like --gap / --flow-space are **not global tokens**. They’re **controls**.
    
