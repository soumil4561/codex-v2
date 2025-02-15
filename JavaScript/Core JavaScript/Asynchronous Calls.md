**Synchronous:** two or more objects or events that do not exist or happen at the same time.
**Asynchronous:** When multiple related things happen without any being dependent on the completion of previous happenings.

## Callback Function
A **callback function** is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action. 

There are two ways in which the callback may be called: _synchronous_ and _asynchronous_. Synchronous callbacks are called immediately after the invocation of the outer function, with no intervening asynchronous tasks, while asynchronous callbacks are called at some point later.

**Functions as First-Class Citizens**: Since JavaScript treats functions as first-class objects, they can be passed as arguments.

***Synchronous Callback Example:***
```js
function greet(name, callback) {
  console.log("Hello " + name);
  callback();
}

greet("John", () => {
  console.log("Greeting finished!");
});

// Output:
// Hello John
// Greeting finished!
```

***Asynchronous Callback Example:***
```js
function fetchData(callback) {
  setTimeout(() => {
    const data = "Fetched Data";
    callback(data);
  }, 2000);
}

fetchData((result) => {
  console.log(result);  // Output after 2 seconds: "Fetched Data"
});
```

### What is Callback hell?
As callbacks are nested inside each other for managing multiple asynchronous operations, the code can become deeply nested and hard to maintain. This phenomenon is known as "callback hell" or "pyramid of doom."

```js
asyncOperation1(() => {
  asyncOperation2(() => {
    asyncOperation3(() => {
      asyncOperation4(() => {
        console.log("All operations complete");
      });
    });
  });
});
```

Solutions to Callback hell:
1. Use Modularity: Break down large callback chains into smaller, reusable functions.
2. Use Promises 
3. Use Async/Await

## Promises
[[Promise]] is an object representing the eventual completion/failure of an asynchronous operation. 
*a promise is a returned object to which you attach callbacks, instead of passing callbacks into a function*

```js
function successCallback(result) {
  console.log(`Audio file ready at URL: ${result}`);
}

function failureCallback(error) {
  console.error(`Error generating audio file: ${error}`);
}

createAudioFileAsync(audioSettings, successCallback, failureCallback);
```

Here, the `createAudioFileAsync` is a function expecting 2 callbacks. If it were to return a promise then: 
```js
createAudioFileAsync(audioSettings).then(successCallback, failureCallback);
```

### Chaining 
**Promises** help solve the *callback hell* problem by allowing you to attach callbacks to a returned promise object, rather than passing them directly into the function. This way, you can avoid deeply nested code and make asynchronous workflows more readable.

```js
const promise = doSomething();
const promise2 = promise.then(successCallback, failureCallback);
```

- When you call `doSomething()`, it returns a promise (`promise`).
- You can then attach a `.then()` method to `promise`. This method allows you to pass two callback functions: `successCallback` and `failureCallback`.
    - `successCallback` runs if the promise is resolved (success).
    - `failureCallback` runs if the promise is rejected (failure).

The `.then()` function returns a new promise. This new promise represents the completion of:
1. The original asynchronous operation (`doSomething`).
2. The execution of the callback function we provided (`successCallback` or `failureCallback`).

```js
doSomething()
  .then(result => doSomethingElse(result))  // When doSomething is done, doSomethingElse runs
  .then(newResult => doThirdThing(newResult))  // Then doThirdThing runs when doSomethingElse is done
  .then(finalResult => console.log(`Got the final result: ${finalResult}`))  // Final result
  .catch(failureCallback);  // Handles any error in the chain
```

*The benefit here is that the catch method has to be written only once.* 

>[!Important]
>It is important to always return promises from `then` callbacks, even if the promise always resolves to `undefined`
>If the previous handler started a promise but did not return it, there's no way to track its settlement anymore, and the promise is said to be "floating".

```js
doSomething()
  .then((url) => {
    // Missing `return` keyword in front of fetch(url).
    fetch(url);
  })
  .then((result) => {
    // result is undefined, because nothing is returned from the previous
    // handler. There's no way to know the return value of the fetch()
    // call anymore, or whether it succeeded at all.
  });
```

```js
doSomething()
  .then((url) => {
    // `return` keyword added
    return fetch(url);
  })
  .then((result) => {
    // result is a Response object
  });
```

*If the `successCallback` or `failureCallback` returns another promise, **the new promise (`promise2`) will not resolve until that promise resolves**. This enables **chaining** multiple asynchronous operations.*

>[!Tip]
>as a rule of thumb, whenever your operation encounters a promise, return it and defer its handling to the next `then` handler.

Using [`async`/`await`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) can help we write code that's more intuitive and resembles synchronous code.
```js
async function logIngredients() {
  const url = await doSomething();
  const res = await fetch(url);
  const data = await res.json();
  listOfIngredients.push(data);
  console.log(listOfIngredients);
}
```


### Error Handling
Promises solve a fundamental flaw with the callback pyramid of doom, by catching all errors, even thrown exceptions and programming errors. All errors are now handled by the [`catch()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method at the end of the chain.

**Nesting**
Nesting is a control structure to limit the scope of `catch` statements. Specifically, a nested `catch` only catches failures in its scope and below, not errors higher up in the chain outside the nested scope.

```js
doSomethingCritical()
  .then((result) =>
    doSomethingOptional(result)
      .then((optionalResult) => doSomethingExtraNice(optionalResult))
      .catch((e) => {}),
  ) // Ignore if optional stuff fails; proceed.
  .then(() => moreCriticalStuff())
  .catch((e) => console.error(`Critical failure: ${e.message}`));
```

The inner error-silencing `catch` handler only catches failures from `doSomethingOptional()` and `doSomethingExtraNice()`, after which the code resumes with `moreCriticalStuff()`. Importantly, if `doSomethingCritical()` fails, its error is caught by the final (outer) `catch` only, and does not get swallowed by the inner `catch` handler. 

>[!Good to know]
>It's possible to chain _after_ a failure, i.e. a `catch`, which is useful to accomplish new actions even after an action failed in the chain.

```js
doSomething()
  .then(() => {
    throw new Error("Something failed");

    console.log("Do this");
  })
  .catch(() => {
    console.error("Do that");
  })
  .then(() => {
    console.log("Do this, no matter what happened before");
  });
```

Here, the last `.then()` will always run. When the first then throws an error, it is handled by the catch statement and the operation is resumed.

#### Handling Promise rejection events

**On the web**
whenever a promise is rejected, one of two events is sent to the global scope:
- `unhandledRejection`: Sent when a promise is rejected but there is no rejection handler available.
- `rejectionHandled`: Sent when a handler is attached to a rejected promise that has already caused an `unhandledrejection` event.

**In Node.js**
In Node.js, handling promise rejection is slightly different. You capture unhandled rejections by adding a handler for the Node.js. 
```js
process.on("unhandledRejection", (reason, promise) => {
  // Add code here to examine the "promise" and "reason" values
});
```

### Creating Promise around old Callback API
In an ideal world, all asynchronous functions would already return promises. Unfortunately, some APIs still expect success and/or failure callbacks to be passed in the old way. The most obvious example is the [`setTimeout()`](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout) function.

Luckily we can wrap `setTimeout` in a promise. The best practice is to wrap the callback-accepting functions at the lowest possible level, and then never call them directly again.

```js
const wait = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

wait(10 * 1000)
  .then(() => saySomething("10 seconds"))
  .catch(failureCallback);
```

***`resolve()`** signals that the promise has been fulfilled, meaning the asynchronous operation (waiting for `ms` milliseconds) is complete. Once the promise is resolved, the `.then()` block is triggered.*

>[!The State of Zalgo]
>In the context of designing asynchronous APIs, this means a callback is called synchronously in some cases but asynchronously in other cases, creating ambiguity for the caller.

```js
function doSomething(callback) {
  if (Math.random() > 0.5) {
    callback();
  } else {
    setTimeout(() => callback(), 1000);
  }
}
```

**The Inversion of Control**
The API implementor does not control when the callback gets called. Instead, the job of maintaining the callback queue and deciding when to call the callbacks is delegated to the promise implementation. 

- Callbacks added with [`then()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then) will never be invoked before the [completion of the current run](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop#run-to-completion) of the JavaScript event loop.
- These callbacks will be invoked even if they were added _after_ the success or failure of the asynchronous operation that the promise represents.
- Multiple callbacks may be added by calling [`then()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then) several times. They will be invoked one after another, in the order in which they were inserted.

```js
Promise.resolve().then(() => console.log(2));
console.log(1);
// Logs: 1, 2
```

Instead of running immediately, the passed-in function is put on a **microtask queue**, which means it runs later (only after the function which created it exits, and when the JavaScript execution stack is empty), just before control is returned to the event loop

**Example**
```js
const wait = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

wait(0).then(() => console.log(4));
Promise.resolve()
  .then(() => console.log(2))
  .then(() => console.log(3));
console.log(1); // 1, 2, 3, 4
```

Here:
- 1 is printed as it is synchronous.
- Because of the `Promise.resolve` the thens are immediately pushed into the microtask queue becuase of the creation of a resolved promise object.
- Finally because the `setTimeout` is considered as a macrotask, it gets pushed to the task queue instead of the microtask queue.

## Async/Await

***`async/await`** is a syntactic sugar built on top of promises. It allows you to write asynchronous code that looks more like synchronous code, making it easier to read and maintain.*

The **`async function`** declaration creates a [binding](https://developer.mozilla.org/en-US/docs/Glossary/Binding) of a new async function to a given name. The `await` keyword is permitted within the function body, enabling asynchronous, promise-based behavior to be written in a cleaner style and avoiding the need to explicitly configure promise chains.

*The Async/Await also enables the use of ordinary try/catch blocks around asynchronous code*

>[!The More you Know]
>The purpose of `async`/`await` is to simplify the syntax necessary to consume promise-based APIs. The behavior of `async`/`await` is similar to combining [generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_generators) and promises.

Each time when an async function is called, it returns a new [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) which will be resolved with the value returned by the async function, or rejected with an exception uncaught within the async function.

Async functions can contain zero or more [`await`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await) expressions. Await expressions make promise-returning functions behave as though they're synchronous by suspending execution until the returned promise is fulfilled or rejected. The resolved value of the promise is treated as the return value of the await expression.

```js
async function foo() {
  return 1;
}
```

is similar to 
```js
function foo() {
  return Promise.resolve(1);
}
```

**Difference between Promise.resolve() and async**
- **`Promise.resolve()`**: If the value passed to `Promise.resolve()` is already a promise, it returns the **same promise reference**.
- **`async function`**: When an `async` function is called, it **always returns a new promise**, regardless of whether the return value inside the function is a promise or not.

```js
const p = new Promise((res, rej) => {
  res(1);
});

async function asyncReturn() {
  return p; // Returns a promise, but wraps it in a new promise
}

function basicReturn() {
  return Promise.resolve(p); // Returns the same promise reference as 'p'
}

console.log(p === basicReturn()); // true
console.log(p === asyncReturn()); // false
```

