---
title: Gray-matter
type: learning-note
status: draft
date: 2025-10-22
updated: 2025-10-22
summary:
tech_stack:
keywords:
---
# Gray-matter

## What I Learned
gray-matter is a **Markdown front-matter parser**.
It reads .md files and separates them into two parts:
- **Metadata** — the YAML, JSON, or TOML block at the top (e.g. title, date, tags)
- **Content** — the main Markdown body text
    
This makes it easy to use Markdown files (such as Obsidian notes) as structured data in a site or app, for example, to dynamically build blog posts, case studies, or note cards.

## Example / Code Snippet
```js
const { data, content } = matter(markdownFile);
```

This parses the frontmatter (data) and get the markdown body (content).
## Why It Matters
This setup is efficient because it lets me extract metadata from the frontmatter (like title, tags, and date) to display on cards or lists, while the Markdown content itself renders beautifully on the page.  
To achieve this, I combine [Gray-matter](learning-notes/Gray-matter.md) for parsing, [Remark](Remark.md) and [remark-html](remark-html) for converting Markdown to HTML, and [Tailwind’s Prose Classes](learning-notes/Tailwind’s%20Prose%20Classes.md) for styling.  
If I include code blocks in my notes, I can optionally use [rehype-highlight](rehype-highlight.md) for syntax highlighting.

## Related 
- [Remark](Remark.md)
- [remark-html](remark-html)
- [Tailwind’s Prose Classes](learning-notes/Tailwind’s%20Prose%20Classes.md)

## References
- [Gray Matter Docs GitHub](https://github.com/jonschlinkert/gray-matter)
- [Gray Matter Docs npmjs](https://www.npmjs.com/package/gray-matter)
- 