## **Writing an Accessibility Statement (Law Firm Website)**

  

### **Primary source (authoritative)**

- **W3C – Accessibility Statements Guidance**
	- https://www.w3.org/WAI/planning/statements/
- Accessibility Statement Generation
	- https://www.w3.org/WAI/planning/statements/generator/#create
    

  

This is the correct source to work from.

It explains:

- what an Accessibility Statement **must contain**
    
- how organisations **phrase compliance**
    
- what’s acceptable when you are **partially compliant**
    
- how to show **good-faith effort** without an audit
    

---

## **What is expected (Irish solicitor website context)**

  

For a **small, informational solicitor site**, regulators expect **good-faith compliance**, not perfection.

  

### **They expect:**

- reasonable technical accessibility
    
- evidence you tried
    
- a public Accessibility Statement
    

  

### **They do** 

### **not**

###  **expect:**

- a paid audit
    
- formal certification
    
- WCAG AAA
    
- specialist accessibility tooling
    

---

## **A. Technical “good-faith” requirements**

  

Your site should meet these basics:

  

### **Markup & layout**

- Semantic HTML (nav, main, footer, headings in order)
    
- Logical reading order
    
- No content hidden only by colour
    

  

### **Interaction**

- Fully keyboard navigable
    
- Visible focus states
    
- No keyboard traps (especially menus/dialogs)
    

  

### **Visual**

- Readable colour contrast (WCAG 2.1 AA)
    
- Text resizable to **200%**
    
- Responsive reflow (no horizontal scrolling at ~320px)
    

  

### **Forms (important for law firms)**

- Every input has a visible <label>
    
- Errors are:
    
    - shown in **text**
        
    - explained clearly
        
    - placed near the field
        
    
- Keyboard-only submission works
    

---

## **B. Manual testing (this is required)**

  

This is what counts as **reasonable testing**.

  

You must do:

- Tab through the entire site
    
- Use keyboard only (Tab / Shift+Tab / Enter / Esc)
    
- Open + close mobile menu with keyboard
    
- Resize text to **200%**
    
- Submit a form with missing/invalid input
    
- Confirm focus order makes sense
    

  

If you’ve done this, you can **truthfully say** you tested accessibility.

---

## **C. Automated testing (Lighthouse)**

  

Use Chrome Lighthouse:

- Aim for **90+**, not 100
    
- Fix all **serious** accessibility issues
    
- Ignore cosmetic warnings if justified
    

  

Lighthouse alone is **not enough**, but Lighthouse + manual testing **is**.

---

## **D. What Lighthouse does** 

## **not**

##  **catch (you must check)**

- Keyboard-only navigation works everywhere
    
- Focus order is logical
    
- Mobile menu/dialog is not a keyboard trap
    
- Link text makes sense out of context
    
    (“Read more” → ❌, “Read more about conveyancing” → ✅)
    
- Forms show usable, readable errors
    

---

## **E. Accessibility Statement (what it must contain)**

  

Your statement should include:

  

### **1. Commitment**

- Reference **WCAG 2.1 AA**
    
- State intention to provide accessible service
    

  

### **2. Compliance status**

- Fully compliant / partially compliant (most small sites are **partially**)
    

  

### **3. Testing carried out**

- Lighthouse testing
    
- Manual keyboard testing
    
- Text resize / responsive testing
    

  

### **4. Known limitations (if any)**

- Example: third-party maps, PDFs, embedded content
    

  

### **5. Contact method**

  

**Must be the law firm’s details**, not the developer.

  

Use:

- monitored email (e.g. info@firm.ie)
    
- optional phone number
    

  

### **6. Date last updated**

---

## **Whose details go in the statement?**

  

**The law firm’s. Always.**

  

Reason:

- They are the service provider
    
- They are responsible under GDPR / EAA
    
- This matches Law Society expectations
    

  

Never put your developer contact details.

---

## **Summary: what is “enough”**

  

For a solicitor site:

  

✅ Semantic HTML

✅ Keyboard accessible

✅ Responsive reflow

✅ Accessible forms

✅ Lighthouse 90+

✅ Manual testing

✅ W3C-based Accessibility Statement

  

That is **sufficient, defensible, and standard practice**.

---

If you want next:

- I can write a **copy-paste Accessibility Statement** specifically for a small Irish law firm
    
- or give you a **reusable checklist** you can apply to every client site