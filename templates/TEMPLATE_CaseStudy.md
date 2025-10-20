
<%*

/**

 * Build the entire frontmatter in one go so YAML is valid.

 */

const folderPath = tp.file.folder(true) || "";

const lastSegment = folderPath.split("/").filter(Boolean).pop() || "untagged";

  

// make a safe tag: lowercase, dashes for spaces, strip non [a-z0-9_-]

const safeTag = lastSegment

  .toLowerCase()

  .replace(/\s+/g, "-")

  .replace(/[^a-z0-9_-]/g, "") || "untagged";

  

const fm = `---

title: ${tp.file.title}

folderTag: ${safeTag}

tags:

  - ${safeTag}

status: draft

date: ${tp.date.now("YYYY-MM-DD")}

summary:

---

`;

  

tR = fm;

%>
# <% tp.file.title %>

Write your content here…