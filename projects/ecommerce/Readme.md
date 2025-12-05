
# **✅ README NOTES — FOR DEVELOPERS (Technical, concise)**

  

**Purpose of README:**

Describe _how the project works internally_, _how to run it_, and _where to find things in the code_.

  

### **✔ Technical Summary**

- Next.js App Router
    
- MongoDB + Mongoose
    
- OpenAI models for embeddings + RAG
    
- Tailwind CSS v4 + shadcn/ui
    

  

### **✔ Features (technical wording)**

- /products: semantic search powered by vector similarity
    
- /products/[slug]: RAG Q&A component, embeddings retrieval
    
- /cart: add/remove/update items
    
- /admin/product + /admin/product/[slug]: CRUD interfaces
    
- MongoDB schema includes product chunks + embedding vectors
    

  

### **✔ Architecture Notes**

- Product ingestion → chunking → embedding → stored in MongoDB
    
- Query pipeline:
    
    1. Encode user query
        
    2. Vector search against product chunks
        
    3. Build context
        
    4. Generate grounded answer
        
    
- API routes:
    
    - /api/products/ask → Q&A
        
    - /api/products/search → semantic search
        
    - MongoDB connection in /lib or per-route pattern
        
    

  

### **✔ File Structure Highlights**

- app/features/products/* → schemas, forms, components
    
- app/features/embedding/* → chunking + embeddings logic
    
- app/api/products/* → backend logic
    
- app/cart/* → cart UI + store
    
- app/layout.tsx → nav, global layout
    

  

### **✔ Installation Steps (README only)**

- Clone repo
    
- Install deps
    
- Add environment variables (OpenAI key, MongoDB URI)
    
- Run dev server
    
- (Optional) Run embeddings rebuild script
    

  

### **✔ Limitations**

- No auth
    
- No payments
    
- AI costs tokens
    
- Demonstration only
    

---

# **✅ Summary Format (how you reuse this for every file you give me)**

  

When you give me a file next time, I’ll produce these two sections automatically:

---

## **ABOUT PAGE NOTES**

  

✔ Public-facing explanation

✔ Why the feature matters

✔ What users can do with it

✔ High-level non-technical notes

---

## **README NOTES**

  

✔ Technical behaviour summary

✔ Where it lives in the codebase

✔ How it integrates with other features

✔ Any setup or configuration needed

---

If you want, I can take ALL your major feature files one by one and produce a full **documentation pack** containing:

- Public About Page text
    
- README sections
    
- Developer notes
    
- CV bullet points
    

  

Just say: **“Do the next file.”**


# **✅** 

# **2. README Notes (technical, clear, for developers)**

  

These describe how the pages work internally and what a developer needs to know.

---

## **Create Product Page — Developer Notes**

  

**File:** app/admin/products/new/page.tsx

**Purpose:** Render a blank product form for creating new products.

  

### **Key Implementations**

- Imports shadcn <Card> components for consistent UI structure.
    
- Uses ProductForm (the shared form component for CREATE operations).
    
- Contains no business logic — all submission logic is handled inside the form component.
    
- Automatically integrates with validation (Zod) and backend route for creating a product.
    

  

### **Where the logic lives**

- Form definition: app/features/products/forms/product-form.tsx
    
- Validation schema: app/features/products/schemas/product.schema.ts
    
- API endpoint: app/api/products/route.ts (POST)
    

---

## **Edit Product Page — Developer Notes**

  

**File:** app/admin/products/[slug]/page.tsx

**Purpose:** Load an existing product and render a pre-filled edit form.

  

### **Key Implementations**

- Fetches product by slug using getProductBySlug().
    
- Handles 404 using Next.js notFound() if product doesn’t exist.
    
- Converts Mongoose document → plain object with .toObject() to avoid serialization errors.
    
- Extracts and normalizes:
    
    - specs (Mongoose Map → JS object)
        
    - specRows (for editable UI tables)
        
    - images, reviews arrays
        
    

  

### **Important Details**

- Uses ProductEditForm (extends the base form for UPDATE operations).
    
- Passes initialData into the form for pre-filling fields.
    
- Ensures clean serialization for server → client boundary (App Router requirement).
    

  

### **Where the logic lives**

- Data fetching:
    
    app/features/products/api/getProductBySlug.ts
    
- Form:
    
    app/features/products/forms/product-edit-form.tsx
    
- Transform helpers:
    
    - mapToObject.ts
        
    - transform-spec-rows.ts
        
    

---

# **✅ If you want, I can generate:**

  

✔ A full professional README section titled **“Admin Product Management”**

✔ An About-page-ready visual description with screenshots placeholders

✔ CV bullet points for this feature (technical + business value)

✔ Architecture diagram (ASCII or image-group)

  

Just tell me _which one you want next_



# **✅** 

# **README DOCUMENTATION SUMMARY (Technical, Precise)**

  

This project exposes several backend API routes that power semantic search, product CRUD, and product-specific RAG Q&A.

  

### **/api/products/[slug] — GET & PUT**

- **GET**: Fetch single product by slug.
    
- **PUT**: Validate request body via updateProductSchema (Zod) and update product using validateAndUpdateProduct.
    
- Handles typed errors (ZodError, BadRequestError, NotFoundError).
    
- Serializes Mongoose documents safely for Next.js.
    

---

### **/api/products — GET & POST**

- **GET**: Optional query ?q= for keyword-based product filtering.
    
- **POST**: Creates a product after strict validation via productSchema.
    
- Returns structured JSON & treeified Zod errors.
    

---

### **/api/products/ask — Product RAG Q&A (POST)**

  

Pipeline:

1. Validate productId & question.
    
2. Generate embedding for question.
    
3. Run $vectorSearch filtered by productId to retrieve top semantic matches.
    
4. Apply MIN_SCORE threshold.
    
5. Build context string from retrieved chunks.
    
6. Call OpenAI (gpt-4.1-mini) with grounding instructions:
    
    - Only use context
        
    - Summaries only
        
    - No invented facts
        
    
7. Return generated answer + the chunk texts used.
    

---

### **/api/search — Semantic Search (GET)**

- Requires query param ?q=.
    
- Embeds query text.
    
- Performs $vectorSearch across **all** product chunks.
    
- Applies score threshold (MIN_SCORE).
    
- Deduplicates and ranks product IDs based on similarity score.
    
- Fetches matched products and returns them in similarity order.
    

---

### **/api/products/chunks/[productId] — GET**

- Returns all stored product chunks (text + embedding).
    
- Validates ObjectId format.
    
- Used mainly for debugging embeddings/RAG.
    

---

### **Shared Features Across Routes**

- Automatic DB connection (dbConnect) on each request.
    
- Strong error handling with JSON error responses.
    
- Uses Mongoose .lean() or .toObject() to avoid serialization issues.
    
- Fully typed using TypeScript + Mongoose HydratedDocument.
    

---

If you want, I can now also create:

  

✅ A polished README file

✅ A polished About page text

✅ A polished CV project entry

✅ Architecture diagram

✅ Data flow diagram (Search + RAG)

Just tell me what you want next.


# **✅** 

# **README VERSION — clear, technical, implementation-focused**

  

Use this for documentation of how the cart works internally.

---

## **Cart Architecture Overview**

  

The cart is implemented as a **client-side state container** backed by localStorage.

The system consists of:

  

### **###** 

### **1. CartContext (state + actions)**

  

File: app/features/cart/context/CartContext.tsx

- Stores cart as:
    

```
Record<string, number> // slug → quantity
```

-   
    
- Hydrates state from localStorage on first render.
    
- Persists updates automatically with a useEffect side-effect.
    
- Exposes actions:
    
    - addItem(slug)
        
    - increase(slug)
        
    - decrease(slug) (removes item if qty = 0)
        
    - remove(slug)
        
    
- Provides totalCount and memoized value for performance.
    
- Must wrap the app in a <CartProvider>.
    

---

## **2. Local Storage Utilities**

  

File: cart-storage.ts

  

Handles persistence:

- addToLocalStorage(key, value) — JSON encode + write.
    
- getFromLocalStorage(key) — parse + handle read errors gracefully.
    

  

LocalStorage is only accessed **client-side** and guarded to avoid SSR issues.

---

## **3. Merging Local Cart With Product Data**

  

File: mergeCartItems.ts

  

Cart only stores slugs + quantities.

This helper merges cart entries with actual product documents fetched from the server:

```
{ slug, quantity, product }
```

Used by CartPage to build the final UI structure.

---

## **4. Cart Page Rendering**

  

File: app/cart/page.tsx

  

Key behaviours:

- Avoids SSR hydration mismatch using a mounted flag.
    
- Converts cart slugs to an array, fetches product data via /api/products/[slug].
    
- Uses mergeCartItems to combine quantities + product info.
    
- Calculates:
    
    - Total items
        
    - Line totals
        
    - Order total
        
    
- Renders:
    
    - Empty cart state
        
    - Summary panel
        
    - Item list (CartLine)
        
    - Quantity selector (1–8)
        
    - Remove item button
        
    

---

## **5. Add to Cart Button**

  

File: AddToCartButton.tsx

- Calls addItem(slug) from CartContext.
    
- Shows toast confirmation.
    
- Purely client-side React component.
    

---

## **6. DisplayProduct (alternate cart view component)**

  

File: DisplayProduct.tsx

- Displays product image + title + qty + total.
    
- Uses CartContext actions for + / – / remove.
    
- Includes a skeleton component while loading.
    

---

## **7. Error Boundary**

  

File: CartErrorBoundary.tsx

  

Purpose:

- Prevent cart errors from breaking the main app.
    
- Logs internal errors and shows a fallback UI.
    

---

## **8. fetchCartProducts**

  

File: fetchCartProducts.ts

- Accepts an array of slugs.
    
- Fetches each product through /api/products/<slug>.
    
- Returns hydrated product documents for rendering.
    

---

# **If you want, I can also create:**

  

✅ A full polished README.md

✅ A full “About this project” page rewrite

✅ A CV bullet-point block summarizing the stack and patterns

✅ A diagram of data flow (cart → merge → fetch → UI)

  

Just say the word.


# **README SECTION (detailed, technical, implementation-focused)**

  

## **ProductChunk Schema**

  

Defines the MongoDB model used to store individual text chunks + embeddings.

  

**Key points:**

- Fields: productId, text, embedding[]
    
- Embeddings stored as numeric arrays
    
- Timestamps enabled
    
- Exported as a reusable Mongoose model
    
- Used by both search API & embeddings rebuild script
    

  

## **Embeddings Rebuild Script**

  

File: _rebuildAllEmbeddings.ts_

  

**Purpose:**

- Clears all existing chunks
    
- Fetches all products
    
- Calls storeProductEmbeddings() for each
    
- Ensures embedding data stays up-to-date
    
- Useful when changing embedding model or product dataset
    

  

**Commands:**

```
npm run rebuild
```

## **buildProductText()**

- Converts a product into a concatenated text document.
    
- Combines:
    
    **title, shortDescription, longDescription, specs, reviews**.
    
- Normalizes structure for consistent chunking + embeddings.
    

  

## **chunkText()**

- Splits long product text into ~500-character chunks.
    
- Preserves word boundaries (cut at last space).
    
- Returns clean { text }[] array for embedding.
    

  

## **generateEmbedding()**

- Wraps OpenAI embedding call (text-embedding-3-small).
    
- Validates input text.
    
- Throws descriptive errors on failure.
    
- Returns raw number[] embedding vector.
    

  

## **storeProductEmbeddings()**

  

Core pipeline step.

  

**Workflow:**

1. Connect to MongoDB
    
2. Build full product text → buildProductText()
    
3. Chunk into pieces → chunkText()
    
4. Convert product ID → mongoose.ObjectId
    
5. Delete old embeddings for that product
    
6. Generate embedding per chunk → generateEmbedding()
    
7. Insert all chunks into ProductChunk collection
    
8. Return { chunkCount }
    

  

**Used by:**

- Rebuild script
    
- Admin updates (optional)
    
- Q&A and semantic search APIs
    

---



---

# **✅** 

# **2. README Notes (Technical, Developer-Facing)**

  

### **MainNavbar**

  

**Location:** app/features/layout/components/MainNavbar.tsx

**Purpose:** Global navigation for the entire Next.js application.

  

**Key Details:**

- Client component containing the top navigation bar.
    
- Built with NavigationMenu from **@/components/ui/navigation-menu** (shadcn/ui).
    
- Uses Link from Next.js for client-side navigation.
    
- Contains three primary routes:
    
    - /products
        
    - /cart
        
    - /about
        
    
- Encapsulated inside the app/layout.tsx root layout for global reuse.
    
- Layout is responsive, uses TailwindCSS utility classes, and supports theming via design tokens (background, border, muted, foreground, etc.)
    

---

### **MainFooter**

  

**Location:** app/features/layout/components/MainFooter.tsx

**Purpose:** Global footer providing brand attribution + social links.

  

**Key Details:**

- Client component for footer rendering across all pages.
    
- Contains a static socials config array mapping names → href → SVG icon component.
    
- Uses Next.js Link for navigation and accessibility labels (aria-label).
    
- Icons defined as **inline SVG components** to avoid external dependencies and allow full Tailwind styling.
    
- Responsive flex layout with spacing, ordering, and theming handled by Tailwind.
    
- Included in app/layout.tsx under the layout wrapper.
  


# **✅** 

# **SECTION 2 — README Notes (developer-facing)**

  

### **SearchBar Component**

  

**File:** app/features/products/components/SearchBar.tsx

  

A controlled input component used for both keyword search and semantic embedding search.

  

#### **Props**

```
{
  value: string                 // controlled input value
  onChange: (query: string) => void   // updates search query state
  onSearch: () => void          // triggers when user clicks Search or presses Enter
  placeholder?: string
  className?: string
}
```

#### **Behaviour**

- Pressing **Enter** triggers onSearch().
    
- Search button calls onSearch() manually.
    
- The input has **no internal state** — parents must manage the query.
    
- Uses cn() for class merging and shadcn Input/Button for consistent styling.
    
- Layout:
    
    - full-width
        
    - rounded container
        
    - border + shadow
        
    - responsive spacing
        
    - large text input
        
    

  

#### **Usage Example**

```
<SearchBar
  value={query}
  onChange={setQuery}
  onSearch={() => handleSearch(query)}
  placeholder="Search for phones, jackets, laptops…"
/>
```

#### **Purpose in the project**

- Used on /products to trigger **vector-based search** (via OpenAI embeddings + MongoDB $vectorSearch).
    
- Optionally can fallback to keyword search when embeddings aren’t present.
    
- Keeps UI logic separate from search logic for easier maintenance.
    

---

# **📘** 

# **README Section (technical, implementation-level)**

  

This is everything that belongs in your README _because it explains the architecture_.

  

You do NOT put this in your portfolio “About Me” page — this is project-internal documentation.

---

## **🧩 1. Domain Model & Validation**

  

### **product.schema.ts**

- Defines strict Zod validation for:
    
    - title, descriptions, specs, reviews, price, images, slug.
        
    
- Adds specRows for the form layer.
    
- Shared between frontend + backend for type-safety.
    

---

## **🗃 2. Mongoose Model**

  

### **ProductModel**

- Enforces DB-level constraints.
    
- Validates lengths against PRODUCT_LIMITS.
    
- Stores specs as **Map<string,string>**.
    
- Supports timestamps.
    

---

## **🔧 3. Data Conversion Utilities**

  

### **convertSpecRowsToSpecs()**

###  **/** 

### **convertSpecsToSpecRows()**

- Converts between:
    
    - **form format** → array of {key,value}
        
    - **database format** → object/map
        
    

  

### **mapToObject()**

- Safely converts a Mongoose Map to a plain JS object.
    

  

### **formatProductPayload()**

- Removes specRows, injects real specs object.
    

---

## **🌐 4. API Services**

  

### **products-api.ts**

- Client-side API wrapper for:
    
    - getAll()
        
    - search()
        
    - getOne()
        
    
- Normalizes API responses.
    

  

### **createProduct()**

###  **+** 

### **updateProduct()**

- POST/PUT requests with proper error handling.
    

  

### **Backend services (**

### **createProduct**

### **,** 

### **getProductBySlug**

### **,** 

### **updateProduct**

### **, etc.)**

- Connect to MongoDB.
    
- Perform validation.
    
- Update or fetch data.
    
- Converts spec objects to Maps before saving.
    

---

## **🧵 5. Hooks**

  

### **useProductFormSubmit()**

- Handles:
    
    - formatting payload
        
    - calling POST endpoint
        
    - success/error toasts
        
    - resetting the form
        
    

  

### **useProductEditFormSubmit()**

- Handles:
    
    - formatting payload
        
    - calling PUT endpoint
        
    - refreshing page
        
    - showing toast notifications
        
    

  

### **useProducts()**

- Search logic
    
- State management
    
- Initial fetch
    
- Exposes:
    
    - products
        
    - search
        
    - hasProducts
        
    - loading
        
    - hasSearched
        
    

---

## **🖥 6. Form Components**

  

Reusable form fields:

- TitleField
    
- SlugField
    
- PriceField
    
- ShortDescriptionField
    
- LongDescriptionField
    
- ImagesFieldArray (dynamic inputs)
    
- ReviewsFieldArray (dynamic inputs)
    
- SpecsFieldArray (dynamic key/value rows)
    

  

Forms:

- ProductForm → used on create page
    
- ProductEditForm → used on edit page
    

  

Both powered by **React Hook Form + Zod**.

---

## **🧩 7. UI Components**

- ProductsGrid
    
- ProductCard
    
- ProductImageGallery
    
- ProductPageLayout
    
- EmptyProductList
    

---

## **🛒 8. Cart Integration**

  

(Not selected text, but appears in your project context.)

- CartContext (global provider)
    
- AddToCartButton
    
- CartPage + cart line items
    
- LocalStorage syncing
    

---

## **🤖 9. AI Embedding Pipeline**

  

### **buildProductText()**

- Merges title, descriptions, specs, reviews into a single text block.
    

  

### **chunkText()**

- Splits text into 500-char chunks at word boundaries.
    

  

### **generateEmbedding()**

- Calls OpenAI text-embedding-3-small.
    

  

### **ProductChunk**

###  **model**

- Stores embeddings per chunk.
    

  

### **storeProductEmbeddings()**

- Cleans old chunks
    
- Generates new embeddings
    
- Inserts them into MongoDB
    

---

## **10. Error Classes**

  

### **BadRequestError**

###  **/** 

### **NotFoundError**

- Normalized error handling for API routes.
    

---


# **2. README VERSION (clear, technical, accurate)**

  

This is the version for your README.md.

It should _explain_, not _sell_.

  

## **Overview**

  

This project is an AI-powered e-commerce demo built with Next.js (App Router), showcasing semantic product search and RAG-style Q&A for each product. Product data is stored in MongoDB; embeddings and vector search are used to enhance discoverability and user interaction.

---

## **Features**

  

### **Product Catalogue**

- Fetches products from MongoDB via Next.js API routes.
    
- Natural-language search powered by OpenAI embeddings.
    
- Semantic ranking using MongoDB $vectorSearch with score thresholding.
    
- Results displayed in responsive grid (ProductsGrid).
    

---

### **Product Detail Page (/products/[slug])**

- Server component fetches product data via fetch.
    
- Display includes:
    
    - Product image gallery with thumbnail selector.
        
    - Price, title, descriptions, specs, reviews.
        
    - Add to Cart button.
        
    
- Expandable sections using details/summary elements for specs & reviews.
    
- Integrated **Product Q&A** widget using RAG pipeline.
    

---

### **AI Features (Semantic Search + RAG)**

  

**Embedding pipeline**

- Text assembled from product title, descriptions, specs, reviews.
    
- Chunked into ~500-char segments.
    
- Embedded using OpenAI text-embedding-3-small.
    
- Stored in ProductChunk model with productId, text, and embedding.
    

  

**Semantic Search**

- Query embedded.
    
- $vectorSearch retrieves top matching chunks.
    
- Products ranked by similarity threshold (MIN_SCORE).
    

  

**RAG-based Product Q&A**

- User provides productId and question.
    
- Question embedded → vector search filtered by productId.
    
- Context passed into gpt-4.1-mini with restricted prompt (no invented info).
    
- Response returned to client.
    

---

### **Admin Features (/admin/product & /admin/product/[slug])**

- Create and edit products with full Zod validation.
    
- Dynamic field arrays for:
    
    - Images
        
    - Reviews
        
    - Specifications (key/value)
        
    
- Form UX powered by:
    
    - react-hook-form
        
    - shadcn’s Field, Input, Textarea, Button components
        
    - Custom helpers (formatProductPayload, specRow converters)
        
    

---

### **Cart System**

- Fully client-side via React Context.
    
- Supports:
    
    - Add item
        
    - Remove item
        
    - Increase/decrease quantity
        
    
- Persists state in localStorage.
    
- Cart page shows merged product data + quantity + totals.
    

---

### **Backend Architecture**

- Next.js Route Handlers for all CRUD and AI operations.
    
- Centralized error handling (BadRequestError, NotFoundError).
    
- Mongoose used for:
    
    - Product schema/model
        
    - ProductChunk schema/model
        
    
- Utilities:
    
    - mapToObject for Mongoose Maps
        
    - chunkText and buildProductText
        
    - Embedding generator and rebuild scripts
        
    

---

### **Tech Stack**

  

**Frontend:**

Next.js App Router, React, TypeScript, Tailwind v4, shadcn/ui (Radix), sonner

  

**Backend:**

Next.js route handlers, MongoDB + Mongoose, Zod for validation

  

**AI:**

OpenAI embeddings (text-embedding-3-small)

OpenAI responses API (gpt-4.1-mini)

  

**Tooling:**

Biome (lint/format), tsx scripts, TypeScript strict mode

---

## **Scripts**

  

From package.json:

- dev – Start local environment
    
- build – Build Next.js
    
- rebuild – Rebuild all embeddings (app/features/embedding/scripts/rebuildAllEmbeddings.ts)
    
- t – Test embeddings text example script
    
- format / lint / tidy – Biome format/lint
    

---

## **Folder Structure Highlights**


app/
 ├─ api/products/...         # CRUD, search, ask
 ├─ features/
 │    ├─ products/           # UI, hooks, schema, forms, layouts
 │    ├─ cart/               # cart context + components
 │    ├─ embedding/          # chunking / embeddings / RAG pipeline
 │    └─ layout/             # navbar, footer
 ├─ products/                # public catalogue pages
 ├─ cart/                    # cart page
 └─ admin/                   # admin create/edit pages