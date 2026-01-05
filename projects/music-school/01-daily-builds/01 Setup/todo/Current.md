## **Info Pack Delivery Service**

  

We created a **service layer** for the Info Pack feature to handle all side-effects in one place.

  

### **Purpose**

  

The service represents the **business action**:

**“Deliver an info pack submission.”**

  

It is responsible for:

- Coordinating what happens after a valid form submission
    
- Calling external systems (email, storage, logging)
    
- Keeping actions/routes thin and focused
    

  

### **Why a Service**

- Separates **business logic** from UI and routing
    
- Makes the feature easier to extend (email + Google Sheets later)
    
- Keeps external APIs (Resend, Sheets) isolated
    
- Matches **Bulletproof React** architecture
    

  

### **Location**

```
src/features/InfoPack/service/deliverInfoPack.ts
```

### **Role in the flow**

```
Form Action → deliverInfoPack (service) → APIs (email, storage)
```

The service does **not**:

- Render UI
    
- Validate input (handled by schema)
    
- Know about HTTP or forms
    

  

It only answers:

**“What should happen when an info pack is submitted?”**

```ts
export async function deliverInfoPack(input: {

fullName: string;

email: string;

phone: string;

studentAge?: number;

message?: string;

}): Promise<void> {

// TODO: Implement email sending (Resend, SMTP, etc.)

void input;

}
```