# p-limit

Run multiple promise-returning & async functions with limited concurrency.
Heavily inspired by [sindresorhus/p-limit](https://github.com/sindresorhus/p-limit).

## Usage

```ts
import { makeRunWithLimit } from "https://denopkg.com/alextes/p-limit";

const { runWithLimit } = makeRunWithLimit(1);

(async () => {
  // Only one promise is run at once
  const result = await Promise.all([
  runWithLimit(() => fetchSomething("foo")),
  runWithLimit(() => fetchSomething("bar")),
  runWithLimit(() => doSomething()),
]);
  console.log(result);
})();
```

## API

### makeRunWithLimit(concurrency)
`Number => { runWithLimit, getActiveCount, getPendingCount }`
Takes a number, setting the concurrency for the promise queue. Returns a set of functions to use the queue.

#### runWithLimit
`<A>(() => Promise<A>) => Promise<A>`

Pass a thunk to this function that returns a promise. i.e. pass a function that when invoked, kicks off an asynchronous computation, and if you need the result, returns a promise. `runWithLimit` will resolve with said result as soon as it resolves.

#### getActiveCount
`() => Number`

Call to get the number of promises that are currently running.

#### getPendingCount
`() => Number`

Call to check how many promises are still waiting to start execution.
