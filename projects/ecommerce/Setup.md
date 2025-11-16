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

### **18. Product Schema (TypeScript + Mongoose)**

📘 **Reference:** [Mongoose TypeScript Guide](https://mongoosejs.com/docs/typescript.html)!

📘 **Timestamps Option:** [Mongoose Guide – timestamps](https://mongoosejs.com/docs/guide.html#timestamps)!

#### Final code
```ts
import { model, Schema } from "mongoose";

interface Product {
	title: string;
	shortDescription: string;
	longDescription: string;
	specs?: Record<string, string>;
	reviews?: string[];
	price: number;
	images: string[];
	slug: string;
}

const ProductSchema = new Schema<Product>(
	{
		title: {
			type: String,
			required: [true, "Please provide a title for this product"],
			maxLength: [100, "Title cannot exceed 100 characters"],
		},
		shortDescription: {
			type: String,
			required: [true, "Please provide a short description"],
			maxLength: [300, "Short description cannot exceed 300 characters"],
		},
		longDescription: {
			type: String,
			required: [true, "Please provide a detailed description"],
			maxLength: [3000, "Long description cannot exceed 3000 characters"],
		},
		specs: {
			type: Map,
			of: String,
			default: {},
		},
		reviews: {
			type: [String],
			default: [],
		},
		price: {
			type: Number,
			required: [true, "Please provide a price"],
			min: [0, "Price cannot be negative"],
		},
		images: {
			type: [String],
			required: [true, "Please provide at least one image URL"],
		},
		slug: {
			type: String,
			required: [true, "Please provide a slug (unique identifier)"],
			unique: true,
		},
	},
	{ timestamps: true },
);

// Creates (or reuses) a MongoDB collection named "products"
export const Product = model<Product>("Product", ProductSchema);
```

According to the official [Mongoose + TypeScript documentation](https://mongoosejs.com/docs/typescript.html), the correct pattern for defining a typed model is to:

1. **Define an interface** describing the structure of the document (in this case, Product), which represents what a product object looks like in MongoDB.
    
2. **Create a** **Schema** **instance** using `new Schema<Product>(...)` where the generic type parameter` <Product>` ensures TypeScript type-safety.
    
3. **Specify field properties** inside the schema object, including:
    
    - type: the data type (String, Number, Map, etc.)
        
    - required: true or `[true, "error message"`] to make the field mandatory
        
    - maxLength / min: validation limits with messages
        
    - unique: true to create a MongoDB unique index
        
    - default values to avoid undefined data later
        
    
4. **Handle key–value data** with Map, since Mongoose doesn’t have a native Record type. Here specs: `{ type: Map, of: String }` means the map will store string → string pairs.
    
5. **Add timestamps** with { timestamps: true }, which automatically adds and maintains createdAt and updatedAt fields in the document.
    
6. **Export the model** using:
    

```ts
export const Product = model<Product>("Product", ProductSchema);
```

- `model<Product> `is the Mongoose factory function for creating a model typed as Product. It gives intellisense.

- The first argument ("Product") becomes the **collection name** (products) in MongoDB (Mongoose pluralises it automatically).

- The second argument (ProductSchema) defines the rules and validation that the collection must follow.

- Assigning and exporting it in one line (export const Product = ...) makes the model immediately available wherever you import it.

---

Here’s the corrected and finalized version of your note with the proper **MongoDB doc link** and an accurate citation to the section you found:

---

### **19. Correct MongoDB URI Structure**

  

> 🧩 **Docs reference:** [MongoDB Connection String Formats](https://www.mongodb.com/docs/manual/reference/connection-string-formats/)!

> “You can specify a default database in the [/defaultauthdb] field of the connection string. The client uses the specified [/defaultauthdb] database as the default database.

> **If unspecified by the connection string, MongoDB uses the test database as the default.**”


When connecting Mongoose to MongoDB, the **database name** must be specified **before the question mark (?)** in your connection URI.


If you omit it, MongoDB defaults to the **test** database.

---

**✅ Correct format:**

```bash
MONGODB_URI=mongodb+srv://<username>:<password>@<cluster-name>.<randomid>.mongodb.net/<database-name>?appName=<app-name>
```

**Explanation:**

- ecommerce → is the **database name** where all your collections (like products) will live.
    
- MongoDB will **not** automatically use your collection name to select a database — that was a misunderstanding.
    
- Mongoose handles **collections** (e.g., Product → products) **inside** the database you specify in the URI.
    

**Note:**


> I initially thought the database name in the URI referred to the table (collection), but it actually refers to the **database** that holds all the collections.


---
### **20. Product Schema and Initial GET Route**


> 🧩 **Docs reference:**
    
- > [Next.js Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/router-handlers)!

#### **Goal**

Create and verify an API route (/api/products) that connects to MongoDB, fetches all products, and returns them as JSON.

---

#### **Files Created/Modified**

- models/Product.ts → defines the Product schema
    
- app/api/products/route.ts → defines the GET route
    
- lib/mongodb.ts → handles database connection (already existed)
    

---

#### **✅ Final Code (GET route)**

app/api/products/route.ts

```ts
import { NextResponse } from "next/server";
import { Product } from "@/models/Product";
import dbConnect from "../../lib/mongodb";

export async function GET(): Promise<NextResponse> {
  try {
    await dbConnect();
    const items = await Product.find({});
    return NextResponse.json({ success: true, data: items });
  } catch (error: unknown) {
    return handleError(error);
  }
}

function handleError(error: unknown, status = 500): NextResponse {
  const message = error instanceof Error ? error.message : "Unknown error";
  return NextResponse.json({ success: false, error: message }, { status });
}
```

---

#### **How It Works**

- Connects to MongoDB via dbConnect() before querying.
    
- Uses the Mongoose Product model to fetch all documents from the products collection.
    
- Returns a standardised JSON response with either:
    
    - success: true and the list of products, or
        
    - success: false and an error message.
        
    
- The helper handleError() ensures consistent error formatting.
    

---

#### **Verification**
A test product was **manually inserted** into the MongoDB Atlas ecommerce database under the products collection:

| **Field**        | **Value**                            |
| ---------------- | ------------------------------------ |
| _id              | 691349b812a81bf26f780ae1             |
| title            | "Test Product"                       |
| shortDescription | "Short description"                  |
| longDescription  | "Longer description of the product." |
| price            | 19.99                                |
| images           | ["https://example.com/img.png"]      |
| slug             | "test-product"                       |

Tested the route using a GET request from Postman to:

```
http://localhost:3000/api/products
```

Result confirmed successful database connection and returned inserted product data.

  

Example response:

```json
{
  "success": true,
  "data": [
    {
      "_id": "691349b812a81bf26f780ae1",
      "title": "Test Product",
      "shortDescription": "Short description",
      "longDescription": "Longer description of the product.",
      "price": 19.99,
      "images": ["https://example.com/img.png"],
      "slug": "test-product",
      "specs": {},
      "reviews": []
    }
  ]
}
```

---
### **21. Add POST Route for Creating Products**

> **Docs Reference:**

- > [Next.js — Route Handlers (App Router)](https://nextjs.org/docs/app/getting-started/route-handlers)!

    > _Explains how to define_ _GET__,_ _POST__, and other HTTP methods inside_ _route.ts_ _files using the Web Request and Response APIs._
    
- > [Next.js — Backend for Frontend Guide](https://nextjs.org/docs/app/guides/backend-for-frontend)!
    
    > _Covers consuming request payloads with_ _request.json()_ _and_ _request.formData()__, and using_ _NextRequest_ _/_ _NextResponse_ _for structured responses and error handling._
    
- > [Next.js — Route File API Reference](https://nextjs.org/docs/app/api-reference/file-conventions/route)!
    
    > _Provides full reference for route configuration, supported methods, parameters, and examples._

### **Goal**

Allow new products to be added to the MongoDB database via a POST request to /api/products.

### **Changes Made**

1. **Added a POST method** to /app/api/products/route.ts.
    
2. **Moved the Product interface** to /app/types/product.ts to make it reusable across backend and frontend.
    
3. **Renamed Product model** → ProductModel to avoid naming conflicts between the model and type.
    



### **Final Code**

```ts
import type { HydratedDocument } from "mongoose";
import { type NextRequest, NextResponse } from "next/server";
import type { Product } from "@/app/types/product";
import { ProductModel } from "@/models/Product";
import dbConnect from "../../lib/mongodb";

export async function GET(): Promise<NextResponse> {
	try {
		await dbConnect();
		const items = await ProductModel.find({});
		return NextResponse.json({ success: true, data: items });
	} catch (error: unknown) {
		return handleError(error);
	}
}

export async function POST(request: NextRequest): Promise<NextResponse> {
	try {
		const body: Product = await request.json();
		await dbConnect();

		const product: HydratedDocument<Product> = await ProductModel.create(body);
		return NextResponse.json({ success: true, data: product }, { status: 201 });
	} catch (error: unknown) {
		return handleError(error);
	}
}

function handleError(error: unknown, status = 500): NextResponse {
	const message = error instanceof Error ? error.message : "Unknown error";
	return NextResponse.json({ success: false, error: message }, { status });
}
```

---

### **How It Works**

1. **NextRequest** **&** **NextResponse**
    
    - Both are **Next.js wrappers** around Node’s native Request and Response objects.
        
    - They provide extra utilities such as cookies, headers, and typed responses (see [Next.js docs](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)!).
    
2. **Request Handling**
    
    - The POST method accepts a NextRequest, representing the incoming HTTP request.
        
    
3. **Database Connection**
    
    - await dbConnect() ensures an active Mongoose connection before interacting with the database.
        
    - You **don’t manually close** the connection, Next.js’s API routes are short-lived, and dbConnect() caches connections automatically to reuse them efficiently.
        
    
4. **Saving the Product**
    
    - ProductModel.create(body) inserts a new document into the products collection.
        
    - The return type HydratedDocument`<Product>` means:
        
        > A Mongoose document enriched with schema-based methods like .save(), .deleteOne(), etc.
        
    
5. **Response**
    
    - On success → returns 201 Created with the inserted product as JSON.
        
    - On failure → handleError() catches it and responds with { success: false, error: message }.
        
    

---

### **Example Request (Postman)**

  
**POST** → http://localhost:3000/api/products

**Body (JSON):**

```json
{
  "title": "Test Product 2",
  "shortDescription": "A short summary",
  "longDescription": "Detailed product description",
  "price": 49.99,
  "images": ["https://example.com/image.png"],
  "slug": "test-product-2"
}
```

**Response:**

```json
{
  "success": true,
  "data": {
    "_id": "69148bef350f4b5de7ddc076",
    "title": "Test Product 2",
    "price": 49.99,
    "slug": "test-product-2",
    "createdAt": "2025-11-12T13:30:23.726Z",
    "updatedAt": "2025-11-12T13:30:23.726Z"
  }
}
```

---
## 22 Simplify Tidy Script 

**File Updated:**  

- `package.json`


**Summary:**  

Replaced the chained tidy command (`npm run format && npm run lint && npm run check`)  

with a single unified Biome command: `biome check . --write`.

  

**Reasoning:**  

- `biome check` already includes linting and formatting.

- The `--write` flag automatically applies safe fixes.

- Simplifies maintenance and saves time during pre-commit cleanup.

  

**Outcome:**  

- One unified cleanup command: `npm run tidy`

- Consistent and faster developer workflow.

---

## 23 **Product Schema (app/features/products/schemas/product.schema.ts)**

> **Official docs:** [https://zod.dev](https://zod.dev)
> **Docs:** [Zod Error Formatting](https://zod.dev/error-formatting)
> **Docs:** [Next.js Route Handlers (App Router)](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)
  
This file defines the **validation rules** and **TypeScript typing** for all product data in the app.

It ensures data sent to the API is structured, valid, and safe before being written to MongoDB.

### **Code**

```ts
import { z } from "zod";

export const productSchema = z.object({
	title: z
		.string()
		.min(1, "Title is required")
		.max(100, "Max 100 characters for title"),

	shortDescription: z
		.string()
		.min(1, "Short description is required")
		.max(300, "max 300 characters for short description"),

	longDescription: z
		.string()
		.min(1, "Long description is required")
		.max(3000, "max 3'000 characters for long descriptions"),

	specs: z.record(z.string(), z.string()).default({}), // maps of key: string
	reviews: z.array(z.string()).default([]),

	price: z.coerce.number().positive("Price must be positive"),

	images: z
		.array(z.string().url("Invalid image URL"))
		.min(1, "At least one image is required"),

	slug: z.string().min(1, "Slug is required"),
});

// 👇 Automatically infer a TypeScript type from the schema
export type Product = z.infer<typeof productSchema>;
```

#### **2. Define the schema**

```
export const productSchema = z.object({...});
```

This creates an **object schema** describing what a valid product looks like.

Each key defines a field, its type, and validation rules.


#### **3. Field-by-field rules**

|**Field**|**Type / Rule**|**Description**|
|---|---|---|
|title|string, required, max 100 chars|Product title displayed in listings|
|shortDescription|string, required, max 300 chars|Short summary for previews/cards|
|longDescription|string, required, max 3000 chars|Full description for product page|
|specs|Record<string, string>, default {}|Key/value map for specs like "color": "red"|
|reviews|string[], default []|List of review strings or IDs|
|price|number, positive, coercion enabled|Accepts "19.99" as input and converts to 19.99|
|images|string[], URLs only, at least one required|Ensures products always have images|
|slug|string, required|URL-safe identifier, e.g., "iphone-15-pro"|

---

#### **4. Defaults**

Zod’s .default() ensures missing optional fields (specs, reviews) are safely initialized:

```ts
specs: z.record(z.string(), z.string()).default({}),
reviews: z.array(z.string()).default([]),
```

---

#### **5. Exporting the inferred type**

You can re-export the inferred type from a dedicated types file

so that all other parts of the app can import Product cleanly:

```ts
// app/types/product.ts
export type { Product } from "@/app/features/products/schemas/product.schema";
```

This keeps your **schema definition** and **type imports** modular —

frontend components, API routes, and models can all share the same type.

  

Example of the inferred type:

```ts
type Product = {
	title: string;
	shortDescription: string;
	longDescription: string;
	specs: Record<string, string>;
	reviews: string[];
	price: number;
	images: string[];
	slug: string;
};
```

### **Why This Matters**

- **Single source of truth:** You define your product structure once; TypeScript and runtime validation both use it.
    
- **Runtime safety:** Prevents invalid or malicious data from being stored in MongoDB.
    
- **Type safety:** Eliminates mismatches between backend validation and frontend expectations.
    
- **Scalability:** Adding new fields in the future automatically updates both schema and type system.

---
### **How it’s used in your API route**

Below is the server route where productSchema and the inferred Product type are used together to ensure data integrity end-to-end

```ts
import type { HydratedDocument } from "mongoose";
import { type NextRequest, NextResponse } from "next/server";
import z, { ZodError } from "zod";
import { productSchema } from "@/app/features/products/schemas/product.schema";
import type { Product } from "@/app/types/product";
import { ProductModel } from "@/models/Product";
import dbConnect from "../../lib/mongodb";

export async function GET(): Promise<NextResponse> {
	try {
		await dbConnect();
		const items = await ProductModel.find({});
		return NextResponse.json({ success: true, data: items });
	} catch (error: unknown) {
		return handleError(error);
	}
}

export async function POST(request: NextRequest): Promise<NextResponse> {
	try {
		// Parse and validate incoming JSON
		const json: unknown = await request.json();
		const parsedBody: Product = productSchema.parse(json);

		// Connect to DB and create a new product
		await dbConnect();
		const product: HydratedDocument<Product> =
			await ProductModel.create(parsedBody);

		return NextResponse.json({ success: true, data: product }, { status: 201 });
	} catch (error: unknown) {
		return handleError(error);
	}
}

function handleError(error: unknown, status = 500): NextResponse {
	if (error instanceof ZodError) {
		const tree = z.treeifyError(error);
		return NextResponse.json({ success: false, errors: tree }, { status: 400 });
	}
	const message = error instanceof Error ? error.message : "Unknown error";
	return NextResponse.json({ success: false, error: message }, { status });
}
```

### **Explanation**

- **Zod schema enforces validation** at runtime (rejects invalid API data).
    
- **z.infer** **gives the exact same structure as your schema**, preventing drift between your runtime and compile-time definitions.
    
- **Type-safe integration with Mongoose:**
```ts
const product: HydratedDocument<Product> = await ProductModel.create(parsedBody);
```

- **Error handling:**
    
    Uses z.treeifyError(error) (from Zod v4) to return a structured, human-readable error tree.

---
# **24. Introduce Service Layer Pattern for API Architecture**


> Docs: [Scalable Next.js Architecture – DEV article](https://dev.to/melvinprince/the-complete-guide-to-scalable-nextjs-architecture-39o0)

> (Specifically section **“3. Optimizing API Routes for Performance and Maintainability”** — external-facing controllers + separate internal API logic.)

### **Goal**

Separate your backend logic into clear layers so the API becomes easier to scale, maintain, and extend.

This introduces three layers:

- **Route Handlers** → Handle HTTP requests and responses
    
- **Service Functions** → Contain business logic and database operations
    
- **Models** → Define the database structure (Mongoose schema)


This is the recommended architecture for large Next.js apps.

# **24.1 Route Handler Layer**

  

**Location:**

app/api/products/route.ts

  

**Role:**

- Accept incoming requests
    
- Validate input
    
- Call a service function
    
- Catch errors
    
- Return JSON responses
    

  

**No business logic here.**

**No database operations here.**

  

### **Final Route Handler Code**

```ts
import type { HydratedDocument } from "mongoose";
import { type NextRequest, NextResponse } from "next/server";
import z, { ZodError } from "zod";

import { productSchema } from "@/app/features/products/schemas/product.schema";
import type { Product } from "@/app/features/products/types/product";

import { getAllProducts } from "@/app/features/products/api/getProducts";
import { createProduct } from "@/app/features/products/api/createProduct";

import dbConnect from "../../lib/mongodb";

export async function GET(): Promise<NextResponse> {
  try {
    const products: Product[] = await getAllProducts();
    return NextResponse.json({ success: true, data: products });
  } catch (error: unknown) {
    return handleError(error);
  }
}

export async function POST(request: NextRequest): Promise<NextResponse> {
  try {
    const json: unknown = await request.json();
    const parsedBody: Product = productSchema.parse(json);

    const product: HydratedDocument<Product> =
      await createProduct(parsedBody);

    return NextResponse.json({ success: true, data: product }, { status: 201 });
  } catch (error: unknown) {
    return handleError(error);
  }
}

function handleError(error: unknown, status = 500): NextResponse {
  if (error instanceof ZodError) {
    const tree = z.treeifyError(error);
    return NextResponse.json({ success: false, errors: tree }, { status: 400 });
  }

  const message =
    error instanceof Error ? error.message : "Unknown error";

  return NextResponse.json({ success: false, error: message }, { status });
}
```

---

# **24.2 Service Layer**

  

### **Purpose**

  

Service functions:

- Execute business logic
    
- Query the database
    
- Throw errors automatically
    
- Never catch errors (errors bubble into the route handler)
    

  

This makes service functions **lightweight, reusable, and testable**.

---

### **24.2.1 Get Products Service**

  

**File:**

app/features/products/api/getProducts.ts

```ts
import dbConnect from "@/app/lib/mongodb";
import { ProductModel } from "../models/Product";
import type { Product } from "../schemas/product.schema";

export async function getAllProducts(): Promise<Product[]> {
  await dbConnect();
  return await ProductModel.find({});
}
```

#### **Key Details**

- No try/catch — errors are thrown and caught by the route handler.
    
- Uses the inferred `Product[]` type directly (always consistent with your Zod schema).
    
- Clean and reusable.
    

---

### **24.2.2 Create Product Service**

  

**File:**

app/features/products/api/createProduct.ts

```ts
import dbConnect from "@/app/lib/mongodb";
import { ProductModel } from "../models/Product";
import type { Product } from "../schemas/product.schema";
import type { HydratedDocument } from "mongoose";

export async function createProduct(
  product: Product
): Promise<HydratedDocument<Product>> {
  await dbConnect();
  return await ProductModel.create(product);
}
```

#### **Key Details**

- Also, no try/catch — same reasoning.
    
- HydratedDocument gives correct typing for a real Mongo document.
    
- Reusable anywhere (route handler, cron job, admin panel, etc.).
    

---

# **24.3 Model Layer**

  

**Location:**

app/features/products/models/Product.ts

  

This remains unchanged — it defines the MongoDB structure.

  

This is the database layer only.

---

# **24.4 Why Service Layer?**

  

### **Benefits**

- **Cleaner route handlers**
    
    Your API routes focus only on HTTP input/output.
    
- **Reusable logic**
    
    Service functions can be used by:
    
    - admin dashboards
        
    - background tasks
        
    - CLI scripts
        
    - batch jobs
        
    
- **Centralised business logic**
    
    All CRUD operations are in one place.
    
- **Fewer bugs**
    
    All DB code lives in one predictable layer.
    
- **Easier to scale**
    
    Adding PUT, DELETE, filtering, pagination, etc. becomes trivial.
    

---

# **24.5 Error Handling Explanation**

  

You asked:

  

> “Where do errors get thrown?”

  

If a service function fails:

- Mongoose throws
    
- Zod throws
    
- dbConnect throws
    

  

This automatically bubbles up to the **closest try/catch**, which is the route handler.

  

That’s why service functions _should not_ swallow errors — they allow the handler to respond properly.

---

# **24.6 Full Folder Structure After Applying the Pattern**

```
app/
  api/
    products/
      route.ts                # route handler (HTTP)
  lib/
    mongodb.ts               # database connection

features/
  products/
    api/
      getProducts.ts         # service
      createProduct.ts       # service
    models/
      Product.ts             # mongoose model
    schemas/
      product.schema.ts      # zod schema + types
    types/
      product.ts             # re-export of inferred type
```

---

# **24.7 Summary
- Route handler = receives request, validates, catches errors.
    
- Service = executes logic, queries DB, throws errors upward.
    
- Model = defines MongoDB collection.
    
- Zod schema = runtime validation + generates the Product type.
    
- This pattern is scalable, predictable, and easy to maintain.
    

---


# **React Hook Form + Zod Notes (Based on My Questions)**

  

### **useForm() creates and stores ALL form state**

  

When I call:

```
const form = useForm<...>({...});
```

React Hook Form immediately creates an internal state container.

It stores:

- current field values
    
- touched fields
    
- dirty tracking
    
- validation errors
    
- methods like handleSubmit, reset, control, etc.
    

  

I don’t see any of this directly — useForm() handles it.

---

# **Type Parameters (the generics) do NOT run validation**

  

These:

```
z.input<typeof productSchema>,
undefined,
z.output<typeof productSchema>
```

Only tell TypeScript what types I expect:

- **input type** → raw data _before_ Zod transforms anything
    
- **context** → unused here (undefined)
    
- **output type** → final validated + transformed data from Zod
    

  

They do **not** validate anything.

They just give me strict type safety.

---

# **Validation is ONLY done through the resolver**

  

This line is what actually triggers Zod:

```
resolver: zodResolver(productSchema)
```

The resolver acts as a bridge between:

- React Hook Form
    
- Zod
    

  

RHF calls the resolver with the current form values → resolver runs Zod → Zod returns success or errors → resolver translates errors back into RHF’s format → RHF stores them in form.formState.errors.

  

The resolver is the _only_ validation mechanism.

---

# **mode: “onBlur”**

  

This means:

- User types → nothing happens
    
- User leaves the field (blur event) → RHF triggers validation → Zod runs → errors appear or disappear
    

  

Changing mode changes when validation fires.

---

# **defaultValues**

  

These values populate the form before the user types anything:

```
defaultValues: {
  title: "",
  shortDescription: "",
  longDescription: "",
  specs: {},
  reviews: [],
  price: 0,
  images: [],
  slug: "",
}
```

RHF uses this to initialize its state container.

---

# **Where methods like handleSubmit come from**

  

RHF attaches these methods to the object returned from useForm():

```
form.handleSubmit
form.reset
form.trigger
form.control
form.formState.errors
```

I don’t “get” these from anywhere else — useForm() provides them.

---

# **How the UI fields connect to the form**

  

Controller + form.control wires my input components to React Hook Form:

```
<Controller
  name="title"
  control={form.control}
  render={({ field, fieldState }) => (
    <Input {...field} />
  )}
/>
```

field contains props RHF injects:

- value
    
- onChange
    
- onBlur
    

  

fieldState contains validation status:

- invalid
    
- error
    

---

# **How errors show on the screen**

  

Zod → resolver → RHF → fieldState.error → my UI.

  

If Zod returns an error for that field:

- RHF stores it in form.formState.errors
    
- fieldState.invalid becomes true
    
- fieldState.error contains the Zod message
    
- UI components like <FieldError /> display it
    

  

Example:

```
{fieldState.invalid && <FieldError errors={[fieldState.error]} />}
```

---

# **The full flow of data (very important)**

  

### **1. User types into** 

  

<Input {...field} /> sends updates to RHF.

  

### **2. User leaves the field (blur)**

  

Because of mode: "onBlur", RHF triggers validation.

  

### **3. RHF calls the resolver**

  

Resolver runs Zod with current form values.

  

### **4. Zod returns either:**

- success → parsed, validated values
    
- failure → error messages
    

  

### **5. Resolver converts Zod errors for RHF**

  

RHF stores them in form.formState.errors.

  

### **6. UI reads errors through fieldState**

  

Errors render on the screen via `<FieldError>`.

  

### **7. On submit**

  

form.handleSubmit(onSubmit) validates again and gives me ONLY the fully validated, Zod-safe output.

---

# **FINAL SUMMARY (for quick memory)**

- useForm() → creates form state + methods
    
- Generics → ONLY tell TypeScript about input/output types
    
- resolver: zodResolver(schema) → runs Zod validation
    
- mode: "onBlur" → validate when user leaves a field
    
- Controller → connects UI components to the form system
    
- fieldState.error → how I access errors for each field
    
- handleSubmit → gets validated Zod output
    

--- 

for this code

```ts
  

return (

<Card className="w-full sm:max-w-md">

<CardHeader>

<CardTitle>Create Product</CardTitle>

<CardDescription>

Enter the details of the product below.

</CardDescription>

</CardHeader>

<CardContent>

<form id="form-new-product" onSubmit={form.handleSubmit(onSubmit)}>

<FieldGroup>

<Controller

name="title"

control={form.control}

render={({ field, fieldState }) => (

<Field data-invalid={fieldState.invalid}>

<FieldLabel htmlFor="form-rhf-demo-title">

Bug Title

</FieldLabel>

<Input

{...field}

id="form-rhf-demo-title"

aria-invalid={fieldState.invalid}

placeholder="Login button not working on mobile"

autoComplete="off"

/>

{fieldState.invalid && (

<FieldError errors={[fieldState.error]} />

)}

</Field>

)}

/>

<Controller

name="shortDescription"

control={form.control}

render={({ field, fieldState }) => (

<Field data-invalid={fieldState.invalid}>

<FieldLabel htmlFor="form-rhf-demo-description">

Description

</FieldLabel>

<InputGroup>

<InputGroupTextarea

{...field}

id="form-rhf-demo-description"

placeholder="I'm having an issue with the login button on mobile."

rows={6}

className="min-h-24 resize-none"

aria-invalid={fieldState.invalid}

/>

<InputGroupAddon align="block-end">

<InputGroupText className="tabular-nums">

{field.value.length}/100 characters

</InputGroupText>

</InputGroupAddon>

</InputGroup>

<FieldDescription>

Include steps to reproduce, expected behavior, and what

actually happened.

</FieldDescription>

{fieldState.invalid && (

<FieldError errors={[fieldState.error]} />

)}

</Field>

)}

/>

</FieldGroup>

</form>

</CardContent>

<CardFooter>

<Field orientation="horizontal">

<Button type="button" variant="outline" onClick={() => form.reset()}>

Reset

</Button>

<Button type="submit" form="form-new-product">

Submit

</Button>

</Field>

</CardFooter>

</Card>

);
```


first we are chagnig the ids to match what we are actualyl doing so the first is to chagne the  card content child where it is form, that id to form-new-product
chagne it for the button too

```ts
<Button type="submit" form="form-new-product">

Submit

</Button>
```

=
# **NOTE FOR YOUR DOCUMENTS (copy this)**

  

### **Native HTML Inputs vs Custom UI Components**

  

**Native inputs** `(<input>, <textarea>, <select>):`

- Can directly use register()
    
- Example:
    

```ts
<input {...register("title")} />
```

  

  

**Custom UI components** (shadcn Input, Select, Checkbox, RadioGroup, etc.):

- CANNOT use register() directly
    
- They don’t expose standard onChange, onBlur, value props
    
- Therefore they must use:
    

```
<Controller name="title" control={form.control} render={({ field }) => <Input {...field} />} />
```

  

  

**Controller** connects custom components to RHF, so they can:

- Update form state
    
- Trigger validation
    
- Receive errors
    

---




