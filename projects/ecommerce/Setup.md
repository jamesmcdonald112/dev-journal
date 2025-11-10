### **1. Create GitHub Repository**

- Go to [GitHub](https://github.com) → click **New Repository**.
- Name it (e.g., ecommerce-project).
- Add:
    - ✅ **MIT License**
    - ✅ **.gitignore** → choose the **Next** template.
- Leave it public
- Do **not** initialise with a README if you plan to clone and create the project locally.
    

---

### **2. Clone Repository to Local Machine**

  

In your terminal:

```
git clone https://github.com/<your-username>/ecommerce-project.git
cd ecommerce-project
```

---

### **3. Create a New Next.js Project**

  

Inside the cloned folder:

```
npx create-next-app@latest .
```

When prompted:

- ✅ **TypeScript:** Yes
    
- ✅ **Tailwind CSS:** Yes
    
- ❌ **ESLint:** No (you’ll use Biome instead)
    
- ✅ **App Router:** Yes
    
- ✅ **Import Alias:** Yes (@/*)
    

---

### **4. Remove ESLint (if Next.js added it)**

  

If eslint or eslint-config-next are in your package.json, remove them:

```
npm uninstall eslint eslint-config-next
```

You no longer need them — Biome will handle linting and formatting.

---

### **5. Install Biome**

  

Follow the official [Biome setup guide](https://biomejs.dev/guides/getting-started/)![Attachment.tiff](file:///Attachment.tiff):

```
npm i -D -E @biomejs/biome
```

- -D installs as a dev dependency.
    
- -E pins the exact version (important for consistency).
    

  

Then initialize Biome:

```
npx @biomejs/biome init
```

This creates a biome.json configuration file at your project root.

---

### **6. Enable Tailwind Support**

  

Open your new biome.json and add:

```
{
  "$schema": "https://biomejs.dev/schemas/1.7.3/schema.json",
  "css": {
    "parser": {
      "tailwindDirectives": true
    }
  }
}
```

This ensures Biome correctly parses Tailwind syntax like @theme, @apply, etc.

---

### **7. Add Biome Scripts**

  

In package.json, under "scripts", add:

```
"format": "biome format . --write",
"lint": "biome lint . --write",
"check": "biome check ."
```

These commands let you:

- npm run format → format code
    
- npm run lint → lint and apply safe fixes
    
- npm run check → check all files (format + lint + organize imports)
    

---

### **8. (Optional) Set Up VS Code Integration**

  

To use Biome automatically on save:

1. Open VS Code → Extensions (Ctrl+Shift+X / Cmd+Shift+X).
    
2. Install **Biome** (biomejs.biome) official extension.
    
3. Go to Settings → search “Format on Save” → enable it.
    
4. Disable Prettier and ESLint extensions to avoid conflicts.
    
5. Restart VS Code.
    

  

Now Biome will auto-format and lint when you save.

---

### **9. Verify Everything Works**

  

Run:

```
npm run check
```

You should see either:

- ✅ “Checked X files” (no issues)
    
- or a list of style fixes (run with --write to apply).
    

---

Would you like me to continue your notes with the **next setup phase (MongoDB + connection file + env setup)** so you can keep one unified setup guide?