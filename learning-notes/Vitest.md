---
title: <% tp.file.title %>
type: case-study
status: draft
date: <% tp.date.now("YYYY-MM-DD") %>
updated: <% tp.file.last_modified_date("YYYY-MM-DD") %>
summary:
tech_stack:
keywords:
---
# <% tp.file.title %>

## What I Learned
Briefly describe the concept or issue.

## Example / Code Snippet
Add an example or code block here.

## Why It Matters
Explain how this connects to your projects or knowledge.

## Related 

## References
- [GitHub Repository](#)
- [Live Demo](#)
- [Official Docs](https://react.dev)
- [Article](#)
- [Video](#)
- Obsidian link
  
  
  
My notes
I foudn a command to start a project with vitest setup
npx create-next-app@latest --example with-vitest with-vitest-app

this is making me think abot gettig a command that can set up multiple things at once. Lets look into it.

Vitest and React Testing Library are frequently used together for **Unit Testing**.
 make sure to install coverage 
 ```bash
 npm install -D @vitest/coverage-v8
 ```
otherwise the ci file will fail




Resources 
- https://nextjs.org/docs/app/guides/testing/vitest