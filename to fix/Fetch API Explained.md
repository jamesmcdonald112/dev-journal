---
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
# Fetch API Explained

## What I Learned
- The Fetch API provides a JavaScript interface for making HTTP requests and handling responses.
- It replaces XMLHttpRequest and is promise-based, making it easier to use with async/await.
- fetch(url) returns a Promise that resolves to a Response object.
- You can check request success using response.ok (true if status is 200–299).
- Always handle potential network or HTTP errors using try/catch.
- Use response.json() to parse JSON data or response.text() for plain text.
- The default HTTP method is **GET**, but you can specify others with the method option.
- For POST/PUT requests, include a body and set appropriate headers (like Content-Type).

## Example / Code Snippet
1. Example from [Mozilla](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch):
```js
async function getData() {
  const url = "https://example.org/products.json";
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }

    const result = await response.json();
    console.log(result);
  } catch (error) {
    console.error(error.message);
  }
}
```

2. POST Request:
```js
const response = await fetch("https://example.org/post", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({ username: "example" }),
});
```

3. **Reusable Utility Function (TypeScript best practice)**:
```TypeScript
export async function fetchJSON<T>(url: string): Promise<T> {
  try {
    const res = await fetch(url, {
      headers: { Accept: "application/json" },
    });

    if (!res.ok) {
      throw new Error(`Fetch failed: ${res.status} ${res.statusText}`);
    }

    return await res.json();
  } catch (err) {
    console.error("Fetch error:", err);
    throw err;
  }
}

// Example usage:
const notes = await fetchJSON<Note[]>(
  "https://api.github.com/repos/jamesmcdonald112/dev-journal/contents/"
);
```

##  **Best Practices**
- Always check response.ok, fetch does **not** throw on HTTP errors.
- Wrap your requests in try/catch for network safety and clean error messages.
- Include headers like Accept or Content-Type when sending or expecting JSON.
- Prefer async/await over .then() for clarity and maintainability.
- Avoid re-reading the same response; clone it first if needed (response.clone()).
- Cache responses manually or use frameworks (like Next.js) that handle caching for you.
## Why It Matters
The Fetch API is the foundation of almost every modern web data interaction, from REST calls in front-end apps to server-side fetching in Next.js. Understanding fetch deeply ensures you can:
- Build robust API layers
- Handle both browser and Node environments
- Manage caching, credentials, and streaming efficiently
- Integrate safely with third-party APIs like GitHub

## Related 
- [[MDN)](MDN|HTTP Caching (MDN)]]))
- [[Headers API]]
- [[App Router)](App Router|Fetching Data in Next.js (App Router)]]))
- [[Error Handling in JavaScript]]
- [[Example]]

## References
- [MDN – Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [[https://nodejs.org/api/globals.html#fetch]]
- [[MDN)](MDN|HTTP Caching (MDN)]]))