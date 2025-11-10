### **1. Create GitHub Repository**

- Go to [GitHub](https://github.com) → click **New Repository**.
- Name it (e.g., ecommerce-project).
- Add:
    - ✅ **MIT License**
    - ✅ **.gitignore** → choose the **Next** template.
- Leave it public
- Do **not** initialise with a README.

---

### **2. Clone Repository to Local Machine**

In your terminal:

```bash
git clone https://github.com/jamesmcdonald112/ecommerce-project
cd ecommerce-project
```

---

### **3. Create a New Next.js Project**

Inside the cloned folder:

```bash
npx create-next-app@latest .
```

When prompted, accept the default settings.

---

### **4. Remove ESLint (if Next.js added it)**

If eslint or eslint-config-next are in your package.json, remove them:

```bash
npm uninstall eslint eslint-config-next
```

You no longer need them, Biome will handle linting and formatting.

---

### **5. Install Biome**

Follow the official [Biome setup guide](https://biomejs.dev/guides/getting-started/):

```bash
npm i -D -E @biomejs/biome
```

- -D installs as a dev dependency.
    
- -E pins the exact version (important for consistency).
  
Then initialise Biome:

```bash
npx @biomejs/biome init
```

This creates a biome.json configuration file at your project root.

---

### **6. Enable Tailwind Support**

Open your new biome.json and add:

```json
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


In package.json, under "scripts", remove anything remaining from eslint and  add:

```bash
"format": "biome format . --write",
"lint": "biome lint . --write",
"check": "biome check ."
```

These commands let you:

- npm run format → format code
- npm run lint → lint and apply safe fixes
- npm run check → check all files (format + lint + organise imports)
    

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

- “Checked X files” (no issues)
- or a list of style fixes (run with --write to apply).
---










Excellent — let’s continue your setup notes cleanly from there, keeping the same format and crediting sources where relevant.

---

## **Phase 0 – MongoDB Setup + React Rules Integration**

  

### **10. Set Up MongoDB Atlas (Cloud Database)**

  

📘 Source: [MongoDB Atlas Docs](https://www.mongodb.com/docs/atlas/getting-started/)![Attachment.tiff](file:///Attachment.tiff)

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)![Attachment.tiff](file:///Attachment.tiff) → Create a Free Cluster (M0).
    
2. Choose provider **AWS**, region eu-west-1 (Ireland or closest).
    
3. Add IP Access:
    
    - Current IP (auto-added)
        
    - Optionally 0.0.0.0/0 for dev access from any device.
        
    
4. Create a database user:
    
    - Username ecommerce_user
        
    - Password (copy and store securely).
        
    
5. Wait for cluster to deploy → **Connect → Drivers → Node.js**.
    
6. Copy connection URI:
    

```
mongodb+srv://ecommerce_user:<password>@ecommerce-cluster.xxxxx.mongodb.net/ecommerce
```

6.   
    
7. In your project root, create .env.local:
    

```
MONGODB_URI=mongodb+srv://ecommerce_user:<password>@ecommerce-cluster.xxxxx.mongodb.net/ecommerce
```

  

---

### **11. Install Mongoose and Configure Connection**

  

📘 Source: [Mongoose Docs](https://mongoosejs.com/docs/connections.html)![Attachment.tiff](file:///Attachment.tiff) + [Medium Tutorial (Next.js + MongoDB CRUD)](https://medium.com/@turingvang/next-js-beginner-mongodb-crud-example-tutorial-db2afdb68e25)![Attachment.tiff](file:///Attachment.tiff)

  

Install:

```
npm install mongoose
```

Create a folder and connection file:

```
lib/mongodb.js
```

```
import mongoose from "mongoose";

const MONGODB_URI = process.env.MONGODB_URI;
if (!MONGODB_URI) throw new Error("Please define the MONGODB_URI environment variable");

let cached = global.mongoose || { conn: null, promise: null };

global.mongoose = cached;

export default async function dbConnect() {
  if (cached.conn) return cached.conn;
  if (!cached.promise) cached.promise = mongoose.connect(MONGODB_URI).then((m) => m);
  cached.conn = await cached.promise;
  return cached.conn;
}
```

---

### **12. Create Your First Model**

  

📘 Source: Mongoose Schemas – [Docs](https://mongoosejs.com/docs/models.html)![Attachment.tiff](file:///Attachment.tiff)

  

Create a folder:

```
models/Item.js
```

```
import mongoose from "mongoose";

const ItemSchema = new mongoose.Schema({
  name: { type: String, required: true },
  description: String,
});

export default mongoose.models.Item || mongoose.model("Item", ItemSchema);
```

---

### **13. Test Your Connection**

  

Create API route:

```
app/api/test-db/route.js
```

```
import dbConnect from "@/lib/mongodb";
import Item from "@/models/Item";
import { NextResponse } from "next/server";

export async function GET() {
  try {
    await dbConnect();
    const item = await Item.create({ name: "Sample Product", description: "Test entry" });
    return NextResponse.json({ success: true, item });
  } catch (err) {
    console.error(err);
    return NextResponse.json({ success: false, error: err.message });
  }
}
```

Run:

```
npm run dev
```

Visit [http://localhost:3000/api/test-db](http://localhost:3000/api/test-db)![Attachment.tiff](file:///Attachment.tiff) → ✅ should return Sample Product.

---

### **14. Fix React Hook Dependency Warnings**

  

📘 Source: [React Official Docs – Rules of Hooks](https://react.dev/reference/rules)![Attachment.tiff](file:///Attachment.tiff)

  

Problem:

```
useEffect(() => {
  fetchItems();
}, []); // ❌ warning: missing dependency
```

Solution (React-approved):

```
import { useState, useEffect, useCallback } from "react";

export default function Home() {
  const [items, setItems] = useState([]);

  const fetchItems = useCallback(async () => {
    const res = await fetch("/api/items");
    const data = await res.json();
    setItems(data.data);
  }, []); // ✅ stable dependency

  useEffect(() => {
    fetchItems();
  }, [fetchItems]); // ✅ satisfies React & Biome
}
```

This follows React’s **Rules of Hooks** and Biome’s useExhaustiveDependencies rule, ensuring predictable and pure component behavior.

---

### **15. Confirm Your Setup So Far**

|**Component**|**Tool**|**Status**|
|---|---|---|
|GitHub repo|✅ created & cloned||
|Next.js + Tailwind|✅ initialized||
|Biome|✅ configured||
|MongoDB Atlas|✅ connected||
|Mongoose|✅ installed & tested||
|React Hooks|✅ following official rules||

---

Would you like me to continue with the next phase — **building the /api/items CRUD routes and connecting them to your Home component form** — and write that in the same documentation style?