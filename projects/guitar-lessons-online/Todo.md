## **1) Make the build deploy-clean (Next.js)**

- npm run build locally. Fix any errors/warnings that matter.
    
- Ensure **images are local** or stable URLs.
    
- Make sure your **Calendly embed** works without console errors.
    

  

## **2) Deploy to Vercel**

- Import GitHub repo → Vercel → Deploy.
    
- Set the project to **Production** (default).
    
- Verify the deployed URL works on mobile.
    

  

## **3) Connect your domain**

- Buy domain (if you haven’t).
    
- Vercel → Project → **Settings → Domains** → add domain.
    
- Update DNS records as Vercel instructs.
    
- Turn on **HTTPS** (Vercel does it automatically once DNS is correct).
    

  

## **4) SEO essentials (minimum viable, but correct)**

  

In Next (App Router or Pages Router), add:

- Title + meta description (unique, not generic)
    
- Open Graph + Twitter cards (use your OG image)
    
- Canonical URL
    
- Favicon
    

  

Also add:

- robots.txt
    
- sitemap.xml (simple is fine for a one-page site)
    

  

If you don’t want to hand-roll this, use next-sitemap.

  

## **5) Contact form that actually delivers messages**

  

Pick one path:

  

### **Fastest (recommended):**

  

Use a form backend (no server code):

- Formspree / Basin / Getform / Tally
    
- Add spam protection (honeypot or their built-in)
    

  

### **More control:**

  

Use a Next API route + an email provider (Resend is common)

- Add basic rate limiting + honeypot
    

  

If you do nothing else: make sure form submissions **reach your inbox** and you have a **confirmation message** on submit.

  

## **6) Calendly setup (conversion-critical)**

- Make one booking link that matches your offer.
    
- Set availability, buffer times, cancellation/reschedule rules.
    
- Collect what you need: name, email, goal, experience level.
    
- Add automatic confirmation + reminder emails.
    

  

## **7) Analytics (so you’re not guessing)**

  

Install one:

- Plausible (simple) or GA4 (common)
    

  

Track:

- Clicks on “Book a Lesson”
    
- Calendly booking completed (Calendly has tracking options; at minimum track outbound click)
    
- Contact form submits
    

  

## **8) Trust + friction reducers**

  

Add (small, not a new section):

- Clear timezone note (e.g., “Times shown in your local timezone” if Calendly handles it)
    
- Response time promise near contact (“Reply within 24 hours” — only if true)
    
- A simple policy note (cancellation/reschedule)
    

  

## **9) Legal basics (don’t be sloppy)**

  

You’re collecting user data (form + Calendly), so add:

- Privacy Policy page (can be short)
    
- Cookie notice only if you use tracking cookies (GA often implies this; Plausible is lighter)
    

  

You can keep these as minimal extra pages linked in footer.

  

## **10) Performance + QA checklist (quick)**

- Mobile: headline not wrapping badly, buttons tappable, embeds not broken
    
- Lighthouse check (don’t obsess; just avoid obvious issues)
    
- Test on Safari + Chrome
    
- Test contact form end-to-end
    
- Test booking end-to-end
    

  

## **11) Launch plan (what “successful” means)**

  

Define your metrics for the next 2 weeks:

- Target sessions (traffic)
    
- Booking rate goal (e.g., 1–3%)
    
- Cost per lead/booking (if ads)
    

  

Then drive traffic:

- One channel only to start (Google Search ads OR Meta ads OR Reddit/YouTube content). Don’t scatter.
    

---

### **I will not give you a time estimate.**

  

But if you follow the list above and don’t get distracted, this is a straightforward “ship” task, not a multi-week project.

  

Two quick questions (answer in one line each) and I’ll give you the exact implementation path:

1. Are you using **Next App Router** (app/ folder) or **Pages Router** (pages/ folder)?
    
2. For the contact form: **no-backend service** or **Next API route + email**?