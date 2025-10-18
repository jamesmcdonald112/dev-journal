
## **🧭 PHASE 1 — Foundation: Understand the Moving Parts**

|**#**|**Topic**|**Why You Need It**|
|---|---|---|
|1|**What Obsidian’s graph view actually stores**|Understand how Obsidian links [[Note Names]], how backlinks work, and how .md filenames map to nodes.|
|2|**GitHub as a remote vault**|Learn how GitHub stores your Markdown and how to access it via the [GitHub REST API /contents endpoint](https://docs.github.com/en/rest/repos/contents).|
|3|**Basic Next.js App Router structure**|You’ll be building /app/api/mindmap/route.ts and /app/mindmap/page.tsx. Know how API routes and pages work in App Router.|
|4|**React state and effects**|You’ll use useState and useEffect to fetch and render live data from your API route.|
|5|**Dynamic routing in Next.js**|Clicking a node should open /dev-journal/[slug] — you need to know how dynamic params and page rendering work.|

---

## **⚙️ PHASE 2 — Fetching Your Data from GitHub**

|**#**|**Topic**|**Why You Need It**|
|---|---|---|
|6|**GitHub REST API – “Get repository contents”**|You’ll use this to list .md files and pull each file’s raw Markdown.|
|7|**Raw GitHub file URLs**|You’ll fetch each file’s contents via https://raw.githubusercontent.com/{user}/{repo}/main/{file}.|
|8|**Rate limiting and caching**|GitHub’s API limits unauthenticated requests (60/hr). Learn to cache or use a GitHub token if needed.|
|9|**Parsing Markdown links**|Understand RegExp for \[\[Note Name\]\] and what to do if a linked note doesn’t exist yet (add “ghost” nodes).|
|10|**Building the graph data structure**|Learn to create nodes and links arrays and avoid duplicate links.|

---

## **🧠 PHASE 3 — Graph Visualization**

|**#**|**Topic**|**Why You Need It**|
|---|---|---|
|11|**What a force-directed graph is**|Understand nodes, links, and how “forces” position them dynamically.|
|12|**react-force-graph-2d basics**|Learn about props like graphData, nodeLabel, nodeAutoColorBy, onNodeClick, and nodeCanvasObject.|
|13|**Custom node rendering (nodeCanvasObject)**|Add labels or icons to nodes — this is how you’ll show file names visually.|
|14|**Click handling (onNodeClick)**|Link each node to its corresponding /dev-journal/[slug] page.|
|15|**Styling the graph**|Adjust link colors, font sizes, and background for readability.|
|16|**Performance optimization**|Optional: learn how to debounce layout, simplify long link chains, or use caching for large vaults.|

---

## **🌐 PHASE 4 — Rendering the Notes**

|**#**|**Topic**|**Why You Need It**|
|---|---|---|
|17|**MDX rendering with next-mdx-remote**|Lets you render your Markdown (from GitHub) into styled HTML/React.|
|18|**Tailwind Typography plugin (@tailwindcss/typography)**|Styles your Markdown content cleanly and consistently.|
|19|**Clickable links inside Markdown**|Learn to replace [[Note Name]] with <Link href="/dev-journal/note-name">Note Name</Link> during render or parsing.|
|20|**Handling missing notes**|Display a placeholder “Note not found” page if a slug doesn’t exist.|

---

## **🧩 PHASE 5 — Sync & Interactivity**

|**#**|**Topic**|**Why You Need It**|
|---|---|---|
|21|**Live syncing via GitHub fetch**|The /api/mindmap route fetches real-time data; understand how to refresh or re-fetch on interval.|
|22|**Optional: GitHub webhook → rebuild trigger**|If deployed on Vercel, use GitHub webhooks to trigger re-fetch/rebuild when you push new notes.|
|23|**Cross-link highlighting**|Optional UX improvement — highlight connected nodes when hovering over one.|
|24|**Error handling + fallback UI**|Handle API failures gracefully (“Could not fetch notes”).|
|25|**Accessibility & responsive layout**|Make the graph usable on mobile and screen readers.|

---

## **🧱 PHASE 6 — Quality & Hosting**

|**#**|**Topic**|**Why You Need It**|
|---|---|---|
|26|**Biome setup**|Lint + format everything consistently (replaces ESLint + Prettier).|
|27|**TypeScript types for your graph**|Define interfaces like GraphNode and GraphLink for reliability.|
|28|**Environment variables (optional)**|Store GitHub tokens or API keys safely.|
|29|**Vercel deployment setup**|Make sure your API routes and graph work once deployed.|
|30|**SEO & sitemap update**|Add /mindmap and /dev-journal pages to your sitemap for discoverability.|

---

## **🧠 Stretch Goals (Optional Enhancements)**

|**#**|**Topic**|**Why It’s Cool**|
|---|---|---|
|31|**Recursive folder support**|Parse nested folders inside your GitHub repo for a bigger vault.|
|32|**Backlinks on note pages**|Add “Linked from” sections to each note using the same graph data.|
|33|**Custom graph themes**|Switch color palettes (dark, light, minimal) with Tailwind classes.|
|34|**3D mode (react-force-graph-3d)**|Render the same data in 3D space.|
|35|**VR / AR mode**|The library also supports WebXR, letting you explore your vault like an actual network. 🤯|

---

## **✅ Summary: “From Nothing” Roadmap**

1. Learn how Obsidian’s linking system works
    
2. Set up a Next.js + TypeScript + Biome + Tailwind project
    
3. Build /api/mindmap → fetch & parse GitHub Markdown
    
4. Build /mindmap/page.tsx → visualize links dynamically
    
5. Build /dev-journal/[slug] → render and style Markdown
    
6. Add interactivity: clickable nodes, backlinks, ghost nodes
    
7. Optional: enable recursive folders + auto-sync on commit
    
8. Deploy to Vercel and optimize
    

---

Would you like me to turn this into a **Notion-style learning plan or checklist** (with links to docs and estimated difficulty per topic)?

That would make it easier to tick off each milestone as you go.