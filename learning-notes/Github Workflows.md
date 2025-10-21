Must be added to a folder called `.github/workflows/`

Example [[Biome]] workflow found on their site:

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


Every .yml file inside
```
.github/workflows/
```

is a GitHub workflow definition.

Each file describes:
- When it should run (on:)
- What jobs to run (jobs:)
- What steps each job should perform (steps:)

### Name
You can name it anything you want. This is what appears in the GitHub "Actions" tab.

### On: - When the Workflow Runs
[GitHub Docs → “Workflow syntax: on”](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#on) defines the triggers, i.e., what causes this workflow to run. [GitHub Docs → Event Trigger Workflows](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows) gives a list of possible triggers to use such as: 
```yaml
on:
	push:
	pull_request:
```

The docs go into detail about using triggers and their filters and schedules, allowing you to make it more specific:
```yaml
on:
  push:
    branches: [main]
  pull_request:
    branches: [main, dev]
  workflow_dispatch:  # lets you run it manually from the Actions tab
```


### Jobs: 
[GitHub Docs → “Workflow syntax: job](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows)

Each workflow must have at least one job:
```yaml
jobs:
  build-and-test:
```
- jobs: keyword (required)
- build-and-test: your chosen name (you can call it anything you like). It only needs to be unique in that file.

If a part of the file requires the other another ob o the file to be complete you can sepcify that:

```yaml
jobs:
  job1:
  job2:
    needs: job1
  job3:
    needs: [job1, job2]
```

### Runners:


You must also select what machine to use to execute these commands. Commly used is:

```yaml
runs-on: ubuntu-latest
```

### Steps
👉 [GitHub Docs → “jobs.<job_id>.steps”](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_idsteps)
There are two options. Use a predefined action:
```yaml
uses: actions/checkout@v4
```

or run a custom command:
```yaml
run: npm ci
```

you can mix both freely.


### Uses
- actions/checkout@v4 = clones your repo so you can access files
- actions/setup-node@v4 = installs and configures Node.js
- biomejs/setup-biome@v2 = installs Biome
- actions/upload-artifact@v4 = uploads files from your job (e.g., test reports)

The pattern is always:

{owner}/{repo}@{version}

## Resources
- https://biomejs.dev/recipes/continuous-integration/
- https://docs.github.com/en/actions/how-tos/write-workflows
- https://github.com/marketplace?type=actions
- https://docs.github.com/en/actions/tutorials/build-and-test-code/nodejs
- https://stevekinney.com/courses/testing/continuous-integration
- https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax#on