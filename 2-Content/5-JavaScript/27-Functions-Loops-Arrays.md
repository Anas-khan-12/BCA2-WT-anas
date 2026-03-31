# Day 27 — JavaScript Functions, Loops & Arrays

🔬 **Type:** Theory + Practical
🕐 **Duration:** 2 Hours
📚 **Unit:** 5 — JavaScript
🧪 **Lab Experiment:** 18

---

## Learning Objectives

By the end of this session, students will be able to:
- Write reusable functions (declaration, expression, arrow)
- Use for, while, and for...of loops
- Work with arrays and common array methods
- Solve real-world problems using loops and functions

---

## 1. Functions

> **Analogy:** A function is like a **vending machine** — you give it input (money + selection), it processes internally, and returns output (the snack). You don't need to know how it works inside — just what goes in and what comes out.

### Function Declaration

```javascript
// Function declaration — named function, hoisted (can be called before this line)
function greet(name) {
    return `Hello, ${name}!`;   // Template literal: backticks with ${variable}
}

console.log(greet("Rahul"));   // "Hello, Rahul!"
```

> **Code Explanation:**
> - `function greet(name)` declares a function named `greet` that takes one **parameter** called `name`
> - `return` sends back a value to whoever called the function — here it returns a greeting string
> - `` `Hello, ${name}!` `` is a **template literal** — it uses backticks (`` ` ``) instead of quotes, and `${name}` inserts the variable's value into the string
> - `greet("Rahul")` calls the function with `"Rahul"` as the **argument** — the parameter `name` receives this value
> - Function declarations are **hoisted** — JavaScript moves them to the top internally, so you can call `greet()` even before the line where it's defined

### Function Expression

```javascript
// Anonymous function (no name) stored in a variable
const add = function(a, b) {
    return a + b;
};

console.log(add(5, 3));   // 8
```

> **Code Explanation:**
> - `const add = function(a, b) { ... };` stores an **anonymous function** (a function without a name) inside a variable called `add`
> - Unlike function declarations, function expressions are **not hoisted** — you cannot call `add()` before this line in your code
> - `add(5, 3)` calls the function with arguments `5` and `3`, which returns their sum `8`
> - The semicolon `;` after the closing `}` is required because this is a variable assignment statement (like `const x = 5;`)

### Arrow Function (ES6+) ✅ Modern Syntax

```javascript
// Single-line arrow function — result is returned automatically
const multiply = (a, b) => a * b;

console.log(multiply(4, 5));   // 20

// Multi-line arrow function — needs explicit 'return'
const getGrade = (marks) => {
    if (marks >= 90) return 'A+';
    if (marks >= 75) return 'A';
    if (marks >= 60) return 'B';
    return 'C';
};
```

> **Code Explanation:**
> - `const multiply = (a, b) => a * b;` — A one-line arrow function. The `=>` (fat arrow) replaces the `function` keyword. For single expressions, the result is returned **automatically** (no `return` keyword needed)
> - `const getGrade = (marks) => { ... };` — A multi-line arrow function that uses curly braces `{}`. When using braces, you **must** write `return` explicitly
> - Arrow functions are shorter to write but work the same way as regular functions for most use cases
> - **Note:** Arrow functions are ES6+ syntax (2015). In very old browsers, use regular `function()` instead

### Default Parameters

```javascript
function welcome(name = "Student") {
    return `Welcome, ${name}!`;
}

console.log(welcome());        // "Welcome, Student!"  — no argument, uses default
console.log(welcome("Priya")); // "Welcome, Priya!"   — argument overrides default
```

> **Code Explanation:**
> - `name = "Student"` in the parameter list sets a **default value** — if no argument is passed when calling the function, `name` automatically becomes `"Student"`
> - `welcome()` is called without any argument → `name` uses the default value `"Student"` → returns `"Welcome, Student!"`
> - `welcome("Priya")` passes `"Priya"` as an argument → this overrides the default → returns `"Welcome, Priya!"`
> - Default parameters prevent `undefined` errors when callers forget to pass arguments

### Recursive Functions — Functions That Call Themselves

> **Analogy:** Imagine you're standing in a long queue at the railway booking counter. You ask the person in front of you "What's my position?". They ask the person in front of THEM the same question, and so on. The first person in the queue says "I'm position 1". Then each person adds 1 and passes the answer back. That's recursion — a function calling itself until it reaches a base case.

```javascript
// Factorial using recursion: 5! = 5 × 4 × 3 × 2 × 1 = 120
function factorial(n) {
    // Base case — stop recursion here
    if (n <= 1) {
        return 1;
    }
    // Recursive case — function calls itself with smaller value
    return n * factorial(n - 1);
}

console.log(factorial(5));   // 120
console.log(factorial(0));   // 1

// How it works step by step:
// factorial(5) → 5 * factorial(4)
//              → 5 * 4 * factorial(3)
//              → 5 * 4 * 3 * factorial(2)
//              → 5 * 4 * 3 * 2 * factorial(1)
//              → 5 * 4 * 3 * 2 * 1
//              → 120
```

> **Code Explanation:**
> - `factorial(n)` calls **itself** with `n - 1` each time, breaking a big problem into smaller identical problems
> - **Base case:** `if (n <= 1) return 1;` — this is the stopping condition. Without it, the function would call itself forever and crash
> - **Recursive case:** `return n * factorial(n - 1);` — multiplies `n` by the factorial of the next smaller number
> - `factorial(5)` triggers a chain: `5 * 4 * 3 * 2 * 1 = 120`. Each call waits for the inner call to return before multiplying
> - `factorial(0)` immediately returns `1` because `0 <= 1` satisfies the base case

```javascript
// Countdown using recursion
function countdown(n) {
    if (n <= 0) {               // Base case — stop when we reach 0
        console.log("🚀 Launch!");
        return;
    }
    console.log(n + "...");     // Print current number
    countdown(n - 1);           // Call itself with smaller value
}

countdown(5);
// Output: 5... 4... 3... 2... 1... 🚀 Launch!
```

> **Code Explanation:**
> - `countdown(5)` prints `"5..."`, then calls `countdown(4)`, which prints `"4..."`, and so on
> - **Base case:** `if (n <= 0)` — when `n` reaches 0, it prints "🚀 Launch!" and `return` stops further recursion
> - Each call reduces `n` by 1, guaranteeing the base case is eventually reached
> - This is a simpler example of recursion where the function doesn't return a computed value — it just performs actions (printing to console)

> **⚠️ Important:** Every recursive function MUST have a **base case** (a condition that stops the recursion). Without it, the function calls itself forever and crashes the browser with a "Maximum call stack exceeded" error — like an infinite loop.

---

## 2. Loops

### For Loop

```javascript
// Syntax: for (initialization; condition; update)
for (let i = 1; i <= 5; i++) {
    console.log(`Iteration ${i}`);
}
```

> **Code Explanation:**
> - `for (let i = 1; i <= 5; i++)` has three parts separated by semicolons: **initialization** (`let i = 1` — runs once), **condition** (`i <= 5` — checked before each iteration), and **update** (`i++` — runs after each iteration)
> - The loop runs as long as the condition is `true` — here it runs 5 times (i = 1, 2, 3, 4, 5)
> - `` `Iteration ${i}` `` is a template literal — backticks with `${}` insert the variable value into the string
> - `for` loops are best when you know exactly how many times to iterate

### While Loop

```javascript
let count = 1;                      // Initialize counter BEFORE the loop
while (count <= 5) {                 // Check condition BEFORE each iteration
    console.log(`Count: ${count}`);
    count++;                         // Increment — forgetting this causes infinite loop!
}
```

> **Code Explanation:**
> - `let count = 1;` initializes the counter variable before the loop starts
> - `while (count <= 5)` checks the condition **before** each iteration — if the condition is false initially, the loop body never runs at all
> - `count++` increments the counter inside the loop. **Forgetting this line causes an infinite loop** that freezes the browser!
> - Use a `while` loop when you don't know in advance how many times the loop should run (e.g., keep asking for input until the user types "quit")

### Do...While Loop

```javascript
let num = 1;
do {
    console.log(num);   // Runs FIRST, then checks condition
    num++;
} while (num <= 5);    // Condition checked AFTER each iteration
```

> **Code Explanation:**
> - `do...while` executes the code block **at least once** before checking the condition — unlike `while`, which checks first
> - The `do { }` block runs first (prints `num` and increments it), then the `while (num <= 5)` condition is checked
> - If the condition is still `true`, the loop runs again. If `false`, the loop stops
> - **Key difference from `while`:** Even if you set `let num = 100;`, the `do` block would still execute once (printing 100) before checking and stopping. A `while` loop would skip entirely

### For...of Loop (for arrays/strings)

```javascript
const subjects = ["HTML", "CSS", "JavaScript"];
for (const subject of subjects) {    // 'subject' gets each value directly (not the index)
    console.log(subject);
}
```

> **Code Explanation:**
> - `for...of` is a modern loop designed for iterating over arrays, strings, and other iterable objects
> - `const subject of subjects` — in each iteration, the variable `subject` automatically takes the **value** of the next array element (not the index number)
> - Iteration 1: `subject = "HTML"` → Iteration 2: `subject = "CSS"` → Iteration 3: `subject = "JavaScript"`
> - This is cleaner than a regular `for` loop when you don't need the index — you directly get each value without writing `subjects[i]`

---

## 3. Arrays

### Creating & Accessing Arrays

```javascript
const fruits = ["Apple", "Mango", "Banana", "Orange"];

console.log(fruits[0]);       // "Apple" (index starts at 0)
console.log(fruits.length);   // 4
console.log(fruits[fruits.length - 1]);  // "Orange" (last item)
```

> **Code Explanation:**
> - `const fruits = ["Apple", "Mango", "Banana", "Orange"]` creates an array with 4 string elements. Arrays use square brackets `[]` and items are separated by commas
> - `fruits[0]` accesses the first element — array indexing starts at `0`, not `1`. So `fruits[0]` is `"Apple"`, `fruits[1]` is `"Mango"`, and so on
> - `fruits.length` returns the total number of elements in the array (4 in this case)
> - `fruits[fruits.length - 1]` is a common pattern to get the **last element**. Since `length` is 4, `fruits[4 - 1]` = `fruits[3]` = `"Orange"`

### Array Methods

| Method | Purpose | Example |
|--------|---------|---------|
| `push(item)` | Add to end | `fruits.push("Grape")` |
| `pop()` | Remove from end | `fruits.pop()` |
| `unshift(item)` | Add to start | `fruits.unshift("Kiwi")` |
| `shift()` | Remove from start | `fruits.shift()` |
| `indexOf(item)` | Find index | `fruits.indexOf("Mango")` → 1 |
| `includes(item)` | Check existence | `fruits.includes("Apple")` → true |
| `splice(i, n)` | Remove n items at index i | `fruits.splice(1, 1)` |
| `slice(start, end)` | Copy portion | `fruits.slice(1, 3)` |
| `join(sep)` | Array → String | `fruits.join(", ")` |
| `reverse()` | Reverse in place | `fruits.reverse()` |
| `sort()` | Sort alphabetically | `fruits.sort()` |

### Iterating Arrays

```javascript
const numbers = [10, 20, 30, 40, 50];

// forEach
numbers.forEach((num, index) => {
    console.log(`${index}: ${num}`);
});

// map — creates new array
const doubled = numbers.map(num => num * 2);
console.log(doubled);  // [20, 40, 60, 80, 100]

// filter — keeps items that pass test
const big = numbers.filter(num => num > 25);
console.log(big);  // [30, 40, 50]

// reduce — collapse to single value
const sum = numbers.reduce((total, num) => total + num, 0);
console.log(sum);  // 150
```

> **Code Explanation:**
> - `numbers.forEach((num, index) => { ... })` — Loops through each element and executes the function. The callback receives the current **value** (`num`) and its **index**. `forEach` returns nothing (`undefined`) — it's used for side effects like printing
> - `numbers.map(num => num * 2)` — Creates a **new array** by transforming each element. Here, every number is doubled. The original `numbers` array stays unchanged
> - `numbers.filter(num => num > 25)` — Creates a **new array** containing only elements that pass the test (return `true`). Here, only numbers greater than 25 are kept: `[30, 40, 50]`
> - `numbers.reduce((total, num) => total + num, 0)` — Reduces the entire array to a **single value**. The `0` is the initial value of `total`. In each iteration, the current `num` is added to the running `total`

### Choosing the Right Array Method — `forEach()` vs `map()` vs `filter()` vs `reduce()`

| Method | Returns | Original Array | Use When |
|--------|---------|---------------|----------|
| `forEach()` | Nothing (`undefined`) | Unchanged | You want to DO something with each item (print, update DOM) |
| `map()` | New array (same length) | Unchanged | You want to TRANSFORM each item (double values, extract names) |
| `filter()` | New array (shorter or same) | Unchanged | You want to KEEP items that match a condition |
| `reduce()` | Single value | Unchanged | You want to COMBINE all items into one result (sum, count) |

```javascript
var studentMarks = [85, 42, 73, 91, 38, 67, 55];

// forEach — just print each mark (returns nothing)
studentMarks.forEach(function(mark, i) {
    console.log("Student " + (i + 1) + ": " + mark);
});

// map — add 5 grace marks to each (returns new array)
var withGrace = studentMarks.map(function(mark) {
    return mark + 5;
});
console.log(withGrace);  // [90, 47, 78, 96, 43, 72, 60]

// filter — keep only passed students (returns filtered array)
var passed = studentMarks.filter(function(mark) {
    return mark >= 40;
});
console.log(passed);  // [85, 42, 73, 91, 67, 55]

// reduce — calculate total marks (returns single value)
var total = studentMarks.reduce(function(sum, mark) {
    return sum + mark;
}, 0);
console.log("Total: " + total);  // Total: 451
```

> **Code Explanation:**
> - `var studentMarks = [85, 42, 73, 91, 38, 67, 55];` — An array of 7 student marks used for all examples below
> - **forEach:** Loops through each mark and prints it with the student number. `(i + 1)` converts zero-based index to 1-based. Returns nothing — you cannot store its result
> - **map:** Creates a new array `withGrace` where each mark is increased by 5. The original `studentMarks` array is NOT changed. The new array has the same length (7 items)
> - **filter:** Creates a new array `passed` containing only marks ≥ 40. Students with 38 are excluded. The new array can be shorter than the original
> - **reduce:** Collapses all marks into a single total. Starts with `sum = 0` (the second argument), then adds each `mark` to `sum` one by one. Final result: `85 + 42 + 73 + 91 + 38 + 67 + 55 = 451`

### Array Destructuring — Unpacking Array Values into Variables

> **Analogy:** Array destructuring is like opening a tiffin box with 3 compartments. Instead of taking out each item one by one (`box[0]`, `box[1]`, `box[2]`), you open the lid and all items are directly on your plate — `var [roti, sabzi, dal] = tiffinBox;`

```javascript
// Without destructuring (old way)
var cities = ["Mandsaur", "Indore", "Bhopal"];
var first = cities[0];    // "Mandsaur"
var second = cities[1];   // "Indore"
var third = cities[2];    // "Bhopal"

// With destructuring (modern way)
var [city1, city2, city3] = ["Mandsaur", "Indore", "Bhopal"];
console.log(city1);  // "Mandsaur"
console.log(city2);  // "Indore"
console.log(city3);  // "Bhopal"

// Skip items using commas
var [, secondCity] = ["Mandsaur", "Indore", "Bhopal"];
console.log(secondCity);  // "Indore"

// Default values
var [a, b, c, d = "Ujjain"] = ["Mandsaur", "Indore", "Bhopal"];
console.log(d);  // "Ujjain" (default used since array had only 3 items)

// Swap two variables without temp variable
var x = 10, y = 20;
[x, y] = [y, x];
console.log(x, y);  // 20 10
```

> **Code Explanation:**
> - **Old way:** You access each element using its index (`cities[0]`, `cities[1]`, etc.) and assign to separate variables — 3 lines of code for 3 values
> - **Destructuring:** `var [city1, city2, city3] = [...]` unpacks all 3 values in a single line. The variable names on the left correspond to array positions on the right
> - **Skipping items:** `var [, secondCity]` — the empty space before the comma skips the first element. Only `"Indore"` (index 1) is assigned to `secondCity`
> - **Default values:** `d = "Ujjain"` provides a fallback. Since the array has only 3 items, there is no 4th item, so `d` gets the default value `"Ujjain"`
> - **Swapping:** `[x, y] = [y, x]` swaps two variables without needing a temporary variable. JavaScript creates a temporary array `[20, 10]` and destructures it back into `x` and `y`

### Spread Operator (`...`) — Copying and Merging Arrays

> **Analogy:** The spread operator is like photocopying a document. The original stays the same, and you get a fresh copy to work with. You can also staple multiple photocopies together (merge arrays).

```javascript
// Copying an array (creates a NEW independent copy)
var originalMarks = [85, 92, 78];
var copyMarks = [...originalMarks];
copyMarks.push(95);
console.log(originalMarks);  // [85, 92, 78] — original unchanged!
console.log(copyMarks);      // [85, 92, 78, 95] — only copy changed

// Merging two arrays
var morningBatch = ["Rahul", "Priya", "Amit"];
var eveningBatch = ["Sneha", "Vikram", "Neha"];
var allStudents = [...morningBatch, ...eveningBatch];
console.log(allStudents);  // ["Rahul", "Priya", "Amit", "Sneha", "Vikram", "Neha"]

// Adding items while spreading
var subjects = ["HTML", "CSS"];
var allSubjects = [...subjects, "JavaScript", "Bootstrap"];
console.log(allSubjects);  // ["HTML", "CSS", "JavaScript", "Bootstrap"]

// Using spread with Math functions
var examScores = [78, 92, 65, 88, 45];
console.log(Math.max(...examScores));  // 92
console.log(Math.min(...examScores));  // 45
```

> **Code Explanation:**
> - `[...originalMarks]` — The three dots `...` **spread** (unpack) all elements of `originalMarks` into a new array. Changes to `copyMarks` do NOT affect `originalMarks` because it's a completely independent copy
> - `[...morningBatch, ...eveningBatch]` — Spreads both arrays into a single new array, effectively merging them. The order is preserved: morning students first, then evening students
> - `[...subjects, "JavaScript", "Bootstrap"]` — You can mix spread elements with new items. The spread elements come first, followed by the new items
> - `Math.max(...examScores)` — `Math.max()` normally takes individual numbers like `Math.max(78, 92, 65)`. The spread operator unpacks the array into individual arguments, so `Math.max(...[78, 92, 65, 88, 45])` becomes `Math.max(78, 92, 65, 88, 45)`. Same for `Math.min()`

---

## Practical Session

### 🧪 Lab Experiment 18: Functions, Loops & Arrays

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JS Functions, Loops & Arrays</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: 'Segoe UI', sans-serif; background: #f4f4f4; padding: 30px; }
        .container { max-width: 650px; margin: 0 auto; }
        h1 { color: #1565C0; text-align: center; margin-bottom: 25px; }
        .card {
            background: white; border-radius: 10px; padding: 25px;
            margin-bottom: 20px; box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }
        .card h3 { color: #333; margin-bottom: 10px; }
        input {
            width: 100%; padding: 10px; margin: 5px 0 10px;
            border: 1px solid #ddd; border-radius: 5px; font-size: 15px;
        }
        button {
            padding: 10px 25px; background: #1565C0; color: white;
            border: none; border-radius: 5px; cursor: pointer; font-size: 15px;
        }
        button:hover { background: #0D47A1; }
        .result {
            margin-top: 10px; padding: 15px; background: #E8F5E9;
            border-radius: 5px; white-space: pre-line; display: none;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>JavaScript Functions & Arrays</h1>

        <!-- 1. Multiplication Table -->
        <div class="card">
            <h3>1. Multiplication Table</h3>
            <input type="number" id="tableNum" placeholder="Enter a number (e.g., 7)">
            <button onclick="showTable()">Generate Table</button>
            <div class="result" id="tableResult"></div>
        </div>

        <!-- 2. Fibonacci Series -->
        <div class="card">
            <h3>2. Fibonacci Series</h3>
            <input type="number" id="fibCount" placeholder="How many terms? (e.g., 10)">
            <button onclick="showFibonacci()">Generate</button>
            <div class="result" id="fibResult"></div>
        </div>

        <!-- 3. Array Operations -->
        <div class="card">
            <h3>3. Student Marks Analyzer</h3>
            <input type="text" id="marksInput" placeholder="Enter marks separated by commas (e.g., 78, 92, 65, 88, 45)">
            <button onclick="analyzeMarks()">Analyze</button>
            <div class="result" id="marksResult"></div>
        </div>

        <!-- 4. Factorial -->
        <div class="card">
            <h3>4. Factorial Calculator</h3>
            <input type="number" id="factNum" placeholder="Enter a number (e.g., 6)">
            <button onclick="showFactorial()">Calculate</button>
            <div class="result" id="factResult"></div>
        </div>
    </div>

    <script>
        function showResult(id, text) {
            const el = document.getElementById(id);
            el.textContent = text;
            el.style.display = 'block';
        }

        // 1. Multiplication Table using loop
        function showTable() {
            const num = parseInt(document.getElementById('tableNum').value);
            if (isNaN(num)) { showResult('tableResult', 'Enter a valid number!'); return; }
            
            let table = '';
            for (let i = 1; i <= 10; i++) {
                table += `${num} × ${i} = ${num * i}\n`;
            }
            showResult('tableResult', table);
        }

        // 2. Fibonacci using function
        function fibonacci(n) {
            const fib = [0, 1];
            for (let i = 2; i < n; i++) {
                fib.push(fib[i - 1] + fib[i - 2]);
            }
            return fib.slice(0, n);
        }

        function showFibonacci() {
            const n = parseInt(document.getElementById('fibCount').value);
            if (isNaN(n) || n < 1 || n > 50) {
                showResult('fibResult', 'Enter a number between 1 and 50!');
                return;
            }
            const series = fibonacci(n);
            showResult('fibResult', `Fibonacci (${n} terms):\n${series.join(', ')}`);
        }

        // 3. Array Analysis
        function analyzeMarks() {
            const input = document.getElementById('marksInput').value;
            const marks = input.split(',').map(s => parseFloat(s.trim())).filter(n => !isNaN(n));
            
            if (marks.length === 0) {
                showResult('marksResult', 'Enter valid marks!');
                return;
            }

            const sum = marks.reduce((a, b) => a + b, 0);
            const avg = sum / marks.length;
            const max = Math.max(...marks);
            const min = Math.min(...marks);
            const sorted = [...marks].sort((a, b) => b - a);
            const passed = marks.filter(m => m >= 40).length;
            const failed = marks.length - passed;

            showResult('marksResult',
                `Total Students: ${marks.length}\n` +
                `Sum: ${sum}\n` +
                `Average: ${avg.toFixed(2)}\n` +
                `Highest: ${max}\n` +
                `Lowest: ${min}\n` +
                `Passed (≥40): ${passed}\n` +
                `Failed (<40): ${failed}\n` +
                `Sorted (desc): ${sorted.join(', ')}`
            );
        }

        // 4. Factorial using recursion
        function factorial(n) {
            if (n <= 1) return 1;
            return n * factorial(n - 1);
        }

        function showFactorial() {
            const n = parseInt(document.getElementById('factNum').value);
            if (isNaN(n) || n < 0 || n > 20) {
                showResult('factResult', 'Enter a number between 0 and 20!');
                return;
            }
            showResult('factResult', `${n}! = ${factorial(n)}`);
        }
    </script>

</body>
</html>
```

> **Code Explanation:**
> - `showResult(id, text)` — A reusable helper function that finds an HTML element by its `id`, sets its text content using `textContent`, and makes it visible by changing `display` to `'block'`. This avoids repeating the same 3 lines in every function
> - `showTable()` — Reads a number from the input field using `document.getElementById().value`, converts it to an integer with `parseInt()`, and validates using `isNaN()`. Then uses a `for` loop (1 to 10) to build a multiplication table string, appending each line with `+=` and a newline character `\n`
> - `fibonacci(n)` — Creates an array starting with `[0, 1]`, then uses a `for` loop starting from index 2 to calculate each next term by adding the previous two terms (`fib[i-1] + fib[i-2]`). Uses `push()` to add each new term and returns the first `n` terms using `slice(0, n)`
> - `showFibonacci()` — Validates the input (must be between 1 and 50), calls the `fibonacci()` function, and displays the series using `join(', ')` to convert the array into a comma-separated string
> - `analyzeMarks()` — The most complex function. It splits the comma-separated input string into an array using `split(',')`, converts each piece to a number with `map()` and `parseFloat()`, and filters out invalid entries with `filter(n => !isNaN(n))`. Then it calculates: **sum** using `reduce()`, **average** by dividing sum by length, **highest/lowest** using `Math.max(...marks)` and `Math.min(...marks)` with the spread operator, a **sorted copy** using `[...marks].sort((a, b) => b - a)` (spread creates a copy so the original is unchanged), and **pass/fail counts** using `filter(m => m >= 40).length`
> - `factorial(n)` — A recursive function that returns `1` when `n <= 1` (base case), otherwise returns `n * factorial(n - 1)`. Each call multiplies `n` by the result of calling itself with a smaller number, until it hits the base case
> - `showFactorial()` — Validates input (0 to 20 range to prevent extremely large numbers), then calls `factorial()` and displays the result

---

## Summary

| Concept | Key Syntax |
|---------|-----------|
| Function declaration | `function name(params) { }` |
| Arrow function | `const fn = (a, b) => a + b;` |
| For loop | `for (let i = 0; i < n; i++) { }` |
| For...of | `for (const item of array) { }` |
| Array push/pop | `arr.push()`, `arr.pop()` |
| forEach/map/filter | `arr.map(fn)`, `arr.filter(fn)` |
| reduce | `arr.reduce((sum, n) => sum + n, 0)` |

---

*Day 27 of 55 | Unit 5 — JavaScript | Web Technology (25BCA060)*
