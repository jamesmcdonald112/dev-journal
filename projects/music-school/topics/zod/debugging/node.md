## **1. Use TSX to run TypeScript test files**

  

Zod schemas can be debugged by running small .ts files directly through Node.

To do this cleanly, use **TSX**, the zero-config TypeScript runner.

  

See:

[[projects/music-school/topics/node/debugging/TSX|TSX]]

  

TSX lets you:

- execute .ts files instantly (no build step)
    
- use import syntax (ESM)
    
- debug Zod schemas with full error formatting
    
- run temporary scripts without affecting the main project
    

  

Use TSX whenever you want to quickly test a schema, validate form data, or inspect Zod’s formatted errors.
