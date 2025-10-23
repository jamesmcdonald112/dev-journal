## Setup
- [[Setting Up Obsidian for Technical Note-Taking]]
- [[Next]]
- [[Biome]]
- [[Husky]]
- [[Lint-Staged]]
- [[Dependabot Setup]]
- [[README Badges]]
- [[VS Code Workspace Settings]]
- [[Commitlint]]
- [[Github Workflows]]
- [[Testing Environment Setup]]
- [[Vitest]]
- [[Jsdom]]
- [[React Testing Library]]
- [[Zod]]
- [[Jest-Dom]]
- [[User-Event]]
- [[Tailwind]]
- [[README]]
- [[Vercel]]
- [[GitHub Projects]]



## Github notes
- [[Gray-matter]]
- [[Remark]]

## Next Pages
- Create a page for the notes. [[Creating Pages in Next.js 14+]]
- [[GitHub REST API — Contents (Get repository content)]]


## Issues

### GitHub REST API
Calling from the original GItHub REST API endpoint `https://api.github.com/repos/<user>/<repo>/contents/<file>`, for my project it is `https://api.github.com/repos/jamesmcdonald112/dev-journal/contents/Tailwind.md` gives back files like:
```json
1. content: "CgoKYWRkIGN1c3Rvb....."
2. download_url: "https://raw.githubusercontent.com/jamesmcdonald112/dev-journal/main/Tailwind.md"
3. encoding: "base64"
4. git_url: "https://api.github.com/repos/jamesmcdonald112/dev-journal/git/blobs/4a729b175407f69181d8aa40e90e6e117e8ec14a"
5. html_url: "https://github.com/jamesmcdonald112/dev-journal/blob/main/Tailwind.md"
6. name: "Tailwind.md"
7. path: "Tailwind.md"
8. sha: "4a729b175407f69181d8aa40e90e6e117e8ec14a"
9. size: 549
10. type: "file"
11. url: "https://api.github.com/repos/jamesmcdonald112/dev-journal/contents/Tailwind.md?ref=main"
12. _links: {self: 'https://api.github.com/repos/jamesmcdonald112/dev-journal/contents/Tailwind.md?ref=main', git: 'https://api.github.com/repos/jamesmcdonald112/dev-…it/blobs/4a729b175407f69181d8aa40e90e6e117e8ec14a', html: 'https://github.com/jamesmcdonald112/dev-journal/blob/main/Tailwind.md'}
13. [[Prototype]]: Object
```
With this infomation the content must be decoded first usinf a buffer and I cannot difectly transform it to markdown renders liek [[Gray-matter]] or [[Remark]] without decoding it first. One of the parts of the JSON file include the downlouad_url which is the raw data. This give me the exact markdown string.

#### Why we are not using application/vnd.github.html+json
It may seem convient to have or files converted to html from markdown  but it is restricted.
- There is no control over rendering so they cannot be styled using [[Tailwind’s Prose Classes]]
- Github styled markdown only meaning it cannot be extened with custom markdown
However, it is convienet for previews.

After switchign to `application/vnd.github.raw+json`, the isssue that arose was to change the response type to parse text
```ts
const data: string = await res.text();
```



## Commands 

### Commiting
To run [[Commitlint]] prompt generator. 
```bash
git add .
npm run commit
```
