---
title: Remark
type: learning-note
status: draft
date: 2025-10-22
updated: 2025-10-22
summary:
tech_stack:
keywords:
---
# Remark

## What I Learned
It parses a Markdown file and builds an **Abstract Syntax Tree (AST)** representation of it, making the content understandable and transformable by the JavaScript ecosystem.  

This structure allows plugins (like [[remark-html]]) to modify, analyse, or convert Markdown in powerful ways before rendering.

## Example / Code Snippet
Install with:
```sh
npm install remark
```

Example use:
```js
import {remark} from 'remark'
import remarkToc from 'remark-toc' // Generates a table of contents

const value = `
# Pluto

Pluto is a dwarf planet in the Kuiper belt.

## Contents

## History

### Discovery

In the 1840s, Urbain Le Verrier used Newtonian mechanics to predict the position of…

### Name and symbol

The name Pluto is for the Roman god of the underworld, from a Greek epithet for Hades…

### Planet X disproved

Once Pluto was found, its faintness and lack of a viewable disc cast doubt…

## Orbit

Pluto's orbital period is about 248 years…
`

const file = await remark().use(remarkToc).process(value)

console.error(String(file))
```

…running that with `node example.js` yields:
```markdown
# Pluto

Pluto is a dwarf planet in the Kuiper belt.

## Contents

* [[#history|History]]
  * [[#discovery|Discovery]]
  * [[#name-and-symbol|Name and symbol]]
  * [[#planet-x-disproved|Planet X disproved]]
* [[#orbit|Orbit]]

## History

### Discovery

In the 1840s, Urbain Le Verrier used Newtonian mechanics to predict the position of…

### Name and symbol

The name Pluto is for the Roman god of the underworld, from a Greek epithet for Hades…

### Planet X disproved

Once Pluto was found, its faintness and lack of a viewable disc cast doubt…

## Orbit

Pluto's orbital period is about 248 years…
```
## Why It Matters
By separating the parsing and transformation stages, remark gives developers full control over how Markdown is processed.  

For example, you can add custom plugins to handle things like tables, footnotes, or syntax highlighting before sending the content to the browser.
## Related 
- [[remark-html]]
- [[remark-toc]]

## References
- [[Remark) Docs](Remark|Unified.js (Remark) Docs]]%20Docs)%20Docs)
- [Remark GitHub Repository](https://github.com/remarkjs/remark)