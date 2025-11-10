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

### **10. Set Up MongoDB Atlas (Cloud Database)**

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) → Create a Free Cluster (M0).
2. Choose provider **AWS**, region eu-west-1 (Ireland or closest).
3. Add IP Access:
    - Current IP (auto-added)
4. Create a database user:
    - Username ecommerce_user
    - Password (copy and store securely).
5. Wait for cluster to deploy → **Connect → Drivers → Node.js**.
6. Copy connection URI:
```
mongodb+srv://ecommerce_user:<password>@ecommerce-cluster.xxxxx.mongodb.net/ecommerce
```
    
7. In your project root, create .env.local:

```
MONGODB_URI=mongodb+srv://ecommerce_user:<password>@ecommerce-cluster.xxxxx.mongodb.net/ecommerce
```


---

### **11. Install Mongoose and Configure Connection**

Following an example for Next.js and MongoDB CRUD.

Source: [Medium Tutorial (Next.js + MongoDB CRUD)](https://medium.com/@turingvang/next-js-beginner-mongodb-crud-example-tutorial-db2afdb68e25)
  
Install:

```
npm install mongoose
```

Create a folder and connection file:

```
lib/mongodb.js
```

- Basic **JavaScript** version in the original source that used:
```
global.mongoose = { conn: null, promise: null };
```

- ❌ Works but not TypeScript-safe.

### **Improved Reference**

- Based on official Vercel example:
    🔗 [with-mongodb-mongoose on GitHub](https://github.com/vercel/next.js/blob/canary/examples/with-mongodb-mongoose/lib/dbConnect.ts)
- Adapted for **TypeScript** with explicit types and renamed global variable (mongooseCache) to avoid name collisions.

### **Final Working Version — lib/mongodb.ts**

```ts
import mongoose, { type Mongoose } from "mongoose";

interface MongooseCache {
  conn: Mongoose | null;
  promise: Promise<Mongoose> | null;
}

// Declare a global variable to persist the connection cache between hot reloads
declare global {
  // Must use `var` for global augmentation
  var mongooseCache: MongooseCache | undefined;
}

// Reuse existing cache or initialize a new one
const cached: MongooseCache = global.mongooseCache ?? {
  conn: null,
  promise: null,
};
global.mongooseCache = cached;

export default async function dbConnect(): Promise<Mongoose> {
  const MONGODB_URI: string | undefined = process.env.MONGODB_URI;

  if (!MONGODB_URI) {
    throw new Error(
      "Please define the MONGODB_URI environment variable inside .env.local"
    );
  }

  // Return existing connection if available
  if (cached.conn) return cached.conn;

  // Otherwise, create a new connection
  if (!cached.promise) {
    const opts = { bufferCommands: false }; // disables query buffering
    cached.promise = mongoose.connect(MONGODB_URI, opts);
  }

  try {
    cached.conn = await cached.promise;
  } catch (e: unknown) {
    cached.promise = null; // reset promise on failure
    throw e;
  }

  return cached.conn;
}
```

---

### **12. Define Mongoose Model**

Create a folder:

```
models/Item.js
```

Create a **Mongoose model**, which acts like a **table definition** in SQL. It describes the structure (fields, types, and validation rules) for documents stored in a MongoDB collection.
### **🧩** 

### **Docs Used**

- [Mongoose Models — Official Docs](https://mongoosejs.com/docs/models.html)![Attachment.tiff](file:///Attachment.tiff)
    
- [Next.js + MongoDB Example (Vercel)](https://github.com/vercel/next.js/blob/canary/examples/with-mongodb-mongoose/lib/dbConnect.ts)![Attachment.tiff](file:///Attachment.tiff)
    
- [Mongoose TypeScript Guide](https://mongoosejs.com/docs/typescript.html)![Attachment.tiff](file:///Attachment.tiff)
    

---

### **🧠** 

### **Summary**

- A **model** defines what each MongoDB document looks like and enforces validation.
    
- We used **TypeScript** for type safety.
    
- The Item interface extends mongoose.Document, so each item automatically gets all built-in Mongoose methods (save, delete, populate, etc.).
    
- The mongoose.model() call **compiles** the schema into a model (or reuses it if it already exists via mongoose.models).
    
- The first argument "Item" becomes the **collection name** items in MongoDB (Mongoose pluralizes it automatically).
    

---

### **✅** 

### **Final Code**

```
import mongoose from "mongoose";

interface Item extends mongoose.Document {
  name: string;
  descriptions: string;
}

const ItemSchema = new mongoose.Schema<Item>({
  name: {
    type: String,
    required: [true, "Please provide a name for this item"],
    maxLength: [60, "The item name cannot be more than 60 characters"],
  },
  descriptions: {
    type: String,
    required: [true, "Please provide a description for this item"],
    maxLength: [200, "The item description cannot be more than 200 characters"],
  },
});

export default mongoose.models.Item || mongoose.model<Item>("Item", ItemSchema);
```

---

### **🧾** 

### **Key Notes**

- mongoose.models.Item prevents model recompilation during hot reloads in Next.js.
    
- "Item" (singular) → MongoDB collection name becomes items (plural).
    
- Each field inside the schema supports many built-in **SchemaType options** such as:
    
    - type, required, maxLength, default, enum, unique, etc.
        
        (see [Mongoose SchemaType Options](https://mongoosejs.com/docs/schematypes.html)![Attachment.tiff](file:///Attachment.tiff))
        
    

---

Would you like me to write the **next section (Step 13: Testing the Database with curl and verifying in Atlas)** in the same format?
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