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

Ensure you install husdky as a dev dependancy
```bash
npm install husky --save-dev
```

To avoid errors in the ci.yml workflow file, use the follwong code found on the husky offical docs - https://typicode.github.io/husky/how-to.html
```js
// Skip Husky install in production and CI
if (process.env.NODE_ENV === 'production' || process.env.CI === 'true') {
  process.exit(0);
}

const husky = (await import('husky')).default;
console.log(husky());
```

This ensures Husky only installs _locally_ —

it quietly skips on CI or production servers.

Update package.json to use the fule instad of calling Husky directly
```json
"prepare": "node .husky/install.mjs"
```

This prevents CI from ever calling husky directly.
## Resources
- https://typicode.github.io/husky/
  
  Related
- [[to fix/learning-notes/Github Workflows]]