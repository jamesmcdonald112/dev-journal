Commitlint checks that your **commit messages** follow a conventional format, e.g.:
```bash
feat: add mind map feature
fix: handle markdown parsing bug
chore: update dependencies
```

This feature is not covered by [[to fix/learning-notes/Biome]], as Biome only cares about your code, not your [[Git]] history.

Commitlint is a [[to fix/learning-notes/Husky]] hook, and the structure will be like this:

```
git commit -m "something"
        ↓
Husky triggers pre-commit (Biome)
        ↓
Husky triggers commit-msg (Commitlint)
        ↓
Fails if message doesn’t match pattern
```

This is required to get the CLI (the checker) and the rule set (based on Conventional Commits):
```bash
npm install -D @commitlint/cli @commitlint/config-conventional
```

And this creates the minimum required config file:
```bash
echo "export default { extends: ['@commitlint/config-conventional'] };" > commitlint.config.js
```

### Git Hook
To add the git hook, ensure [[to fix/learning-notes/Husky]] is installed and run the following to run Commitlint each time you make a commit:
```bash
echo "npx --no -- commitlint --edit \$1" > .husky/commit-msg
```

With this, if you type git commits that are incorrect, like:
```bash
git commit -m "update stuff"
```

It will fail. Instead, you need:
```bash
git commit -m "fix: correct typo in form
```

## Github Actions
check out their YAML file [their site](https://commitlint.js.org/guides/ci-setup.html). This ensures even if someone bypasses Husky locally, GitHub will still block commits with invalid messages:
```yaml
# .github/workflows/commitlint.yml
name: CI

on: [push, pull_request]

permissions:
  contents: read

jobs:
  commitlint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - name: Setup node
        uses: actions/setup-node@v4
        with:
          node-version: lts/*
          cache: npm
      - name: Install commitlint
        run: npm install -D @commitlint/cli @commitlint/config-conventional
      - name: Print versions
        run: |
          git --version
          node --version
          npm --version
          npx commitlint --version

      - name: Validate current commit (last commit) with commitlint
        if: github.event_name == 'push'
        run: npx commitlint --last --verbose

      - name: Validate PR commits with commitlint
        if: github.event_name == 'pull_request'
        run: npx commitlint --from ${{ github.event.pull_request.base.sha }} --to ${{ github.event.pull_request.head.sha }} --verbosegithub.event.pull_request.head.sha }} --verbose
```

## Prompt Helper
The prompt helper is purely a usability feature, it helps to write messages correctly, not check them.

```bash
npm install --save-dev @commitlint/cli @commitlint/config-conventional @commitlint/prompt-cli
```


example use:
```bash
git add .
npm run commit
```

It will ask:
```bash
type (feat, fix, docs...):
scope (optional):
description:
```

and build a valid commit message automatically.
## Resources
- https://commitlint.js.org/guides/getting-started.html
- https://commitlint.js.org/guides/ci-setup.html