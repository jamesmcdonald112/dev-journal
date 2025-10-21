---
title: README
type: documentation
status: draft
date: 2025-10-21
updated: 2025-10-20
summary: A README is the entry point to your project, a living document that explains what your project is, why it exists, how to use it, and how to contribute.It’s both a pitch to users and a manual for developers.
tech_stack:
  - markdown
  - badges
  - embedded media
keywords:
  - readme
  - documentation
  - markdown
  - github
---
# README

## What I Learned
A great README balances _clarity, brevity, and persuasion_.
It communicates **what your project does** and **why someone should care**, while making it effortless to install and contribute.
After studying community standards from across GitHub and open source leaders, these principles stand out:

---
### 1. Write the README before the code

**Tom Preston-Werner’s “Readme Driven Development”** (2010)

📖 [https://tom.preston-werner.com/2010/08/23/readme-driven-development.html](https://tom.preston-werner.com/2010/08/23/readme-driven-development.html)

> “A perfect implementation of the wrong specification is worthless.

> Write your README first.”

Writing your README _first_ forces you to think through purpose, users, and API design before touching code.

---
### **2. Keep it Elegant and Minimal**

**Yegor Bugayenko – “Elegant READMEs”**

📖 [https://www.yegor256.com/2019/04/23/elegant-readme.html](https://www.yegor256.com/2019/04/23/elegant-readme.html)

Yegor emphasises:

- Use **no more than five badges per line**
- Keep each section **under 80 characters per line**
- Start with a **logo**, **badges**, and **one concise paragraph**
- Avoid philosophy or essays, stay practical

---
### **3. Structure Matters — Use the “Cognitive Funnel”**

**The Art of README (Kira McLean)**

📖 [https://github.com/hackergrrl/art-of-readme#readme](https://github.com/hackergrrl/art-of-readme#readme)

Readers unconsciously follow a cognitive funnel:

1. What is this?
2. Why should I care?
3. How do I install or use it?
4. How do I contribute?
5. Where can I learn more?

Order your sections to help them “short-circuit” early if it’s not relevant, this _respects their time_ and _improves discoverability_.

---
### **4. Make it Visually Scannable**

  **Thoughtbot – “How to Write a Great README”**

📖 [https://thoughtbot.com/blog/how-to-write-a-great-readme](https://thoughtbot.com/blog/how-to-write-a-great-readme)

Tips:
- Use headings and spacing like a good essay.
- Include _code examples_ early.
- Use **images**, **bold keywords**, and **links** to clarify points.
- Treat your README like _technical storytelling_

---
### **5. Separate Deep Documentation**

**Matklad – “Why You Need an ARCHITECTURE.md”**

📖 [https://matklad.github.io/2021/02/06/ARCHITECTURE.md.html](https://matklad.github.io/2021/02/06/ARCHITECTURE.md.html)

Keep the README high-level.

Move technical deep dives into separate files like:
- ARCHITECTURE.md
- CONTRIBUTING.md
- CODE_OF_CONDUCT.md
- CHANGELOG.md

This keeps your README focused and readable.

---

## **Use Real Examples and Demos**

### 🚀 My Next.js Portfolio

[![Build](https://github.com/jamesmcdonald112/modern-portfolio/actions/workflows/ci.yml/badge.svg)](https://github.com/jamesmcdonald112/modern-portfolio/actions/workflows/ci.yml)
![License](https://img.shields.io/github/license/jamesmcdonald112/modern-portfolio)

A sleek developer portfolio built with **Next.js**, **TypeScript**, and **Biome**.  
It demonstrates CI/CD, linting, and testing best practices.

#### Setup
```bash
git clone https://github.com/jamesmcdonald112/modern-portfolio.git
cd modern-portfolio
npm ci
npm run dev
```

#### Testing
```bash
npm run test
npm run lint
```

#### Deployment
Deployed with [Vercel](https://vercel.com).

---

### **8. Study Great Examples**
**John Jago – “Great Docs: Redis & esbuild”**  
📖 [https://johnjago.com/great-docs/](https://johnjago.com/great-docs/)

- **Redis README:** focuses on both *users* and *contributors*, with architecture and internals clearly mapped.  
- **esbuild README:** short, visual, and links to deeper docs — “the user never gets lost.”

---

### **9. Use GitHub’s Built-In Features**  
**GitHub Docs – “About READMEs”**  
📖 [https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes)

Tips:
- Place your README in the repo root, `.github/`, or `docs/`.  
- GitHub auto-generates **table of contents** from headings.  
- You can use **relative links** to other files (`[Contribution guidelines](docs/CONTRIBUTING.md)`).

---  

## [[README Template]]

---

## Why It Matters  

A great README:

- **Builds trust** — shows you understand process and quality.  
- **Improves visibility** — more stars, forks, and engagement.  
- **Saves time** — clear install steps reduce support and confusion.  
- **Teaches discipline** — makes you design before coding.  
- **Acts as your “developer résumé”** — recruiters and teams evaluate your docs first.  

---
  
## 🔗 Related  

- [[GitHub Workflows]]  
- [[README Template]]  
- [[Architecture Docs]]  

  

---

  

## 📚 References  

  

| Source | Author / Org | Link |

|--------|----------------|------|

| **GitHub Docs: About READMEs** | GitHub | [https://docs.github.com/en/.../about-readmes](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes) |

| **Elegant READMEs** | Yegor Bugayenko | [https://www.yegor256.com/2019/04/23/elegant-readme.html](https://www.yegor256.com/2019/04/23/elegant-readme.html) |

| **Readme Driven Development** | Tom Preston-Werner | [https://tom.preston-werner.com/2010/08/23/readme-driven-development.html](https://tom.preston-werner.com/2010/08/23/readme-driven-development.html) |

| **Art of README** | Kira McLean | [https://github.com/hackergrrl/art-of-readme#readme](https://github.com/hackergrrl/art-of-readme#readme) |

| **How to Write a Great README** | Thoughtbot | [https://thoughtbot.com/blog/how-to-write-a-great-readme](https://thoughtbot.com/blog/how-to-write-a-great-readme) |

| **What I Learned From a Project That Got 3,000 Stars** | FreeCodeCamp | [https://www.freecodecamp.org/news/what-i-learned-from-an-old-github-project-that-won-3-000-stars-in-a-week-628349a5ee14/](https://www.freecodecamp.org/news/what-i-learned-from-an-old-github-project-that-won-3-000-stars-in-a-week-628349a5ee14/) |

| **Great Docs: Redis & esbuild** | John Jago | [https://johnjago.com/great-docs/](https://johnjago.com/great-docs/) |

| **ARCHITECTURE.md** | Matklad | [https://matklad.github.io/2021/02/06/ARCHITECTURE.md.html](https://matklad.github.io/2021/02/06/ARCHITECTURE.md.html) |

| **Awesome README (Community)** | Matiassingers | [https://github.com/matiassingers/awesome-readme](https://github.com/matiassingers/awesome-readme) |

  

---