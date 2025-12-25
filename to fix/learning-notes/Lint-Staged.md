Runs a command (e.g. Biome) only on staged files.

Install lint-staged:

```shell
npm install --save-dev lint-staged # requires further setup
```


Add this line to the [[to fix/learning-notes/Husky]] / pre-commit file

```bash
npx lint-staged
```

In the package.json file, add this
```json
"lint-staged": {

  "*.{js,ts,tsx,jsx,json,md}": "biome check --write --no-errors-on-unmatched"

}
```
Biome will now automatically:

- format your code,
    
- lint it,
    
- apply safe fixes,
    
- re-stage the clean files before committing.
## Resources
- https://github.com/lint-staged/lint-staged
- https://biomejs.dev/recipes/git-hooks/#lint-staged