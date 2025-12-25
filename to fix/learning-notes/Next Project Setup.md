
I want ot make a note for next project set ups in my obsidian, basically i want to say what is useful to have in the project, so we have next, we have tailwind, things like that, we alos want stuff like auto complete for tailwind and all

run a tailwind test to make sure it works

## Biome

It is Prettier + ESLint + ImportSorter all in one. 

It Formats code like Pretteir fixing spaces, semicolons, quotes etc:
```bash
# Format all files

npx @biomejs/biome format --write

# Format specific files

npx @biomejs/biome format --write <files>
```


it lints code like ESLin, making it detects logic and style problems:
```bash
# Lint files and apply safe fixes to all files

npx @biomejs/biome lint --write

# Lint files and apply safe fixes to specific files

npx @biomejs/biome lint --write <files>
```


It Organises Importats, sorting and remving usused imports:
```bash
# Format, lint, and organize imports of all files

npx @biomejs/biome check --write

# Format, lint, and organize imports of specific files

npx @biomejs/biome check --write <files>
```

This cna be done all at once using, ensuring that all files are formatted, Lint appiled safely, imports organised:
```bash
npx @biomejs/biome check --write
```


run biome check . next to make sure that works
if biome finds errors you can let it fix it automatically by using the Biome Vs code extension - **Biome VS Code extension** (Biome by biomejs).

biome allows you to make adjustments using the command ```
npx @biomejs/biome init to generate a biome.json file, so we will do it here, it actualyl alredy comes iwth the next setup.
its getting start page has a lot of useful commands such as 

to see if anything is incorrectly formatted
 npx biome check .            

```bash
# Format all files

npx @biomejs/biome format --write

# Format specific files

npx @biomejs/biome format --write <files>

# Lint files and apply safe fixes to all files

npx @biomejs/biome lint --write

# Lint files and apply safe fixes to specific files

npx @biomejs/biome lint --write <files>

# Format, lint, and organize imports of all files

npx @biomejs/biome check --write

# Format, lint, and organize imports of specific files

npx @biomejs/biome check --write <files>
```


### Continuous Integration Pipelines with Biome
For this pipeline, Biome will not write changes, it only fails if issues exist, so your pipeline can stop bad code from merging. This code will be used in a .github/workflows/ yml file.

Example in a GitHub Action:
```yaml
- name: Run Biome CI
  run: npx @biomejs/biome ci
```

It's equivalent to:
```bash
npx @biomejs/biome check
```


i need to make a reference to yml or YAML or whatever they are called and learn how to make them. 

## Why not install Biome Globally?
- Installing globally means that one copy of Biome for all projects but diffenet prokects h=may have differnt verisons or rules.
	- Exmaple, project A using Biome v1.9.4 and Project B uses Biome v2.0.0 (with new formatting defaults),
	- Formatting changes between them randomally break commits.
- Local installiation `npx @biomejs/biome` ensures that each project uses the exact Bione version listed in its package.json.

## Accessibility
Biome's accessibility (a11y) rules check for common HTML/JSX mistakes that break screen readers or keyboard navigation. Elements like Missing alt text, invalid header order (`<h3> before <h2>`), clickable divs, missing label etc. This is added as part of the defult setup for Biome



Run Biome automatically _before every commit_ on your local machine.

Usually done with **Husky** or **lint-staged**:
### References 
- https://biomejs.dev/guides/getting-started/
- 