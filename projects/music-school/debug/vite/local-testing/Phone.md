
## **Vite – Enable Network Access (--host)**

  
**Command:**

```
npm run dev -- --host
```

**What it does:**

Tells Vite to bind the dev server to 0.0.0.0 instead of localhost.

  

**Why:**

Allows other devices on the same Wi-Fi network (phone/tablet/laptop) to access your development site.

  

**When to use:**

- Testing form inputs on mobile
    
- Checking responsive layouts on real devices
    
- Previewing on another computer
    

  

**Security:**

Only accessible inside your local network.

Does _not_ expose anything to the internet.
