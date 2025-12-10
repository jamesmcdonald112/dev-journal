# **GENERAL TESTS (Before field-specific tests)**

  

### **1. Required attributes block empty submission**

- Leave everything blank
    
- Click submit
    
    **Expected:**
    
- Browser prevents submission
    
- No network request
    
- No inline errors
    
- No toast
    

  

### **2. Disable required temporarily to test Zod**

  

You already know this, but document it:

  

When required is removed:

- Send empty form
    
- Zod should return the correct Expected string, received null → translated into **inline error**.
    

---

# **🔹** 

# **FULL NAME TESTS**

  

Schema rules:

- min length = FULL_NAME.min
    
- max length = FULL_NAME.max
    
- trimmed
    

  

### **1. Blank (after removing required)**

  

Input: ""

Expected: inline error:

**“Name must be at least 2 characters”** (or whatever your min is)

  

### **2. Only spaces**

  

Input: "     "

Expected: same as blank → inline error.

  

### **3. Too short**

  

Input: "A"

Expected: inline error:

**“Name must be at least X characters”**

  

### **4. Too long**

  

Input: string > max

Expected: inline error:

**“Name cannot exceed X characters”**

  

### **5. Valid name**

  

Input: "John Doe"

Expected:

- No inline error
    
- Submits successfully
    

---

# **🔹** 

# **EMAIL TESTS**

  

Schema rules:

- must be valid email
    
- lowercased
    
- trimmed
    
- max length
    

  

### **1. Blank (with required removed)**

  

Expected: inline error: **“Enter a valid email”**

  

### **2. Invalid format**

  

Test each:

- "abc"
    
- "abc@"
    
- "abc@com"
    
- "abc@domain."
    
    Expected: **inline error**
    

  

### **3. Spaces**

  

Input: "   test@example.com   "

Expected:

- Trims correctly
    
- Valid email → no error
    

  

### **4. Too long email**

  

Use a generated very long email > EMAIL.max

Expected: inline error: **“Email is too long”**

  

### **5. Valid email**

  

"chloe@example.com"

→ No error

---

# **🔹** 

# **PHONE TESTS**

  

Schema rule:

regex(PHONE.regex, "Enter a valid phone number")

  

### **1. Blank (if required removed)**

  

Expected: inline error

  

### **2. Invalid formats**

  

Test each:

- "abc"
    
- "123"
    
- "087-"
    
- "++3530871234567"
    

  

Expected: inline error

  

### **3. Valid phone numbers (your regex decides)**

  

Examples (Ireland):

- "0871234567"
    
- "0839876543"
    
    Expected: no errors
    

  

### **4. Leading/trailing spaces**

  

" 0871234567 "

Expected: trimmed → valid

---

# **🔹** 

# **STUDENT AGE TESTS**

#  **(optional field)**

  

Schema rules:

- coerced to number
    
- integer
    
- min = 1
    
- max = STUDENT_AGE.max
    
- optional
    

  

### **1. Leave blank**

  

Expected: no error (because optional)

  

### **2. Non-number**

  

Input: "abc"

Expected: inline error (coerce will fail)

  

### **3. Decimal numbers**

  

Input: "4.5"

Expected: inline error (int only)

  

### **4. Too low**

  

"0"

Expected: inline error: **“Age must be at least 1”**

  

### **5. Too high**

  

Input: something above max

Expected: inline error: **“Age too high”**

  

### **6. Valid ages**

  

Test:

- "1"
    
- "5"
    
- "17"
    

  

Expected: valid

  

### **7. Spaces**

  

"   10   " → trimmed + coerced

Expected: valid

---

# **🔹** 

# **MESSAGE FIELD TESTS (optional)**

  

Schema rules:

- string
    
- trimmed
    
- max length
    

  

### **1. Leave blank**

  

Expected: valid

  

### **2. Spaces**

  

Input: "     "

Expected: becomes empty string → valid

  

### **3. Too long**

  

Paste a string > MESSAGE.max

Expected: inline error:

**“Message is too long”**

  

### **4. Typical short message**

  

Works fine.

---

# **🔹** 

# **FORM-LEVEL ERROR TESTS**

  

These test your **isActionError** and **unexpected error handling**.

---

## **1. Simulate server failure**

  

In your action:

```
throw new ActionError({
  code: "INTERNAL_SERVER_ERROR",
  message: "Failed to send email"
});
```

Expected:

- Toast appears: **“Failed to send email”**
    
- No inline errors
    
- No redirect
    

---

## **2. Simulate unexpected crash**

  

Example:

```
JSON.parse("invalid");
```

Expected:

- Toast: **“Unexpected error”**
    
- Error logged in console
    
- No inline errors
    
- No redirect
    

---

## **3. Simulate slow network**

  

Inside the handler:

```
await new Promise((resolve) => setTimeout(resolve, 2000));
```

Expected:

- Spinner appears
    
- Fields disabled
    
- After 2 seconds → success or error behaviour triggers
    

---

# **🔹** 

# **SUCCESS FLOW TEST**

  

### **1. Valid data**

  

Fill all fields correctly.

  

Expected:

- No inline errors
    
- Loading spinner briefly (if delay inserted)
    
- Redirect to /thank-you
    
- No toast (your design choice)
    

---

# **🔥** 

# **BOOKMARK-READY VERSION (compact)**

  

If you want, I can rewrite this entire plan into a **compact checklist** for Obsidian.

---

If you want, I’ll also generate:

  

✅ Automated Vitest test plan

✅ Test case table in markdown

✅ A “QA-ready” version you can send to a client or coworker

  

Just say **“give me the QA version”**.