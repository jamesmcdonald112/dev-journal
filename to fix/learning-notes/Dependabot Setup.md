---
title: Dependabot Setup
type: setup-guide
status: complete
date: 2025-10-20
updated: 2025-10-20
summary: Configuring Dependabot to automatically update npm and GitHub Actions dependencies on a weekly schedule.
tech_stack:
  - GitHub Actions
  - npm
keywords:
  - dependabot
  - github
  - security
  - automation
  - updates
---
# Dependabot Setup

## What I Learned
Dependabot automatically checks for outdated or vulnerable dependencies and creates pull requests to keep them up-to-date. It helps maintain project security and stability with minimal manual work.

## Example / Code Snippet
```yaml
version: 2
updates:
  # Keep npm dependencies updated
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"

  # Keep GitHub Actions workflows updated
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
```

### Notes

- The file must live directly in .github/ — **not** in .github/workflows/.
- You can view and manage Dependabot alerts in:
    **Repository → Settings → Code security and analysis.**
## Why It Matters
This ensures your dependencies stay secure and modern without manual checking.
Dependabot helps prevent vulnerabilities from leaking into production and keeps GitHub Actions workflows compatible with the latest versions.

## Related 
- [[to fix/learning-notes/Github Workflows]]
- [[to fix/learning-notes/Commitlint]]
- [[to fix/learning-notes/Husky]]
- [[to fix/learning-notes/Lint-Staged]]
- [[to fix/learning-notes/Biome]]

## References
- https://docs.github.com/en/code-security/dependabot/working-with-dependabot/dependabot-options-reference
- https://docs.github.com/en/code-security/dependabot/dependabot-quickstart-guide