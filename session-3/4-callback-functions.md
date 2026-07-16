# 🎓 JavaScript Callback Functions

## 📚 What is a Callback Function?

A **callback function** is a function that is passed as an argument to another function and is executed later.

Since functions are **first-class citizens** in JavaScript, they can be:

- Stored in variables
- Passed as arguments
- Returned from other functions

Callbacks are one of the most common ways to use functions.

### General Syntax

```javascript
function doSomething(callback) {
  // Perform some work

  callback();
}
```

Example:

```javascript
function greet(name) {
  console.log(`Hello ${name}`);
}

function processUser(callback) {
  const username = "Esraa";

  callback(username);
}

processUser(greet);
```

Output

```text
Hello Esraa
```

---

# Callback Functions are NOT Always Asynchronous

Many beginners think callbacks automatically mean asynchronous code.

This is **not true**.

A callback is simply **a function passed into another function**.

It can be executed immediately (synchronously) or later (asynchronously).

---

# Callback in Synchronous Functions

When a callback is used in synchronous code, it executes immediately before the function finishes.

## Example 1

```javascript
function login(username, password, callback) {
  console.log(`Logging in with username: ${username}`);

  callback(username, password, showWelcome);
}

function checkPassword(username, password, callback) {
  if (username === "esraa" && password === "12345") {
    callback(username);
  } else {
    console.log("Invalid username or password.");
  }
}

function showWelcome(username) {
  console.log(`Welcome ${username}!`);
}

login("esraa", "12345", checkPassword);
```

Output

```text
Logging in with username: esraa
Welcome esraa!
```

---

## Example 2

Callbacks are commonly used by array methods.

```javascript
function processNumbers(callback) {
  const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

  return callback(numbers);
}

console.log(processNumbers((numbers) => numbers.filter((num) => num > 5)));

console.log(
  processNumbers((numbers) => numbers.filter((num) => num % 2 === 0)),
);
```

Output

```text
[6,7,8,9,10]
[2,4,6,8,10]
```

---

## Built-in Callback Example

Methods like `filter()`, `map()`, and `forEach()` all receive callback functions.

```javascript
const numbers = [5, 3, 1];

const result = numbers.filter((number) => number >= 3);

console.log(result);
```

Output

```text
[5,3]
```

The callback

```javascript
(number) => number >= 3;
```

is executed once for every element in the array.

---

# Callback in Asynchronous Functions

Callbacks become much more useful when working with asynchronous operations.

Examples include:

- Timers
- API Requests
- Reading files
- Database queries

Unlike synchronous callbacks, asynchronous callbacks are executed **after another task finishes**.

---

## Example 1

```javascript
console.log("Hello 1");

setTimeout(() => {
  console.log("Hello from setTimeout");
}, 2000);

console.log("Hello 2");
```

Output

```text
Hello 1
Hello 2
Hello from setTimeout
```

### Why?

`setTimeout()` starts a timer.

JavaScript continues executing the remaining code while waiting for the timer to finish.

---

## Multiple Timers

```javascript
console.log("Hello 1");

setTimeout(() => {
  console.log("Timer 1");
}, 2000);

setTimeout(() => {
  console.log("Timer 2");
}, 2000);

console.log("Hello 2");
```

Output

```text
Hello 1
Hello 2
Timer 1
Timer 2
```

Both timers finish after approximately two seconds.

Since they were registered in the same order, their callbacks execute in that order.

---

## Different Delay Times

```javascript
console.log("Hello 1");

setTimeout(() => {
  console.log("Timer 1");
}, 3000);

setTimeout(() => {
  console.log("Timer 2");
}, 2000);

console.log("Hello 2");
```

Output

```text
Hello 1
Hello 2
Timer 2
Timer 1
```

The timer with the shorter delay finishes first.

---

# The Problem with Asynchronous Code

Suppose retrieving users takes one second.

You might write:

```javascript
function getUsers() {
  let users = [];

  setTimeout(() => {
    users = [
      { name: "user1", salary: 4000 },
      { name: "user2", salary: 5000 },
      { name: "user3", salary: 6000 },
    ];
  }, 1000);

  return users;
}

console.log(getUsers());
```

Output

```text
[]
```

### Why?

The function returns immediately.

The timer has **not finished yet**, so `users` is still empty.

---

# First Attempt (Not a Real Solution)

```javascript
function getUsers() {
  setTimeout(() => {
    const users = [
      { name: "user1", salary: 4000 },
      { name: "user2", salary: 5000 },
      { name: "user3", salary: 6000 },
    ];

    console.log(users);
  }, 1000);
}

getUsers();
```

This prints the users correctly.

However, the data cannot be used outside the function.

---

# Using a Callback (Correct Solution)

Instead of returning the data immediately, pass it to a callback after it becomes available.

```javascript
function getUsers(callback) {
  setTimeout(() => {
    const users = [
      { name: "user1", salary: 4000 },
      { name: "user2", salary: 5000 },
      { name: "user3", salary: 6000 },
    ];

    callback(users);
  }, 1000);
}

getUsers(console.log);
```

Output

```text
[
 { name: "user1", salary: 4000 },
 { name: "user2", salary: 5000 },
 { name: "user3", salary: 6000 }
]
```

Now the callback receives the data **after** it is ready.

---

# Working with the Returned Data

Instead of printing the users, we can process them.

```javascript
function getUsers(callback) {
  setTimeout(() => {
    const users = [
      { name: "user1", salary: 4000 },
      { name: "user2", salary: 5000 },
      { name: "user3", salary: 6000 },
    ];

    callback(users);
  }, 1000);
}

function getUserByName(name) {
  getUsers((users) => {
    const user = users.find((u) => u.name === name);

    console.log(user);
  });
}

getUserByName("user2");
```

Output

```text
{ name: "user2", salary: 5000 }
```

---

# Nested Callbacks

Sometimes one asynchronous task depends on another.

For example:

1. Get all users.
2. Find one user.
3. Get that user's salary.

```javascript
function getUsers(callback) {
  setTimeout(() => {
    const users = [
      { name: "user1", salary: 4000 },
      { name: "user2", salary: 5000 },
      { name: "user3", salary: 6000 },
    ];
    callback(users);
  }, 1000);
}

function getUserByName(name, users, callback) {
  setTimeout(() => {
    callback(users.find((user) => user.name === name));
  }, 1000);
}

function getSalary(user, callback) {
  setTimeout(() => {
    callback(user.salary);
  }, 500);
}

getUsers((users) => {
  console.log(users);

  getUserByName("user2", users, (user) => {
    console.log(user);

    getSalary(user, (salary) => {
      console.log(salary);
    });
  });
});
```

Output

```text
All users
Selected user
5000
```

---

# Callback Hell

As applications grow, callbacks become deeply nested.

```javascript
makeDough(() => {
  addToppings(() => {
    bakePizza(() => {
      console.log("Pizza is ready!");
    });
  });
});
```

This pyramid shape is called **Callback Hell**.

Problems include:

- Difficult to read
- Hard to debug
- Hard to maintain
- Difficult to handle errors

---

## Real Example

```javascript
function makeDough(callback) {
  setTimeout(() => {
    console.log("Step 1: Dough is ready");

    callback();
  }, 1000);
}

function addToppings(callback) {
  setTimeout(() => {
    console.log("Step 2: Toppings added");

    callback();
  }, 1000);
}

function bakePizza(callback) {
  setTimeout(() => {
    console.log("Step 3: Pizza baked");

    callback();
  }, 1000);
}

makeDough(() => {
  addToppings(() => {
    bakePizza(() => {
      console.log("Pizza is ready to serve!");
    });
  });
});
```

Output

```text
Step 1: Dough is ready
Step 2: Toppings added
Step 3: Pizza baked
Pizza is ready to serve!
```

---

# Why Did JavaScript Move Beyond Callbacks?

Callbacks solve asynchronous problems, but large applications become difficult to manage because of nested callbacks.

To make asynchronous code cleaner and easier to maintain, JavaScript introduced:

1. **Promises**
2. **Async/Await**

These provide a more readable way to write asynchronous code while avoiding Callback Hell.

---

# Summary

### Callback Function

- A function passed as an argument to another function.
- Can be synchronous or asynchronous.

### Synchronous Callback

- Executes immediately.
- Common in methods like `filter()`, `map()`, and `forEach()`.

### Asynchronous Callback

- Executes after an asynchronous operation completes.
- Common with `setTimeout()`, API requests, and file operations.

### Callback Hell

- Happens when callbacks become deeply nested.
- Makes code difficult to read and maintain.
- Solved using **Promises** and **Async/Await**.
