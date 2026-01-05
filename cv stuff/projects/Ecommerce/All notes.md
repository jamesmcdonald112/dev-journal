Got it. I’ll treat this as **“max bullets mode”** and generate everything now.

  

I’ll assume target roles: **Graduate / Junior Full-Stack or Frontend (React/Next.js, TypeScript)**.

---

## **SECTION 1 — FULL TECHNICAL ANALYSIS**

  

### **Architecture**

- **App type:** Full-stack AI-powered e-commerce demo built on **Next.js 16 App Router** with both server and client components.
    
- **Frontend:** **React 19**, **TypeScript**, **Tailwind CSS v4**, **shadcn/ui (Radix primitives)**, and custom components for layout, search, product pages, cart, and admin UI.
    
- **Backend:** **Next.js Route Handlers** under /api/** acting as the backend layer for CRUD, semantic search, and RAG Q&A.
    
- **Database:** **MongoDB** with **Mongoose** models for products and product chunks (embeddings).
    
- **AI Layer:** **OpenAI** integration for embeddings (text-embedding-3-small) and question answering (gpt-4.1-mini) using a retrieval-augmented generation (RAG) pipeline.
    
- **State Management:** Local UI state via React + **React Context** for cart state and other shared stateful features.
    
- **Persistence:** Client-side cart persistence via localStorage, combined with server-side product fetches and merging logic.
    
- **Structure:** Feature-based folder organisation: features/products, features/cart, features/embedding, features/layout, plus route groups for /products, /cart, /admin, /api.
    

  

### **Technologies Used**

- **Core stack:** Next.js 16, React 19, TypeScript 5, MongoDB, Mongoose, OpenAI, TailwindCSS v4, shadcn/ui, Zod, React Hook Form.
    
- **UI & styling:** Tailwind v4, shadcn/ui, Radix NavigationMenu, class-variance-authority, tailwind-merge, clsx, custom SVG icons with lucide-react.
    
- **Validation & types:** Zod schemas shared across frontend and backend; @hookform/resolvers integration.
    
- **Tooling:** **Biome** (@biomejs/biome) for formatting, linting, and checks; tsx for running TypeScript scripts; commitlint tooling.
    
- **Notifications & UX:** sonner for toasts.
    
- **Animations:** tw-animate-css to enhance motion/UX where needed.
    
- **Baseline compatibility:** baseline-browser-mapping to keep an eye on browser support.
    

  

### **Complexity Signals**

- End-to-end **RAG flow**: product text building → chunking → embedding → storing → vector search → GPT answer generation.
    
- Real **semantic search** using OpenAI embeddings and **MongoDB Atlas** **$vectorSearch**, not just simple keyword queries.
    
- **Per-product** grounding for Q&A (filtering chunks by productId) to reduce hallucinations.
    
- **Admin product management system**: create/edit pages, dynamic forms, field arrays, spec mapping, image/review lists.
    
- **Cart system** with React Context + persistence + SSR/hydration handling.
    
- Clear **error abstraction**: custom error classes, RAG safeguards (min score thresholds, empty-input checks).
    
- **Type-safe domain model** from form input → Zod schema → API payload → Mongoose model → DB.
    

  

### **Design Patterns**

- **Feature-based architecture** (products, cart, embedding, layout).
    
- **Schema-first development** with Zod driving both UI forms and API validation.
    
- **Separation of concerns**:
    
    - Form components vs submit hooks.
        
    - API routes vs service functions.
        
    - Embedding utilities vs product CRUD.
        
    
- **Context provider pattern** for cart state.
    
- **Error boundary** specifically for cart to isolate failure modes.
    
- **Script-based maintenance** pattern (embedding rebuild script invoked via npm run rebuild).
    
- **DTO/transformation utilities** (spec rows ↔ spec map, mapToObject, payload formatting).
    

  

### **API Structure**

- **CRUD routes:**
    
    - GET /api/products — list/search products.
        
    - POST /api/products — create product with Zod validation.
        
    - GET /api/products/[slug] — fetch single product.
        
    - PUT /api/products/[slug] — update existing product with validation.
        
    
- **AI routes:**
    
    - POST /api/products/ask — product-level RAG Q&A.
        
    - GET /api/search — global semantic search across product chunks.
        
    - GET /api/products/chunks/[productId] — inspect product embeddings (debug).
        
    
- **Shared behaviours:**
    
    - DB connection via a shared dbConnect or similar util.
        
    - Strong JSON error responses; typed error classes (BadRequestError, NotFoundError).
        
    - Mongoose document serialization (lean() or .toObject()) to comply with App Router.
        
    

  

### **Testing Practices (implied)**

- No explicit test libraries in package.json, but:
    
    - **Biome** is used aggressively for linting/formatting/checking.
        
    - TypeScript strictness + Zod validation + typed errors function as a “static safety net”.
        
    - Clear separation of services/utility functions makes the system testable even if tests aren’t written yet.
        
    

  

### **Tooling & Developer Experience**

- **Scripts:**
    
    - dev, build, start for Next.js.
        
    - format, lint, check, tidy using Biome for consistent style and static checks.
        
    - t for running embedding text example script with tsx and .env.
        
    - rebuild for rebuilding all embeddings.
        
    
- **TypeScript-first DX**: everywhere from UI components to API handlers.
    
- **Commit discipline:** @commitlint/* to enforce conventional commit messages.
    
- **Tailwind v4** and @tailwindcss/postcss indicate modern Tailwind pipeline.
    

  

### **Performance Considerations**

- **Vector search** with **score thresholds** to avoid low-quality results.
    
- Product text **chunking (~500 chars)** to balance recall vs token count.
    
- **Local caching/persistence** of cart in localStorage to avoid repeated server calls.
    
- **SSR/hydration handling** to avoid mismatches (mounted-flag patterns).
    
- Memoized context values and minimal re-renders in cart.
    

  

### **Data Modelling**

- **Product model:**
    
    - Title, slug, short/long descriptions, specs (Map), images, reviews, price, timestamps.
        
    - Zod schema matching the Mongoose schema for end-to-end type safety.
        
    
- **ProductChunk model:**
    
    - productId, text, embedding: number[], timestamps.
        
    - Supports $vectorSearch via MongoDB Atlas vector index.
        
    
- **Form-layer models:**
    
    - specRows as editable key/value arrays to map to specs map.
        
    - Review and image arrays for dynamic UI.
        
    

  

### **Evidence of Engineering Maturity**

- Uses **modern, non-trivial features**: embeddings, vector search, RAG, not just CRUD.
    
- Clean separation between **public demo UX** and **admin/demo-only** pages with disclaimers.
    
- **Defensive programming**:
    
    - Validation everywhere (Zod).
        
    - Empty-input rejection.
        
    - ObjectId checks.
        
    - Strong error classes.
        
    
- **Production-style DX**: Biome, commitlint, environment variable management, TypeScript.
    
- Thoughtful UX: error boundaries, toasts, skeleton/loading states, responsive layouts, accessibility (keyboard handling in search).
    

---

## **SECTION 2 — MAX BULLET LIBRARY**

  

### **A) Architecture & System Design**

- Designed a full-stack **AI-powered e-commerce platform** using **Next.js 16 App Router**, React 19, and MongoDB.
    
- Architected a feature-based structure (features/products, features/cart, features/embedding, features/layout) to keep UI, logic, and data concerns separate.
    
- Implemented a **multi-layered backend** using Next.js Route Handlers for CRUD, semantic search, and RAG Q&A endpoints.
    
- Structured the system so product CRUD, search, embeddings, and cart all **compose around a shared Product model** in MongoDB.
    
- Integrated **OpenAI embeddings and GPT models** as a first-class layer in the architecture instead of a bolted-on feature.
    
- Separated **public user flows** (browse/search/cart/Q&A) from **admin-only flows** (product create/edit) for clarity and security posture.
    
- Standardised request/response patterns across /api/products, /api/search, and /api/products/ask to simplify client integration.
    
- Used **script-based maintenance** (embedding rebuild scripts) to keep AI-derived data aligned with source product data.
    
- Modelled the cart as a **client-side state container** with server-sourced product data, avoiding server-side cart sessions.
    
- Ensured the entire stack (UI → API → DB → AI) is **type-safe** with TypeScript and shared schemas.
    

  

### **B) Frontend Engineering**

- Built the UI with **Next.js 16 App Router** and **React 19** to leverage server components and modern routing.
    
- Implemented a **semantic product search page** with a reusable **SearchBar** component wired to vector-based APIs.
    
- Developed **product listing views** using responsive Tailwind v4 grids and reusable product card components.
    
- Created detailed **product pages** with galleries, specs, review sections, and integrated add-to-cart + Q&A widgets.
    
- Built a robust **cart page** that merges client-side cart state with server-fetched product records for accurate totals.
    
- Implemented a **MainNavbar** using shadcn’s NavigationMenu + Tailwind for consistent global navigation across Products, Cart, and About.
    
- Developed a **MainFooter** with responsive layout and inline SVG social icons styled via Tailwind.
    
- Used **React Context** and custom hooks to expose cart data and actions to any component without prop drilling.
    
- Implemented **skeleton states and loading UX** for product/cart displays to keep the UI responsive while data loads.
    
- Used lucide-react and shadcn/ui to build a clean, modern design without heavy custom CSS.
    

  

### **C) Backend Engineering**

- Built REST-style API endpoints with **Next.js Route Handlers** (GET, POST, PUT) for products and AI features.
    
- Implemented **product CRUD** endpoints that validate incoming payloads with Zod before touching MongoDB via Mongoose.
    
- Wrapped MongoDB access in dedicated service functions (e.g. getProductBySlug, createProduct, updateProduct) for reuse and testability.
    
- Designed **RAG Q&A** endpoint /api/products/ask that orchestrates validation, embedding, vector search, and OpenAI calls.
    
- Implemented global **semantic search** endpoint /api/search that embeds queries and runs $vectorSearch across all product chunks.
    
- Added a **debug endpoint** for inspecting product chunks /api/products/chunks/[productId], aiding observability of embeddings.
    
- Integrated robust **error handling**, mapping Zod validation errors and domain errors (BadRequestError, NotFoundError) to clean JSON responses.
    
- Used tsx scripts to run backend maintenance tasks in TypeScript, including rebuilding all embeddings with a single npm command.
    
- Ensured backend logic avoids sending raw Mongoose documents to the client by converting via .toObject() or .lean().
    
- Applied score thresholds in vector search to filter out low-confidence matches and improve answer quality.
    

  

### **D) Database & Data Modelling**

- Modelled **Product** entities in Mongoose with fields for title, slug, descriptions, specs, images, reviews, and price.
    
- Represented **specifications** as a Map<string,string> in MongoDB while exposing them as editable specRows arrays on the frontend.
    
- Introduced a **ProductChunk** model to store per-chunk embeddings: { productId, text, embedding: number[] }.
    
- Linked embeddings to their parent product via productId, enabling fast per-product RAG queries.
    
- Used timestamps on models to track when products and embeddings were last updated.
    
- Implemented utility functions to convert Mongoose Maps to plain objects for safe serialization in Next.js server components.
    
- Normalised form data into a canonical payload before it hits MongoDB, ensuring a clean separation between UI structure and DB schema.
    
- Embedded **reviews and images** as arrays, giving the UI flexibility to render carousels and multiple user opinions.
    
- Used consistent _id / slug relationships so both users and APIs can refer to products via human-readable slugs.
    

  

### **E) Performance, Optimisation & Reliability**

- Implemented **chunking** of product content (~500 characters) to keep vector search performant and context windows efficient.
    
- Applied **similarity score thresholds** (MIN_SCORE) to drop low-quality matches from semantic search and RAG context.
    
- Avoided repeated API calls for cart items by batching slug-based product fetches in helper functions like fetchCartProducts.
    
- Used a **mounted flag** on the cart page to avoid React hydration mismatches caused by localStorage access.
    
- Memoized the cart context value (state + actions) to prevent unnecessary rendering of consumers.
    
- Stored only minimal data in cart state (slug → quantity) and pulled full product details on demand for better payload efficiency.
    
- Encapsulated OpenAI calls in reusable helpers to handle errors centrally and reduce duplicate network logic.
    
- Used Tailwind utility classes to keep CSS payloads small and avoid bloated stylesheets.
    
- Leveraged server components for product pages where possible to reduce client bundle size and improve initial load.
    

  

### **F) Security & Compliance**

- Kept **OpenAI API keys** and MongoDB URIs in environment variables (.env), never hard-coded in the client.
    
- Restricted admin product pages as **demo-only**, explicitly communicating they are not production-secure.
    
- Validated all incoming API payloads with Zod to prevent malformed or malicious data reaching the database.
    
- Checked that IDs (like productId) are valid MongoDB ObjectIds before querying, preventing injection-like misuse.
    
- Avoided exposing internal embedding vectors or raw DB structures to end users except via controlled debug endpoints.
    
- Ensured cart logic never trusts the client for prices; prices are derived from server-fetched product data.
    
- Structured AI prompts with explicit instructions not to invent facts and to only use provided context, reducing hallucination risk.
    
- Kept dependencies up to date (Next 16, React 19, Tailwind v4) to reduce known vulnerability risk in older frameworks.
    

  

### **G) Tooling, Dev Experience & Testing**

- Configured **Biome** for formatting (format), linting (lint), checking (check), and auto-fix (tidy) to enforce a consistent codebase.
    
- Used **TypeScript 5** across the project, including API handlers, React components, and scripts.
    
- Integrated **commitlint** (@commitlint/cli, @commitlint/config-conventional, @commitlint/prompt-cli) to enforce clean, conventional commit messages.
    
- Leveraged tsx to run TypeScript scripts (embedding examples and rebuild tasks) without separate build steps.
    
- Adopted tailwind-merge and class-variance-authority to manage conditional classNames without duplication.
    
- Included baseline-browser-mapping to keep an eye on browser feature support for modern APIs.
    
- Used tw-animate-css to integrate Tailwind-friendly animations without hand-writing keyframes.
    
- Structured file paths to be intuitive: features/products/schemas, features/embedding/scripts, api/products, cart, layout.
    
- Built reusable UI primitives (SearchBar, ProductForm, ProductEditForm, ProductsGrid) to speed up iteration and reduce bugs.
    
- Kept README and About documentation aligned with implementation details so future contributors can ramp up quickly.
    

  

### **H) AI / Embeddings / Automation**

- Implemented a **full embeddings pipeline**: build text → chunk → embed with OpenAI text-embedding-3-small → store in MongoDB.
    
- Created a **rebuildAllEmbeddings** script triggered via npm run rebuild to refresh embeddings for all products.
    
- Wrapped OpenAI embedding calls in a dedicated generateEmbedding() helper with validation and error handling.
    
- Used **ProductChunk** documents as the storage layer for all chunk text + embeddings.
    
- Implemented **semantic search** using MongoDB $vectorSearch with query embeddings and score filters.
    
- Built **product-specific RAG Q&A** where question embeddings are filtered by productId before context building.
    
- Constructed strict **grounding prompts** for GPT-4.1-mini, instructing it to only answer from contextual chunks and avoid inventing facts.
    
- Added **embedding inspection routes** to help debug semantic relevance and ensure RAG quality.
    
- Implemented safeguards against empty text and invalid inputs in the embedding pipeline.
    
- Used an embeddings text example script (npm run t) to quickly validate embedding behaviour and tune the pipeline.
    

  

### **I) Project Ownership & Delivery**

- Conceived and implemented the project from scratch to demonstrate **AI + e-commerce integration** end-to-end.
    
- Defined the project’s purpose: showcase **semantic search, per-product RAG, and a complete shopping flow** for portfolio and interviews.
    
- Designed user-facing **About** content that clearly explains semantic search and Q&A in non-technical language.
    
- Wrote **developer-focused README notes** to document architecture, APIs, embedding pipeline, and setup steps.
    
- Implemented both **user-facing flows** (browse, search, Q&A, cart) and **internal admin tools** (product create/edit).
    
- Took responsibility for **data modelling, API design, AI integration, and UX design** rather than only one layer.
    
- Integrated **tooling, scripts, and linting** to make the project maintainable over time, not just “demo code”.
    
- Verified that features like cart, semantic search, and Q&A integrate cleanly together into a realistic user journey.
    
- Built the project specifically to support **CV bullets, interview stories, and live demos** for recruiters and hiring managers.
    
- Iterated documentation (About, README, notes) so the project is understandable at multiple levels: user, recruiter, and developer.
    

---

## **SECTION 3 — CV VERSIONS**

  

### **Version A — ATS-Optimised CV Bullets (keyword-heavy)**

- Built a full-stack **AI-powered e-commerce platform** using **Next.js 16 (App Router), React 19, TypeScript 5, TailwindCSS v4, shadcn/ui, and MongoDB/Mongoose**.
    
- Implemented **product CRUD APIs** with Next.js Route Handlers, **Zod validation**, and **Mongoose** models for products and product chunks.
    
- Designed and integrated a **semantic search** pipeline using **OpenAI text-embedding-3-small** and **MongoDB Atlas** **$vectorSearch** for natural-language product queries.
    
- Developed a **product-level RAG Q&A** system using **OpenAI GPT-4.1-mini**, chunked product text, and score-filtered vector retrieval to reduce hallucinations.
    
- Created an admin interface for **product management** (create/edit) using **React Hook Form**, **@hookform/resolvers**, and reusable form components with dynamic field arrays.
    
- Built a **cart system** with **React Context**, custom hooks, and **localStorage** persistence, including add/remove/increase/decrease logic and hydration-safe rendering.
    
- Implemented reusable UI components (SearchBar, ProductsGrid, ProductPageLayout, navbar, footer) styled with **Tailwind v4**, **shadcn/ui**, **Radix NavigationMenu**, and lucide-react icons.
    
- Added **TypeScript-based scripts** using tsx to rebuild embeddings (npm run rebuild) and test embedding behaviours (npm run t).
    
- Enforced code quality with **Biome** (format, lint, check, tidy) and **commitlint** (conventional commits) for a consistent, production-style workflow.
    
- Documented the system with **developer-focused README sections** and user-friendly About text describing semantic search, AI Q&A, and admin flows.
    

  

### **Version B — Hiring Manager / Human CV Bullets (impact-focused)**

- Designed and built an AI-powered e-commerce demo that combines semantic search, per-product Q&A, and a full shopping flow using Next.js, TypeScript, MongoDB, and OpenAI.
    
- Implemented the entire semantic search and RAG pipeline—from product ingestion and chunking to embeddings, vector search, and grounded GPT responses—so users can ask natural-language questions about any product.
    
- Delivered a realistic e-commerce experience with product catalog, detail pages, and a persistent cart, while also providing admin tools for creating and editing products via schema-driven forms.
    
- Put strong emphasis on engineering quality by using shared Zod schemas, typed API routes, custom error handling, and clear separation between frontend features, backend services, and AI utilities.
    
- Introduced scriptable workflows for rebuilding embeddings and managing AI-related data, making it easy to evolve the model or product set without manual DB work.
    
- Documented the project at multiple levels (About page, README, developer notes) so non-technical stakeholders, interviewers, and other developers can quickly understand what the system does and how it’s built.
    

---

## **SECTION 4 — INTERVIEW TALKING POINTS**

  

### **4.1 Technical Talking Points (10–15)**

1. How you used **Next.js App Router** to structure routes for products, cart, admin pages, and API endpoints.
    
2. The design of the **Product** and **ProductChunk** models in MongoDB/Mongoose and why you separated content and embeddings.
    
3. The **embeddings pipeline**: buildProductText → chunkText → generateEmbedding → storeProductEmbeddings.
    
4. How **MongoDB** **$vectorSearch** works in your implementation and how you used score thresholds.
    
5. The differences between **keyword search** and **semantic search**, and how your API supports both.
    
6. The architecture of the **RAG Q&A endpoint** /api/products/ask, from input validation to GPT-4.1-mini calls.
    
7. How you structured **Zod schemas** and reused them across React Hook Form and API handlers.
    
8. The **cart architecture**: React Context design, localStorage persistence, SSR/hydration considerations.
    
9. How you integrated **shadcn/ui** and Tailwind v4 to build a consistent design system for navbar, footer, forms, and product display.
    
10. The **script-based operations** (npm run rebuild, npm run t) and how they help maintain AI-related data.
    
11. Your approach to **error handling** with BadRequestError/NotFoundError and mapping them to HTTP responses.
    
12. How you ensured **type safety** across the stack with TypeScript and Mongoose, including serialization via .toObject() or .lean().
    
13. How you handle **performance trade-offs** around chunk size, score thresholds, and how much context to feed into GPT.
    
14. The rationale for modelling **specifications** as key/value maps vs simple arrays and how you bridged that with form specRows.
    
15. How this project could be extended to add **auth, payments, and production security** if you were to take it further.
    

  

### **4.2 Problem-Solving Stories (Mini STAR Snippets, 5–10)**

  

You can expand these into full STAR answers later.

1. **Improving search relevance with vector search**
    
    - _Situation:_ Simple keyword search felt too limited for product discovery.
        
    - _Task:_ Allow users to search naturally (“phone for travel/photography”).
        
    - _Action:_ Implemented OpenAI embeddings + MongoDB $vectorSearch with score thresholds.
        
    - _Result:_ Search results match user intent instead of just exact keywords.
        
    
2. **Reducing hallucinations in AI answers**
    
    - _Situation:_ Initial GPT answers could drift beyond actual product data.
        
    - _Task:_ Make answers strictly grounded in stored product information.
        
    - _Action:_ Built a RAG pipeline using per-product embeddings and a strict system prompt that forbids invented facts.
        
    - _Result:_ Answers are concise, relevant, and limited to real product specs/reviews.
        
    
3. **Handling cart persistence without breaking SSR**
    
    - _Situation:_ Accessing localStorage caused hydration issues in Next.js.
        
    - _Task:_ Persist cart in localStorage while remaining SSR-safe.
        
    - _Action:_ Implemented a mounted flag and client-only logic in the cart context to delay localStorage reads until after mount.
        
    - _Result:_ Cart state persists correctly, with no hydration warnings or crashes.
        
    
4. **Keeping embeddings in sync with changing products**
    
    - _Situation:_ Updating products could leave embeddings stale and out of sync.
        
    - _Task:_ Provide a repeatable way to regenerate embeddings for all products.
        
    - _Action:_ Wrote a TypeScript script (npm run rebuild) to clear existing chunks, re-embed all products, and repopulate ProductChunk.
        
    - _Result:_ A single command ensures vector data stays aligned with product data.
        
    
5. **Making the admin forms maintainable**
    
    - _Situation:_ Product forms for create/edit were complex and risked diverging.
        
    - _Task:_ Share as much logic as possible between the create and edit forms.
        
    - _Action:_ Built a core ProductForm with reusable field components and submit hooks, then wrapped it in ProductEditForm with initial data.
        
    - _Result:_ Changes to validation or fields are made once and applied consistently.
        
    
6. **Serializing Mongoose data for App Router**
    
    - _Situation:_ Passing raw Mongoose documents into server components caused serialization errors.
        
    - _Task:_ Make all data compatible with Next.js server components.
        
    - _Action:_ Used .toObject() / .lean() and custom converters to map Mongoose Maps to plain objects.
        
    - _Result:_ Clean boundaries between DB, APIs, and React components with no runtime serialization issues.
        
    
7. **Designing the product specs model**
    
    - _Situation:_ Needed specs editable in a table, but stored efficiently in the DB.
        
    - _Task:_ Make specs user-friendly in the UI and structured in MongoDB.
        
    - _Action:_ Created a conversion layer between specRows arrays for forms and specs Map for MongoDB.
        
    - _Result:_ Editors get a nice UX; the database keeps a compact, queryable representation.
        
    
8. **Ensuring codebase consistency over time**
    
    - _Situation:_ Growing project risked inconsistent styles and ad-hoc patterns.
        
    - _Task:_ Keep code consistent and maintainable.
        
    - _Action:_ Adopted Biome for lint/format/check/tidy and commitlint for conventional commits, and enforced their use via package scripts.
        
    - _Result:_ The codebase remains clean and predictable, even as features are added.
        
    
9. **Communicating complex AI features to non-technical users**
    
    - _Situation:_ Semantic search and RAG are difficult concepts for users to understand.
        
    - _Task:_ Explain the AI features clearly on the About page.
        
    - _Action:_ Wrote simple explanations (“search by meaning, not keyword”; “answers based only on stored product data”).
        
    - _Result:_ Users understand what the AI does without needing ML background.
        
    
10. **Balancing demo scope with realistic architecture**
    
    - _Situation:_ Wanted a project that is impressive but still shippable as a solo developer.
        
    - _Task:_ Choose a scope that demonstrates serious engineering without overcomplicating.
        
    - _Action:_ Focused on one domain (e-commerce) but implemented full flows: admin, catalogue, cart, semantic search, and RAG.
        
    - _Result:_ A realistic demo that’s deep enough technically and still manageable for one person.
        
    

  

### **4.3 Likely Follow-Up Questions**

1. How does your semantic search endpoint differ from a simple text search endpoint in implementation and behaviour?
    
2. Why did you choose MongoDB and Mongoose instead of another database (Postgres, Prisma, etc.) for this project?
    
3. How do you prevent or reduce hallucinations in your RAG Q&A system?
    
4. What trade-offs did you make when choosing chunk size and the number of chunks to include in a GPT prompt?
    
5. How would you extend this project to include user authentication and order history?
    
6. How would you productionise this system (logging, monitoring, rate limiting, testing)?
    
7. What are the main failure modes of your AI features (e.g., OpenAI down, bad embeddings) and how would you handle them?
    
8. How do you manage environment variables (OpenAI keys, MongoDB URI) and avoid exposing secrets in the client?
    
9. If you needed to support thousands of products, how would you scale the embeddings and $vectorSearch setup?
    
10. Can you walk me through the lifecycle of a product from creation in the admin UI to being searchable via semantic search?
    
11. How would you add unit or integration tests around your API routes and embedding pipeline?
    
12. Why did you decide to store only slug + quantity in the cart state rather than full product objects?
    
13. How would you adapt this project to use another embedding provider or a self-hosted vector database?
    
14. What would you change if this system had to handle real payments and sensitive customer data?
    
15. Which parts of this project best demonstrate your suitability for a junior full-stack or frontend role, and why?
    
