### **Ongoing maintenance needed for Astro?**

  

**Almost none.**

At most:

- Occasional dependency updates (every few months)
    
- Checking forms still submit properly
    
- Updating business info if it changes
    

  

This is _very low_ maintenance compared to WordPress.


# Astro Security Problems  problems?

  

If you use **Astro + a safe third-party form handler (Resend, Netlify Forms, Formspree) + Calendly**, then:

  

### **✔ You avoid** 

### **all**

###  **the WordPress-style hacks**

  

Because:

- No admin panel
    
- No plugins
    
- No PHP
    
- No database
    
- No login system
    

  

There is nothing for attackers to take over.

  

### **The only things you must avoid:**

  

**(And these are under your control as the developer)**

  

#### **• Misconfigured form endpoint**

If you accidentally expose your API endpoint publicly, someone could spam it.

Easy fix: rate-limit or use a trusted service.

  

#### **• Insecure third-party embed**

If you embed a shady script (you won’t).

Calendly is safe.

  

#### **• Bad hosting configuration**

If you store secrets in the client code.

Avoid by using environment variables.

  

#### **• Exposed API keys**

If you upload a secret to GitHub accidentally.

Don’t do that.
