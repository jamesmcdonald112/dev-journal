---
title: <% tp.file.title %>
type: shared-note
topic: 
tags: []
updated: <% tp.date.now("YYYY-MM-DD") %>
---

# <% tp.file.title %>

## 🧠 Overview
(What this tool/concept is in your words — one paragraph)

## ⚙️ Common Setup / Patterns
(Your go-to config or setup checklist)

## 🧩 Common Issues
(Bullets with short fixes or links to sub-sections)

## 🗂 Used In Projects
```dataview
LIST FROM "projects"
WHERE contains(file.name, this.topic) OR contains(file.name, lower(this.topic))
SORT file.folder ASC, file.name ASC
```

## **📚 References**

- (Official docs, GitHub repo, or best tutorial)