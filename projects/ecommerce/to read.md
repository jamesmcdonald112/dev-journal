https://react.dev/learn/updating-objects-in-state

https://react.dev/learn/rendering-lists

https://react.dev/reference/react-dom/components/input



// `specRows` is added because React Hook Form cannot directly manage a Zod
// `z.record()` value. A record is an object with dynamic string keys, which
// RHF does not treat as a controllable field structure.
//
// To support dynamic “key/value” specification inputs (add/remove rows,
// validate each entry, track field state, etc.), the form exposes an array:
//
//   specRows: Array<{ key: string; value: string }>
//
// Users interact with this array in the UI. On submission, the array is
// transformed into the `specs` record expected by the base `productSchema`
// and by the database:
//
//   [{ key: "color", value: "red" }]
//   → { color: "red" }
//
// `productSchema` retains `specs` as `z.record(z.string())` for validation
// and persistence, while `productFormSchema` extends it with `specRows` to
// support a dynamic form interface. This is the standard pattern for managing
// key/value inputs in React Hook Form.