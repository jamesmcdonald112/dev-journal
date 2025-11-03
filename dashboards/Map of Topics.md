## 🧪 Recent Projects

```dataview

TABLE status, updated

FROM "projects"

SORT updated DESC

LIMIT 10
```

## **💡 Recent Insights**
```dataview
TABLE summary, updated
FROM "insights"
SORT updated DESC
LIMIT 10
```

## **🔗 Connections**
```dataview
TABLE summary, updated
FROM "connections"
SORT updated DESC
LIMIT 10
```


```dataview
TABLE file.name AS "Project File", file.folder AS "Path", updated
FROM "projects"
WHERE type = "project-log"
SORT file.folder ASC, file.name ASC
```
