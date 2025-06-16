# Asynchronous JavaScript CHeat Sheet

## 1. How JavaScript Executes the Code
JavaScript uses a single-threaded synchronous execution model based on a Call Stack. It executes one line at a time from top to bottom. For asynchronous behavior, it uses the `Event Loop`, `Web APIs`, and `Callback Queue` to manage tasks like fetching data or handling timers.


## 2. Difference Between Synchronous and Asynchronous

| Feature         | Synchronous                      | Asynchronous                        |
|----------------|----------------------------------|-------------------------------------|
| Execution       | One task at a time               | Multiple tasks without waiting      |
| Blocking        | Blocking                         | Non-blocking                        |
| Example         | Looping over an array            | setTimeout, fetch, file reading     |


## 3. Ways to Make Code Asynchronous
- `setTimeout`, `setInterval`
- `fetch`, `XMLHttpRequest`
- Promises
- async/await
- Web APIs (like DOM events)
- `fs.readFile` (Node.js)


## 4. What Are Web Browser APIs?
Web APIs are provided by the browser (or Node.js) to perform asynchronous operations like:
- DOM manipulation
- Timers (`setTimeout`)
- HTTP requests (`fetch`)
- Event handling


## 5. What is Event Loop?
The `Event Loop` coordinates between the `Call Stack` and `Callback Queue`. It pushes callbacks to the call stack only when it is empty, enabling non-blocking behavior in JS.


## 6. Callback Hell
Callback hell refers to nested callbacks that become hard to read and maintain:
```js
getData(function(a){
  parseData(a, function(b){
    processData(b, function(c){
      displayData(c);
    });
  });
});
```


## 7. Inversion of Control in Callbacks
When using callbacks, you pass control to a third-party function. This is risky as you don’t control when or how the callback will be executed. Promises solve this by returning an object.

# Promises

## 8. What is a Promise?
A **Promise** is an object representing the eventual completion or failure of an asynchronous operation.


## 9. How to Create a New Promise
```js
let promise = new Promise((resolve, reject) => {
});
```

## 10. States of a Promise
- **Pending**: Initial state
- **Fulfilled**: Operation completed successfully
- **Rejected**: Operation failed


## 11. Consuming a Promise
```js
promise.then(result => {
  //success
}).catch(error => {
  //Failed
});
```


## 12. Chaining Promises Using `.then`
```js
doTask()
  .then(result => nextTask(result))
  .then(finalResult => display(finalResult))
```

## 13. Handling Errors in a Chain Using `.catch`
```js
doSomething()
  .then(result => nextStep(result))
  .catch(error => console.error(error));
```



## 14. `finally` Block in a Promise Chain
```js
fetchData()
  .then(data => handleData(data))
  .catch(err => console.error(err))
  .finally(() => console.log("Operation complete"));
```

## 15. Error Thrown in `.then` With `.catch`
If an error is thrown inside `.then`, it will be caught by the next `.catch`.


## 16. Error Thrown in `.then` Without `.catch`
If no `.catch` exists in the chain, the error goes unhandled and may crash the application.


## 17. Why `.catch` Should Be at the End
To ensure any error in the chain gets caught and handled properly.


## 18. Chaining Multiple Promises
```js
first()
  .then(second)
  .then(third)
  .catch(error => console.error(error));
```


## 19. Using `Promise.all`
Waits for all promises to resolve or rejects if any fail.
```js
Promise.all([p1, p2, p3])
  .then(results => console.log(results))
  .catch(err => console.error(err));
```


## 20. Error Handling in Promises
Always use `.catch` or `try/catch` (with async/await) to handle potential rejections.


## 21. Why Error Handling is Crucial
Uncaught promise errors can lead to silent failures, making debugging hard.


## 22. Promisifying Callback-Based Functions
Convert old-style callbacks to promises:
```js
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}
```

Example with `fs.readFile` in Node.js:
```js
const fs = require('fs').promises;

fs.readFile('file.txt', 'utf8')
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

# Useful Promise Utility Methods

## 23. `Promise.resolve`
Returns a promise that is already resolved.
```js
Promise.resolve('done').then(console.log);
```

## 24. `Promise.reject`
Returns a promise that is already rejected.
```js
Promise.reject('error').catch(console.error);
```

## 25. `Promise.all`
Waits for all promises to fulfill, or rejects if one fails.

## 26. `Promise.allSettled`
Waits for all promises to finish regardless of resolve or reject.
```js
Promise.allSettled([p1, p2])
  .then(results => console.log(results));
```

## 27. `Promise.any`
Returns the first fulfilled promise. Ignores rejections unless all fail.
```js
Promise.any([p1, p2]).then(console.log).catch(console.error);
```

## 28. `Promise.race`
Returns the first settled promise (either resolved or rejected).
```js
Promise.race([p1, p2]).then(console.log);
```


