# 🎓 JavaScript Synchronous Code

# 📚 What is Synchronous Code?

By default, JavaScript executes code **synchronously**.

This means that JavaScript executes **one statement at a time**, in the exact order they appear in the program.

Each line of code must finish executing before JavaScript moves to the next line.

Think of it as standing in a queue:

- The first person is served.
- Only after they finish does the second person get served.
- Then the third person, and so on.

The next task **cannot begin** until the current task has completed.

---

# How Synchronous Execution Works

When JavaScript runs a program, it follows these steps:

1. Execute the first statement.
2. Wait until it finishes.
3. Move to the next statement.
4. Continue until the program ends.

Execution Order

```text
Start
   │
   ▼
Statement 1
   │
   ▼
Statement 2
   │
   ▼
Statement 3
   │
   ▼
Statement 4
   │
   ▼
 End
```

Every statement waits for the previous one to complete.

---

# Characteristics of Synchronous Code

- Code executes line by line.
- Statements execute in order.
- Only one statement runs at a time.
- The next statement waits until the current one finishes.
- Easy to understand because the execution order is predictable.

---

# Example 1: Sequential Execution

```javascript
console.log("1: Start");
console.log("2: Processing");
console.log("3: End");
```

### Output

```text
1: Start
2: Processing
3: End
```


# Example 2: Calling a Function

```javascript
function sayHi() {
  console.log("2: Inside Function");
}

console.log("1: Before calling function");
sayHi();
console.log("3: After calling function");
```

### Output

```text
1: Before calling function
2: Inside Function
3: After calling function
```


# Example 3: Synchronous Loops

```javascript
console.log("Start Loop");

for (let i = 1; i <= 3; i++) {
  console.log("Loop number: " + i);
}

console.log("End Loop");
```

### Output

```text
Start Loop
Loop number: 1
Loop number: 2
Loop number: 3
End Loop
```


# Blocking Behavior

One limitation of synchronous code is that **long-running operations block the execution of the rest of the program**.

While JavaScript is busy executing one task, it cannot execute anything else.

This is called **blocking**.

---

# Example 4: Blocking Code

```javascript
console.log("Start");

alert("This alert blocks the program");

console.log("End");
```

### What Happens?

1. JavaScript prints `"Start"`.
2. The `alert()` dialog appears.
3. The program pauses completely.
4. No other code runs until the user closes the alert.
5. After closing the alert, JavaScript prints `"End"`.

### Output

```text
Start
(Alert appears and waits)
End
```

Execution Flow

```text
Print "Start"
      │
      ▼
 Show Alert
      │
 Wait for User
      │
      ▼
Close Alert
      │
      ▼
Print "End"
```

---



# Why Can Blocking Be a Problem?

Suppose a web page needs to:

- Load data from a server.
- Wait for a user to click a button.
- Download an image.

If JavaScript waited synchronously for each task to finish, the page would become unresponsive.

For this reason, JavaScript also supports **asynchronous programming**, which allows long-running tasks to happen without blocking the rest of the program.

---

# Synchronous vs Blocking

Synchronous code is usually blocking because:

- One task executes at a time.
- The next task must wait.
- Long operations delay everything that comes after them.

---

# Real-Life Analogy

Imagine you are at a supermarket with only one cashier.

```text
Customer 1
      │
      ▼
Customer 2 waits
      │
      ▼
Customer 3 waits
      │
      ▼
Customer 4 waits
```

Each customer must wait until the previous customer finishes paying.

This is exactly how synchronous code works.

---

# Summary

## Synchronous Code

- Executes one statement at a time.
- Runs from top to bottom.
- Each statement waits for the previous one to finish.
- Function calls execute immediately when reached.
- Loops complete before the next statement executes.
- Long-running operations block the rest of the program.

---
