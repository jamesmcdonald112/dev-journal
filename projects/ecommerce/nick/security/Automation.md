# **✅** 

# **1. What you CAN automate (Website-side automation)**

  

These are things you can 100% automate using Astro, serverless functions, JSON configs, and build scripts.

---

## **1. Schema Markup Automation**

  

You create **one config file**, e.g.:

  

src/config/business.json

  

Example:

```
{
  "name": "McDonald Solicitors",
  "address": "123 High Street, Kilkenny",
  "phone": "+3531234567",
  "type": "LegalService"
}
```

Then your layout imports it:

```
<script type="application/ld+json">
  {JSON.stringify(schemaData)}
</script>
```

### **What this automates:**

  

If the business ever moves or changes phone number → you only update **one file**.

Schema updates across the whole site automatically.

---

## **2. Auto-generated sitemap**

  

Astro plugins can generate a new sitemap on every build.

  

### **What this automates:**

- New pages automatically appear
    
- Old pages automatically drop
    
- Google gets a clean sitemap without you touching anything
    

  

No manual work.

---

## **3. Automated meta titles + descriptions**

  

You can store SEO metadata in:

- one JSON file
    
- frontmatter
    
- a CMS (if you use one)
    

  

Example in Astro:

```
{
  "title": "Solicitor in Kilkenny",
  "description": "Expert legal services in Kilkenny..."
}
```

Your layout pulls from this automatically.

  

### **What this automates:**

  

No rewriting meta tags manually.

---

## **4. Automatic OpenGraph (social sharing) images**

  

You can generate OG images dynamically using:

- Vercel OG Image
    
- Satori
    
- Astro integrations
    

  

This is optional, but it looks professional.

---

## **5. Automatic builds + deployment**

  

Using Netlify or Vercel:

- Push change to GitHub → site updates itself
    
- No server maintenance
    
- No manual deploys
    

---

## **6. Form validation + spam protection automation**

  

You can:

- use Recaptcha
    
- rate-limit form submissions
    
- reject spam automatically
    
- log errors in a serverless function
    

  

No manual work managing spam.

---

## **7. Uptime monitoring**

  

Use:

- UptimeRobot
    
- BetterStack
    

  

Set it once → automatic.

Alerts you if the site goes down.

  

(Static Astro sites almost never go down.)

---

# **🚫** 

# **2. What you CANNOT automate (Google-side limitations)**

  

These are tasks controlled by Google’s systems — you cannot automate them because Google blocks automation for security and anti–spam reasons.

---

## **1. Google Business Profile updates**

  

You **cannot** automatically:

- change hours
    
- upload photos
    
- respond to reviews
    
- update services
    

  

Google intentionally blocks automation to prevent spam and fake businesses.

---

## **2. Local directory citations**

  

You **cannot** auto-update:

- GoldenPages
    
- Yelp
    
- Cylex
    
- Local solicitor directories
    
- Law associations
    

  

They all have different systems and intentionally block bots.

---

## **3. Monitoring ranking in Google Maps**

  

You can’t automatically:

- check where they rank for “solicitor near me”
    
- track local pack movement
    
- measure competitor ranking
    

  

Google blocks automated scraping → against their TOS.

  

You can manually check or use paid tools (€50–150/month), but not automate it for free.

---

## **4. Fixing citation issues**

  

If another website has:

- wrong phone number
    
- old address
    
- duplicate listing
    

  

You must fix it manually.

---

# **⚠️** 

# **3. What you can SEMI-automate (partial automation)**

  

These can be automated _inside the website_ but not externally.

---

## **1. Business info updates**

  

You can create:

src/config/business.json

  

This controls:

- schema
    
- contact page
    
- footer
    
- meta tags
    
- headings
    

  

Change ONE file → updates everywhere.

  

This saves you hours.

---

## **2. Checking for broken links**

  

You can use:

- GitHub Actions
    
- Link checkers
    

  

They run automatically on every deploy.

  

If something breaks → you get a notification.

---

## **3. SEO health checks**

  

You can use automated tools:

- Ahrefs Webmaster Tools (free)
    
- Google Search Console
    
- PageSpeed Insights API
    

  

Running automatically, but interpreting results still requires a human.

---

# **💰** 

# **4. What maintenance remains if everything is automated?**

  

For an Astro site:

- Occasional dependency updates (once every 6–12 months)
    
- Update business info if they change office/number
    
- Fix form-related issues if Resend changes something
    
- Very rare bug fixes
    

  

For Local SEO:

- Uploading GBP photos (they can do it themselves)
    
- Replying to reviews (they can do it themselves)
    
- Checking citations
    
- Handling ranking drops
    
- Reporting spam/fake reviews
    

  

This is why ongoing Local SEO maintenance is a **service**, not a technical necessity.

---

# **🧾** 

# **5. How you can package this as a service**

  

You can offer **two options**:

---

## **Option A — “DIY Local SEO” (No monthly fee)**

  

Tell them:

- “I will automate everything on the website so you never need to touch it.”
    
- “You will manage your Google Business Profile yourselves.”
    
- “If you ever move office or change phone numbers, I update the site – you pay per-hour (€40).”
    

  

Perfect for clients who want low cost.

---

## **Option B — “Fully Managed Local SEO” (€30–€70/month)**

  

You handle:

- GBP photo updates
    
- Review replies (or at least monitoring)
    
- Fixing citation errors
    
- Ranking check-ins
    
- Search Console monitoring
    

  

This is worth money because **they won’t do it themselves**.

---

# **🔥** 

# **6. Final Summary — Automation Reality**

  

### **You CAN automate:**

- Schema
    
- Sitemaps
    
- Meta SEO
    
- Titles/descriptions
    
- OG images
    
- Builds/deploys
    
- Broken link checks
    
- Uptime monitoring
    
- Form validation + spam filtering
    

  

### **You CANNOT automate:**

- GBP updates
    
- Reviews and Q&A
    
- Citations
    
- Directory fixes
    
- Local ranking checks
    

  

### **Astro reduces your workload massively**

  

Maintenance for the site becomes:

- 1–2 small updates per year
    
- plus optional SEO/GBP help if they pay for it
    

---

If you want, I can help you design:

- A full automation plan
    
- A maintenance package
    
- A proposal for the client
    
- A pricing model that makes you money with minimal work
    

  

Just say the word.