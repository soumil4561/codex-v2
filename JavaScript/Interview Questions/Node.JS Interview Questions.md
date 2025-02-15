---
Link: https://gist.github.com/paulfranco/9f88a2879b7b7d88de5d1921aef2093b#q5-what-is-an-error-first-callback-
---
[[Asynchronous Calls]]

#### How does Node.JS handle concurrency if its single threaded?
Node internally uses multiple POSIX threads for various I/O operations such as File, DNS, Network calls etc.
When Node gets I/O request it creates or uses a thread to perform that I/O operation and once the operation is done, it pushes the result to the event queue. On each such event, event loop runs and checks the queue and if the execution stack of Node is empty then it adds the queue result to execution stack.

#### What is V8?
V8 library enables Node.JS to execute javascript in its environment. Developed by Google. V8 converts higher level code to machine code to be executed on CPU. 

*Node.js comes with a [[REPL]] (read eval print loop) environment for runtime execution*

#### What is Event Loop?
Node.js is a single threaded application but it support concurrency via concept of event and callbacks. Node.JS apis are asynchronous but Node.js is single threaded, so it uses async function to maintain concurrency. Node thread keeps an event loop and whenever any task get completed, it fires the corresponding event which signals the event listener function to get executed.


#### What's a buffer class?
A Buffer is a kind of an array of integers and corresponds to a raw memory allocation outside the V8 heap. A Buffer cannot be resized.

#### What's a stream?
Streams are objects that let you read data from a source or write data to a destination in a continuous fashion.
Types of stream available:
- Readable: used in reading a large chunk of data from a source.
- Writable: used in writing a large chunk of data to the destination.
- Duplex: Duplex streams are both readable and writable
- Transform: Transform stream is the duplex stream which is used in modifying the data

#### What is piping/chaining in Node.JS?
In Node.js, "piping" refers to the method of connecting the output of one stream directly to the input of another. This is especially useful when working with data streams, such as reading files, handling HTTP requests, or processing large datasets in chunks rather than loading everything into memory at once.

```javascript
readableStream.pipe(writableStream);
```

#### !!!Explain How does Node.JS work
A Node.JS app creates a single thread on invocation. Whenever a request comes in, Node.JS processes it completely before moving to the next request. It works asynchronously using event loop and callbacks. All the non-blocking functions are provided with a callback function. When the external event (I/O, Network , File etc.) is completed the associated callback function is run to serve the result of the event. In this way Node.JS handles multiple request simultaneously.

*whenever its response is ready, an event is called which triggers the associated callback function to send this response.*

#### How to catch unhandled exceptions in Node.js 
Unhandled exception could occur during synchronous (or async) operations. For this, we can add a handler at `Process` level using process.on(): 

```js
process.on('uncaughtException', function(err) {
  console.log('Caught exception: ' + err);
});
```

#### Streams are EventEmitter?
Each type of Stream is an **EventEmitter** instance and throws several events at different instance of times. 
- **data** - This event is fired when there is data is available to read.
- **end** - This event is fired when there is no more data to read.
- **error** - This event is fired when there is any error receiving or writing data.
- **finish** - This event is fired when all data has been flushed to underlying system

#### What are stubs?
**Stubs** are functions/programs that simulate the behaviors of components/modules. Stubs provide canned answers to function calls made during test cases. Also, you can assert on with what these stubs were called.
They are dummy functions or methods that simulate the behavior of real components in your codebase. Stubs allow you to control the behavior of dependencies in your tests, making it easier to isolate and test individual units of code.

```js
var fs = require('fs');

var readFileStub = sinon.stub(fs, 'readFile', function(path, cb) {
  return cb(null, 'filecontent');
});

expect(readFileStub).to.be.called;
readFileStub.restore();
```

*Node.js can facilitate deployment on multi-core systems where it does use the additional hardware. It packages with a Cluster module which is capable of starting multiple Node.js worker processes that will share the same port.*

#### Rewrite promise-based Node.js applications to Async/Await
```js
function asyncTask() {
    return functionA()
        .then((valueA) => functionB(valueA))
        .then((valueB) => functionC(valueB))
        .then((valueC) => functionD(valueC))
        .catch((err) => logger.error(err))
}
```

```js
async function asyncTask() {
    try {
        const valueA = await functionA()
        const valueB = await functionB(valueA)
        const valueC = await functionC(valueB)
        return await functionD(valueC)
    } catch (err) {
        logger.error(err)
    }
}
```

#### Difference b/w Node.js and ES6 modules?
Node.js and ES6 modules are two different systems for organizing and using code modules in JavaScript. Node.js follows CommonJS guidelines while ES6 are modern javascript rules:

Node:
```js
const moduleName = require('module-name');
module.exports = someFunctionOrVariable;
```

ES6:
```js
import moduleName from 'module-name';
export const someVariable = value;
```

#### Timing Features of Node.JS
The Timers module in Node.js contains functions that execute code after a set period of time.

- **setTimeout/clearTimeout** - can be used to schedule code execution after a designated amount of milliseconds
- **setInterval/clearInterval** - can be used to execute a block of code multiple times
- **setImmediate/clearImmediate** - will execute code at the end of the current event loop cycle
- **process.nextTick** - used to schedule a callback function to be invoked in the next iteration of the Event Loop

#### Difference b/w dependencies, devDependencies and peerDependencies
- **dependencies** - Dependencies that your project needs to run, like a library that provides functions that you call from your code. They are installed transitively (if A depends on B depends on C, npm install on A will install B and C).
- **devDependencies** - Dependencies you only need during development or releasing, like compilers that take your code and compile it into javascript, test frameworks or documentation generators. They are not installed transitively (if A depends on B dev-depends on C, npm install on A will install B only).
- **peerDependencies** - Dependencies that your project hooks into, or modifies, in the parent project, usually a plugin for some other library or tool. It is just intended to be a check, making sure that the parent project (project that will depend on your project) has a dependency on the project you hook into.

#### Convert existing callback API to promises
```js
function divisionAPI (number, divider, successCallback, errorCallback) {
    if (divider == 0) {
        return errorCallback( new Error("Division by zero") )
    }
    successCallback( number / divider )
}
```

```js
function divisionAPI(number, divider) {
    return new Promise(function(fulfilled, rejected) {
        if (divider == 0) {
            return rejected(new Error("Division by zero"))
        }
        fulfilled(number / divider)
    })
}

// Promise can be used with together async\await in ES7 to make the program flow wait for a fullfiled result
async function foo() {
    var result = await divisionAPI(1, 2); // awaits for a fulfilled result!
    console.log(result);
}
```

