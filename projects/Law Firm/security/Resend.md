# **Are contact forms safe if we use Resend → Gmail?**

  

Yes. This setup is **very safe**, and it’s one of the most secure options for a solicitor’s website.

---

### **Why it’s safe**

- **HTTPS encryption:** All form data is encrypted while being sent.
    
- **Resend only forwards the message:** It processes the email and passes it to Gmail; it does not store personal data long-term unless you explicitly turn that on.
    
- **Gmail is secure:** The final message lands in a protected inbox.
    
- **The website stores nothing:** No personal information ever sits on your server or in your code — so there’s nothing for attackers to steal.
    

---

### **How the setup works (simple explanation)**

  

**Astro (static website)**

- Shows the pages (Home, About, Contact).
    
- Completely static → extremely hard to hack.
    

  

**Astro Serverless API Route**

- A small backend function that runs only when the form is submitted.
    
- Keeps the Resend API key **private** — never exposed to visitors.
    
- Prevents people from directly sending spam to your email by bypassing the form.
    

  

**Resend**

- Safely sends the email using its secure infrastructure.
    
- Passes the message to Gmail.
    

  

**Gmail inbox**

- The solicitor receives the enquiry securely.
    

  

So the full chain is:

  

**Astro page → Serverless API route → Resend → Gmail**

---

### **What this setup avoids**

- No database to hack
    
- No admin login for attackers to break into
    
- No WordPress plugins (the most common attack point)
    
- No server running 24/7 that can be exploited
    
- No personal data stored on the website
    

  

This dramatically reduces the risk compared to traditional WordPress contact forms.

---

### **Why this matters (especially for a solicitor)**

  

Solicitors handle personal and sometimes sensitive information.

With this setup:

- Even if the website is attacked, **there is nothing stored on the site to leak**.
    
- All sensitive handling is done by secure providers (Resend + Gmail).
    
- The Astro site remains static, fast, and very difficult to compromise.
    

  

This is one of the safest modern form setups you can build today.