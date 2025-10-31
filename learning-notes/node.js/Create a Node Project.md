---
title: Create a Node Project
type: setup-guide
status: draft
date: 2025-10-21
updated: 2025-10-21
summary: Learn how to initialise a new Node.js project using npm, generate package.json and package-lock.json, and understand their purpose.
tech_stack:
  - Node.js
  - npm
  - JavaScript
keywords:
  - Node.js
  - npm
  - package.json
  - backend
  - setup
---
# Create a Node Project

## What I Learned
To create a [Node.js](Node.js) project, you use [npm](npm) (Node Package Manager) to generate a [package.json](package.json) file, which stores your project’s metadata and dependencies.  
When you install packages, npm also creates a [package-lock.json](package-lock.json.md) file to lock down the exact versions used.

You initialise a project with:
```bash
npm init
```

This doesn’t download anything from the npm registry, it just **creates a local [package.json](package.json) file** based on your answers (or defaults if you use -y).

Later, when you install packages (e.g. npm install express), npm downloads them into the local [node_modules](node_modules) folder and updates both [package.json](package.json) and [package-lock.json](package-lock.json.md).

## Example / Code Snippet
```bash
npm init
```

This creates a [Node.js](Node.js) project and generates the [package.json](package.json) file.

Once you define scripts like:
```json
"scripts": {
  "start": "node server.js"
}
```

You can run:
```bash
npm start
```

Instead of:
```bash
node server.js
```
## Why It Matters
- [package.json](package.json) holds metadata (name, version, author, dependencies, scripts).
- [package-lock.json](package-lock.json.md) ensures exact dependency versions for consistent builds.
- Using [npm](npm) scripts simplifies running and automating project tasks.

## Related 
- [Node.js](Node.js)
- [package.json](package.json)
- [package-lock.json](package-lock.json.md)
- [npm](npm)
- [node_modules](node_modules)

## References
- [npm Docs – package.json](https://docs.npmjs.com/cli/v10/configuring-npm/package-json)
- [npm Docs – package-lock.json](https://docs.npmjs.com/cli/v10/configuring-npm/package-lock-json)