
### **During development**

  

Use:

```
console.log
console.error
console.warn
console.debug
```

to inspect values, debug logic, and understand what’s happening.

  

### **✔ During production**

  

Use:

```
Sentry.captureException()
Sentry.captureMessage()
```

to record:

- uncaught errors
    
- failed API calls
    
- form submission failures
    
- unexpected states
    

  