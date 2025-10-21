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

The docs go into detail about using triggers and their filters.

## Resources
- https://biomejs.dev/recipes/continuous-integration/
- https://docs.github.com/en/actions/how-tos/write-workflows
- https://github.com/marketplace?type=actions
- https://docs.github.com/en/actions/tutorials/build-and-test-code/nodejs
- https://stevekinney.com/courses/testing/continuous-integration
- https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax#on