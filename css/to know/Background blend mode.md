**background-blend-mode — note**

- background-blend-mode controls **how multiple background layers of the same element are blended together**.
    
- Works when you have **more than one background** (e.g. background-image + background-color, or multiple images).
    
- Often used to **tint or darken images** without extra elements.
    

  

Example use:

```
.hero {
  background-image: url(hero.jpg);
  background-color: rgba(0,0,0,0.5);
  background-blend-mode: multiply;
}
```

Common values:

- multiply → darkens image
    
- screen → lightens
    
- overlay → increases contrast
    
- darken
    
- lighten
    
- normal → no blending
    

  

Important:

- background-blend-mode only blends **background layers of the same element**.
    
- To blend an element with **other elements behind it**, use mix-blend-mode.