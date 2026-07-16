# 🎓 JavaScript Runtime & Event Loop

# 📚 Introduction

JavaScript is a **single-threaded programming language**.

This means that JavaScript can execute **only one task at a time**.

For example, if one function is currently running, JavaScript cannot execute another function until the current one finishes.

Although JavaScript is single-threaded, modern web applications still perform many tasks simultaneously, such as:

* Running timers
* Sending API requests
* Handling user clicks
* Reading files
* Loading images

JavaScript achieves this by working together with its **Runtime Environment**.

The runtime provides additional components that allow asynchronous operations to work without blocking the execution of JavaScript code.

The JavaScript Runtime consists of four main parts:

```text
Call Stack
Web APIs
Task Queues
Event Loop
```

---

# JavaScript Runtime Architecture

The following diagram shows how these components work together.

```text
               JavaScript Runtime

           ┌─────────────────────┐
           │     Call Stack      │
           └─────────┬───────────┘
                     │
                     ▼
              Executes Code
                     │
                     ▼
           ┌─────────────────────┐
           │      Web APIs       │
           └─────────┬───────────┘
                     │
      Finished Asynchronous Task
                     │
                     ▼
          ┌──────────────────────┐
          │   Task Queues        │
          │ Microtask Queue      │
          │ Macrotask Queue      │
          └─────────┬────────────┘
                    │
                    ▼
             Event Loop
                    │
                    ▼
              Back to Call Stack
```

---

# 1. Call Stack

## What is the Call Stack?

The **Call Stack** is the part of the JavaScript engine responsible for executing JavaScript code.

Whenever a function is called:

* It is pushed onto the stack.
* JavaScript executes its code.
* When it finishes, it is removed (popped) from the stack.

Since JavaScript is single-threaded, **only one function can execute on the Call Stack at any given moment**.

---

## How the Call Stack Works

```text
Function Called
       │
       ▼
Push onto Stack
       │
       ▼
Execute Function
       │
       ▼
Function Finished
       │
       ▼
Remove from Stack
```

---

## Example

```javascript
function first() {
  second();
}

function second() {
  console.log("Hello");
}

first();
```

---

### Execution

Initially, the stack is empty.

```text
Call Stack

Empty
```

---

When `first()` is called:

```text
Call Stack

first()
```

---

Inside `first()`, JavaScript calls `second()`.

```text
Call Stack

second()
first()
```

---

Inside `second()`, JavaScript executes:

```javascript
console.log("Hello");
```

```text
Call Stack

console.log()
second()
first()
```

---

After `console.log()` finishes:

```text
Call Stack

second()
first()
```

---

After `second()` finishes:

```text
Call Stack

first()
```

---

Finally:

```text
Call Stack

Empty
```

Output

```text
Hello
```

---

# 2. The Problem

Since JavaScript can execute only one task at a time, what happens when it encounters a task that takes several seconds?

Consider this example:

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Done");
}, 3000);

console.log("End");
```

Output

```text
Start
End
Done
```

A common question is:

> Why didn't JavaScript wait three seconds before printing "End"?

If JavaScript waited for every timer or network request to finish, the entire application would freeze.

Instead, JavaScript delegates long-running operations to another part of the runtime.

---

# 3. Web APIs

## What are Web APIs?

Web APIs are features provided by the browser (or the runtime environment), not by the JavaScript language itself.

Some common Web APIs include:

* `setTimeout()`
* `setInterval()`
* `fetch()`
* DOM Events
* Geolocation
* Local Storage

When JavaScript encounters one of these APIs, it asks the browser to handle the task.

This allows JavaScript to continue executing the remaining code.

---

## Example

```javascript
setTimeout(() => {
  console.log("Done");
}, 3000);
```

Execution

```text
Call Stack
      │
      ▼
Encounter setTimeout()
      │
      ▼
Browser starts timer
      │
      ▼
Call Stack becomes free
```

After three seconds, the callback becomes ready for execution.

It does **not** execute immediately.

Instead, it is placed inside a task queue.

---

# 4. Task Queues

When asynchronous operations finish, their callback functions wait inside special queues until JavaScript is ready to execute them.

JavaScript mainly uses two queues:

```text
Microtask Queue
Macrotask Queue
```

The Event Loop decides when callbacks move from these queues to the Call Stack.

---

# 5. Macrotask Queue

The **Macrotask Queue** stores callbacks from operations such as:

* `setTimeout()`
* `setInterval()`
* DOM Events
* `MessageChannel`

Example:

```javascript
setTimeout(() => {
  console.log("Timer");
}, 0);
```

Even though the delay is `0`, the callback is **not executed immediately**.

It first waits inside the Macrotask Queue until the Call Stack becomes empty.

---

# 6. Microtask Queue

The **Microtask Queue** stores callbacks created by:

* `Promise.then()`
* `Promise.catch()`
* `Promise.finally()`
* `queueMicrotask()`

Example:

```javascript
Promise.resolve().then(() => {
  console.log("Promise");
});
```

After the Promise is resolved, its callback is placed inside the Microtask Queue.

---

# 7. Event Loop

## What is the Event Loop?

The **Event Loop** is responsible for coordinating asynchronous execution.

It continuously checks:

> Is the Call Stack empty?

If the Call Stack is not empty:

* Wait.

If the Call Stack is empty:

* Execute waiting tasks.

Without the Event Loop, asynchronous callbacks would never execute.

---

## Event Loop Workflow

```text
Call Stack Empty?
        │
   ┌────┴─────┐
   │          │
 No         Yes
   │          │
Wait      Execute
          Microtasks
              │
              ▼
      Execute One Macrotask
              │
              ▼
         Repeat Forever
```

---

# 8. Queue Priority

Microtasks always have **higher priority** than Macrotasks.

The Event Loop follows this order:

```text
1. Execute Call Stack

2. Execute ALL Microtasks

3. Execute ONE Macrotask

4. Execute ALL New Microtasks

5. Repeat
```

This rule explains why Promise callbacks execute before `setTimeout()` callbacks.

---

# Example 1

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timer");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise");
});

console.log("End");
```

Output

```text
Start
End
Promise
Timer
```

Explanation

* `"Start"` prints immediately.
* `setTimeout()` registers its callback.
* Promise callback enters the Microtask Queue.
* `"End"` prints.
* Call Stack becomes empty.
* Event Loop executes all Microtasks.
* Finally, the timer callback executes.

---

# Example 2

```javascript
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

console.log("C");
```

Output

```text
A
C
B
```

The timer callback waits until the synchronous code finishes.

---

# Example 3

```javascript
console.log(1);

Promise.resolve().then(() => {
  console.log(2);
});

Promise.resolve().then(() => {
  console.log(3);
});

setTimeout(() => {
  console.log(4);
}, 0);

console.log(5);
```

Output

```text
1
5
2
3
4
```

Explanation

* Synchronous code executes first.
* Promise callbacks execute next.
* Timer callback executes last.

---

# Example 4

```javascript
setTimeout(() => {
  console.log("Timer 1");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise 1");
});

setTimeout(() => {
  console.log("Timer 2");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise 2");
});
```

Output

```text
Promise 1
Promise 2
Timer 1
Timer 2
```

Both Promise callbacks execute before any timer because Microtasks always have higher priority.

---

# Example 5

```javascript
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

Promise.resolve().then(() => {
  console.log("C");
});

console.log("D");
```

Output

```text
A
D
C
B
```

Execution Order

1. Execute synchronous code.
2. Execute Promise callback.
3. Execute timer callback.

---

# Example 6

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timer");
}, 0);

Promise.resolve()
  .then(() => {
    console.log("Promise 1");
  })
  .then(() => {
    console.log("Promise 2");
  });

console.log("End");
```

Output

```text
Start
End
Promise 1
Promise 2
Timer
```

Each `.then()` callback is a Microtask, so they both execute before the timer.

---

# Example 7: Nested Microtasks

```javascript
console.log("A");

Promise.resolve().then(() => {
  console.log("B");

  Promise.resolve().then(() => {
    console.log("C");
  });
});

setTimeout(() => {
  console.log("D");
}, 0);
```

Output

```text
A
B
C
D
```

Explanation

While executing `"B"`, another Promise callback is created.

The Event Loop must finish **all Microtasks** before moving to the Macrotask Queue.

---

# Complete Execution Order

```text
Synchronous Code
        │
        ▼
Call Stack Empty
        │
        ▼
Execute ALL Microtasks
        │
        ▼
Execute ONE Macrotask
        │
        ▼
Repeat
```

---

# Summary

## Call Stack

```text
Executes JavaScript code.
One function executes at a time.
```

---

## Web APIs

```text
setTimeout()
setInterval()
fetch()
DOM Events
```

These operations are handled outside the Call Stack.

---

## Microtask Queue

```text
Promise.then()
Promise.catch()
Promise.finally()
queueMicrotask()
```

Higher Priority ✅

---

## Macrotask Queue

```text
setTimeout()
setInterval()
DOM Events
```

Lower Priority ✅

---

## Event Loop

The Event Loop continuously checks whether the Call Stack is empty.

When it is empty, it moves callbacks from the queues to the Call Stack.

---

# Important Rules

* JavaScript is single-threaded.
* Only one task executes on the Call Stack at a time.
* Long-running tasks are handled by Web APIs.
* Asynchronous callbacks wait inside task queues.
* The Event Loop coordinates the execution of asynchronous tasks.
* **Microtasks always execute before Macrotasks.**
