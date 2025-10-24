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

So my next goal is to render one note beautifully os i need [[Remark]] ecosystem. First i need [[Remark]] to parse the markdown into an abstract syntax tree (AST), meaning it identifies headings, list, bold text etc.




## Commands 

### Commiting
To run [[Commitlint]] prompt generator. 
```bash
git add .
npm run commit
```
