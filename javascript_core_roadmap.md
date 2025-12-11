# JavaScript Core Concepts Interview Prep

This document contains important JavaScript core concepts with reference links. You can use this as a learning roadmap.

---

## 📌 Table of Contents

1. [How JavaScript Works / Event Loop](#1-how-javascript-works--event-loop)
2. [Execution Context & Hoisting](#2-execution-context--hoisting)
3. [Scope Chain](#3-scope-chain)
4. [Var, Let, Const & Temporal Dead Zone](#4-var-let-const--temporal-dead-zone)
5. [This Keyword, Call, Apply, Bind & Closures](#5-this-keyword-call-apply-bind--closures)
6. [Constructor Functions & Classes](#6-constructor-functions--classes)
7. [Prototypal Inheritance](#7-prototypal-inheritance)
8. [Currying](#8-currying)
9. [Promises](#9-promises)
10. [Spread, Rest & Destructuring](#10-spread-rest--destructuring)
11. [Event Delegation & Event Bubbling](#11-event-delegation--event-bubbling)
12. [Generator Functions](#12-generator-functions)
13. [Debounce](#13-debounce)
14. [Throttle](#14-throttle)
15. [Polyfill for map](#15-polyfill-for-map)
16. [Polyfill for filter](#16-polyfill-for-filter)
17. [Polyfill for reduce](#17-polyfill-for-reduce)
18. [Flatten Array](#18-flatten-array)
19. [Polyfill for Flat()](#19-polyfill-for-flat-method)
20. [Object keys and values change](#20-given-an-object-make-the-keys-as-values-and-values-as-keys-Handle-duplicates-assuming-values-are-only-strings)
21. [Currying Implementaion](#21-currying-implementation)
22. [Promises Implementation](#22-promises-implementation)
23. [Promise.all and other methods](#23-promiseall-and-other-methods)
24. [Implement setIntercal using setTimeout](#24-implement-setintercal-using-settimeout)
25. [Call,Apply and Bind](#25-callapply-and-bind)
26. [Memoize function](#26-memoize-function)

---

## 1. How JavaScript Works / Event Loop
🔗 https://www.javascripttutorial.net/javascript-event-loop/

The JavaScript event loop handles asynchronous operations by managing the call stack, event queue, microtask queue, and Web APIs.

---

## 2. Execution Context & Hoisting
🔗 https://dev.to/jahid6597/javascript-execution-context-a-deep-dive-4kno

Execution context determines how variables and code are executed. Hoisting allows variable and function declarations to be accessible before initialization.

---

## 3. Scope Chain
🔗 https://medium.com/@mohdtalib.dev/scope-chain-1e3cd8cfaae8

Scope chain controls variable lookup and accessibility across nested scopes.

---

## 4. Var, Let, Const & Temporal Dead Zone
🔗 https://medium.com/@robinviktorsson/understanding-the-differences-between-var-let-and-const-in-javascript-and-typescript-0ddd90c0b672

`var` has function scope; `let` and `const` have block scope and exist in the *Temporal Dead Zone* until initialized.

---

## 5. This Keyword, Call, Apply, Bind & Closures
🔗 https://dev.to/antonzo/lexical-scope-lexical-environment-execution-context-closure-in-javascript-5bn6

The `this` keyword behaves differently depending on invocation. `call`, `apply`, and `bind` manually set `this`. Closure allows a function to remember its lexical scope.

---

## 6. Constructor Functions & Classes
🔗 https://javascript.info/constructor-new
🔗 https://javascript.info/class

Constructor functions and ES6 classes are used to create reusable object structures.

---

## 7. Prototypal Inheritance
🔗 https://javascript.info/prototype-inheritance

Prototype-based inheritance allows objects to inherit properties and methods from other objects.

---

## 8. Currying
🔗 https://javascript.info/currying-partials

Currying transforms a function with multiple parameters into nested single-parameter functions.

---

## 9. Promises
🔗 https://javascript.info/promise-basics

Promises represent the eventual completion of an asynchronous operation.

---

## 10. Spread, Rest, Destructuring
🔗 https://javascript.info/rest-parameters-spread
🔗 https://javascript.info/destructuring-assignment

Spread expands elements; rest collects them. Destructuring extracts values into variables.

---

## 11. Event Delegation & Event Bubbling
🔗 https://thenomadtechie.medium.com/mastering-javascript-event-handling-techniques-bubbling-capturing-delegation-and-propagation-0cdbe56f0b39

Event delegation uses event bubbling to efficiently manage many events.

---

## 12. Generator Functions
🔗 https://javascript.info/generators

Generators allow pausing execution using the `yield` keyword.

---

## 13. Debounce

setTimeout only schedules something once. But when you use it inside events that fire rapidly (like typing, scroll, resize), it runs a new timeout every time the event triggers.So Debounce is needed as its not just delaying a function but also preventing unnecessary repeated calls.

Debouncing is a strategy to enhance performance by ensuring a function runs only after a specified delay. If invoked repeatedly, the delay resets. In simpler terms, debouncing transforms a function so that it executes after a specified period of inactivity.

When to Use Debouncing?
  - Search Inputs:
      Avoid firing a search query on every keystroke, which can overwhelm your server with   requests.
  - Resize Events:
      Prevent multiple recalculations during window resizing, which can slow down your application.
  - Scroll Events:
      Optimize tasks like infinite scrolling or lazy loading, ensuring that they only trigger after scrolling has stopped.

Debounce Implementation
```js
export default function debounce(func, wait) {
  let timer;

  return (...args) => {
    clearTimeout(timer) // resets timer 
    timer = setTimeout(() => func(...args),wait) // func runs after wait 
  }
}

```
Without Debounce
- A (call) → P (call) → I (call) → space (call)

With debounce
- Type → Reset timer
- Type → Reset timer
- Type → Reset timer
- STOP typing → After delay → fn runs

We can use debounce in react by creating custom hook if we want to debounce a value

```js
export default function debounce(value, delay) {
  const [keyword,setKeyword] = useState(value)

  useEffect(()=>{
    const timeout = setTimeout(()=> setKeyword(value),delay)
    return () => clearTimeout(timeout)
  },[value,delay])

  return keyword;
}

```
Whenever value changes (e.g., user types in an input):
- React runs this useEffect.
- Inside it, we start a new timer via setTimeout.
- That timer will, after delay ms, set debouncedValue to the latest value.

This cleanup runs before the next effect call.

So if the user types:
- "c" → effect runs → timer A starts
- "ch" → value changes → effect runs again
- cleanup clears timer A
- new timer B starts
- "chi" → cleanup clears timer B → timer C starts
- …and so on

---

## 14. Throttle

Throttle ensures that a function runs at most once in a given time interval, even if it’s triggered many times.
👉 If an event triggers the function repeatedly (like scroll, mousemove, resize), throttle limits execution frequency.

setInterval runs repeatedly on its own schedule, regardless of user actions.
Throttle runs only when events happen, but ensures controlled frequency.
So throttle reacts to user interaction, while setInterval runs independently — not tied to events.

Implement throttle
```js
function throttle(fn, limit) {
  let lastCall = 0;

  return function (...args) {
    const now = Date.now();

    if (now - lastCall >= limit) {
      lastCall = now;
      fn(...args);
    }
  };
}
```

Custom throttle hook in React
```js
import { useEffect, useState } from "react";

export function useThrottle(value, delay) {
  const [throttledValue, setThrottledValue] = useState(value);
  const [lastExecuted, setLastExecuted] = useState(Date.now());

  useEffect(() => {
    const now = Date.now();
    const timeRemaining = delay - (now - lastExecuted);

    if (timeRemaining <= 0) {
      // Enough time has passed → update immediately
      setThrottledValue(value);
      setLastExecuted(now);
    } else {
      // Wait the remaining time before updating
      const timeoutId = setTimeout(() => {
        setThrottledValue(value);
        setLastExecuted(Date.now());
      }, timeRemaining);

      return () => clearTimeout(timeoutId); // Cleanup
    }
  }, [value, delay, lastExecuted]);

  return throttledValue;
}

```
Flow:

- Every time value changes, the effect runs.
- It checks if enough time has passed since the last execution.
    - If yes:
      update immediately.
    - If no:
      schedule an update after the remaining time.
- Cleanup ensures pending updates don’t stack — just like throttle behavior demands.

---

## 15. Polyfill for map

```js

Array.prototype.map = (callbackFn,thisArg) => {// map takes callback(value,i,array) function as paramter
  const newArr = []
  //this here denotes to array on which map is applied
  for(let i=0;i<this.length;i++){
    if(i in this){
      newArr[i] = callbackFn.call(thisArg,this[i],i,this) // callback(currentValue,index,array)
    }
  }
  return newArr;//map return new array
}

```
so if we take example : [1,2,3].map(x => x * 2)

This is how polyfill will work :- 
- this = [1, 2, 3]
- callback = x => x * 2
- thisArg = undefined

So internally, callback is invoked as:
- callback.call(undefined, 1, 0, [1,2,3]) → 2
- callback.call(undefined, 2, 1, [1,2,3]) → 4
- callback.call(undefined, 3, 2, [1,2,3]) → 6

---

## 16. Polyfill for filter

```js
Array.prototype.filter = (callbackFn,thisArg) => {// filter takes callback(value,i,array) function as paramter
  const newArr = []
  //this here denotes to array on which filter is applied
  for(let i=0;i<this.length;i++){
    if(i in this){
      if(callbackFn.call(thisArg,this[i],i,this)){
        newArr.push(this[i]) //pushing only value which macthes condition of callback function
      } 
    }
  }
  return newArr;//filter also returns new array
}
```
---

## 17. Polyfill for reduce

```js
Array.prototype.reduce = (callback,initalValue) => {
  let acc;
  let startIndex = 0

  if(intialValue !== undefined){
    acc = intialValue
  }else{
    acc = this[0]
    startIndex = 1
  }

  for(let i=startIndex;i<this.length;i++){
    acc = callback(acc,this[i],i,this)
  }
  return acc
}
```
---

## 18. Flatten Array

[1, [2, 3], [4, [5, 6]]] -> [1, 2, 3, 4, 5, 6]

Ways of flattening the array 
  1. Array.prototype.flat()
     ```js
     const arr = [1, [2, 3], [4, [5, 6]]];
     console.log(arr.flat(1));
     // [1, 2, 3, 4, [5, 6]]
     console.log(arr.flat(2)); 
     // [1, 2, 3, 4, 5, 6]
     console.log(arr.flat(Infinity));//for all levels
     ```
  2. Recursive method
      ```js
      const flatten = (arr) => {
        let newArr=[]
        for(let x in arr){
          if(Array.isArray(x)){
            newArr = newArr.concat(flatten(x))
          }else{
            newArr.push(x)
          }
        }
        return newArr
      }
      ```
      - Working :-
        - flatten([1, [2, 3], [4, [5, 6]]])
          - 1 → not array → result = [1]
          - [2, 3] → is array → call flatten([2,3]) → returns [2,3]
            - result = [1].concat([2,3]) = [1,2,3]
          - [4, [5, 6]] → is array → call flatten([4, [5,6]])
            - Inside that:
              - 4 → not array → [4]
              - [5,6] → is array → flatten([5,6]) → [5,6]
              - returns [4,5,6]
            - outer result: [1,2,3].concat([4,5,6]) = [1,2,3,4,5,6]

  3. Using Array.reduce
     ```js
        const flattenReduce = (arr) =>
          arr.reduce((acc, item) => {
            if (Array.isArray(item)) {
              return acc.concat(flattenReduce(item));
            }
          return acc.concat(item);
        }, []);
     ```
  4. Using Stack(iteratively)
     ```js
      function flattenIterative(arr) {
        const stack = [...arr]; // clone original
        const result = [];

        while (stack.length) {
          const value = stack.pop();
          if (Array.isArray(value)) {
            stack.push(...value); // unpack inner array back into stack
          } else {
            result.push(value);   // collect value
          }
        }
        return result.reverse(); // fix order
      }
     ```
     - Working :-
        arr=[1, [2, 3], [4, [5, 6]]]
        - pop() → [4,[5,6]] (array)
        - → stack.push(4, [5,6])
        - stack: [1, [2,3], 4, [5,6]]
        - pop() → [5,6] (array)
        - → stack.push(5, 6)
        - stack: [1, [2,3], 4, 5, 6]
        - pop() → 6 (value)
        - → result = [6]
        - pop() → 5
        - → result = [6,5]
        - pop() → 4
        - → result = [6,5,4]
        - pop() → [2,3] (array)
        - → stack.push(2,3)
        - stack: [1, 2, 3]
        - pop() → 3 → result [6,5,4,3]
        - pop() → 2 → result [6,5,4,3,2]
        - pop() → 1 → result [6,5,4,3,2,1]
        - result.reverse() -> [1,2,3,4,5,6]

---

## 19. Polyfill for Flat() method
Flat takes depth as argument which tells how many level we have to flatten array
```js
Array.prototype.myFlat = (depth = 1) => {
  let result = []

  (function flat(arr,currDepth){
    for(const x of arr){
      if(Array.isArray(x) && currDepth > 0){
        flat(x,currDepth - 1)
      }else{
        result.push(x)
      }
    }
  })(this,depth)

  return result
}
```
---

## 20. Given an object make the keys as values and values as keys. Handle duplicates, assuming values are only strings
```js
const invertObj =(obj) => {
  const res = {}

  for(key in obj){
    const value = obj[key]

    if(!(value in res)){ //if value is not duplicated yet store key as string
      res[value] = key
    }else{
      if(typeof res[value] === "string"){//if value appears multiple times store keys in the array
        res[value] = [res[value]] // as existing value is string converted it to array 
      }
      res[value].push(key) // then new key is pushed
    }
  }
  return res
}
```

const obj = {          
  a: "x",
  b: "y",       
  c: "x",
};

res = {
  x: ["a", "c"],
  y: ["b"]
}

---

## 21. Currying Implementation

Currying transform the function add(a,b,c) into add(a)(b)(c) .It allows passing arguments one at a time and is useful in functional programming, partial application, and composing functions.

```js
function curry(fn){
  return function curried(...args){
    if(args.length>=fn.length){ //if we have all arguments then we can finally call original function fn
      return fn(...args)
    }
    return function(...nextArgs){ // if we dont have all arguments we will return another function until we get all arguments
      return curried(...args,nextArgs);//finally we call curried on all previous args and new ones also
    }
  }
}
```
For function where we dont have arguments number
```js
function infiniteCurry(fn) {
  return function curried(...args) {
    return function (...next) {
      if (next.length === 0) {
        return fn(...args);
      }
      return curried(...args, ...next);
    };
  };
}
```
In currying every call should return a new function until all required arguments are collected.

---

## 22. Promises Implementation

A Promise is a JavaScript object that represents a value that will be available in the future (or an error if something goes wrong).

A Promise has 3 states:
  - pending → initial state
  - fulfilled → operation completed successfully
  - rejected → operation failed
- .then() -> what to do after promise is fulfilled
- .catch() -> do this if promise is rejected

Promise polyfill
```js
class myPromise{
    constructor(executor){
        this.state = "pending" //current state
        this.value = undefined // resolved or rejected value
        this.handlers = [] // array of .then/.catch callbacks waiting to be run
        const resolve = (value) => { //this moves promise to fulfilled
            this.updateState("fullfilled",value)
        }

        const reject = (error) => { //this moves promise to rejected
            this.updateState("rejected",error)
        }
        try{  //try/catch → if executor throws synchronously, we treat it as a rejection.
             executor(resolve,reject)   //executor is the function you pass to new Promise((resolve, reject) => { ... })We call it immediately and give it resolve and reject.
        }catch(error){
            reject(error)
        }
    }

    updateState(newState,val){
        if(this.state !== "pending"){  //Promise can only settle once.If it’s already fulfilled / rejected, ignore further resolve/reject.
            return
        }

        this.state = newState // save new statet or value
        this.value = val

        this.runHandlers()//Once settled, process all .then/.catch callbacks that were queued.
    }

    runHandlers(){
        // setTimeout(..., 0) to make handler execution async
        setTimeout(()=>{
            while(this.handlers.length>0){
                const {onFullfilled,onRejected,resolve,reject} = this.handlers.shift()
                    // onFulfilled – .then success callback
                    // onRejected – .then error handler or .catch
                    // resolve / reject – are for the next promise in the chain.
                try{
                    // If the promise is fulfilled:
                    //     If user provided a success handler:
                    //             Call it → result may be:
                    //             a normal value → passed to next promise as resolved value
                    //             or a thrown error → caught below
                    //     If no handler → pass the value unchanged down the chain.
                    if(this.state == "fullfilled"){
                        if(typeof onFullfilled == "function"){
                            const result = onFullfilled(this.value)
                            resolve(result)
                        }else{
                            resolve(this.value)
                        }
                    }
                    // If the promise is rejected:
                    //     If an error handler exists:
                    //         Call it, and resolve the next promise with whatever it returns
                    //             (This is why .catch can “recover” and continue the chain)
                    //     If no error handler:
                    //         Propagate the error by rejecting the next promise.
                    //     If any handler throws → we reject the next promise with that error.
                    else if(this.state == "rejected"){
                         if(typeof onRejected == "function"){
                            const result = onRejected(this.value)
                            resolve(result)
                        }else{
                            reject(this.value)
                        }
                    }
                }catch(err){
                    reject(err)
                }
            }
        },0)
    }
    // .then() always returns a new promise (to allow chaining).
    // We push a handler object into this.handlers:
    //     onFulfilled, onRejected: user callbacks
    //     resolve, reject: functions to settle the next promise in chain.
    //  If the promise is already fulfilled/rejected at the time .then() is called:
    //     We run handlers immediately (asyncly) rather than waiting forever.
    then(onFullfilled,onRejected){
        return new myPromise((resolve,reject) => {
            this.handlers.push({
                onFullfilled,
                onRejected,
                resolve,
                reject,
            })
            if(this.state !== "pending"){
                this.runHandlers()
            }
        })
    }
    // .catch is just .then with only an error handler.
    // If the promise rejects anywhere in the chain, this handler will catch it.
    catch(onRejected) {
      return this.then(undefined, onRejected);
    }

    // Static helper: Promise.resolve
    static resolve(value) {  //Promise.resolve(value) → returns a promise that is already fulfilled with value.
      return new MyPromise((resolve) => resolve(value));
    }

    // Static helper: Promise.reject
    static reject(reason) { //Promise.reject(reason) → returns a promise that is already rejected with reason.
      return new MyPromise((_, reject) => reject(reason));
    }
}

const p = new myPromise((resolve, reject) => {
  console.log("Executor runs");

  setTimeout(() => {
    resolve("Step 1 done");
  }, 1000);
});

p.then((value) => {
  console.log("Then 1:", value);
  return value + " + Step 2";
})
  .then((value) => {
    console.log("Then 2:", value);
    throw new Error("Something broke in Step 3");
  })
  .catch((error) => {
    console.log("Caught:", error.message);
  });
```
Working on above code :-
- constructor runs:
  - state = "pending"
  - value = undefined
  - handlers = []

- executor(resolve, reject) is called:
  - Logs: "Executor runs"
  - Sets a timeout for 1s → then calls resolve("Step 1 done").

- then creates a new promise (call it p2).
- Adds a handler object to p.handlers:
- p is still pending, so runHandlers doesn’t run yet.

- After 1 second → resolve("Step 1 done")
   - updateState(FULFILLED, "Step 1 done"):
   - state becomes "fulfilled"
   - value becomes "Step 1 done"
   - calls runHandlers()

- Inside runHandlers:
  - Async via setTimeout(..., 0) → then:
  - Takes the first handler from p.handlers
  - Since p.state === "fulfilled":
    - Calls onFulfilled(this.value) -> logs -> Then 1: Step 1 done
    - Returns "Step 1 done + Step 2"
    - Calls resolve_of_p2("Step 1 done + Step 2")
      - So p2 becomes fulfilled with that value.

- then creates promise p3.
- Adds a handler in p2.handlers with onFulfilled being the second then callback.

- So p2.runHandlers() executes (async):
  - p2.state === "fulfilled":
    - onFulfilled(value) is called with "Step 1 done + Step 2"
      - Logs: "Then 2: Step 1 done + Step 2"
      - Then throws an error → caught by try...catch in runHandlers
    - That catch calls reject_of_p3(error) → so p3 becomes rejected.
  - .catch is then(undefined, onRejected), so:
  - Adds error handler to p3.handlers.

- Since p3 is already rejected:
  - p3.runHandlers() runs.
  - state === "rejected":
  - Runs onRejected(this.value):
    - Logs: "Caught: Something broke in Step 3"

- Final Output:
  - Executor runs
  - (after 1 second) Then 1: Step 1 done
  - Then 2: Step 1 done + Step 2
  - Caught: Something broke in Step 3

---

## 23. Promise.all and other methods
  1. Promise.all()

     Waits for all promises to fulfill, then returns an array of results.
     - If any promise rejects → the whole thing rejects immediately.

     Polyfill
      ```js
        Promise.myAll = function(promises){
          return new Promise((resolve,reject)=>{
            const res = []
            const comp = 0
            if(promise.length===0){
              return resolve([])
            }
            promises.forEach((promise,i)=>{
              Promise.resolve(promise)
                .then((value)=>{
                  res[i] = value
                  comp++
    
                  if(comp === promises.length){
                    resolve(res)
                  }
                })
                .catch((err)=>{
                  reject(err)
                })
            })
          })
        }
      ```

  3. Promise.allSettled()

      Waits for all promises to finish, regardless of success or failure and returns array of        objects.
      - (e.g., multiple API calls where you don't want one failure to stop others).

      Polyfill
      ```js
        Promise.myAllSettled = function (promises) {
          return new Promise((resolve) => {
            const results = [];
            let completed = 0;
    
            if (promises.length === 0) {
              return resolve([]);
            }
    
            promises.forEach((promise, index) => {
              Promise.resolve(promise)
                .then((value) => {
                  results[index] = { status: "fulfilled", value };
                })
                .catch((reason) => {
                  results[index] = { status: "rejected", reason };
                })
                .finally(() => {
                  completed++;
                  if (completed === promises.length) {
                    resolve(results);
                  }
                });
            });
          });
        };
    
      ```
  
  3. Promise.race()
  
      Returns a promise that resolves or rejects as soon as the FIRST promise settles.
      - If the first promise to finish is a rejection, it rejects.

      Polyfill
      ```js
        Promise.myRace = function (promises) {
          return new Promise((resolve, reject) => {
            // If empty array, NEVER resolve or reject (matches native behavior)
            if (promises.length === 0) return;
    
            promises.forEach((promise) => {
              // Convert non-promises into resolved promises
              Promise.resolve(promise)
                .then(resolve)   // first to resolve wins
                .catch(reject);  // first to reject wins
            });
          });
        };
    
      ```
  
  4. Promise.any()
     
     Waits for the first fulfilled promise.
     - Ignores rejections
     - If all promises reject → rejects with AggregateError
     Polyfill
      ```js
        Promise.myAny = function (promises) {
          return new Promise((resolve, reject) => {
            let errors = [];
            let rejectedCount = 0;
    
            // If empty array → reject with AggregateError (like native behavior)
            if (promises.length === 0) {
              return reject(new AggregateError([], "All promises were rejected"));
            }
    
            promises.forEach((promise, index) => {
              Promise.resolve(promise)
                .then((value) => {
                  // First fulfilled promise wins
                  resolve(value);
                })
                .catch((error) => {
                  errors[index] = error;
                  rejectedCount++;
    
                  // If ALL promises reject → reject with AggregateError
                  if (rejectedCount === promises.length) {
                    reject(new AggregateError(errors, "All promises were rejected"));
                  }
                });
            });
          });
        };
    
      ```
---

## 24. Implement setIntercal using setTimeout

setInterval does NOT wait for callback execution to finish.
If callback is slow → intervals overlap → causes bugs.

Therefore this recursive setTimeout approach is used in which:

- 👉 Next execution happens only after callback finishes.
- 👉 No overlapping.
- 👉 Perfect timing accuracy.
- 👉 Stop control is easy.

```js
function mySetInterval(callback, delay) {
  let timerId = null;

  function repeat() {// inner loop function which will run repeatively recursively
    callback(); //run actual function
    timerId = setTimeout(repeat, delay); //we manually schedule the next execution using setTimeout.
      // timeout → run callback → schedule another timeout → run callback → ...
      // This gives better control than real setInterval because
     // it waits for callback to finish before scheduling next call.
  }

  timerId = setTimeout(repeat, delay);// starting first timeout which kick offs the loop

  return { //We return an object that allows stopping the interval manually.
    clear: () => clearTimeout(timerId),
  };
}

let count = 0;

const runner = mySetInterval(() => {
  console.log("Count:", ++count);
}, 1000);

setTimeout(() => {
  runner.clear();
  console.log("Stopped!");
}, 5000);

//Output:
// Count: 1
// Count: 2
// Count: 3
// Count: 4
// Stopped!
```
---

## 25. Call,Apply and Bind
  1. call()
  - call() is a built-in method available on every JavaScript function.
  - It allows you to manually set the value of this and invoke the function immediately.
    1. Borrowing methods between objects

      You can use one object’s method on another object.
    2. Controlling this inside a function

      Helps when the function is not inside an object, or context is lost.
    3. Using constructor functions on existing objects
    
      Allows reusing initialization logic.
    4. Inheritance / Polymorphism
    
      Used to implement classical or functional inheritance.
    5. Avoiding code duplication
    
      E.g., borrow array methods for array-like objects.

   Polyfill
   ```js
    Function.prototype.myCall = function (context,...args){
      if(typeof this !== 'function'){
        throw new error(this + "mycall must be called on function")
      }

      context.fn = this
      context.fn(...args)
    }
   ```
  2. apply()
  - apply() is similar to call(), but the difference is in how you pass arguments.
    Use apply() when:
      - You already have arguments in an array
      - You want to operate on array-like objects
      - You want to call a function with a custom this and array arguments
  Polyfill
  ```js
    Function.prototype.myApply = function (context,args=[]){
      if(typeof this !== 'function'){
        throw new error(this + "mycall must be called on function")
      }

      if(!Array.isArray(args)){
        throw new TypeError("args should be array")
      }

      context.fn = this
      context.fn(...args)
    }
  ```
  3. bind()
  - bind() creates a new function with:
    - A fixed this context
    - Optional pre-set (partial) arguments
    - Does NOT execute immediately (unlike call and apply)
  - Use cases:
      1. Fixing this inside callback functions
      ```js
      const user = {
        name: "Chirag",
        greet() {
          setTimeout(function () {
            console.log("Hello " + this.name);
          }.bind(this), 1000);
        }
      };
        user.greet();
        // Hello Chirag
      ```
      2. Borrowing methods with fixed context
      ```js
      const person = {
        name: "Chirag",
        print() {
          console.log(this.name);
        }
      };
      const printer = person.print.bind({ name: "Rahul" });
      printer();  // Rahul
      ```
      3. Partial function application (Currying-like behavior)
      ```js
      function multiply(a, b) {
        return a * b;
      }
      const double = multiply.bind(null, 2);
      console.log(double(10)); // 20
      const triple = multiply.bind(null, 3);
      console.log(triple(10)); // 30
      ```
      4. Event handlers in JavaScript
      ```js
      const obj = {
        name: "Chirag",
        handleClick() {
          console.log(this.name);
        }
      };
      document.getElementById("btn")
        .addEventListener("click", obj.handleClick.bind(obj));
      ```
      5. Reuse class methods while preserving context
      ```js
      class User {
        constructor(name) {
          this.name = name;
        }

        say() {
          console.log("Hi " + this.name);
        }
      }
      const u = new User("Chirag");
      const fn = u.say.bind(u);
      fn(); // Hi Chirag

      ```
      6. In React
      ```js
      class App extends React.Component {
        constructor() {
          super();
          this.handleClick = this.handleClick.bind(this);
        }

        handleClick() {
          console.log("Clicked", this);
        }

        render() {
          return <button onClick={this.handleClick}>Click</button>;
        }
      }
      ```
      7. Prevent losing this when passing functions as references
      ```js
      const obj = {
        x: 100,
        getX() {
          return this.x;
        }
      };

      const getX = obj.getX;
      console.log(getX()); // undefined

      const boundGetX = obj.getX.bind(obj);
      console.log(boundGetX()); // 100

      ```
  Polyfill
  ```js
    Function.prototype.myBind = function (context,...args){
      if(typeof this !== 'function'){
        throw new error(this + "mycall must be called on function")
      }

      context.fn = this
      return function(...newArgs){
        return context.fn(...args,...newArgs)
      }
    }
  ```
---

## 26. Memoize function 
Memoization in JavaScript is a performance optimization technique where you store (cache) the result of a function call so that if the same inputs occur again, you return the cached result instead of recomputing.

```js
function myMemoize(fn){
  const res = {} // res will look like this res={'5,6':30}
  return function(...args){
    var cacheArgs = JSON.stringify(args)
    if(!res[cacheArgs]){
      res[cacheArgs] = fn.call(this,...args)
    }
    return res[cacheArgs]
  }
}
```
