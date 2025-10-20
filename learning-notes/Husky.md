Sets up Git hooks (e.g. run a command _before committing_).

```sh
npm install -D husky lint-staged
```

This should create a `.husky/pre-commit` file. Add this so it runs lint before each commit:
```sh
# .husky/pre-commit
npx lint-staged
```

The pre-commit file should look like this:
```bash
npm test
npx lint-staged
```
## Resources
- https://typicode.github.io/husky/