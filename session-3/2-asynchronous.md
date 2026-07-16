# 🎓 JavaScript Asynchronous Programming

# 📚 What is Asynchronous Programming?

By default, JavaScript executes code **synchronously**, meaning one statement runs after another.

However, some operations take time to complete, such as:

* Downloading data from a server
* Reading a file
* Waiting for a timer
* Waiting for a user to click a button

If JavaScript waited for these operations to finish before continuing, the entire program would become unresponsive.

To solve this problem, JavaScript supports **asynchronous programming**.

Asynchronous code allows long-running tasks to execute **in the background** while JavaScript continues executing the remaining code.

When the asynchronous task finishes, JavaScript executes a **callback function** (or another completion handler).

---

# Characteristics of Asynchronous Code

* Runs in the background.
* Does not block the rest of the program.
* Allows JavaScript to continue executing other code.
* Executes a callback function after the task finishes.
* Keeps applications responsive.

---

# Why Do We Need Asynchronous Programming?

Imagine opening a website that needs to download user information from a server.

If JavaScript waited synchronously:

```text
Request Data
      │
Wait...
      │
Wait...
      │
Wait...
      │
Show Page
```

The page would freeze until the request finished.

With asynchronous programming:

```text
Request Data
      │
      ├──────────────► Download in Background
      │
Continue Running Other Code
      │
      ▼
When Download Finishes
      │
      ▼
Run Callback Function
```

The page remains responsive while the data is loading.

---

# Real-World Examples of Asynchronous Operations

Many common programming tasks are asynchronous, including:

* API Requests
* Database Queries
* Reading Files
* Timers
* User Events (clicks, keyboard input, mouse movement)

These tasks may take an unknown amount of time, so JavaScript lets them run in the background.

---

# Callback Functions

A callback function is a function that is passed to another function and is executed **after a task has completed**.

For example, `setTimeout()` receives a callback function.

```javascript
setTimeout(function () {
  console.log("Finished");
}, 2000);
```

The callback is not executed immediately.

Instead:

1. JavaScript starts the timer.
2. Continues executing the remaining code.
3. Executes the callback after the timer finishes.

---

# Example 1: Updating the Page After a Delay

```javascript
const p = document.querySelector(".p");

setTimeout(function () {
  p.textContent = "My name is Ahmed";
}, 5000);

p.style.color = "red";
```

### What Happens?

JavaScript executes the code in this order:

1. Select the paragraph.
2. Start a 5-second timer.
3. Continue immediately to the next line.
4. Change the paragraph color to red.
5. After 5 seconds, change the paragraph text.

### Execution Order

```text
Select Paragraph
        │
        ▼
Start Timer (5 seconds)
        │
        ▼
Change Text Color to Red
        │
        ▼
Wait 5 Seconds
        │
        ▼
Execute Callback
        │
        ▼
Change Paragraph Text
```

Notice that JavaScript does **not** wait five seconds before changing the text color.

The timer runs in the background.

---

# setTimeout()

`setTimeout()` is one of the most common asynchronous functions in JavaScript.

### Syntax

```javascript
setTimeout(callbackFunction, delayInMilliseconds);
```

Parameters:

* **callbackFunction** → The function to execute later.
* **delayInMilliseconds** → The amount of time to wait before executing the callback.

Example:

```javascript
setTimeout(() => {
  console.log("Hello");
}, 3000);
```

The callback executes after approximately **3 seconds**.

---

# Example 2: JavaScript Does Not Wait

```javascript
console.log("Hello 1");

setTimeout(() => {
  console.log("Hello from setTimeout");
}, 2000);

console.log("Hello 2");
```

### Output

```text
Hello 1
Hello 2
Hello from setTimeout
```

### Why?

When JavaScript reaches `setTimeout()`:

* It starts the timer.
* It does **not** wait.
* It immediately continues executing the next statement.

Execution Flow

```text
Print Hello 1
       │
       ▼
Start Timer
       │
       ▼
Print Hello 2
       │
       ▼
Wait 2 Seconds
       │
       ▼
Execute Callback
```

This demonstrates that asynchronous code is **non-blocking**.

---

# Example 3: Multiple Timers with the Same Delay

```javascript
console.log("Hello 1");

setTimeout(() => {
  console.log("Hello from setTimeout 1");
}, 2000);

setTimeout(() => {
  console.log("Hello from setTimeout 2");
}, 2000);

console.log("Hello 2");
```

### Output

```text
Hello 1
Hello 2
Hello from setTimeout 1
Hello from setTimeout 2
```

### Explanation

Both timers start almost at the same time.

After approximately two seconds:

* The first callback executes.
* Then the second callback executes.

Execution Flow

```text
Print Hello 1
        │
        ▼
Start Timer 1
        │
        ▼
Start Timer 2
        │
        ▼
Print Hello 2
        │
        ▼
Wait 2 Seconds
        │
        ▼
Run Callback 1
        │
        ▼
Run Callback 2
```

Since both timers have the same delay, the callback that was registered first executes first.

---

# Example 4: Different Timer Delays

```javascript
console.log("Hello 1");

setTimeout(() => {
  console.log("Hello from setTimeout 1");
}, 3000);

setTimeout(() => {
  console.log("Hello from setTimeout 2");
}, 2000);

console.log("Hello 2");
```

### Output

```text
Hello 1
Hello 2
Hello from setTimeout 2
Hello from setTimeout 1
```

### Explanation

Although the first timer was created first, it has a longer delay.

Timer 2 finishes first because it only waits two seconds.

Execution Flow

```text
Print Hello 1
        │
        ▼
Start Timer 1 (3 sec)
        │
        ▼
Start Timer 2 (2 sec)
        │
        ▼
Print Hello 2
        │
        ▼
After 2 Seconds
        │
        ▼
Run Callback 2
        │
        ▼
After 3 Seconds
        │
        ▼
Run Callback 1
```

The callback whose timer finishes first is executed first.

---

# Synchronous vs Asynchronous

| Synchronous                             | Asynchronous                                 |
| --------------------------------------- | -------------------------------------------- |
| Executes one statement at a time        | Long-running tasks execute in the background |
| Blocks the next statement               | Does not block the next statement            |
| Code waits for each operation to finish | JavaScript continues executing other code    |
| Easier to predict                       | More suitable for long-running operations    |

---

# Real-Life Analogy

Imagine ordering food at a restaurant.

### Synchronous

```text
Order Food
      │
Wait Until Finished
      │
Eat
      │
Leave
```

You cannot do anything while waiting.

---

### Asynchronous

```text
Order Food
      │
      ▼
Kitchen Starts Cooking
      │
      ▼
Talk with Friends
      │
      ▼
Food Ready
      │
      ▼
Eat
```

The kitchen prepares the food in the background while you continue doing other things.

This is similar to how asynchronous programming works.

---

# Summary

## Asynchronous Programming

* Runs long tasks in the background.
* Prevents the program from freezing.
* Continues executing the remaining code immediately.
* Executes a callback function when the task finishes.
* Keeps applications responsive.

---

# Common Asynchronous Operations

```text
setTimeout()
API Requests
Database Queries
File Reading
User Events
```

---

# Key Points

* Asynchronous code is **non-blocking**.
* JavaScript continues executing the remaining code without waiting.
* Callback functions execute after asynchronous tasks finish.
* `setTimeout()` schedules code to run after a specified delay.
* Multiple asynchronous tasks can run at the same time.
* The callback whose task finishes first executes first.
