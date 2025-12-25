Short answer to your first question:

**Yes**, you should absolutely position it as:

  

> “Modern, extremely safe, low-maintenance sites (no WordPress, no plugins to hack)”

  

…but don’t drown them in the word “Astro”. Lead with outcomes, then mention Astro if they ask _how_ you’re doing it.

  

I’ll give you:

1. **Exactly how to talk about Astro vs WordPress**
    
2. **A step-by-step plan** of what you need to set up, tools included
    

---

## **1. How to talk about Astro vs WordPress**

  

Use this kind of line with solicitors / music schools:

  

> “Most small business sites are built on WordPress, which needs constant plugin and security updates and is the main way sites get hacked.

>   

> I build your site on a modern, static setup (I use a framework called Astro). There’s no login area, no plugins, and no database to attack. It’s extremely fast, very hard to hack, and much cheaper to maintain. That’s why my monthly fee is low — there’s far less that can break.”

  

That’s enough. If they ask technical questions, _then_ you say:

  

> “Astro prebuilds the pages into static files, and we only use a tiny secure function for your contact form. Compared to WordPress, there’s almost no attack surface.”

---

## **2. Step-by-step: what you should set up (and tools you need)**

  

Think of this in **phases**.

---

### **Phase 0 – Define your offer (on paper)**

  

**Goal:** Know exactly what you’re selling before talking to firms.

1. Decide your base package:
    
    - 5–10 page site
        
    - Professional design
        
    - Contact/consult form
        
    - On-page SEO + local SEO setup
        
    - Deployment + handover
        
    
2. Lock in your prices:
    
    - Website build: **€1,800–€2,500**
        
    - Maintenance (optional): **€40–€50/month**
        
    - Hourly for big changes: **€40–€60/hour**
        
    
3. Write 3 bullet “positioning” lines:
    
    - “Fast, modern, highly secure (no WordPress, no plugins)”
        
    - “Low, predictable maintenance — or none if you prefer hourly only”
        
    - “You own the domain, hosting and site — no lock-in”
        
    

  

**Tools:** Google Docs / Notion / Obsidian – whatever you use to write.

---

### **Phase 1 – Build your base Astro starter**

  

**Goal:** One rock-solid Astro project you can reuse for every client.

4. Create a fresh Astro project:
    
    - npm create astro@latest
        
    - Add:
        
        - TailwindCSS
            
        - Your basic layout (Header, Footer, Main layout)
            
        - Components for:
            
            - Hero
                
            - Services list
                
            - Team cards
                
            - Testimonials
                
            - Contact section
                
            
        
    
5. Implement your “standard contact form”:
    
    - Form fields: name, email, phone, message
        
    - Astro serverless route (e.g. /api/contact.ts)
        
    - Integrate with **Resend → Gmail** as discussed:
        
        - Env vars for API key and destination email
            
        - Basic validation + spam protection
            
        
    
6. Bake in basic SEO utilities:
    
    - A helper to set:
        
        - <title>
            
        - <meta name="description">
            
        - OpenGraph tags
            
        
    - A JSON config file for site info
        
    
7. Add local schema support:
    
    - business.json with fields like name, address, phone, url, @type: "LegalService" or "MusicSchool"
        
    - Layout injects a <script type="application/ld+json"> with this schema
        
    
8. Deploy this starter to **your own Vercel or Netlify** for testing only.
    

  

**Tools:**

- Astro
    
- TailwindCSS
    
- Resend
    
- Vercel or Netlify
    
- GitHub
    

---

### **Phase 2 – Get a design system you can reuse**

  

**Goal:** Not starting “from zero design” every time.

9. Hire 1 good designer on **Upwork** to create:
    
    - A Figma “Solicitor Site Kit”
        
    - A Figma “Music School Site Kit”
        
        Each kit should include:
        
    - Home, Services, About, Contact, Team pages
        
    - Variants of hero sections
        
    - Button styles, cards, form styles
        
    - 2–3 colour palettes
        
    

  

Budget: **€300–€500 per kit**.

10. Implement the chosen kit into your base Astro project:
    
    - Translate Figma styles → Tailwind config (colours, fonts, spacing)
        
    - Create components that match the design kit
        
    

  

Now every future site for that niche = 80% reuse.

  

**Tools:**

- Upwork
    
- Figma
    

---

### **Phase 3 – Set up your “ops” and automation**

  

**Goal:** Make maintenance near-zero.

11. Put your starter project in a GitHub repo.
    
12. Configure **Dependabot**:
    

  

- .github/dependabot.yml to update npm dependencies monthly
    

  

13. Set up basic monitoring:
    

  

- **UptimeRobot** free plan to ping each client site
    
- A simple Google Sheet listing:
    
    - Client name
        
    - Domain
        
    - Hosting platform
        
    - Maintenance: Yes/No
        
    - Start date, price
        
    

  

**Tools:**

- GitHub
    
- Dependabot
    
- UptimeRobot
    
- Google Sheets
    

---

### **Phase 4 – Build your “sales toolkit”**

  

**Goal:** Have everything ready before talking to firms.

14. Outreach email template (for solicitors):
    

  

- Short, value-first (“I noticed a few performance/SEO/security issues, happy to send a short audit and redesign proposal”)
    

  

15. Audit template:
    

  

- A Google Docs template you copy per firm with:
    
    - Lighthouse performance screenshots
        
    - 5–10 SEO issues
        
    - 3–5 UX issues
        
    - Security concerns (if WordPress)
        
    - One page at the end: “What I would do and exact price”
        
    

  

16. Simple proposal/contract:
    

  

- 50% upfront, 50% on completion
    
- Two revision rounds included
    
- Maintenance optional, separate from build fee
    
- Ownership: they own domain, hosting, site
    

  

**Tools:**

- Google Docs (export as PDF)
    
- Any e-signature tool (SignWell, HelloSign, etc.)
    

---

### **Phase 5 – Lead generation & outreach**

  

**Goal:** Start getting actual solicitor and music school clients.

17. Build a list:
    

  

- Use Law Society directory + Google Maps to list local/regional firms
    
- Columns: Firm, Contact name, Email, Website, Notes (outdated? WP? etc.)
    

  

18. For each firm:
    

  

- Quickly run **Lighthouse** (Chrome devtools) on their homepage
    
- Note:
    
    - Mobile performance score
        
    - “First Contentful Paint”
        
    - If it’s obviously WordPress (view source, /wp-content/ etc.)
        
    

  

19. Send your outreach email:
    

  

- Mention:
    
    - You specialise in modern, secure, low-maintenance sites
        
    - You can reduce their monthly costs vs WordPress maintenance
        
    - Offer a **free short audit** and a fixed-price redesign
        
    

  

20. When someone replies “Yes, send the audit”:
    

  

- Fill in the audit template
    
- Include:
    
    - Current issues (performance, SEO, security)
        
    - How your approach fixes them:
        
        - Static, hack-resistant build (Astro, no plugins)
            
        - Faster loading (clients less likely to bounce)
            
        - Lower maintenance fee
            
        
    - **Exact offer**: e.g.
        
        “€2,000 one-time, optional €40/month maintenance, cancel any time.”
        
    

  

Optional: 1 quick Figma mock of a homepage hero to visually hook them.

---

### **Phase 6 – Closing & delivery**

  

**Goal:** Finish projects smoothly and get recurring revenue set up.

21. Discovery call:
    

  

- Clarify:
    
    - Their services
        
    - Location(s)
        
    - What they hate about their current site
        
    - What success looks like for them (more enquiries, better image, etc.)
        
    

  

22. Send proposal/contract:
    

  

- Reiterate:
    
    - One-time cost
        
    - Optional maintenance plan
        
    - Timeline (e.g. 3–4 weeks)
        
    - Revision policy
        
    

  

23. Take **50% upfront** before starting.
    
24. Set up hosting in **their** Vercel/Netlify account:
    

  

- They create the account
    
- They invite you as collaborator
    
- You connect GitHub and deploy
    

  

25. Build using your Astro starter:
    

  

- Apply their branding, logo, text, photos
    
- Configure schema for their business
    
- Set up Resend/Gmail integration for forms
    
- Test forms and performance
    

  

26. Final review + revisions:
    

  

- Two revision rounds max (anything beyond = hourly)
    

  

27. Take final 50% payment, then:
    

  

- Switch domain DNS to new site
    
- Record details in your Google Sheet
    
- Record if they’re on maintenance
    

  

28. Record a **short Loom video** showing:
    

  

- How to request edits from you
    
- How to manage Google Business Profile themselves
    
- Where everything lives (domain, hosting, email).
    

  

**Tools:**

- Vercel/Netlify (client’s account)
    
- Loom for training videos
    
- Stripe/Revolut Business/PayPal for payments
    

---

### **Phase 7 – Maintenance**

  

**Goal:** Make the €40–€50/month basically passive.

29. For maintenance clients:
    

  

- Set up recurring invoice (monthly or yearly)
    
- Once a month or quarter:
    
    - Merge Dependabot PRs (if any)
        
    - Check Lighthouse quickly
        
    - Make any small requested changes
        
    
- Track time. If one client starts abusing it:
    
    - Tell them future larger changes are hourly.
        
    

  

30. For non-maintenance clients:
    

  

- If they email you:
    
    - Decide if it’s a 2-minute favour → do it
        
    - If it’s larger → send hourly quote
        
    

---

## **3. How to phrase “Astro vs WordPress” in your pitch**

  

You can literally say:

  

> “I don’t use WordPress. I build your site using a modern static framework called Astro.

>   

> That means:

> • There’s no login page to hack

> • There are no plugins to constantly update

> • It loads much faster, especially on mobile

> • Maintenance is cheaper because there’s almost nothing to break

>   

> For solicitors and schools, it’s a much safer, lower-stress option long term.”

  

That’s enough. Don’t go deeper unless they drag you there.

---

If you want next, I can:

- Write the **exact outreach email** you’ll send
    
- Draft the **audit template** structure
    
- Or help you design the **Astro starter’s page structure** for solicitors specifically.