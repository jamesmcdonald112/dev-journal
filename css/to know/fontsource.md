https://fontsource.org/fonts/source-serif-4/install

## **Issues faced**

  

### **Why the Lighthouse issue occurred (CLS / layout shift)**

- Lighthouse flagged **CLS (Cumulative Layout Shift)** because text moved **after the page had already rendered**, without any user interaction.
    
- This happened because the page initially rendered using a **fallback font**, and then switched to the real fonts once they finished loading.
    
- The font swap caused the heading text (especially the hero `<h1>`) to change size, which shifted the layout.
    

---

## **What fixed it (in plain terms)**

  

### **1. Fonts were loading too late**

- The fallback font loaded first.
    
- The real fonts (Inter and Source Serif 4) were imported via CSS / layout and only discovered **after first paint**.
    
- When the real fonts finally loaded, the browser re-rendered the text → layout shift.
    

---

### **2. Move fonts to** 

### **public/**

- Fonts were moved into public/fonts/ so they have **stable, predictable URLs** like:
    

```
/fonts/inter/inter-latin-wght-normal.woff2
```

-   
    
- This allows the browser to request them immediately, without waiting for CSS parsing.
    
- To do this:
    
    - DevTools → **Network** tab
        
    - Filter by **Font**
        
    - Identify the exact .woff2 files actually being used
        
    - Copy only those files from node_modules/@fontsource-variable/... into public/fonts/
        
    

---

### **3. Define fonts with** 

### **@font-face**

- @font-face tells the browser:
    
    - Which font family name maps to which file
        
    - What weight range the font supports
        
    
- This was placed in reset.css so it loads **before any content renders**.
    
- Without @font-face, the browser can’t reliably match preloaded fonts to rendered text.
    

---

### **4. Preload the exact** 

### **.woff2**

###  **files**

- `<link rel="preload" as="font"> was added in <head> `for each font.
    
- This forces the browser to download fonts **before rendering any text**.
    
- Because the font is already available, there is no fallback → real font swap.
    

---

### **5. Result**

- Text renders correctly on the **first paint**
    
- No layout shift
    
- Lighthouse CLS issue resolved
    

---

## **The minimal mental model**

- **Fallback font → real font = layout shift**
    
- **Preload +** **@font-face** **= stable layout**
    

  

That’s it.

---

## **Key rules to remember**

- Use **.woff2** **only**
    
- Only preload **fonts you actually use**
    
- Put font files in public/fonts/
    
- Preload fonts in `<head>`
    
- @font-face URLs **must match preload URLs exactly**
    

---

## **Short questions (only the important ones)**

  

Answer briefly.

1. What causes CLS when fonts load late?
    
2. Why preload fonts instead of relying on CSS imports?
    
3. Why move fonts to public/?
    
4. What breaks if the preload URL and @font-face URL don’t match?
    
5. What does font-display: swap do?
    

  

Once you answer these, you are **finished with fonts** and we move on.