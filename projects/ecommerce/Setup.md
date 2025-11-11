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
### **6. Set up Commitlint for Conventional Commits**

Follow the official [Commitlint setup guide](https://commitlint.js.org/guides/use-prompt.html):

To enforce clear, consistent commit messages:
1. **Install Commitlint + Prompt CLI**

```zsh
npm install -D @commitlint/cli @commitlint/config-conventional @commitlint/prompt-cli
```

2. **Create the config file**

```zsh
echo "export default { extends: ['@commitlint/config-conventional'] };" > commitlint.config.mjs
```

> (Use .mjs to avoid Node module warnings.)

3. **Add commit script to package.json**

```json
"scripts": {
  "commit": "commit"
}
```

4. **Use it for commits**

```zsh
git add .
npm run commit
```

You’ll be prompted to enter a structured message, e.g.:

```zsh
chore(setup): initialise Next.js project
```

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

### **12. Define Mongoose Model** - Test
### **Docs Used**

- [Next.js + MongoDB Example (Vercel)](https://github.com/vercel/next.js/blob/canary/examples/with-mongodb-mongoose/models/Pet.ts)
- [Mongoose Models — Official Docs](https://mongoosejs.com/docs/models.html)
- [Mongoose TypeScript Guide](https://mongoosejs.com/docs/typescript.html)

Create a folder:

```
models/Item.js
```

Create a **Mongoose model**, which acts like a **table definition** in SQL. It describes the structure (fields, types, and validation rules) for documents stored in a MongoDB collection.
- A **model** defines what each MongoDB document looks like and enforces validation.
- The Item interface extends `mongoose.Document`, so each item automatically gets all built-in Mongoose methods (save, delete, populate, etc.).
- The mongoose.model() call **compiles** the schema into a model (or reuses it if it already exists via mongoose.models).
- The first argument "Item" becomes the **collection name** items in MongoDB (Mongoose pluralises it automatically).
- `mongoose.models.Item` prevents model recompilation during hot reloads in Next.js.

### **Final Code**

```ts
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
### **16. Create and Test Initial API Route**

Based on the official [Next.js 16 App Route Handlers documentation](https://nextjs.org/docs/app/getting-started/route-handlers) and Initially inspired by the [Medium tutorial on Next.js + MongoDB CRUD](https://medium.com/@turingvang/next-js-beginner-mongodb-crud-example-tutorial-db2afdb68e25)!, but rewritten for TypeScript and the App Router structure following **Vercel’s with-mongodb-mongoose example**.

Before building the main `/api/products` endpoint, a temporary route was created for testing MongoDB connectivity.

**File:** `app/api/items/route.ts`

```ts
import { NextResponse } from "next/server";
import Item from "@/models/Item";
import dbConnect from "../../lib/mongodb";

export async function GET(): Promise<NextResponse> {
	try {
		await dbConnect();
		const items = await Item.find({});
		return NextResponse.json({ success: true, data: items });
	} catch (error: unknown) {
		return handleError(error);
	}
}

function handleError(error: unknown, status = 500): NextResponse {
	return NextResponse.json(
		{ success: false, error: (error as Error).message },
		{ status },
	);
}
```

#### **Purpose:**

- Verify successful MongoDB Atlas connection
- Confirm Mongoose model integration with Next.js API routes
- Test API response format using Postman or browser

#### **Result:**
A GET request to [http://localhost:3000/api/items](http://localhost:3000/api/items)returned data from MongoDB. This will be changed to products later to match the brief.

---
### **17. Upload Project to GitHub and Create Project Tracking**

After verifying that Biome, Commitlint, and MongoDB were working, the entire project was pushed to GitHub.
- Repository: [github.com/jamesmcdonald112/ecommerce-project](https://github.com/jamesmcdonald112/ecommerce-project)!
- Created detailed [GitHub Issues](https://github.com/jamesmcdonald112/ecommerce-project/issues)! based on the project brief
- Grouped all issues under the [Phase 1 – Core E-Commerce Pages Milestone](https://github.com/jamesmcdonald112/ecommerce-project/milestones)!
- Added the main epic issue: **AI-Powered E-Commerce Website**
- Each major task (admin pages, catalogue, cart, etc.) is tracked as a linked sub-issue for structured progress monitoring

---
