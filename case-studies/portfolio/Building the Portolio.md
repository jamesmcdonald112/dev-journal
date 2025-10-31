## Setup
- [[Setting Up Obsidian for Technical Note-Taking]]
- [[Next]]
- [[Biome]]
- [[Husky]]
- [[Lint-Staged]]
- [[Dependabot Setup]]
- [[README Badges]]
- [[VS Code Workspace Settings]]
- [[Commitlint]]
- [[Github Workflows]]
- [[Testing Environment Setup]]
- [[Vitest]]
- [[Jsdom]]
- [[React Testing Library]]
- [[Zod]]
- [[Jest-Dom]]
- [[User-Event]]
- [[Tailwind]]
- [[README]]
- [[Vercel]]
- [[GitHub Projects]]



## Github notes
- [[Gray-matter]]
- [[Remark]]

## Next Pages
- Create a page for the notes. [[Creating Pages in Next.js 14+]]
- [[GitHub REST API — Contents (Get repository content)]]


## Issues

### GitHub REST API
Calling from the original GItHub REST API endpoint `https://api.github.com/repos/<user>/<repo>/contents/<file>`, for my project it is `https://api.github.com/repos/jamesmcdonald112/dev-journal/contents/Tailwind.md` gives back files like:
```json
1. content: "CgoKYWRkIGN1c3Rvb....."
2. download_url: "https://raw.githubusercontent.com/jamesmcdonald112/dev-journal/main/Tailwind.md"
3. encoding: "base64"
4. git_url: "https://api.github.com/repos/jamesmcdonald112/dev-journal/git/blobs/4a729b175407f69181d8aa40e90e6e117e8ec14a"
5. html_url: "https://github.com/jamesmcdonald112/dev-journal/blob/main/Tailwind.md"
6. name: "Tailwind.md"
7. path: "Tailwind.md"
8. sha: "4a729b175407f69181d8aa40e90e6e117e8ec14a"
9. size: 549
10. type: "file"
11. url: "https://api.github.com/repos/jamesmcdonald112/dev-journal/contents/Tailwind.md?ref=main"
12. _links: {self: 'https://api.github.com/repos/jamesmcdonald112/dev-journal/contents/Tailwind.md?ref=main', git: 'https://api.github.com/repos/jamesmcdonald112/dev-…it/blobs/4a729b175407f69181d8aa40e90e6e117e8ec14a', html: 'https://github.com/jamesmcdonald112/dev-journal/blob/main/Tailwind.md'}
13. [[Prototype]]: Object
```
With this infomation the content must be decoded first usinf a buffer and I cannot difectly transform it to markdown renders liek [[Gray-matter]] or [[Remark]] without decoding it first. One of the parts of the JSON file include the downlouad_url which is the raw data. This give me the exact markdown string.

#### Why we are not using application/vnd.github.html+json
It may seem convient to have or files converted to html from markdown  but it is restricted.
- There is no control over rendering so they cannot be styled using [[Tailwind’s Prose Classes]]
- Github styled markdown only meaning it cannot be extened with custom markdown
However, it is convienet for previews.

After switchign to `application/vnd.github.raw+json`, the isssue that arose was to change the response type to parse text
```ts
const data: string = await res.text();
```

Now the issue with this is i am just getting the raw text file and none of the meta data so i might revert back to the original way and decode that, so i will undo what i have now.

Revert back to `application/vnd.github+json` in the headers

Our next step is to decode the content field as it is in base64-encoded, which is told in the return of the JSON file `encoding: "base64"`.

After some googling, i found this response on [stack overflow](https://stackoverflow.com/questions/56952405/how-to-decode-encode-string-to-base64-in-typescript-express-server) and it has this line of code:
```javascript
const decode = (str: string):string => Buffer.from(str, 'base64').toString('binary');
```

With some minor adjustments we can use it in our code. All we need to do is change it form binary to utf8 and save it as a variable for now instead of a function.

This doc https://adevait.com/nodejs/convert-node-js-buffer-to-string shows us we can convert it stright to utf8 which is exactlye what we want.

```ts
const markdown = Buffer.from(file.content, "base64").toString("utf-8");
```


OKay so we have that, next is to be able to extact the meta data from the obsidian file that was created uing frontmatter. This is the section of the Obsidian file at the top that looks like this if you console.log the markdown we just parsed:
```md
content ---
title: Fetch API Explained
type: core-web
status: draft
date: 2025-10-22
updated: 2025-10-22
summary: The Fetch API is the modern, promise-based way to make HTTP requests in both browsers and Node.js. It replaces XMLHttpRequest and integrates with newer web standards like CORS, service workers, and streaming responses.
tech_stack:
  - JavaScript
  - TypeScript
  - Node.js
  - Web APIs
keywords:
  - HTTP
  - fetch
  - promises
  - JSON 
  - aysnc/await
  - error handling
  - headers
  - Node.js
---
```

After doing some research and back and forth with chat gpt, it seems like the best approach is to use [[Gray-matter]] and [[Remark]] to have best overall flexibility for my project. This is instead of [next-mdx-remote](https://nextjs.org/docs/app/guides/mdx), which seems like it should be perfect but it comes with a warning on the site:
> “Please proceed with caution. MDX compiles to JavaScript and is executed on the server. You should only fetch MDX content from a trusted source, otherwise this can lead to remote code execution (RCE).”

As I have thouhgt about intregrating chat gpt to some extenet for reading my vault and pissibly interracting with it, I think i will fo with the chat gpt recommended [[Gray-matter]] and [[Remark]].

So folliwng the [Gray-Matter Docs](https://www.npmjs.com/package/gray-matter), I installed the dependancies and used it to get this returned:
```json
1. {content: '# Fetch API Explained\n\n## What I Learned\n- The Fet…eveloper.mozilla.org/en-US/docs/Web/HTTP/Caching)', data: {…}, isEmpty: false, excerpt: '', orig: {…}}

2. content: "# Fetch API Explained\n\n## What I Learned\n- The Fetch API provides a JavaScript interface for making HTTP requests and handling responses.\n- It replaces XMLHttpRequest and is promise-based, making it easier to use with async/await.\n- fetch(url) returns a Promise that resolves to a Response object.\n- You can check request success using response.ok (true if status is 200–299).\n- Always handle potential network or HTTP errors using try/catch.\n- Use response.json() to parse JSON data or response.text() for plain text.\n- The default HTTP method is **GET**, but you can specify others with the method option.\n- For POST/PUT requests, include a body and set appropriate headers (like Content-Type).\n\n## Example / Code Snippet\n1. Example from [Mozilla](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch):\n```js\nasync function getData() {\n const url = \"https://example.org/products.json\";\n try {\n const response = await fetch(url);\n if (!response.ok) {\n throw new Error(`Response status: ${response.status}`);\n }\n\n const result = await response.json();\n console.log(result);\n } catch (error) {\n console.error(error.message);\n }\n}\n```\n\n2. POST Request:\n```js\nconst response = await fetch(\"https://example.org/post\", {\n method: \"POST\",\n headers: {\n \"Content-Type\": \"application/json\",\n },\n body: JSON.stringify({ username: \"example\" }),\n});\n```\n\n3. **Reusable Utility Function (TypeScript best practice)**:\n```TypeScript\nexport async function fetchJSON<T>(url: string): Promise<T> {\n try {\n const res = await fetch(url, {\n headers: { Accept: \"application/json\" },\n });\n\n if (!res.ok) {\n throw new Error(`Fetch failed: ${res.status} ${res.statusText}`);\n }\n\n return await res.json();\n } catch (err) {\n console.error(\"Fetch error:\", err);\n throw err;\n }\n}\n\n// Example usage:\nconst notes = await fetchJSON<Note[]>(\n \"https://api.github.com/repos/jamesmcdonald112/dev-journal/contents/\"\n);\n```\n\n##  **Best Practices**\n- Always check response.ok, fetch does **not** throw on HTTP errors.\n- Wrap your requests in try/catch for network safety and clean error messages.\n- Include headers like Accept or Content-Type when sending or expecting JSON.\n- Prefer async/await over .then() for clarity and maintainability.\n- Avoid re-reading the same response; clone it first if needed (response.clone()).\n- Cache responses manually or use frameworks (like Next.js) that handle caching for you.\n## Why It Matters\nThe Fetch API is the foundation of almost every modern web data interaction, from REST calls in front-end apps to server-side fetching in Next.js. Understanding fetch deeply ensures you can:\n- Build robust API layers\n- Handle both browser and Node environments\n- Manage caching, credentials, and streaming efficiently\n- Integrate safely with third-party APIs like GitHub\n\n## Related \n- [[HTTP Caching (MDN)]]\n- [[Headers API]]\n- [[Fetching Data in Next.js (App Router)]]\n- [[Error Handling in JavaScript]]\n\n## References\n- [MDN – Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)\n- [Node.js Global Fetch Docs](https://nodejs.org/api/globals.html#fetch)\n- [HTTP Caching (MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)"
3. data:

4. date: Wed Oct 22 2025 01:00:00 GMT+0100 (Irish Standard Time)

5. [[Prototype]]: Object

6. keywords: Array(8)

7. 0: "HTTP"
8. 1: "fetch"
9. 2: "promises"
10. 3: "JSON "
11. 4: "aysnc/await"
12. 5: "error handling"
13. 6: "headers"
14. 7: "Node.js"
15. length: 8
16. [[Prototype]]: Array(0)

17. status: "draft"
18. summary: "The Fetch API is the modern, promise-based way to make HTTP requests in both browsers and Node.js. It replaces XMLHttpRequest and integrates with newer web standards like CORS, service workers, and streaming responses."
19. tech_stack: Array(4)

20. 0: "JavaScript"
21. 1: "TypeScript"
22. 2: "Node.js"
23. 3: "Web APIs"
24. length: 4
25. [[Prototype]]: Array(0)

26. title: "Fetch API Explained"
27. type: "core-web"
28. updated: Wed Oct 22 2025 01:00:00 GMT+0100 (Irish Standard Time)

29. [[Prototype]]: Object

30. [[Prototype]]: Object

31. excerpt: ""
32. isEmpty: false
33. orig: {type: 'Buffer', data: Array(3771)}
34. [[Prototype]]: Object

```

So my next goal is to render one note beautifully os i need [[Remark]] ecosystem. First i need [[Remark]] to parse the markdown into an abstract syntax tree (AST), meaning it identifies headings, list, bold text etc. So accroding to the [docs](https://unifiedjs.com/explore/package/remark-rehype/) we need to install:

```
npm install unified remark-parse remark-rehype rehype-stringify
```
This setup installs [[Unified]], [[Remark]],  [[Rehype]], and [[Rehype-Stringify]] the tools we need to convert Markdown into HTML.
- [[Unified]] connects [[Remark]] and [[Rehype]] together in a processing pipeline.
- [[Remark]] converts or markdown into a ATS tree, basically a JSON file with the key as the md and the value as the text.
- [[Rehype]] converts this ats tree into a HTML format.
- [[Rehype-Stringify]] transforms this HTML structure into a readable string that can be safely rendered on the site.

The [unified docs](https://unifiedjs.com/explore/package/unified/?utm_source=chatgpt.com) have a nice way of explain it:
>`unified` is an interface for processing content with syntax trees. Syntax trees are a representation of content understandable to programs. Those programs, called _[plugins](https://unifiedjs.com/explore/package/unified/?utm_source=chatgpt.com#plugin)_, take these trees and inspect and modify them. To get to the syntax tree from text, there is a _[parser](https://unifiedjs.com/explore/package/unified/?utm_source=chatgpt.com#parser)_. To get from that back to text, there is a _[compiler](https://unifiedjs.com/explore/package/unified/?utm_source=chatgpt.com#compiler)_. This is the _[process](https://unifiedjs.com/explore/package/unified/?utm_source=chatgpt.com#processorprocessfile-done)_ of a _processor_.

**Explanation:**

[[Unified]] acts as the **pipeline** that manages how our Markdown content is processed.

It uses **plugins** to interact with and modify a syntax tree, a structured representation of our text that code can understand.

The process works like this:
1. The **parser** converts raw text (like Markdown) into a syntax tree.
2. We can then run **plugins** to inspect, transform, or enhance that tree.
3. Finally, the **compiler** turns the updated tree back into text (for example, HTML).
    

  
In short, **Unified** provides the framework for this flow, from reading Markdown, processing it through plugins, and compiling it back into readable output.

Taking the example from their docs and applying it to our code, we get a process like this:


```ts
const { data, content } = matter(decodedContent);

  

const html = await unified()

.use(remarkParse)

.use(remarkRehype)

.use(rehypeStringify)

.process(content);
```

This outputs my html string:

```sh
VFile {
  cwd: '/Users/jamesmcdonald/Desktop/github stuff/modern-portfolio',
  data: {},
  history: [],
  messages: [],
  value: '<h1>Fetch API Explained</h1>\n' +
    '<h2>What I Learned</h2>\n' +
    '<ul>\n' +
    '<li>The Fetch API provides a JavaScript interface for making HTTP requests and handling responses.</li>\n' +
    '<li>It replaces XMLHttpRequest and is promise-based, making it easier to use with async/await.</li>\n' +
    '<li>fetch(url) returns a Promise that resolves to a Response object.</li>\n' +
    '<li>You can check request success using response.ok (true if status is 200–299).</li>\n' +
    '<li>Always handle potential network or HTTP errors using try/catch.</li>\n' +
    '<li>Use response.json() to parse JSON data or response.text() for plain text.</li>\n' +
    '<li>The default HTTP method is <strong>GET</strong>, but you can specify others with the method option.</li>\n' +
    '<li>For POST/PUT requests, include a body and set appropriate headers (like Content-Type).</li>\n' +
    '</ul>\n' +
    '<h2>Example / Code Snippet</h2>\n' +
    '<ol>\n' +
    '<li>Example from <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch">Mozilla</a>:</li>\n' +
    '</ol>\n' +
    '<pre><code class="language-js">a

........
```

SO the Next step is to make the HTML look nice when render on the page, this will be done using [[Tailwind’s Prose Classes]]. For the docs, [click here](https://github.com/tailwindlabs/tailwindcss-typography). Install:
```sh
npm install -D @tailwindcss/typography
```

I applied the example used in their docs to my code:
```js
<article className="prose lg:prose-xl">

	{note.success && note.htmlString}

</article>
```

However, I got this rendered to the console:

```
<h1>Fetch API Explained</h1> <h2>What I Learned</h2> <ul> <li>The Fetch API provides a JavaScript interface for making HTTP requests and handling responses.</li> <li>It replaces XMLHttpRequest and is promise-based, making it easier to use with async/await.</li> <li>fetch(url) returns a Promise that resolves to a Response object.</li> <li>You can check request success using response.ok (true if status is 200–299).</li> <li>Always handle potential network or HTTP
```

The issue is that i was not injecting it as a JSX element, but as a HTML string. On [Stack Overflow](https://stackoverflow.com/questions/39758136/how-to-render-html-string-as-real-html) I found an answer to use `dangerouslySetInnerHTML` , which lead me to the[ docs here ](https://react.dev/reference/react-dom/components/common#dangerously-setting-the-inner-html). This shoudl only be used when:
- You have trusted HTML content (meaning not user submitted). Mine is coming from my own GitHub repo.
- You need to render that HTML(e.g., Markdown - HTML, blog content, etc). I am converting it safely with remakr and rehype.
- You cannot easily represent the same output using React 


Initially, the project used the **Unified + Remark + Rehype + Rehype-Stringify** pipeline to process Markdown.

That approach parsed Markdown into an Abstract Syntax Tree (AST) using remark-parse, transformed it to HTML via remark-rehype, and then serialized it into an HTML string with rehype-stringify.

### **⚠️ Problems with the Previous Setup**

1. **dangerouslySetInnerHTML** bypasses React’s rendering layer — meaning React can’t efficiently diff or sanitize updates.
    
2. The output was just a **static string**, not a tree of React elements — making it impossible to:
    
    - Style or wrap specific Markdown nodes
        
    - Add interactivity or custom React components (like `<NoteLink>` or `<CopyButton>`).
        
    
3. It was **less flexible** for integrating Obsidian-style note links ([[Note]]), syntax highlighting, or component injection.

  

This method worked — it produced clean HTML, ready to inject into the page.

However, rendering required **dangerouslySetInnerHTML**, which, while safe for trusted Markdown sources, is still **an escape hatch** that bypasses React’s DOM diffing and introduces a potential security and maintenance risk.

The **React Markdown** ecosystem provides a much more React-native way to render Markdown.

  

Instead of producing an HTML string, it **transforms Markdown directly into React elements**.

Under the hood, it still uses the Remark/Rehype ecosystem — but it integrates with React’s rendering lifecycle and security model.

  This switch unlocks the ability to:

- **Render Obsidian-style** **[[Internal Links]]** dynamically (convert them to <Link href="/notes/...">).
    
- **Insert custom React components** (alerts, code blocks with copy buttons, etc.).
    
- **Style specific Markdown nodes** (e.g., bold headings, themed blockquotes).
    
- Maintain full safety — no dangerouslySetInnerHTML.
    

---

We paired it with:

- **remark-gfm** → Adds GitHub-style Markdown features (tables, task lists, strikethrough, etc.)
    
- **rehype-highlight** → Enables syntax highlighting for fenced code blocks
    
- **Tailwind Typography (****prose** **classes)** → Gives beautiful, readable styling for Markdown content



Uninstall the dependindes:
```bash
npm uninstall unified remark-parse remark-rehype rehype-stringify vfile
```


As obsidian uses GitHub-flavoured Markdown under the jhood, adding remark-gfm will make my site render excaetly like it does in Obsidian.
```sh
npm install remark-gfm
```


Use this to unstall primjks themes:
```bash
npm install prismjs
```

Add plugins to the markdown
```ts
<article className="prose prose-emerald lg:prose-xl dark:prose-invert">

<Markdown

rehypePlugins={[rehypePrism]}

>{note.markdown}</Markdown>

</article>
```
rehype needs a theme in order to have the code looked styled. Install:
```bash
npm install prismjs
```


### **Second Approach — React Markdown (Current Setup)**

- Switched to **react-markdown**, which renders Markdown directly as **React components**.
    
- This keeps everything **safe and dynamic** (no dangerouslySetInnerHTML).
    
- Added plugins:
    
    - remark-gfm — for GitHub-flavored Markdown (tables, checklists, etc.)
        
    - rehype-raw — allows safe inline HTML if needed
        
    - rehype-prism-plus — adds syntax highlighting for fenced code blocks
        
    
- Installed theme:
- ```js
  import "prism-themes/themes/prism-vsc-dark-plus.css";
  ```

Code example:
```tsx
<article className="prose prose-emerald lg:prose-xl dark:prose-invert">
  <Markdown
    remarkPlugins={[remarkGfm]}
    rehypePlugins={[rehypeRaw, rehypePrism]}
  >
    {note.markdown}
  </Markdown>
</article>
```

### **5. Styling — Tailwind Prose Classes**

- Installed @tailwindcss/typography for elegant Markdown styling:
```bash
npm install -D @tailwindcss/typography
```

### **Cleanup**

- Uninstalled no-longer-needed dependencies:

```bash
  npm uninstall unified remark-parse remark-rehype rehype-stringify vfile
  ```
- Removed prism-react-renderer in favor of simpler rehype-prism-plus + prismjs setup.


Add this code:
```tsx
remarkWikiLink,

{ hrefTemplate: (permalink: string) => `/page/${permalink}` },

],
```

allwoing us to turn out wiki style links in obsidian (`[[]]`) into clickable links. This brought on a new issue as we werre only fetching a single page so when we click on a page in our website, it will not exist as we have not fetached it. SO the next step is to mkae any page or link we want to get fetched

## **🧱 Static Build & Incremental Sync Strategy**

  

> After exploring multiple approaches for fetching and displaying thousands of Markdown notes from GitHub, I’ve settled on a **static build pipeline** with **incremental syncing** as the most scalable, efficient, and reliable solution.

---

### **🎯 Why This Approach**

  

The earlier plan — fetching notes directly from GitHub’s REST API at runtime — works for smaller repositories, but it quickly hits limitations when scaling:

- GitHub’s **rate limit** (60 unauthenticated or 5,000 authenticated requests/hour) makes fetching thousands of Markdown files individually unsustainable.
    
- Each file fetch (/repos/:owner/:repo/contents/:path) counts as a separate API call.
    
- The site would slow down as the number of notes grows, since each request requires decoding and rendering on demand.
    

  

The new approach trades a bit of **build-time cost** for massive **runtime performance and stability**.


---

### **⚡ The New Direction**

  

#### **✅ Static Build Process**

  

At build time (during deployment or via a scheduled sync), a Node.js or Next.js script will:

1. **Fetch the full repo tree** using GitHub’s Tree API
    
    → GET /repos/:owner/:repo/git/trees/main?recursive=1
    
    This returns all file paths and SHA hashes (unique content identifiers) in **a single request**.
    
2. **Compare file SHAs** to a locally stored cache (e.g., notes-cache.json)
    
    → If a file’s SHA hasn’t changed, skip it.
    
    → If it has changed or is new, fetch and process that file.
    
3. **Download and parse changed files**
    
    → Decode Markdown content
    
    → Extract frontmatter metadata with gray-matter
    
    → Generate or update entries in a static JSON index (e.g., /src/data/notes-index.json).
    
4. **Derive folder-based metadata**
    
    → Automatically tag and categorize notes based on their folder structure.
    
    → Example: notes/Java/Threads.md → { category: "Java", tags: ["Java", "Threads"] }
    
5. **Write updated indexes**
    
    → Store extracted metadata and file SHAs for next sync.
    
    → These JSONs are deployed with the site — no runtime API calls required.
    

---

### **🔍 Benefits**

| **✅ Advantage**             | **💬 Explanation**                                                            |
| --------------------------- | ----------------------------------------------------------------------------- |
| **No runtime API calls**    | Users never touch the GitHub API. All data is local and fast.                 |
| **Blazing-fast rendering**  | The site serves static pages and prebuilt JSON metadata.                      |
| **Scalable to 10k+ notes**  | Only changed files are re-fetched; no exponential slowdown.                   |
| **Accurate folder tagging** | Folder structure directly defines categories/tags — no manual tagging needed. |
| **Offline resilience**      | The app works even if GitHub’s API is down.                                   |
| **CI/CD friendly**          | Works perfectly with Vercel, Netlify, or GitHub Actions builds.               |

|**Limitation**|**Mitigation**|
|---|---|
|**Longer build times** (2–5 minutes for 10k+ files)|Acceptable for static content — builds happen only when notes change.|
|**No live updates** until rebuild|Automate rebuilds via GitHub Actions on commit or schedule (e.g. daily).|
|**Storage overhead**|Storing JSON indexes and cache files adds minimal size (~a few MB).|
---

### **🔧 Implementation Steps**

1. **Set up a build script** (e.g. /scripts/sync-notes.mjs)
    
    - Use the GitHub REST API to get the full tree.
        
    - Compare SHAs with your cache file.
        
    - Download and process changed files only.
        
    
2. **Parse Markdown + Metadata**
    
    - Use gray-matter to extract frontmatter.
        
    - Generate notes-index.json with title, tags, summary, folder info, etc.
        
    
3. **Derive folder-based tags**
    
    - Use the file path to automatically generate categories/tags.
        
    - Example: ["Java", "Threads"] → derived from notes/Java/Threads/.
        
    
4. **Store locally**
    
    - Write JSON outputs to /src/data/ so the site can import them statically.
        
    
5. **Integrate in CI/CD**
    
    - Run the sync script during deployment or on a GitHub Action workflow (e.g., “on push” or “nightly”).
        
    
6. **Render dynamically in Next.js**
    
    - Use getStaticProps or direct JSON import to display note cards, lists, or mind map visualizations.
        
    

---

### **🧠 Example Data Output**

  

/src/data/notes-index.json

```json
[
  {
    "slug": "fetch-api-explained",
    "title": "Fetch API Explained",
    "summary": "Promise-based way to make HTTP requests...",
    "tags": ["Core Web", "HTTP"],
    "category": "Core Web",
    "sha": "abc123"
  },
  {
    "slug": "creating-threads",
    "title": "Creating Threads",
    "tags": ["Java", "Threads"],
    "category": "Java",
    "sha": "def456"
  }
]
```
### **🧩 Next Steps**

- Write the incremental sync script.
    
- Add caching (notes-cache.json).
    
- Automate it in your build pipeline.
    
- Replace runtime GitHub API calls with local JSON imports.
    
- Optionally integrate mind map or graph view using folder/tag data.

## Getting the tree
[Github docs](https://docs.github.com/en/rest/git/trees#get-a-tree) show how to get the tree we need for this structure. We need to get the entire tree and the structure of the files that includes the SHA as that identiys the fiels and updates if the files have been updated, so we know to redownload that file. According to the docs, we need to add a parameter to view the subfolders. That is `recursive=1`. Using theri curl command on our project we get this command:
```bash
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  `https://api.github.com/repos/jamesmcdonald112/dev-journal/git/trees/main?recursive=1`
```

I created an access token so it would incrase my rate limits from 60-5k per hour. I also needed to add single quotes around the url as the zsh was intrepreting the `?` incrroectly and getting an error. For stoirng the key, I put it in .env.local and created a git ignore file using:
```sh
npx gitignore node
```

Before we go any further, I am going to refactor my code so it is more modular and tidy, following the [BulletProof React](https://github.com/alan2207/bulletproof-react/blob/master/docs/project-structure.md) folder structure  and adopting it for [[Next Folder Structure]]. 

## Error response - GITHUB
```json
{

  "message": "Not Found",

  "documentation_url": "https://docs.github.com/rest/repos/contents#get-repository-content",

  "status": "404"

}
```
SO i need to account for these errors so i will make a type for github errors:


I am following this guide for best practicies for API handling - [click here](https://dev.to/dmitrevnik/fetch-wrapper-for-nextjs-a-deep-dive-into-best-practices-53dh)

The idea from this site is to design a Fetch Wrapper Architecture is a univeral client you build once to handle:
- calling APIs
- catching and formatting errors
- parsing JSON safely
- managing auth headers
- ad keeping you app code clean and consistent,

Instead of writing fetch() + try/catch everywhere, you ceate one wrapper that does it for you.

The first step is to build a basic structure for apis that followes [[Next]] coventions. This means creating a Core Wrapper Structure that can be applied to all client APIs:

```ts
interface ApiConfig {
  baseUrl?: string;
  defaultHeaders?: Record<string, string>;
  timeout?: number;
}
```
This defines the configuration options your API client can take. Each field has a ?  as they are optional because:
- Sometimes you may not need a base URL (e.g relative paths in Next.js)
- Sometimes you want custom headers (like auth tokens)
- sometimes you don't care about timeout.

```ts
interface ApiResponse<T = any> {
  data: T;
  status: number;
  headers: Headers;
}
```
This defines what the wrapper will return
- T = any. This means "this can return any shape of data".
	- Sometimes it's a user, sometimes its a list of notes, sometimes a string.
	- Later, we will replace any with generics like T = Note to make it type-safe.
- data = parsed JSON response. Developers make sure data always exists (maybe as {} ) so that consuming code cna always safely expect response.data.
- status = the HTTP status code (eg. 200, 404, 500)
- headers = sp you can inspect things like Content-Type or pagination headers later.

```ts
class ApiError extends Error {
  constructor(
    message: string,
    public status: number,
    public response?: Response
  ) {
    super(message);
    this.name = 'ApiError';
  }
}
```

This defines out custom error type. Normally, if you `throw new Error()`, you just get a message string. Thats not enough for API errors, you often want the status and maybe the raw response. So:
- message: a human-readable explanation (e.g. "Unaurhotised)
- status: the HTTP status code (e.g. 401
- response?: the actual Respose object(optional, beause sometimes you might throw without one, like on a timeout)

Inside the constructor:
```ts
super(message);
this.name = 'ApiError';
```
- super(message) - calls the base Error constructor so the standard JS error behaviour works.
- this.name - ensures when you console.error, you see "ApiError" insterad of just "Error".

Benefit:
You can now catch and check:
```ts
if (error instanceof ApiError) {
  console.log(error.status); // 404
}
```

```ts
class ApiClient {
  private config: Required<ApiConfig>;

  constructor(config: ApiConfig = {}) {
    this.config = {
      baseUrl: config.baseUrl || '',
      defaultHeaders: {
        'Content-Type': 'application/json',
        ...config.defaultHeaders,
      },
      timeout: config.timeout || 10000,
    };
  }

  private async makeRequest<T>(
    endpoint: string,
    options: RequestInit = {}
  ): Promise<ApiResponse<T>> {
    // We'll implement this next
  }
}
```
We'll spit this into two parts. First part:
```ts
class ApiClient {
  private config: Required<ApiConfig>;

  constructor(config: ApiConfig = {}) {
    this.config = {
      baseUrl: config.baseUrl || '',
      defaultHeaders: {
        'Content-Type': 'application/json',
        ...config.defaultHeaders,
      },
      timeout: config.timeout || 10000,
    };
  }
}
```

This is the heart of the wrapper.
- private config: means this object is internal to the class only. 
- `Required<ApiConfig>` - converts all ? fileds into required internally. So inside the class, you can satefyl use them without checking for undefined.
	- They are made requied at this stage so that developers cna use them easily outside this class. For instance a develoepr can do this following
```ts
new ApiClient()                      // okay
new ApiClient({ baseUrl: '/api' })   // also okay
```
- But inisde the class, you want to treat them as defined, to avoid having to di ig(config.baseUrl) checks everywhere.
- The constructor accepts your partial config and merges it with defaults:
	- baseUrl: '' - fallback if none given.
	- defaultHeaders: always includes Content-Type: application/json but merges anything extra (like Authorization)
	- timeout is a defualt of 10 seconds.

This pattern gives your wrapper sensible defaults while letting you override them per instance.

Second part:
```ts
class ApiClient {

  private async makeRequest<T>(
    endpoint: string,
    options: RequestInit = {}
  ): Promise<ApiResponse<T>> {
    // We'll implement this next
  }
}
```
This is the core method where your real fetch logic will live later.
It's private because you don't call it directly, the public methods (get, post, etc.) will use it internally.
Parameters:
- endpoint: like `/notes/123`
- opitions: standard fetch() options, method headers, body
- `<T>`: generic so you can type the expected the response(e.g. `makeRequest<User>`)

Return type:
```ts
Promise<ApiResponse<T>>
```
Means it will return a Promise resolving to { data, status, headers }(where you data will be your parsed JSON of type T).



For the next code that goes into make request as part of our API Client class.
```ts
// Instance variable
const url: string = this.buildUrl(endpoint);

// Helper Method
 private buildUrl(endpoint: string): string {
    // Handle both absolute and relative URLs
    if (endpoint.startsWith('http')) {
      return endpoint; // already a full url
    }
    // Otherwise, combine baseUrl and endpoint safely
    return `${this.config.baseUrl}${endpoint.startsWith('/') ? endpoint : `/${endpoint}`}`;
  }
```
This helper method builds the final request url inside out API client.
It's responsible for joining the baseUrl (if any) with the endpint passed to makeRequest().

```ts
// Instance variable
const requestOptions: RequestInit = this.buildRequestOptions(options);

 private buildRequestOptions(options: RequestInit): RequestInit {
    return {
      ...options,
      headers: {
        ...this.config.defaultHeaders,
        ...options.headers,
      },
    };
  }
```
This builds and merges the request options, especially the headers, for each request made. Request Init is built into typeScript and it defines all possibel options you can pass to fetch(). Fro example:
```ts
fetch(url, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(data),
  signal: abortController.signal,
  cache: "no-store",
});
```
All of those - method, headers, body, signal, etc. - come from the RequestInit type. So out helper method will accept an object shaped just like that. Our function does the following.
Function:
```ts
return {
  ...options,
  headers: {
    ...this.config.defaultHeaders,
    ...options.headers,
  },
};
```
- it start with whatever options you passed when calling makeRequest(). eg. { method: "POST", body: "..." }
- It merges in default headers defined in the clinet config - (like "Content-Type": "application/json", "Accept": "application/vnd.github+json", etc.)
- Then it merges custom headers provided for this specific request, letting themoverride the default if needed.

So if your default header is:
```ts
{ "Content-Type": "application/json" }
```

and you call:
```ts
makeRequest("/notes", { headers: { "X-Auth": "123" } })
```

You end up with:
```ts
headers: {
  "Content-Type": "application/json",
  "X-Auth": "123"
}
```

This ensures that:
- every request automatically includes shared defaults (no repetition)
- You can still customise headers per call (flexible)
- You dont have to worry about overiting the default by accident


For the next part, it will include a try-catch block. This ensures we catch any errors that may occur when fetching.
```ts
try {

    } catch (error: unknown) {
      throw error;
    }
```
So this is standard practice, to catch any error that might come in either as type Error, or string or whatever, that's why we have it as unknown and throw it so the callig method cna handle it.



```ts
try {
  // Create an AbortController for timeout
  const controller = new AbortController();

  // Automatically abort the request after the configured timeout (e.g. 10 seconds)
  const timeoutId = setTimeout(() => controller.abort(), this.config.timeout);

  // Start the fetch request and attach the abort signal
  const response = await fetch(url, {
    ...requestOptions,
    signal: controller.signal,
  });

  // If the request finished in time, clear the timeout
  clearTimeout(timeoutId);

  // ... process the response here later
} catch (error: unknown) {
  // Check if the error is an AbortError (fetch was cancelled due to timeout)
  if (error instanceof Error && error.name === 'AbortError') {
    throw new ApiError('Request timeout', 408);
  }

  // Re-throw any other kind of error (network issues, bad JSON, etc.)
  throw error;
}
```

### **🧠 Line-by-line breakdown**

  - const controller = new AbortController(); - The **AbortController** is a built-in browser API that lets you cancel (abort) asynchronous tasks like fetch requests.
  - const timeoutId = setTimeout(() => controller.abort(), this.config.timeout); - We tell the controller to **abort** the fetch after a certain amount of time (e.g., 10 000 ms). That means: if the API doesn’t respond in time → it triggers an abort.
  - signal: controller.signal - This attaches the abort controller to the fetch call. It tells fetch: “If the controller aborts, cancel this request immediately.”
  - clearTimeout(timeoutId); - If fetch finishes before the timeout expires, we clear it so it doesn’t accidentally abort later.
  - The catch block - If the controller did abort → the fetch API **throws** an error with error.name === 'AbortError'. We catch that and translate it into our standardized ApiError with status 408 (“Request Timeout”). Any other type of error (network error, JSON parse failure, etc.) is simply rethrown for the outer caller to handle.

The next step is checking the status and throw for errors
```ts
// Here's the key: we check the status and throw for errors
if (!response.ok) {
	throw new ApiError(
		`HTTP ${response.status}: ${response.statusText}`,
		response.status,
		response
	);
}
```

This line ensures that the request only continues if the HTTP status code indicates success (200–299).

If it fails (like 404 or 500), it throws a custom ApiError that includes:
- the numeric code (response.status),
- the human-readable status text (response.statusText),
- and the full Response object (for later inspection).

This makes error handling consistent and gives you all the info you might need in one place.



Next, after the make sure the response is a success, we must parse it:
```ts
// Parse JSON safely
const data = await this.parseResponse<T>(response);
      
    
// Helper Method
private async parseResponse<T>(response: Response): Promise<T> {
    const contentType = response.headers.get('content-type');

    if (contentType?.includes('application/json')) {
      try {
        return await response.json();
      } catch (error) {
        throw new ApiError('Invalid JSON response', response.status, response);
      }
    }

    // Handle text responses
    return (await response.text()) as unknown as T;
  }
```

After verifying the response succeeded, we parse it using a helper method. The method inspects the Content-Type header, if it includes application/json, it parses as JSON; otherwise, it falls back to text.

We use a generic `<T>` so the method can return any expected type (like User[] or { message: string }).

The double cast (await response.text()) as unknown as T ensures TypeScript treats even plain text as the same generic type, keeping our return type consistent.


Next, I am just going to add the get method form the code
```ts
public async get<T>(endpoint: string, options?: RequestInit): Promise<ApiResponse<T>> {
    return this.makeRequest<T>(endpoint, { ...options, method: 'GET' });
  }
```

endpoint → this can be either: 
- a relative path like '/users' or '/repos/jamesmcdonald112/notes'
- a full URL like 'https://api.github.com/repos/...'
- Your buildUrl() helper handles both.
    
options - any fetch configuration (headers, etc.).
- It’s optional because you might not need to pass anything extra.

Inside: it calls `this.makeRequest<T>()`
- passes in the endpoint
- merges the options but explicitly sets method: 'GET'

Return: a `Promise<ApiResponse<T>>` 
- meaning you’ll get { data, status, headers } back.


Next, we will add some Authentication config for token usage, as I will use this for my GitHub project. Firs we will add an interface for AuthConfig to descive what is needed in the auth config:

```ts
interface AuthConfig {
  tokenProvider?: () => Promise<string | null> | string | null;
  tokenHeader?: string;
  tokenPrefix?: string;
}
```
### Token Provider
The token provider defines how to get the token. It can be:
- a function that returns the token (synchronously or asynchronously), or
- a direct string value (the token itself)
This is usefula as in some apps, tokens are stored differently, for example:
- in LocalStorage (browser)
- in cookies (server)
- fetch dynamically via another API

BY allowing either a direct value or a function, your client can work in any contect, brower, server or api route.

Example:
```ts
tokenProvider: () => localStorage.getItem("github_token")
```

or :
```ts
tokenProvider: async () => await getAuthTokenFromServer()
```

### Token Header
This sets the HTTP header name used to send the token. Default is "Authorization", which is standard for most APIs (including GitHub).

**Example**:
```http
Authorization: Bearer ghp_abc123
```

You could override it for APIs that use a custom scheme, e.g.:
```ts
tokenHeader: "X-API-Key"
```

Which would produce:
```http
X-API-Key: ghp_abc123
```

### Token Prefix
This defines the word (or format) that comes before the token in the the header value.

The most common is "Bearer", so your header becomes 
```http
Authorization: Bearer <token>
```

If the API uses a different style, you can customize it:
```ts
tokenPrefix: "Token"
```

which becomes:
```http
Authorization: Token <token>
```

Or remove it entirely by setting tokenPrefix: "".


Next, we need to adjust our ApiClient class to include authConfig:
```ts
class ApiClient {
  private authConfig: AuthConfig;

  constructor(config: ApiConfig = {}, authConfig: AuthConfig = {}) {
    // ... previous constructor code
    this.authConfig = {
      tokenHeader: 'Authorization',
      tokenPrefix: 'Bearer',
      ...authConfig,
    };
  }
```

This adds a new private property, authConfig, to the class. Inside the constructor, we merge default values (Authorisation for the header and Beater for the prefix) with any values the developer passes in.

That means:
- If no authConfig is provided, these defaults are used automatically.
- If some fields are provided (like a custom token or header name), those will override the defaults.

This ensures your API client _always_ has valid authentication settings ready to use, even if the caller doesn’t explicitly configure them.


Next we are going to add this:
```ts
private async buildRequestOptions(
	options: RequestInit
): Promise<RequestInit> {
	const headers = { ...this.config.defaultHeaders };

// Add authentication token if available
	if (this.authConfig.tokenProvider) {

		const token = await this.authConfig.tokenProvider();

		if (token) {

			// Safely add auth header, defaulting to "Authorization" if none is set.

			headers[this.authConfig.tokenHeader ?? "Authorization"] = `${this.authConfig.tokenPrefix ?? "Bearer"

			} ${token}`;

		}

	}

	return {
		...options,
		headers: {
			...this.config.defaultHeaders,
			...options.headers,

		},

	};

}
```

We make this function async because the token provider may need to fetch a token from a server or another async source.

We first store the default headers in a const headers variable so we can modify and merge them later.

Next, we check if an authentication configuration exists (this.authConfig.tokenProvider) — since it’s optional, the developer may not have provided one.

If a token provider function exists, we call it and await its result. The await is necessary because the token might come from an asynchronous source (e.g. cookies, localStorage, or a refresh API). Even if it’s synchronous, it will still work correctly.

If a token is returned (if (token)), we inject it into the headers under the correct name.

Instead of hardcoding "Authorization", we use this.authConfig.tokenHeader ?? "Authorization", meaning “use the configured header name or fall back to ‘Authorization’ if none was provided.”

Finally, we return all merged headers — the defaults, any user-provided headers, and the new auth header if one was added.


Also, update this to include await:
```ts
// Update makeRequest to use async buildRequestOptions
  private async makeRequest<T>(
    endpoint: string,
    options: RequestInit = {}
  ): Promise<ApiResponse<T>> {
    const url = this.buildUrl(endpoint);
    const requestOptions = await this.buildRequestOptions(options);
```



In `lib/github/client.ts` create a GitHub Specific API instance
```ts
export const githubApi = new ApiClient(
  { baseUrl: "https://api.github.com" },
  { tokenProvider: () => process.env.GITHUB_TOKEN ?? null }
);
```

This resuses the ApiClient logic, automatically attaches your github token to all requests
sets, default base url to GitHub's REST API. Now we can make authenticated requests like:
```ts
githubApi.get("/repos/jamesmcdonald112/dev-journal/contents/file.md")
```
and it will include my token automatically.

Dont forget to export the ApiClient and ApiError:
```ts
export class ApiError extends Error{...}
export class ApiClient{...}
```



Next we create a feature level function in `features/notes/api/fetchNoteFromGitHub.ts` that combines our GitHub instance (githubApi) with out personal repo and file path to fetch a specific makdown file (or JSON metadata) from GitHub.


## Architectural Principles used at this point 
- [[Facade Pattern]]
- [[Centralised Error Handling]]
- [[Layered Architecture]]
- [[Fail Fast Principle]]



Changed this to unknown to stop [[Biome]] complaining:
```ts
interface ApiResponse<T = unknown> {
	data: T;
	status: number;
	headers: Headers;
}
```
Any basically turns off typescript and disables all type checking, which is unsafe and hides bugs. In modern type script, unknown is better as Typescript forces you to narrow the type before using it:
```ts
let data: unknown;

data = "hello";

if (typeof data === "string") {
  console.log(data.toUpperCase()); //  Safe, because you checked
}
```

If you skip the check:
```ts
console.log(data.toUpperCase()); // Error: Object is of type 'unknown'
```



now, in out `src/app/notes/fetchNotesFromGitHub.ts` we can add this code:
```ts
import { githubApi } from "@/lib/client";
import type { GitHubFileResponse } from "../types";

const USER = "jamesmcdonald112";
const REPO = "dev-journal";

export default async function fetchNoteFromGitHub(
	filePath: string,
): Promise<GitHubFileResponse> {
	const endpoint = `/repos/${USER}/${REPO}/contents/${encodeURIComponent(filePath)}`;
	const response = await githubApi.get<GitHubFileResponse>(endpoint);
	return response.data;
}
```
This function takes a filePath string and retrieves the corresponding note file from the GitHub repository.

Because all error handling happens inside our ApiClient, this function stays clean and focused—it simply fetches the data and returns it.

```ts
import matter from "gray-matter";
export function decodeMarkdown(base64Content: string): string {
	const decoded = Buffer.from(base64Content, "base64").toString("utf8");
	const { content: markdown } = matter(decoded);
return markdown;
}
```
GitHub’s API returns file content in Base64 format, so here we:

1. Decode the Base64 string into plain text (utf8).
    
2. Use **gray-matter** to strip out any front matter (YAML metadata).
    
3. Return the markdown body ready for rendering

Finally, we can use both functions inside our src/app/notes/page.tsx component:
```ts
export default async function Notes(): Promise<JSX.Element> {
	try {
		const fileData: GitHubFileResponse = await fetchNoteFromGitHub("Fetch API Explained.md");
		const markdown: string = decodeMarkdown(fileData.content);

		// ...Render markdown using react-markdown
	} catch (error) {
		return (
			<h1>
				Failed to load note:{" "}
				{error instanceof Error ? error.message : "Unknown error"}
			</h1>
		);
	}
}
```

Here’s what happens step-by-step:

1. We call fetchNoteFromGitHub() with a specific file name (e.g., "Fetch API Explained.md").
    
2. The GitHub API returns the file data in Base64 format.
    
3. We decode and parse it into markdown using decodeMarkdown().
    
4. The result is then rendered to the page (using react-markdown with syntax highlighting, GFM, etc.).



Next I want to create a dynamic notes page that loads any markdown file based on the URL slug (e.g./notes/learning-notes/Biome.md). For this we will use [[Next.js Slugs]]
## Commands 

### Commiting
To run [[Commitlint]] prompt generator. 
```bash
git add .
npm run commit
```



