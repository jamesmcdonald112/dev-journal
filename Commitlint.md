Commitlint checks that your **commit messages** follow a conventional format, e.g.:
```bash
feat: add mind map feature
fix: handle markdown parsing bug
chore: update dependencies
```

This feature is not covered by [[Biome]], as Biome only cares about your code, not your [[Git]] history.

Commitlint is a [[Husky]] hook, and the structure will be like this:

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
```bah
echo "export default { extends: ['@commitlint/config-conventional'] };" > commitlint.config.js
```

### Git Hook
To add the git hook, ensure [[Husky]] is installed and run the following to run Commitlint each time you make a commit:
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
## Resources
- https://commitlint.js.org/guides/getting-started.html