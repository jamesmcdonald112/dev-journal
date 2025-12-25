---
title: README Badges
type: documentation
status: draft
date: 2025-10-21
updated: 2025-10-21
summary: How to add and configure dynamic README badges that display your project’s build, linting, testing, and dependency status directly from GitHub Actions. Explains what each badge represents, how to implement them, and why they matter for credibility and professional presentation.
tech_stack:
  - GitHub Actions
  - Shields.io
  - Biome
  - Vitest
  - Commitlint
keywords:
  - readme
  - github
  - actions
  - badges
  - workflow status
  - ci/di
  - biome
  - vitest
  - commitlint
  - dependabot
  - automation
  - shields.io
---
# README Badges

## What I Learned
README badges are small, dynamic icons that instantly communicate the status of key project metrics like **builds**, **tests**, and **linting**.
They make a README look more professional and trustworthy while providing real-time feedback from CI tools such as **GitHub Actions**, **Biome**, or **Vitest**.

## Example / Code Snippet
Add this section near the top of your README.md (just below your project title):
```markdown
# My Project Name

[![CI](https://github.com/<your-username>/<your-repo>/actions/workflows/ci.yml/badge.svg)](https://github.com/<your-username>/<your-repo>/actions/workflows/ci.yml)
[![Code Quality](https://github.com/<your-username>/<your-repo>/actions/workflows/biome.yml/badge.svg)](https://github.com/<your-username>/<your-repo>/actions/workflows/biome.yml)
[![Commitlint](https://github.com/<your-username>/<your-repo>/actions/workflows/commitlint.yml/badge.svg)](https://github.com/<your-username>/<your-repo>/actions/workflows/commitlint.yml)
```

Replace:

- `<your-username>` → your GitHub username (e.g. jamesmcdonald112)
- `<your-repo>` → your repository name (e.g. portfolio-nextjs)
  

Optional static badge examples from [shields.io](https://shields.io):
```markdown
![License](https://img.shields.io/github/license/<user>/<repo>)
![Coverage](https://img.shields.io/badge/coverage-95%25-brightgreen)
![Built with](https://img.shields.io/badge/Built%20with-Next.js-blue)
```

## Why It Matters
Badges provide instant insight into your project’s health:
- **CI** shows if tests are passing.
- **Biome** shows that code is linted and formatted correctly.
- **Commitlint** confirms commit messages follow standards.
- **Coverage or Dependencies** badges show active maintenance.


They also help recruiters, collaborators, and other developers trust that your codebase follows professional development workflows.

## How to Implement
1. **Ensure workflows exist** in .github/workflows/
    (e.g., ci.yml, biome.yml, commitlint.yml)
2. Copy the badge links into your README.md.
3. Push your changes to GitHub.
4. Wait for the next workflow run, the badges will automatically update.
5. _(Optional)_ Add branch protection rules so failing badges block merges.

## Related 
- [[to fix/learning-notes/Github Workflows]]    
- [[to fix/learning-notes/Biome]]
- [[to fix/learning-notes/Commitlint]]
- [[Dependabot]]
- [[learning-notes/Vitest]]
- [[to fix/learning-notes/README Template]]

## References
- [GitHub Actions – Workflow Status Badges](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/adding-a-workflow-status-badge)
- [Shields.io – Custom Badges](https://shields.io)
- [Biome CI Docs](https://biomejs.dev/recipes/continuous-integration/)
- [Steve Kinney – CI with Vitest](https://stevekinney.com/courses/testing/continuous-integration)
- [GitHub Marketplace – Actions](https://github.com/marketplace?type=actions)