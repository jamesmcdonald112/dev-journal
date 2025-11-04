Perfect 👏

Here’s your **README.md** — written so it reads like documentation for your future self (or for someone you might share the vault with later).

You can paste it directly into the root of your Obsidian vault (/README.md).

  

It’s short, practical, and matches your templates + workflow exactly.

---

# **🧠 Dev Journal — Obsidian System**

  

> A lightweight system for tracking software projects, learning, and reusable technical knowledge.

---

## **⚙️ Overview**

  

This vault is designed to help you **learn, build, and document in parallel** — without clutter.

Each project becomes a living record of what was done, what went wrong, and what was learned.

- **Projects** = What you’re working on right now
    
- **Shared Notes** = Knowledge that applies everywhere
    
- **Templates** = Keep every note consistent
    
- **Dashboards** = Auto-generate overviews with Dataview
    

---

## **📁 Folder Structure**

```
📦 dev-journal
 ┣ 📂 projects            → Each folder = one real project
 ┃ ┣ 📂 <project-name>
 ┃ ┃ ┣ 1-setup.md        → Created from `project-log.md`
 ┃ ┃ ┣ 2-api-client.md
 ┃ ┃ ┣ 3-deploy.md
 ┃ ┃ ┗ 📂 notes           → Per-topic notes inside project
 ┃ ┃   ┣ biome.md
 ┃ ┃   ┗ husky.md
 ┣ 📂 shared-notes        → Global reusable topics (Biome, EC2, etc.)
 ┣ 📂 dashboards          → Dataview-powered overviews
 ┗ 📂 templates           → The 3 core templates
```

---

## **🧩 Templates**

  

### **1️⃣** 

### **project-log.md**

  

Used for each project stage file (1-setup, 2-api, etc.).

  

Captures:

- Goal
    
- What happened (dated logs)
    
- Fixes & insights
    
- Next steps
    
- Links to deeper notes
    

---

### **2️⃣** 

### **project-note.md**

  

Used for detailed notes inside `/projects/<project>/notes/`.

  

Captures:

- Context (where it came up)
    
- Problem
    
- Fix
    
- Insight
    
- Related links
    
- References
    

  

Example: biome.md, husky.md, github-actions.md

---

### **3️⃣** 

### **shared-note.md**

  

Used for reusable topics in /shared-notes/.

  

Captures:

- Overview
    
- Common setup & issues
    
- Auto-generated list of projects where it appears (via Dataview)
    
- References
    

  

Example: shared-notes/biome.md, shared-notes/aws-ec2.md

---

## **📊 Dashboards**

  

Located in /dashboards/.

Use Dataview to automatically list and sort your notes.

  

**Example: /dashboards/Map of Knowledge.md**

```
## 🧱 Projects
TABLE file.name AS "Project File", file.folder AS "Path", updated
FROM "projects"
WHERE type = "project-log"
SORT file.folder ASC, file.name ASC

## 🧩 Project Notes
TABLE project, topic, updated
FROM "projects"
WHERE type = "project-note"
SORT project ASC, topic ASC

## 🌍 Shared Notes
TABLE topic, updated
FROM "shared-notes"
WHERE type = "shared-note"
SORT topic ASC
```

---

## **🔁 Daily Workflow**

1. **Start a new project**
    
    - Create `/projects/<name>/`
        
    - Add 1-setup.md using **project-log** template
        
    
2. **As you work**
    
    - Log what you do each day
        
    - If something needs detail → make a note in /notes/
        
    
3. **If something is reusable**
    
    - Create a shared-note once
        
    - Dataview will auto-list all related projects
        
    
4. **Check your dashboard**
    
    - /dashboards/Map of Knowledge.md shows everything in one view
        
    

---

## **🧠 Philosophy**

- Don’t rewrite history — log reflections as you go
    
- Each note should be **short, clear, and copy-pastable**
    
- Only promote to /shared-notes when a concept truly repeats
    
- Obsidian + Dataview = your personal “learning operating system”
    

---

## **🔍 Plugins**

|**Plugin**|**Purpose**|
|---|---|
|**Templater**|Inserts templates automatically|
|**Dataview**|Generates dashboards and auto-lists shared notes|

---

## **✅ Example Flow**

- Create: projects/portfolio/1-setup.md
    
- Add: projects/portfolio/notes/biome.md
    
- Later: promote to shared-notes/biome.md
    
- Dashboard shows all connections automatically
    

---

## **🧭 Summary**

| **Folder**          | **Purpose**                                 |
| ------------------- | ------------------------------------------- |
| /projects           | Your actual work and logs                   |
| /projects/.../notes | Project-specific deep dives                 |
| /shared-notes       | Knowledge reusable across multiple projects |
| /templates          | Core structure for consistency              |
| /dashboards         | Your map and summary of everything          |



I completed the IBM Coding Assessment for completion James McDonald - 57052 - Software Developer Intern 2026 (6 - 9 months)  on hackerrank on Friday and received an error message upon submission stating that something went wrong. Could you please confirm if my test was received successfully, or if I should resubmit?  
  
This is the link provided in the previous email from IBM

  

Click on [this URL](http://ibmglobal.avature.net/ltrk/eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MzY0OTU1NTUsImhhc2giOiI2ODc0YTg1ZjI2YjRkMDg4OGMxMjEwZmI0ODU3ZjQ2ODc3Njc2OWQ3OTQwMjZjNmMwMmNiY2U1NzQxMjM0ZGZmIn0.aAcGwHgDnNeiYKwf1Yy98MxdPhJppvZXibsX-ABR3TU "http://ibmglobal.avature.net/ltrk/eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MzY0OTU1NTUsImhhc2giOiI2ODc0YTg1ZjI2YjRkMDg4OGMxMjEwZmI0ODU3ZjQ2ODc3Njc2OWQ3OTQwMjZjNmMwMmNiY2U1NzQxMjM0ZGZmIn0.aAcGwHgDnNeiYKwf1Yy98MxdPhJppvZXibsX-ABR3TU") to start the assessment.