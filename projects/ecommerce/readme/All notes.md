````
# AI-Powered E-Commerce Platform (Next.js, MongoDB, OpenAI)

> Full-stack AI e-commerce demo with semantic search, per-product RAG Q&A, and a complete admin + cart flow.

---

## 1. Project Description

This project is a full-stack AI-powered e-commerce platform built with Next.js, MongoDB, and OpenAI.  
It demonstrates semantic product search, per-product retrieval-augmented generation (RAG) Q&A, and a complete admin + user shopping flow.

The goal is to showcase modern full-stack architecture, vector search, and real-world AI integration across:

- Public catalogue: browse products, search by meaning, view details, ask questions, add to cart.
- Admin tools: create and edit products via schema-driven forms.
- AI features: semantic search + grounded Q&A using embeddings and MongoDB `$vectorSearch`.

---

## 2. Features

### 2.1 User-Facing Features

- **Product Catalogue**
  - Browse all products in a responsive grid.
  - View title, price, images, specs, and reviews.
  - Natural-language search (semantic search).

- **Product Detail Page** (`/products/[slug]`)
  - Full product view: gallery, descriptions, specs, reviews.
  - Per-product AI Q&A widget (RAG-based).
  - Add-to-cart button integrated with global cart.

- **Semantic Search**
  - Search by *meaning*, not just keywords.
  - Example: “phone good for travel” returns phones with battery, durability, etc.
  - Results ranked by embedding similarity score.

- **Cart**
  - Add/remove items from anywhere in the app.
  - Increase/decrease quantity.
  - Persistent cart via `localStorage`.
  - Order summary with totals, empty state, and responsive layout.

- **Layout**
  - Global navbar with links to Products, Cart, and About.
  - Global footer with social links and inline SVG icons.
  - Mobile-first, responsive design built with Tailwind.

### 2.2 Admin / Developer Features

- **Admin Product Management**
  - `/admin/products/new` – create new products via a shared `ProductForm`.
  - `/admin/products/[slug]` – edit existing products via `ProductEditForm`.
  - Dynamic field arrays for:
    - Images
    - Reviews
    - Specifications (key/value pairs)
  - Full Zod validation and error reporting.

- **Backend APIs**
  - CRUD endpoints for products.
  - Semantic search endpoint for vector queries.
  - Per-product Q&A endpoint implementing RAG.
  - Embeddings inspection endpoint for debugging.

- **Embedding Pipeline**
  - Product ingestion → text building → chunking → embedding → storing chunks.
  - Rebuild script to regenerate all embeddings when products or models change.

- **Cart Architecture**
  - React Context-based cart with slug → quantity mapping.
  - Local storage utilities and merge helpers for product data.

---

## 3. Architecture Overview

### 3.1 Frontend

- **Framework:** Next.js 16 (App Router), React 19, TypeScript.
- **UI / Styling:** Tailwind CSS v4, shadcn/ui (Radix primitives), `lucide-react`, custom SVG icons.
- **State Management:**
  - React state/hooks for local state.
  - React Context for global cart.
- **Forms:**
  - React Hook Form + Zod for strongly typed form validation.
  - Reusable field components and field arrays for specs, images, reviews.

### 3.2 Backend

- **API Layer:** Next.js Route Handlers under `app/api/**`.
  - `/api/products` – list/create products.
  - `/api/products/[slug]` – fetch/update single product.
  - `/api/products/ask` – per-product Q&A (RAG).
  - `/api/search` – global semantic search.
  - `/api/products/chunks/[productId]` – debug embeddings.
- **Business Logic:**
  - Service functions for CRUD operations.
  - Shared Zod schemas for validation.
  - Custom error classes (BadRequestError, NotFoundError).

### 3.3 Database

- **DB:** MongoDB.
- **ORM:** Mongoose.
- **Core Models:**
  - `Product` – product data (title, slug, descriptions, specs, images, reviews, price).
  - `ProductChunk` – chunked text and embeddings tied to a product.

### 3.4 AI Layer

- **Provider:** OpenAI.
- **Models:**
  - Embeddings: `text-embedding-3-small`.
  - Completion/Q&A: `gpt-4.1-mini`.
- **Capabilities:**
  - Semantic search via embedding → MongoDB `$vectorSearch`.
  - RAG Q&A: question embedding → per-product vector search → GPT answer grounded in retrieved chunks.

### 3.5 Cart System

- **CartContext:** React Context storing `Record<slug, quantity>`.
- **Persistence:** `localStorage` with guarded access (client-only).
- **Helpers:**
  - Merge functions combining cart state with product documents fetched from API.
- **UI:**
  - Cart page, AddToCart button, line-item components, error boundary.

---

## 4. Tech Stack

### Frontend

- Next.js 16 (App Router)
- React 19
- TypeScript 5
- Tailwind CSS v4
- shadcn/ui (Radix primitives)
- `@radix-ui/react-navigation-menu`, `@radix-ui/react-label`, `@radix-ui/react-separator`, `@radix-ui/react-slot`
- `lucide-react` (icons)
- `clsx`, `class-variance-authority`, `tailwind-merge`
- `sonner` (toasts)
- `tw-animate-css`

### Backend

- Next.js Route Handlers (`app/api/**`)
- MongoDB
- Mongoose

### Validation & Forms

- Zod
- React Hook Form
- `@hookform/resolvers`

### AI

- `openai` Node SDK
- Embeddings: `text-embedding-3-small`
- Completions: `gpt-4.1-mini`

### Tooling

- `@biomejs/biome` – linting, formatting, checks
- `tsx` – run TypeScript scripts
- `@commitlint/cli`, `@commitlint/config-conventional`, `@commitlint/prompt-cli`
- `baseline-browser-mapping`
- TypeScript strict mode

---


## **6. API Documentation**

  

All endpoints are under /api/** and return JSON.

  

### **6.1** 

### **GET /api/products**

  

**Purpose:** List products (optionally filter by keyword).

  

**Query params:**

- q (optional) – keyword search string (non-semantic).
    

  

**Example request:**

```
GET /api/products?q=phone
```

**Example response:**

```
{
  "success": true,
  "data": [
    {
      "_id": "65f0...",
      "slug": "travel-phone-x",
      "title": "Travel Phone X",
      "price": 799,
      "shortDescription": "Rugged phone with long battery life.",
      "images": ["/images/travel-phone-x-1.jpg"],
      "specs": {
        "Battery": "5000 mAh",
        "Durability": "IP68"
      },
      "reviews": [
        {
          "author": "Alice",
          "rating": 5,
          "comment": "Perfect for travel."
        }
      ]
    }
  ]
}
```

---

### **6.2** 

### **POST /api/products**

  

**Purpose:** Create a new product.

  

**Body:** Must match productSchema (Zod).

  

**Example request:**

```
POST /api/products
Content-Type: application/json
```

```
{
  "title": "Travel Phone X",
  "slug": "travel-phone-x",
  "price": 799,
  "shortDescription": "Rugged phone with long battery life.",
  "longDescription": "Detailed long description...",
  "images": ["/images/travel-phone-x-1.jpg"],
  "specRows": [
    { "key": "Battery", "value": "5000 mAh" },
    { "key": "Durability", "value": "IP68" }
  ],
  "reviews": [
    { "author": "Alice", "rating": 5, "comment": "Perfect for travel." }
  ]
}
```

**Example response:**

```
{
  "success": true,
  "data": {
    "_id": "65f0...",
    "slug": "travel-phone-x",
    "title": "Travel Phone X",
    "price": 799
  }
}
```

On validation error (Zod):

```
{
  "success": false,
  "error": "ValidationError",
  "details": {
    "title": ["Title is required"],
    "price": ["Price must be a positive number"]
  }
}
```

---

### **6.3** 

### **GET /api/products/[slug]**

  

**Purpose:** Fetch a single product by slug.

  

**Example request:**

```
GET /api/products/travel-phone-x
```

**Example response:**

```
{
  "success": true,
  "data": {
    "_id": "65f0...",
    "slug": "travel-phone-x",
    "title": "Travel Phone X",
    "price": 799,
    "shortDescription": "Rugged phone with long battery life.",
    "longDescription": "Detailed long description...",
    "images": ["/images/travel-phone-x-1.jpg"],
    "specs": {
      "Battery": "5000 mAh",
      "Durability": "IP68"
    },
    "reviews": [
      { "author": "Alice", "rating": 5, "comment": "Perfect for travel." }
    ]
  }
}
```

If not found:

```
{
  "success": false,
  "error": "NotFoundError"
}
```

---

### **6.4** 

### **PUT /api/products/[slug]**

  

**Purpose:** Update an existing product.

  

**Body:** Must match updateProductSchema (Zod). Typically the same shape as creation payload.

  

**Example request:**

```
PUT /api/products/travel-phone-x
Content-Type: application/json
```

```
{
  "title": "Travel Phone X (2025 Edition)",
  "price": 829,
  "shortDescription": "Updated rugged phone.",
  "longDescription": "New long description...",
  "images": ["/images/travel-phone-x-1.jpg", "/images/travel-phone-x-2.jpg"],
  "specRows": [
    { "key": "Battery", "value": "5200 mAh" },
    { "key": "Durability", "value": "IP68" }
  ],
  "reviews": []
}
```

**Example response:**

```
{
  "success": true,
  "data": {
    "_id": "65f0...",
    "slug": "travel-phone-x",
    "title": "Travel Phone X (2025 Edition)",
    "price": 829
  }
}
```

---

### **6.5** 

### **GET /api/search**

  

**Purpose:** Semantic search across all products via vector similarity.

  

**Query params:**

- q (required) – natural-language query.
    

  

**Example request:**

```
GET /api/search?q=phone+good+for+travel
```

**Example response:**

```
{
  "success": true,
  "data": {
    "products": [
      {
        "_id": "65f0...",
        "slug": "travel-phone-x",
        "title": "Travel Phone X",
        "price": 799
      },
      {
        "_id": "65f1...",
        "slug": "endurance-phone-z",
        "title": "Endurance Phone Z",
        "price": 699
      }
    ]
  }
}
```

---

### **6.6** 

### **POST /api/products/ask**

  

**Purpose:** Product-specific Q&A using RAG.

  

**Body:**

- productId (string, Mongo ObjectId)
    
- question (string)
    

  

**Example request:**

```
POST /api/products/ask
Content-Type: application/json
```

```
{
  "productId": "65f0...",
  "question": "Is this phone suitable for long trips without charging?"
}
```

**Example response:**

```
{
  "success": true,
  "data": {
    "answer": "Yes. The phone has a 5000 mAh battery and several reviews mention it lasting multiple days of travel.",
    "chunksUsed": [
      "Battery: 5000 mAh...",
      "Review: I used this for a 3-day trip without charging..."
    ]
  }
}
```

On invalid productId or empty question:

```
{
  "success": false,
  "error": "BadRequestError",
  "message": "productId and question are required."
}
```

---

### **6.7** 

### **GET /api/products/chunks/[productId]**

  

**Purpose:** Debug endpoint to inspect text chunks + embeddings for a product.

  

**Example request:**

```
GET /api/products/chunks/65f0...
```

**Example response:**

```
{
  "success": true,
  "data": [
    {
      "_id": "66a1...",
      "productId": "65f0...",
      "text": "Travel Phone X is designed for long trips...",
      "embedding": [0.0123, -0.0456, ...]
    }
  ]
}
```

---

## **7. Installation**

  

### **7.1 Prerequisites**

- Node.js (LTS recommended)
    
- npm or pnpm
    
- MongoDB instance (local or MongoDB Atlas)
    
- OpenAI API key
    

  

### **7.2 Clone and Install**

```
git clone <YOUR_REPO_URL> ecommerce-project
cd ecommerce-project
npm install
```

### **7.3 Environment Variables**

  

Create a .env file in the root:

```
touch .env
```

Suggested variables:

```
OPENAI_API_KEY=sk-...
MONGODB_URI=mongodb+srv://user:password@cluster/dbname
NEXT_PUBLIC_BASE_URL=http://localhost:3000
# Add any other project-specific env vars here
```

> Note: npm run t and npm run rebuild use --env-file=.env, so ensure this file exists.

  

### **7.4 Run Dev Server**

```
npm run dev
# App will be available at http://localhost:3000
```

### **7.5 Build & Start**

```
npm run build
npm run start
```

### **7.6 (Optional) Rebuild Embeddings**

  

After adding or changing many products:

```
npm run rebuild
```

---

## **8. Scripts (**

## **package.json**

## **)**

```
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "format": "biome format . --write",
    "lint": "biome lint . --write",
    "check": "biome check .",
    "tidy": "biome check . --write",
    "commit": "commit",
    "t": "tsx --env-file=.env app/features/embedding/textExamples.ts",
    "rebuild": "tsx --env-file=.env app/features/embedding/scripts/rebuildAllEmbeddings.ts"
  }
}
```

### **Script Explanations**

- **dev**
    
    Starts the Next.js development server.
    
- **build**
    
    Builds the Next.js application for production.
    
- **start**
    
    Starts the production server after npm run build.
    
- **format**
    
    Formats the entire codebase using Biome with --write.
    
- **lint**
    
    Lints the entire codebase using Biome with --write for autofixable issues.
    
- **check**
    
    Runs Biome checks (lint + format) without writing changes; useful for CI.
    
- **tidy**
    
    Runs Biome check with --write to auto-fix issues and tidy the code.
    
- **commit**
    
    Entry point for @commitlint/prompt-cli (interactive commit messages) or a custom commit script.
    
- **t**
    
    Runs the embeddings text examples script (app/features/embedding/textExamples.ts) with .env loaded via tsx.
    
- **rebuild**
    
    Rebuilds all product embeddings using app/features/embedding/scripts/rebuildAllEmbeddings.ts.
    

---

## **9. Environment Variables**

  

Recommended .env keys:

- OPENAI_API_KEY – OpenAI API key used for embeddings and GPT calls.
    
- MONGODB_URI – MongoDB connection URI (local or Atlas).
    
- NEXT_PUBLIC_BASE_URL – Base URL for API calls (optional, e.g. http://localhost:3000 in dev).
    

  

You can add more vars as needed (e.g. logging, feature flags).

---

## **10. Data Model Schemas**

  

### **10.1 Product**

  

**Mongoose (conceptual):**

```
import { Schema, model } from "mongoose";

const ProductSchema = new Schema(
  {
    title: { type: String, required: true },
    slug: { type: String, required: true, unique: true },
    price: { type: Number, required: true },
    shortDescription: { type: String, required: true },
    longDescription: { type: String, required: true },
    specs: {
      type: Map,
      of: String
    },
    images: [{ type: String }],
    reviews: [
      {
        author: String,
        rating: Number,
        comment: String
      }
    ]
  },
  { timestamps: true }
);

export const Product = model("Product", ProductSchema);
```

**Form-layer (Zod):**

```
import { z } from "zod";

export const specRowSchema = z.object({
  key: z.string().min(1),
  value: z.string().min(1)
});

export const productSchema = z.object({
  title: z.string().min(1),
  slug: z.string().min(1),
  price: z.number().positive(),
  shortDescription: z.string().min(1),
  longDescription: z.string().min(1),
  images: z.array(z.string().url()).default([]),
  specRows: z.array(specRowSchema).default([]),
  reviews: z
    .array(
      z.object({
        author: z.string().min(1),
        rating: z.number().min(1).max(5),
        comment: z.string().min(1)
      })
    )
    .default([])
});
```

Conversion utilities map specRows ↔ specs map.

---

### **10.2 ProductChunk**

  

**Mongoose (conceptual):**

```
import { Schema, model, Types } from "mongoose";

const ProductChunkSchema = new Schema(
  {
    productId: { type: Types.ObjectId, ref: "Product", required: true },
    text: { type: String, required: true },
    embedding: { type: [Number], required: true }
  },
  { timestamps: true }
);

export const ProductChunk = model("ProductChunk", ProductChunkSchema);
```

---

## **11. AI / Embeddings Pipeline**

  

The embeddings pipeline powers both semantic search and product-specific Q&A.

  

### **11.1 Text Building**

- buildProductText(product):
    
    - Concatenates:
        
        - Title
            
        - Short description
            
        - Long description
            
        - Specs (key/value)
            
        - Reviews (author, rating, comment)
            
        
    - Produces one large string per product.
        
    

  

### **11.2 Chunking**

- chunkText(text):
    
    - Splits text into ~500-character segments.
        
    - Splits at word boundaries (last space) to avoid cutting words.
        
    - Returns an array:
        
    

```
type Chunk = { text: string };
```

  

  

### **11.3 Embedding Generation**

- generateEmbedding(text):
    
    - Validates non-empty text.
        
    - Calls OpenAI text-embedding-3-small.
        
    - Returns number[] embedding vector.
        
    - Throws descriptive errors on failure.
        
    

  

### **11.4 Storing Embeddings**

- storeProductEmbeddings(productId):
    
    1. Connect to MongoDB.
        
    2. Load product from DB.
        
    3. Build full text via buildProductText.
        
    4. Chunk text via chunkText.
        
    5. Delete old chunks for this product.
        
    6. Generate embedding for each chunk.
        
    7. Insert all chunks into ProductChunk collection.
        
    8. Return { chunkCount }.
        
    

  

### **11.5 Rebuild Script**

- app/features/embedding/scripts/rebuildAllEmbeddings.ts:
    
    - Clears all ProductChunk documents.
        
    - Iterates over all products.
        
    - Calls storeProductEmbeddings(productId) for each.
        
    - Run via:
        
    

```
npm run rebuild
```

  

---

## **12. Semantic Search**

  

Semantic search is implemented in GET /api/search.

  

### **Workflow**

1. **Input:** User query q (e.g. “phone good for travel”).
    
2. **Embedding:**
    
    - Generate query embedding with text-embedding-3-small.
        
    
3. **Vector Search:**
    
    - Run $vectorSearch on ProductChunk collection.
        
    - Filter by score using a minimum score threshold (MIN_SCORE).
        
    
4. **Aggregation:**
    
    - Group matches by productId.
        
    - Rank products by maximum or average similarity score.
        
    
5. **Fetch Products:**
    
    - Retrieve full Product documents for the top product IDs.
        
    
6. **Response:**
    
    - Return list of matching products ordered by semantic relevance.
        
    

---

## **13. RAG Q&A (Per-Product)**

  

Product-specific Q&A is implemented in POST /api/products/ask.

  

### **Workflow**

1. **Input:**
    
    - productId
        
    - question
        
    
2. **Validation:**
    
    - Validate productId is a valid ObjectId.
        
    - Validate question is non-empty.
        
    
3. **Embedding:**
    
    - Generate question embedding with text-embedding-3-small.
        
    
4. **Vector Search:**
    
    - Run $vectorSearch on ProductChunk where productId matches.
        
    - Apply score threshold to keep only relevant chunks.
        
    
5. **Context Building:**
    
    - Concatenate top chunk texts into a context string.
        
    
6. **GPT Call:**
    
    - Call gpt-4.1-mini with:
        
        - System instructions to answer _only_ based on provided context.
            
        - User question + context.
            
        
    - Forbid invented facts.
        
    
7. **Response:**
    
    - Return generated answer + optionally the chunks used.
        
    

---

## **14. Cart System**

  

The cart is a client-side state container backed by localStorage.

  

### **Components**

1. **CartContext**
    
    - Stores:
        
    

```
type CartState = Record<string, number>; // slug -> quantity
```

1. -   
        
    - Actions:
        
        - addItem(slug)
            
        - increase(slug)
            
        - decrease(slug) (removes if quantity reaches 0)
            
        - remove(slug)
            
        
    - Hydrates from localStorage after mount.
        
    - Persists changes back to localStorage.
        
    - Exposes totalCount and other derived values.
        
    
2. **Local Storage Utilities (****cart-storage.ts****)**
    
    - getFromLocalStorage(key)
        
    - setToLocalStorage(key, value)
        
    - Guards against SSR by checking window.
        
    
3. **mergeCartItems.ts**
    
    - Combines cart state with product data fetched from API:
        
    

```
{
  slug: string;
  quantity: number;
  product: Product;
}
```

3.   
    
4. **Cart Page (****/cart****)**
    
    - Uses CartContext + fetchCartProducts to load product details.
        
    - Renders:
        
        - Empty state if no items.
            
        - List of cart line items.
            
        - Order summary with subtotals and total price.
            
        
    
5. **AddToCartButton**
    
    - Calls addItem(slug) on click.
        
    - Shows toast confirmation via sonner.
        
    
6. **CartErrorBoundary**
    
    - Catches errors in cart components.
        
    - Prevents cart issues from breaking the entire app.
        
    

---

## **15. Screenshots (Placeholders)**

  

Add your own images to docs/screenshots or similar and update the paths.

```
![Products page](./docs/screenshots/products-page.png)
![Product detail with Q&A](./docs/screenshots/product-detail-qa.png)
![Semantic search results](./docs/screenshots/semantic-search.png)
![Cart](./docs/screenshots/cart.png)
![Admin create product](./docs/screenshots/admin-create-product.png)
```

---

## **16. Limitations & Future Improvements**

  

### **Current Limitations**

- No authentication or user accounts.
    
- No payments / checkout flow.
    
- No rate limiting or advanced monitoring on AI endpoints.
    
- AI usage incurs token cost on OpenAI.
    
- Admin pages are for demo only and not production-hardened.
    

  

### **Potential Improvements**

- Add user auth (NextAuth, Auth.js, or custom) and order history.
    
- Integrate a payment provider (Stripe) for a full checkout.
    
- Add caching and rate limiting for AI endpoints.
    
- Add integration and unit tests for:
    
    - API routes
        
    - Embedding pipeline
        
    - Cart logic
        
    
- Add better observability (logging, tracing, metrics) for production use.
    
- Support multiple embedding providers or a self-hosted vector database.
    
- Implement role-based admin access and audit logs for product changes.
    

---

## **17. License**

  

No explicit license has been set yet.

  

By default this means **“All rights reserved”**.

If you intend for others to use/modify this code, add a standard open-source license (e.g. MIT) to LICENSE and update this section accordingly.

```
MIT License (planned)
```

_(Replace with your preferred license text.)_