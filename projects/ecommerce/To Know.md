## API Calls and Post Methods
 How Your Form Connects to Your API Route:

  When you submit the form in product-form.tsx, here's what happens step-by-step:

  1. Form Submission (line 103 in product-form.tsx)
    - User clicks the Submit button
    - React Hook Form calls your onSubmit function
  2. Fetch Request (line 76 in product-form.tsx)
    - Your code sends an HTTP POST request to /api/products
  const response = await fetch("/api/products", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(payload),
  });
  3. Next.js Routing (automatic, you don't write this)
    - Next.js intercepts the POST request to /api/products
    - Next.js looks in the api/products/ folder for a route.ts file
    - Next.js finds your route.ts and looks for a POST function
    - Next.js automatically calls your POST function with the request
  4. API Processing (your POST function in api/products/route.ts)
    - The POST function receives the request
    - It parses the JSON body: const json = await request.json()
    - It validates with Zod: const parsedBody = productSchema.parse(json)
    - It saves to database: const product = await createProduct(parsedBody)
    - It returns the response: NextResponse.json({ success: true, data: product })
  5. Response Back to Form (lines 84-93 in product-form.tsx)
    - Your form receives the response
    - It checks if it was successful: if (!response.ok)
    - It shows a success or error toast
    - It resets the form on success

  In short: You call the API by using fetch("/api/products", ...), and Next.js automatically routes that to your POST function. No explicit function call needed!
  