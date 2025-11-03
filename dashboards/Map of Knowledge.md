## 🧱 Projects
```dataview
TABLE file.name AS "Project File", file.folder AS "Path", updated

FROM "projects"

WHERE type = "project-log"

SORT file.folder ASC, file.name ASC
```

## 🧩 Project Notes
```dataview
TABLE project, topic, updated

FROM "projects"

WHERE type = "project-note"

SORT project ASC, topic ASC
```
## 🌍 Shared Notes
```dataview
TABLE topic, updated

FROM "shared-notes"

WHERE type = "shared-note"

SORT topic ASC
```

