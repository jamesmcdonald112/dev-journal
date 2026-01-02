**Docs**

- CUBE site: https://cube.fyi/
    

  

### **What CUBE is**

  

CUBE is a **way to organise CSS decisions** so your codebase stays predictable as it grows. It doesn’t change CSS. It just answers:

- _What layer is this?_
    
- _Where should it live?_
    
- _How should I name it?_
    

  

### **CUBE = Composition · Utility · Block · Exception**

  

#### **1) Composition**

**Purpose:** layout “skeleton” rules that control **structure and spacing between elements** (page layouts + internal component layout rhythms).

  

**Examples**

- .wrapper (page width + padding)
    
- .flow (vertical rhythm between children)
    
- .cluster (inline group that wraps)
    
- Every Layout patterns (Stack, Cluster, Sidebar, Switcher, Center, etc.)
    

  

**Rules**

- No brand styling (no colors, shadows, fancy visuals)
    
- Designed to be reused everywhere
    
- Often uses CSS variables to allow contextual overrides
    

---

#### **2) Utility**

**Purpose:** single-purpose helpers that do **one job well**, anywhere.

  

**Examples**

- .sr-only
    
- .list-reset
    
- .text-center
    
- .uppercase
    
- token utilities like .bg-primary (if you generate them)
    

  

**Rules**

- Small, generic, predictable
    
- Should not become “mini components”
    
- Should not be used as specificity hacks (!important everywhere)
    

---

#### **3) Block**

**Purpose:** the actual “component styling” layer.

  

**Examples**

- .btn, .btn-primary
    
- .card
    
- .form-input
    
- .nav, .hero
    

  

**Rules**

- Represents a real UI component
    
- Can include multiple properties (it’s allowed to be “bigger” than utilities)
    
- Should stay reasonably small and focused (don’t build a whole design system inside one block)
    

---

#### **4) Exception**

**Purpose:** rare variations of a block (usually state-based).

  

**Examples**

- .card[data-state="reversed"] { … }
    
- .button[aria-disabled="true"] { … }
    

  

**Rules**

- Should be uncommon
    
- Typically lives **next to the block** it modifies
    
- If you’re creating lots of exceptions, you probably need a new block variant (or a new block)
    

---

---

**Typical file/folder structure

- reset.css (or normalize.css)
    
- tokens.css (design tokens / CSS variables)
    
- composition.css (Every Layout patterns)
    
- utilities.css (tiny helpers)
    
- blocks/ (component classes: buttons, forms, cards)
    
- exceptions live inside the relevant block (or a small exceptions.css if needed)
    

  

Key idea: **composition and utilities stay stable across all projects**, blocks are where most per-project work happens.

So you don’t “replace Tailwind with CUBE”. You use CUBE as the organising logic.

---
