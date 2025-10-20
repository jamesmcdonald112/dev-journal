---
title: <% tp.file.title %>
folderTag: <% tp.file.folder().split('/').pop().toLowerCase().replace(/\s+/g,'-').replace(/[^a-z0-9_-]/g,'') %>
tags:
  - <% tp.file.folder().split('/').pop().toLowerCase().replace(/\s+/g,'-').replace(/[^a-z0-9_-]/g,'') %>
status: draft
date: <% tp.date.now("YYYY-MM-DD") %>
summary:
---
# <% tp.file.title %>

Write your content here…