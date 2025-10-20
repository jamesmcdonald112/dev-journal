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


## Resources
- https://biomejs.dev/recipes/continuous-integration/
- https://docs.github.com/en/actions/how-tos/write-workflows
- https://github.com/marketplace?type=actions
- https://docs.github.com/en/actions/tutorials/build-and-test-code/nodejs
- https://stevekinney.com/courses/testing/continuous-integration