---
title: ISSUE_TEMPLATES
type: learning-note
status: draft
date: 2025-10-22
updated: 2025-10-22
summary:
tech_stack:
keywords:
---
# ISSUE_TEMPLATES

## What I Learned
Using **issue and pull request templates** helps maintain consistency and quality across your project.
- The .github/ISSUE_TEMPLATE folder **must be uppercase and underscored** for GitHub to recognise it.
- Inside it, files like bug_report.md and feature_request.md are written in **lowercase**, following a convention used by most open-source projects.
- The PULL_REQUEST_TEMPLATE.md file must be placed in the **root of the** **.github** **folder** and is conventionally written in **uppercase**.
    

GitHub automatically parses these files **by their exact names and paths**, so if the naming differs, they may not appear in the **“New Issue”** or **“New Pull Request”** interface.

This structure keeps your repository **organised, contributor-friendly**, and ready for collaboration.

## Example / Code Snippet
### PULL_REQUEST_TEMPLATE.md
```markdown
## Summary
Briefly describe what this change does and why it's needed.

## Related Issue
Closes #<issue_number> (optional)

## Changes
- [ ] List the key changes in this PR

## Screenshots / Demo (optional)
_Add images, GIFs, or describe how to verify the change manually._

## Testing
- [ ] Added or updated unit tests
- [ ] `npm test` (frontend) and/or `./gradlew test` (backend) pass locally
- [ ] Manually verified that the feature works as expected

## QA Steps
1. Step 1: ...
2. Step 2: ...
3. Verify expected behaviour: ...

## Checklist
- [ ] PR title follows **Conventional Commits** (e.g., `feat(timer): add Start/Pause toggle`)
- [ ] Branch is up to date with `main` (rebased)
- [ ] No generated files committed (e.g., `dist/`, `node_modules/`)
- [ ] Updated docs/README if necessary
```

### ISSUE_TEMPLATE/bug_report.md
```md
---
name: Bug Report
about: Report a bug to help us improve the project
title: 'fix: '
labels: bug
---

# Bug Report

## Describe the bug
A clear and concise description of what the bug is.

## Steps to reproduce
Steps to reproduce the behavior:
- Step 1
- Step 2
- Step 3

## Expected behavior
Describe what you expected to happen.

## Screenshots
If applicable, add screenshots to help explain your problem.

## Environment
- OS: 
- Browser: 
- Version: 

## Additional context
Add any other context about the problem here.
```

### ISSUE_TEMPLATE/feature_request.md
```markdown
---
name: Feature Request
about: Suggest a new feature to improve the project
title: 'feat: '
labels: enhancement
---

# Feature Request

## Goal
Describe the goal of this feature request.

## Tasks
- [ ] Task 1
- [ ] Task 2
- [ ] Task 3

## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3
```

### ISSUE_TEMPLATE/config.yml
```yaml
blank_issues_enabled: false
contact_links:
  - name: Questions & Discussions
    url: https://github.com/jamesmcdonald112/pomodoro/discussions
    about: Please ask and answer questions in GitHub Discussions.
```

## Why It Matters
Templates give developers a clear structure to follow when creating **feature requests**, **bug reports**, or **pull requests**.
They help ensure that every submission includes the **key details** needed for review, such as steps to reproduce, goals, or testing instructions.

This consistency keeps contributors focused, helps maintainers **quickly understand and prioritise issues**, and creates a shared standard that makes your project easier to maintain and collaborate on.

## Related 

## References
- [[GitHub]]
- [Creating a pull request template for your repository — GitHub Docs](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository)
- [Using templates to encourage useful issues and pull requests — GitHub Docs](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/about-issue-and-pull-request-templates)