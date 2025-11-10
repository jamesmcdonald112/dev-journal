In libs/mongodb.ts, ChatGPT recommended to make it type safe using this as an example:
```ts
interface MongooseCache {
  conn: typeof mongoose | null;
  promise: Promise<typeof mongoose> | null;
}
declare global {
  var mongooseCache: MongooseCache | undefined;
}
```
- Issues arose when doing this so i will return to it.
- Other suggestions form chat:
	- Add try/catch only around mongoose.connect (not the env check).
	- aConsider adding connection logging (for debugging).