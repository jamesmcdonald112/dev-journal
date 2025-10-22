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


## Notes
- The fetch API provides a JavaScript interface for making HTTP requests and processing the response.
- Replacement for XMLHttpRequest which used callbacks
- Fetch API is promised-based 
- You pass it a Request object or a string containing the URL to fetch, along with optional arguements to congifure the request
- This returns a promise which is fullfilled with a response onjects tha represnets the servers response.
- You can extract the body from the request status in various formats including text and JSON by calling the appropiate methond the response.
- Fetcg is set to get by defauklt but can be adjust wirht the method option (show code)
- MDN shows both, but recommends async/await for cleaner error handling:
- Use res.json() for JSON APIs, and res.text() for plain responses.

## Questtions
- what is a promise and what are its outcomes.
- Is it best to wrap it in a try catch block?
- what is best to go into the catch? is the catch for the develoepr, user or both?
- This link shows how to add a body etc https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch
- I beleive it is always goof to intoculde content tupe for anythign but a get?
  
  
## What I Learned
Briefly describe the concept or issue.

## Example / Code Snippet
Example from [Mozilla](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch):
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

```js
const response = await fetch("https://example.org/post", {
  method: "POST",
  // …
});```
## Why It Matters
Explain how this connects to your projects or knowledge.

## Related 

## References
- [MDN – Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [Node.js Global Fetch Docs](https://nodejs.org/api/globals.html#fetch)
- [HTTP Caching (MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)