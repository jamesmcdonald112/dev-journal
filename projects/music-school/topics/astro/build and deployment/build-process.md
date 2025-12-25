### **📌 Hosting platforms** 

### **build for you automatically**

  

When deploying from GitHub, platforms like **Vercel, Netlify, Cloudflare Pages**:

- Pull your repo
    
- Run npm install
    
- Run npm run build
    
- Deploy the result
    

  

You **do NOT** upload dist/, and you **do NOT** commit it.

The platform generates it on their servers.

---

# **📌 What** 

# **npm run build**

#  **does**

  

**Creates a production build of your Astro site.**

Astro compiles everything and outputs optimized files into:

```
dist/
```

The build step:

- Turns Astro pages into HTML
    
- Bundles and optimizes JS
    
- Removes unused code
    
- Prepares final assets for hosting
    

  

### **✔ Use for deployment**

  

### **❌ Not needed for normal development**

---

# **📌 What** 

# **dist/**

#  **is**

  

The **final website** your users will see.

  

Contains:

- HTML files
    
- Bundled JS/CSS
    
- Assets
    

  

You don’t edit it or commit it — the hosting provider generates it automatically.

---

# **📌 Local preview of production build**

  

If you want to test the actual production version locally:

```
npm run build
npm run preview
```

---

# **📌 Development vs Production**

  

**Development:**

```
npm run dev
```

- Fast
    
- Hot reload
    
- Unoptimized
    
- Not the final site
    

  

**Production:**

```
npm run build
npm run preview
```

- Optimized
    
- Minified
    
- What gets deployed
    