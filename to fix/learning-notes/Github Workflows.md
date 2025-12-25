---
title: Github Workflows
type: case-study
status: draft
date: 2025-10-21
updated: 2025-10-21
summary: GitHub Actions automates testing, linting, builds, and deployments directly from your repository.Each .yml file inside .github/workflows/ defines when to run, what jobs to perform, and which steps to execute.These workflows ensure your code quality, automate repetitive tasks, and provide visual feedback on pull requests.
tech_stack:
  - GitHub Actions
  - YAML
  - Ubuntu Runner
  - Node.js
  - Biome
  - Vitest
keywords:
  - ci/di
  - yaml
  - workflow
  - jobs
  - actions
  - steps
  - runners
  - automation
---
# Github Workflows

## What I Learned
Workflows in GitHub Actions are written in YAML and define automated tasks that run when triggered (e.g., on a push or pull request).
Each workflow file contains jobs, and each job contains steps. Steps can either **run a command** or **use a predefined Action** from the GitHub Marketplace.

## Example / Code Snippet
Biome Workflow: 
```yaml
name: Code quality

on:
  push:
  pull_request:

jobs:
  quality:
    runs-on: ubuntu-latest
    permissions:
      contents: read
    steps:
      - name: Checkout
        uses: actions/checkout@v5
        with:
          persist-credentials: false
      - name: Setup Biome
        uses: biomejs/setup-biome@v2
        with:
          version: latest
      - name: Run Biome
        run: biome ci .
```

| **Line**     | **Meaning**                                        |
| ------------ | -------------------------------------------------- |
| name:        | Appears in the Actions tab — any label you choose. |
| on:          | Defines triggers (when to run).                    |
| jobs:        | Required keyword — defines one or more jobs.       |
| quality:     | Internal job ID (you choose).                      |
| runs-on:     | Chooses OS runner (e.g., Ubuntu, Windows, macOS).  |
| permissions: | Restricts access to repo contents (optional).      |
| steps:       | List of tasks (either uses: or run:).              |
## Why It Matters
GitHub Actions automatically runs tests, linting, and builds — catching issues before merging to main.
It ensures consistent project quality, automates boring tasks, and integrates tightly with your PR workflow.

## **Core Concepts (Simplified)**

### name:
Visible label in the GitHub Actions UI.
Purely for readability — doesn’t affect logic.

---
### on: — Workflow Triggers
[[https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#on]]

Defines _when_ the workflow should run.
```yaml
on:
  push:
  pull_request:
```

You can target specific branches or allow manual runs:
```yaml
on:
  push:
    branches: [main]
  pull_request:
    branches: [main, dev]
  workflow_dispatch: # manual trigger
```

---
### jobs:
[[https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobs]]

Each workflow must have at least one job:

```yaml
jobs:
  build-and-test:
    name: Build and test project
```

You can control job order with dependencies:
```yaml
jobs:
  job1:
  job2:
    needs: job1
  job3:
    needs: [job1, job2]
```

---
### Runners

Defines what machine executes the job.

Common option:
```yaml
runs-on: ubuntu-latest
```
Other options: windows-latest, macos-latest

---
### Steps
[[https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_idsteps]]

Each job consists of **steps**, which are executed in order.

You can:
- Use a prebuilt **Action**
- Or **run your own shell command**

```yaml
steps:
  - name: Checkout repository
    uses: actions/checkout@v4

  - name: Install dependencies
    run: npm ci
```

---
### uses: — Predefined Actions

Runs existing Actions published on GitHub.

Pattern: {owner}/{repo}@{version}

| **Example**                | **Description**                  | **Docs**                                                      |
| -------------------------- | -------------------------------- | ------------------------------------------------------------- |
| actions/checkout@v4        | Clones your repo into the runner | [actions/checkout](https://github.com/actions/checkout)       |
| actions/setup-node@v4      | Installs Node.js                 | [setup-node](https://github.com/actions/setup-node)           |
| biomejs/setup-biome@v2     | Installs Biome for linting       | [biomejs/setup-biome](https://github.com/biomejs/setup-biome) |
| actions/upload-artifact@v4 | Uploads test/build artifacts     | [upload-artifact](https://github.com/actions/upload-artifact) |

---
### run: — Custom Commands

Executes shell commands (just like in your terminal):

```yaml 
run: npm run lint
run: biome ci .
run: npm run test
```

---

## **Finding Available Actions**

Browse the **GitHub Marketplace** to find thousands of ready-to-use Actions:

👉 [GitHub Marketplace – Actions](https://github.com/marketplace?type=actions)

---

## **Related**

- [[to fix/learning-notes/Biome]]
- [[to fix/learning-notes/Commitlint]]
- [[Dependabot]]
- [[learning-notes/Vitest]]

---
## **References**

- [GitHub Workflow Syntax](https://docs.github.com/en/actions/how-tos/write-workflows)
- [Events That Trigger Workflows](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows)
- [GitHub Marketplace: Actions](https://github.com/marketplace?type=actions)
- [Node.js Build & Test Example](https://docs.github.com/en/actions/tutorials/build-and-test-code/nodejs)
- [Biome CI Example](https://biomejs.dev/recipes/continuous-integration/)
- [Steve Kinney – CI with Vitest](https://stevekinney.com/courses/testing/continuous-integration)