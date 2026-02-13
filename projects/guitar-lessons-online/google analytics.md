### **What we did (high-level, reusable)**

  

You created **GA4 (Google Analytics)** and implemented it in a **GDPR-safe way**:

- GA tag is **present on every page** (in `<head>`), so Google’s “tag detection” works.
    
- Tracking is **blocked by default** using **Consent Mode v2**.
    
- When the visitor clicks **Accept**, you call gtag('consent','update', ...) to allow analytics (and optionally ads).
    

  

That’s why you finally saw:

- region1.google-analytics.com/g/collect in Network
    
- 1 active user in Realtime
    

---

## **What I can and can’t “remember” about your GA setup**

  

I can’t reliably know the exact dropdown options you clicked in GA’s onboarding (industry category, objectives, etc.) unless you paste them. Also: those choices mostly affect recommendations, not whether GA works.

  

So: below is the **sane default selection** you should use again + what to pick for a solicitor site.

---

# **What to select for** 

# **Mary Molloy Solicitors**

#  **(GA4 setup choices)**

  

### **Account**

- **Account name:** something like Mary Molloy Solicitors
    
- Data sharing toggles: **turn OFF anything you don’t need** (keep it minimal).
    
    If you’re unsure: leave only **Technical support** on.
    

  

### **Property**

- **Property name:** Mary Molloy Solicitors Website
    
- **Time zone:** Ireland
    
- **Currency:** EUR
    

  

### **Business details**

- **Industry category:** choose **Legal services** (or closest option like “Professional services” if Legal isn’t listed)
    
- **Business size:** **Small (1–10)**
    

  

### **Business objectives (pick max 2)**

  

For a solicitor website, the only ones that matter are:

1. **Generate leads** (contact form, click-to-call, email clicks)
    
2. **Understand web traffic** (channels, landing pages, SEO results)
    

  

Do **not** pick “Drive sales” unless you’re taking payments online.

  

### **Data stream**

- **Stream type:** Web
    
- **Website URL:** use the real domain if live; otherwise use the current deploy URL temporarily
    
- **Stream name:** marymolloy.ie – web (or similar)
    

---

# **Step-by-step notes: how to add GA to the site (Next.js / v0)**

  

## **A) Create the GA4 property + get the Measurement ID**

1. Create account + property + web stream
    
2. Copy Measurement ID: G-XXXXXXXXXX
    

  

## **B) Add Measurement ID to env**

1. Add to Vercel env vars:
    
    NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX
    
2. Also add to .env.local for dev
    

  

## **C) Put GA snippet in** 

## **`<head>`**

##  **(App Router:** 

## **app/layout.tsx**

## **)**

  

Use Next’s `<Script>` and the **exact order**:

1. **Consent default** (beforeInteractive) — deny everything by default
    
2. Load gtag.js (afterInteractive is fine)
    
3. gtag('js') + gtag('config') (afterInteractive)
    

  

Key point: **Tag must exist on every page**, even if consent denies storage.

  

## **D) Cookie banner**

  

Your cookie banner must do two things:

  

**On Accept**

- Save choice (localStorage or cookie)
    
- Call:
    

```
gtag('consent','update',{
  analytics_storage:'granted',
  ad_storage:'denied',
  ad_user_data:'denied',
  ad_personalization:'denied'
})
```

(For a solicitor site, keep ads denied unless you’re running Google Ads.)

  

**On Reject**

- Save choice
    
- Do nothing else (defaults stay denied)
    

  

**On page load**

- If user previously accepted → call the same consent update on mount.
    

  

## **E) Verify**

1. GA4 → Realtime → open site → accept → you should see active user
    
2. DevTools → Network → confirm requests like:
    
    region1.google-analytics.com/g/collect
    

---

# **If Mary Molloy is an Astro/static site (not Next.js)**

  

Same logic, different placement:

- Put the consent-default script + GA script in the global layout head (Astro: src/layouts/...astro)
    
- Cookie banner JS calls gtag('consent','update', ...) on accept
    

---

# **What you should store as your “standard policy”**

  

For Irish/EU sites without Google Ads:

- Consent default: **deny analytics_storage**
    
- Accept: **grant analytics_storage**
    
- Keep all ad-related consent **denied**
    
- Track conversions later (contact form submit, click-to-call, email clicks)
    

---

If you tell me what platform Mary Molloy is built on (Astro/Next/Webflow), I’ll give you the exact file locations and the exact snippet structure for that platform.