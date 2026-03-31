# Day 26 — Introduction to JavaScript

🔬 **Type:** Theory + Practical
🕐 **Duration:** 2 Hours
📚 **Unit:** 5 — JavaScript
🧪 **Lab Experiment:** 17

---

## Learning Objectives

By the end of this session, students will be able to:
- Explain what JavaScript is and where it runs
- Write JavaScript using internal and external methods
- Use variables, data types, operators, and conditional statements
- Display output using alert, console.log, and document.write

---

## 1. What is JavaScript?

> **Analogy:** If HTML is the **skeleton** and CSS is the **clothing**, then JavaScript is the **muscles and brain** — it makes the body move, react, and think.

JavaScript is a **programming language** that runs in the browser. It adds:
- **Interactivity** — button clicks, form validation, animations
- **Dynamic content** — update page without reloading
- **API interaction** — fetch data from servers

### HTML vs CSS vs JavaScript

| Technology | Role | Example |
|-----------|------|---------|
| HTML | Content/Structure | "Here's a button" |
| CSS | Appearance | "The button is blue and rounded" |
| JavaScript | Behavior | "When clicked, show a message" |

---

## 2. Adding JavaScript to HTML

### Internal (in `<script>` tag)

```html
<body>
    <h1>Hello World</h1>
    <script>
        alert('Welcome to JavaScript!');
    </script>
</body>
```

> **Code Explanation:**
> - `<h1>Hello World</h1>` — A normal HTML heading element
> - `<script>` — This tag tells the browser: "The code inside me is JavaScript, not HTML"
> - `alert('Welcome to JavaScript!');` — The `alert()` function shows a pop-up dialog box with the message. The semicolon `;` marks the end of the statement
> - `</script>` — Closes the JavaScript section
> - The `<script>` tag is placed inside `<body>`, so the HTML loads first, then JavaScript runs

### External (separate .js file) ✅ Best Practice

**script.js:**
```javascript
alert('Welcome to JavaScript!');
```

**index.html:**
```html
<body>
    <h1>Hello World</h1>
    <script src="script.js"></script>
</body>
```

> **Code Explanation:**
> - **script.js file:** Contains the JavaScript code `alert('Welcome to JavaScript!');` in a separate file. This keeps HTML and JavaScript **organized in separate files**
> - `<script src="script.js"></script>` — The `src` attribute tells the browser to **load and run** the JavaScript from the external file `script.js`
> - **Why is this better?** Separating HTML and JavaScript makes code **easier to read, maintain, and reuse**. The same `.js` file can be linked to multiple HTML pages
> - The `<script>` tag with `src` should have **no code inside it** — the browser ignores anything between the tags when `src` is used

> Place `<script>` at the **end of `<body>`** so HTML loads before JavaScript runs.

---

## 3. Variables & Data Types

### Declaring Variables

```javascript
let name = "Rahul";          // Can be changed
const PI = 3.14159;          // Cannot be changed (constant)
var age = 20;                // Old way (avoid — use let instead)
```

> **Code Explanation:**
> - `let name = "Rahul";` — Creates a variable called `name` with the value `"Rahul"`. `let` allows you to **change** this value later (e.g., `name = "Priya";`)
> - `const PI = 3.14159;` — Creates a **constant** called `PI`. Once set, you **cannot change** its value. Use `const` for values that should never change
> - `var age = 20;` — The **old way** to create variables (before ES6/2015). It works but has quirks (see Scope and Hoisting sections below), so prefer `let` and `const` in modern code

| Keyword | Reassignable | Scope | Use When |
|---------|-------------|-------|----------|
| `let` | ✅ Yes | Block | Value changes |
| `const` | ❌ No | Block | Value stays same |
| `var` | ✅ Yes | Function | Avoid (legacy) |

### Variable Scope — Where Can You Access a Variable?

> **Analogy:** Think of scope like **rooms in a house**. A variable declared in the **living room** (global scope) can be seen from anywhere in the house. But a variable declared inside a **bedroom** (block scope) can only be seen inside that bedroom — step outside and it disappears!

```javascript
// Global scope — accessible everywhere in the program
var collegeName = "Mandsaur University";

function showInfo() {
    // Function scope — only accessible inside this function
    var department = "BCA";
    
    if (true) {
        // Block scope — let/const only accessible inside this { }
        let semester = "2nd";
        const subject = "Web Technology";
        var year = 2026;  // var ignores block scope! Accessible in entire function
        
        console.log(semester);    // ✅ Works — let is accessible inside its block
        console.log(subject);     // ✅ Works — const is accessible inside its block
    }
    
    console.log(department);  // ✅ Works (function scope)
    console.log(year);        // ✅ Works (var ignores block scope — accessible in entire function)
    // console.log(semester); // ❌ Error! let is block-scoped — can't access outside { }
    // console.log(subject);  // ❌ Error! const is block-scoped — can't access outside { }
}

showInfo();  // Call the function to run the code inside it

console.log(collegeName);    // ✅ Works (global scope — accessible everywhere)
// console.log(department);  // ❌ Error! department is inside the function (function scope)
```

> **Code Explanation:**
> - `var collegeName = "Mandsaur University";` — Declared **outside** any function, so it's in the **global scope** — accessible from anywhere
> - `var department = "BCA";` — Declared **inside** the function `showInfo()`, so it's in **function scope** — only accessible within that function
> - `let semester = "2nd";` / `const subject = "Web Technology";` — Declared inside the `if` block `{ }`, so they're in **block scope** — only accessible within those curly braces
> - `var year = 2026;` — Even though it's inside the `if` block, `var` **ignores block scope**! It's accessible throughout the entire function. This is a major reason to avoid `var`
> - The commented-out lines (`// console.log(semester)`) would cause **ReferenceError** if uncommented — because those variables don't exist outside their scope

### Scope Comparison Table

| Feature | `var` | `let` | `const` |
|---------|-------|-------|---------|
| Scope | Function | Block | Block |
| Can be redeclared? | ✅ Yes | ❌ No | ❌ No |
| Can be reassigned? | ✅ Yes | ✅ Yes | ❌ No |
| Hoisted? | ✅ Yes (as `undefined`) | ✅ Yes (but not initialized) | ✅ Yes (but not initialized) |

### Hoisting — Why Variables "Move" to the Top

> **Analogy:** Hoisting is like a **class attendance register**. The teacher writes everyone's name at the top of the register before class starts, even if a student arrives late. Similarly, JavaScript "registers" all `var` declarations at the top of their scope before running the code.

#### Hoisting with `var`

```javascript
// With var — hoisted (but value is undefined until assignment)
console.log(city);    // undefined (no error! — declaration was hoisted)
var city = "Mandsaur";
console.log(city);    // "Mandsaur"

// JavaScript internally rearranges this as:
// var city;              ← declaration moved to top (hoisted)
// console.log(city);    ← undefined (declared but not yet assigned)
// city = "Mandsaur";    ← assignment stays in its original place
// console.log(city);    ← "Mandsaur" (now the value is assigned)
```

> **Code Explanation:**
> - `console.log(city);` — This runs **before** `city` is assigned, but `var city` is **hoisted** to the top, so JavaScript knows `city` exists — it just has the value `undefined`
> - `var city = "Mandsaur";` — Now the value `"Mandsaur"` is actually assigned
> - The second `console.log(city)` prints `"Mandsaur"` because the assignment has happened by this point
> - **Key insight:** With `var`, only the **declaration** (`var city`) is hoisted, NOT the **assignment** (`= "Mandsaur"`)

#### Hoisting with `let` (Temporal Dead Zone)

```javascript
// With let — hoisted but NOT initialized (Temporal Dead Zone)
// console.log(state);   // ❌ ReferenceError: Cannot access 'state' before initialization
let state = "Madhya Pradesh";
console.log(state);       // ✅ "Madhya Pradesh"
```

> **Code Explanation:**
> - The commented-out `console.log(state)` would cause a **ReferenceError** if uncommented — even though `let state` is technically hoisted, JavaScript puts it in a **"Temporal Dead Zone" (TDZ)** until the `let` line is reached
> - `let state = "Madhya Pradesh";` — Once this line runs, `state` exits the TDZ and becomes accessible
> - `console.log(state)` — Now it works and prints `"Madhya Pradesh"`

> **💡 Tip:** This is another reason to use `let` and `const` instead of `var`. With `let`/`const`, JavaScript **forces you to declare variables before using them**, which catches bugs early. With `var`, you might accidentally use a variable before assigning it and get `undefined` instead of an error — a silent bug!

### Data Types

```javascript
let text = "Hello";           // String
let count = 42;               // Number
let price = 99.99;            // Number (no separate float)
let isStudent = true;         // Boolean
let grade = null;             // Null (intentionally empty)
let address;                  // Undefined (not assigned)
let fruits = ["Apple", "Mango", "Banana"];  // Array
let student = { name: "Rahul", age: 20 };  // Object
```

> **Code Explanation:**
> - `let text = "Hello";` — A **String** is text, always wrapped in quotes (`"..."` or `'...'`)
> - `let count = 42;` — A **Number** for whole numbers (integers)
> - `let price = 99.99;` — Also a **Number** — JavaScript does NOT have a separate `float` or `double` type like C/Java
> - `let isStudent = true;` — A **Boolean** has only two values: `true` or `false`. Used for yes/no decisions
> - `let grade = null;` — **Null** means "intentionally empty" — you're saying "this variable has no value on purpose"
> - `let address;` — **Undefined** means the variable exists but hasn't been given a value yet
> - `let fruits = ["Apple", "Mango", "Banana"];` — An **Array** is a list of values stored in square brackets. Access items by index: `fruits[0]` → `"Apple"`
> - `let student = { name: "Rahul", age: 20 };` — An **Object** stores data as key-value pairs in curly braces. Access values: `student.name` → `"Rahul"`

### Type Checking

```javascript
console.log(typeof "Hello");     // "string"
console.log(typeof 42);          // "number"
console.log(typeof true);        // "boolean"
console.log(typeof undefined);   // "undefined"
```

> **Code Explanation:**
> - `typeof "Hello"` → `"string"` — The `typeof` operator returns the **data type** of a value as a string
> - `typeof 42` → `"number"` — Both integers and decimals are `"number"` in JavaScript
> - `typeof true` → `"boolean"` — `true` and `false` are booleans
> - `typeof undefined` → `"undefined"` — A variable declared but not assigned has the type `"undefined"`
> - **Usage:** `typeof` is useful when you want to check what kind of data a variable holds before performing operations on it

---

## 4. Operators

### Arithmetic

```javascript
let a = 10, b = 3;
console.log(a + b);    // 13 (Addition)
console.log(a - b);    // 7  (Subtraction)
console.log(a * b);    // 30 (Multiplication)
console.log(a / b);    // 3.333... (Division)
console.log(a % b);    // 1  (Modulus/Remainder)
console.log(a ** b);   // 1000 (Exponent: 10³)
```

> **Code Explanation:**
> - `let a = 10, b = 3;` — Declares two variables in one line: `a` is `10` and `b` is `3`
> - `a + b` → `13` — Addition
> - `a - b` → `7` — Subtraction
> - `a * b` → `30` — Multiplication
> - `a / b` → `3.333...` — Division (JavaScript gives decimal results, not integer division)
> - `a % b` → `1` — **Modulus** (remainder): `10 ÷ 3 = 3 remainder 1`. Useful for checking even/odd numbers
> - `a ** b` → `1000` — **Exponent**: `10³` (10 × 10 × 10 = 1000)

### Comparison

```javascript
console.log(5 == "5");     // true  (loose equality — compares value only)
console.log(5 === "5");    // false (strict equality — value AND type)
console.log(5 != "5");     // false
console.log(5 !== "5");    // true
console.log(5 > 3);        // true
console.log(5 <= 5);       // true
```

> **Code Explanation:**
> - `5 == "5"` → `true` — **Loose equality** (`==`) compares only the **value**, not the type. JavaScript converts `"5"` (string) to `5` (number) before comparing
> - `5 === "5"` → `false` — **Strict equality** (`===`) compares both **value AND type**. `5` is a number and `"5"` is a string, so they're not equal
> - `5 != "5"` → `false` — Loose inequality: since `5 == "5"` is true, `5 != "5"` is false
> - `5 !== "5"` → `true` — Strict inequality: since `5 === "5"` is false, `5 !== "5"` is true
> - `5 > 3` → `true` — Greater than comparison
> - `5 <= 5` → `true` — Less than or equal to comparison

> **Always use `===`** (strict) instead of `==` (loose) to avoid unexpected type coercion bugs.

### Logical

```javascript
console.log(true && false);   // false (AND — both must be true)
console.log(true || false);   // true  (OR — at least one true)
console.log(!true);           // false (NOT — flip the value)
```

> **Code Explanation:**
> - `true && false` — **AND operator**: Returns `true` only if **both** sides are `true`. Since one side is `false`, result is `false`
> - `true || false` — **OR operator**: Returns `true` if **at least one** side is `true`. Since the left side is `true`, result is `true`
> - `!true` — **NOT operator**: Flips the value. `true` becomes `false`, `false` becomes `true`

---

## 4.1 Type Coercion — JavaScript's Automatic Type Conversion

> **Analogy:** Type coercion is like a helpful shopkeeper who accepts both **cash and UPI**. You hand him a ₹500 note (number) and he says "I also accept UPI" and treats both the same. Similarly, JavaScript tries to be "helpful" by **automatically converting** one type to another when you mix types in an expression.

Sometimes JavaScript converts data types **behind the scenes** without you asking. This is called **type coercion**. It can be surprising if you don't know the rules!

### The `+` Operator Is Special (Overloaded)

The `+` operator does **two jobs**:
1. **Addition** — when both sides are numbers: `5 + 3` → `8`
2. **Concatenation** — when one side is a string: `"5" + 3` → `"53"` (string wins!)

All other math operators (`-`, `*`, `/`, `%`) **always** convert strings to numbers:
- `"5" - 3` → `2` (string `"5"` becomes number `5`)
- `"5" * 2` → `10`
- `"5" / 2` → `2.5`

### Type Coercion Rules

| Expression | Result | Why |
|-----------|--------|-----|
| `"5" + 3` | `"53"` | `+` with string → concatenation |
| `"5" - 3` | `2` | `-` forces numeric conversion |
| `"5" * 2` | `10` | `*` forces numeric conversion |
| `"5" / 2` | `2.5` | `/` forces numeric conversion |
| `"hello" - 3` | `NaN` | `"hello"` can't become a number |
| `true + 1` | `2` | `true` becomes `1` |
| `false + 1` | `1` | `false` becomes `0` |
| `"" + 5` | `"5"` | Empty string + number → string |
| `null + 5` | `5` | `null` becomes `0` |

### Code Example

```javascript
// The + operator with strings → concatenation (string wins!)
console.log("5" + 3);        // "53" (string + number = string)
console.log("Rahul" + 100);  // "Rahul100"

// Other operators convert strings to numbers
console.log("5" - 3);        // 2 (string "5" becomes number 5)
console.log("5" * 2);        // 10
console.log("5" / 2);        // 2.5

// When conversion fails → NaN (Not a Number)
console.log("hello" - 3);    // NaN (can't convert "hello" to a number)

// Boolean coercion: true = 1, false = 0
console.log(true + 1);       // 2 (true becomes 1)
console.log(false + 1);      // 1 (false becomes 0)

// null becomes 0, empty string becomes "string" with +
console.log(null + 5);       // 5 (null becomes 0)
console.log("" + 5);         // "5" (empty string + number = string)

// Explicit conversion — the SAFE way ✅
var userInput = "25";
var age = Number(userInput);   // Converts string to number explicitly
console.log(age + 5);         // 30 (correct addition, not "255")
console.log(parseInt("42px")); // 42 (extracts the number from the string)
```

> **Code Explanation:**
> - `"5" + 3` → `"53"` — The `+` operator sees a string on the left, so it converts `3` to `"3"` and **concatenates** (joins) the two strings
> - `"5" - 3` → `2` — The `-` operator only works with numbers, so JavaScript converts `"5"` to `5` and does subtraction
> - `"hello" - 3` → `NaN` — JavaScript tries to convert `"hello"` to a number but can't, so the result is **NaN** (Not a Number)
> - `true + 1` → `2` — `true` is treated as `1` in math operations; similarly, `false` is treated as `0`
> - `null + 5` → `5` — `null` is treated as `0` in math operations
> - `Number("25")` — Explicitly converts the string `"25"` to the number `25`. This is the **safe and recommended** way
> - `parseInt("42px")` → `42` — Extracts the integer from the beginning of the string, ignoring non-numeric characters

> **💡 Tip:** Always use `Number()` or `parseInt()` to **explicitly convert** types instead of relying on automatic coercion. This makes your code predictable and avoids bugs!

---

## 5. Output Methods

```javascript
// Pop-up dialog
alert("Hello World!");

// Browser console (F12 → Console tab)
console.log("Debug message");

// Write into HTML
document.write("<h2>Written by JS</h2>");

// Change element content
document.getElementById("output").textContent = "Hello!";
document.getElementById("output").innerHTML = "<strong>Hello!</strong>";
```

> **Code Explanation:**
> - `alert("Hello World!");` — Shows a pop-up dialog box with the message. The user must click "OK" to continue
> - `console.log("Debug message");` — Prints the message to the browser's developer console (press F12 → Console tab to see it)
> - `document.write("<h2>Written by JS</h2>");` — Writes HTML content directly into the page. **Warning:** If called after the page has loaded, it erases everything!
> - `document.getElementById("output")` — Finds the HTML element with `id="output"`
> - `.textContent = "Hello!";` — Sets the plain text content of that element (no HTML tags)
> - `.innerHTML = "<strong>Hello!</strong>";` — Sets the HTML content of that element (HTML tags like `<strong>` will be rendered)

### Output Method Comparison — When to Use Which?

| Method | What It Does | Blocks Page? | Use When |
|--------|-------------|-------------|----------|
| `alert()` | Pop-up dialog box | ✅ Yes (user must click OK) | Quick debugging, important warnings |
| `console.log()` | Prints to browser console (F12) | ❌ No | Debugging during development |
| `document.write()` | Writes directly into HTML page | ❌ No (but overwrites page if used after load) | Testing only — never in production |
| `innerHTML` | Changes content of a specific element | ❌ No | Updating specific parts of the page |
| `textContent` | Changes text of a specific element (no HTML parsing) | ❌ No | Safely displaying user input |

> **⚠️ Important:** `document.write()` will **erase the entire page** if called after the page has finished loading. Never use it in real projects — use `innerHTML` or `textContent` instead.

### Practical Example — All 5 Output Methods

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Output Methods Demo</title>
</head>
<body>
    <p id="demo">This text will change.</p>
    <p id="demo2">This text will also change.</p>

    <script>
        // Method 1: alert() — shows a pop-up dialog box
        alert("1. This is an alert box!");

        // Method 2: console.log() — prints to browser console (F12)
        console.log("2. This appears in the Console tab (F12)");

        // Method 3: document.write() — writes directly into the page
        document.write("<p>3. Written directly to the page</p>");

        // Method 4: innerHTML — sets HTML content of an element
        document.getElementById("demo").innerHTML = "<b>4. Bold text via innerHTML</b>";

        // Method 5: textContent — sets plain text of an element (no HTML parsing)
        document.getElementById("demo2").textContent = "5. Plain text via textContent";
    </script>
</body>
</html>
```

> **Code Explanation:**
> - `alert("1. This is an alert box!");` — Displays a pop-up. The page **pauses** until the user clicks OK
> - `console.log("2. This appears in the Console tab (F12)");` — Output is invisible on the page; you see it only in the browser's Developer Tools (F12 → Console tab)
> - `document.write("<p>3. Written directly to the page</p>");` — Injects a `<p>` tag directly into the HTML. **Note:** Since this runs while the page is loading, it works fine here. But if you call it after the page loads (e.g., on a button click), it will **erase everything**
> - `document.getElementById("demo").innerHTML = "<b>4. Bold text via innerHTML</b>";` — Finds the element with `id="demo"` and replaces its content with bold HTML text. The `<b>` tag is actually rendered as bold
> - `document.getElementById("demo2").textContent = "5. Plain text via textContent";` — Finds the element with `id="demo2"` and replaces its text. Unlike `innerHTML`, any HTML tags would be shown as plain text, not rendered. This is **safer** when displaying user input

---

## 6. Conditional Statements

```javascript
let marks = 75;

if (marks >= 90) {
    console.log("Grade: A+");
} else if (marks >= 75) {
    console.log("Grade: A");
} else if (marks >= 60) {
    console.log("Grade: B");
} else if (marks >= 40) {
    console.log("Grade: C");
} else {
    console.log("Grade: F (Fail)");
}
```

> **Code Explanation:**
> - `let marks = 75;` — Declares a variable `marks` with value `75`
> - `if (marks >= 90)` — First check: are marks 90 or above? If yes, print "A+". If no, move to the next check
> - `else if (marks >= 75)` — Second check: are marks 75 or above? Since `75 >= 75` is true, this prints "Grade: A"
> - `else if (marks >= 60)` / `else if (marks >= 40)` — Further checks for grades B and C
> - `else` — If none of the above conditions are true (marks below 40), print "Grade: F (Fail)"
> - **Important:** JavaScript checks conditions **top to bottom** and stops at the first `true` condition. That's why the order matters — highest grade first!

### Ternary Operator (Shorthand)

```javascript
let age = 20;
let status = (age >= 18) ? "Adult" : "Minor";
console.log(status);  // "Adult"
```

> **Code Explanation:**
> - `let age = 20;` — Declares a variable `age` with value `20`
> - `(age >= 18) ? "Adult" : "Minor"` — This is the **ternary operator**: if `age >= 18` is true, the value is `"Adult"`, otherwise it's `"Minor"`. Think of it as a one-line `if/else`
> - `let status = ...` — Stores the result (`"Adult"`) in the variable `status`
> - `console.log(status);` — Prints `"Adult"` to the browser console

---

## Practical Session

### 🧪 Lab Experiment 17: JavaScript Basics

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JavaScript Basics</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: 'Segoe UI', sans-serif; background: #f4f4f4; padding: 30px; }
        .container { max-width: 600px; margin: 0 auto; }
        h1 { color: #1565C0; text-align: center; margin-bottom: 20px; }
        
        .card {
            background: white;
            border-radius: 10px;
            padding: 25px;
            margin-bottom: 20px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }
        .card h3 { color: #333; margin-bottom: 10px; }
        
        input, select {
            width: 100%;
            padding: 10px;
            margin: 5px 0 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 15px;
        }
        
        button {
            padding: 10px 25px;
            background: #1565C0;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 15px;
        }
        button:hover { background: #0D47A1; }
        
        .result {
            margin-top: 10px;
            padding: 15px;
            background: #E8F5E9;
            border-radius: 5px;
            font-weight: bold;
            display: none;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>JavaScript Basics Lab</h1>

        <!-- 1. Greeting -->
        <div class="card">
            <h3>1. Personalized Greeting</h3>
            <input type="text" id="nameInput" placeholder="Enter your name">
            <button onclick="greet()">Greet Me!</button>
            <div class="result" id="greetResult"></div>
        </div>

        <!-- 2. Calculator -->
        <div class="card">
            <h3>2. Simple Calculator</h3>
            <input type="number" id="num1" placeholder="First number">
            <input type="number" id="num2" placeholder="Second number">
            <select id="operator">
                <option value="+">+ Add</option>
                <option value="-">− Subtract</option>
                <option value="*">× Multiply</option>
                <option value="/">÷ Divide</option>
            </select>
            <button onclick="calculate()">Calculate</button>
            <div class="result" id="calcResult"></div>
        </div>

        <!-- 3. Grade Checker -->
        <div class="card">
            <h3>3. Grade Checker</h3>
            <input type="number" id="marks" placeholder="Enter marks (0-100)" min="0" max="100">
            <button onclick="checkGrade()">Check Grade</button>
            <div class="result" id="gradeResult"></div>
        </div>

        <!-- 4. Age Calculator -->
        <div class="card">
            <h3>4. Age Calculator</h3>
            <input type="number" id="birthYear" placeholder="Enter birth year (e.g., 2005)">
            <button onclick="calcAge()">Calculate Age</button>
            <div class="result" id="ageResult"></div>
        </div>
    </div>

    <script>
        function showResult(id, message) {
            const el = document.getElementById(id);
            el.textContent = message;
            el.style.display = 'block';
        }

        // 1. Greeting
        function greet() {
            const name = document.getElementById('nameInput').value.trim();
            if (name === '') {
                showResult('greetResult', 'Please enter your name!');
                return;
            }
            const hour = new Date().getHours();
            let greeting;
            if (hour < 12) greeting = 'Good Morning';
            else if (hour < 17) greeting = 'Good Afternoon';
            else greeting = 'Good Evening';
            showResult('greetResult', `${greeting}, ${name}! Welcome to Web Technology.`);
        }

        // 2. Calculator
        function calculate() {
            const a = parseFloat(document.getElementById('num1').value);
            const b = parseFloat(document.getElementById('num2').value);
            const op = document.getElementById('operator').value;
            
            if (isNaN(a) || isNaN(b)) {
                showResult('calcResult', 'Please enter valid numbers!');
                return;
            }
            
            let result;
            switch (op) {
                case '+': result = a + b; break;
                case '-': result = a - b; break;
                case '*': result = a * b; break;
                case '/': 
                    if (b === 0) { showResult('calcResult', 'Cannot divide by zero!'); return; }
                    result = a / b; 
                    break;
            }
            showResult('calcResult', `${a} ${op} ${b} = ${result}`);
        }

        // 3. Grade Checker
        function checkGrade() {
            const marks = parseInt(document.getElementById('marks').value);
            if (isNaN(marks) || marks < 0 || marks > 100) {
                showResult('gradeResult', 'Please enter marks between 0 and 100!');
                return;
            }
            
            let grade, emoji;
            if (marks >= 90) { grade = 'A+ (Outstanding)'; emoji = '🏆'; }
            else if (marks >= 75) { grade = 'A (Excellent)'; emoji = '⭐'; }
            else if (marks >= 60) { grade = 'B (Good)'; emoji = '👍'; }
            else if (marks >= 40) { grade = 'C (Average)'; emoji = '📝'; }
            else { grade = 'F (Fail)'; emoji = '❌'; }
            
            showResult('gradeResult', `${emoji} Marks: ${marks} → Grade: ${grade}`);
        }

        // 4. Age Calculator
        function calcAge() {
            const birthYear = parseInt(document.getElementById('birthYear').value);
            const currentYear = new Date().getFullYear();
            
            if (isNaN(birthYear) || birthYear < 1900 || birthYear > currentYear) {
                showResult('ageResult', 'Please enter a valid birth year!');
                return;
            }
            
            const age = currentYear - birthYear;
            const canVote = age >= 18 ? 'Yes ✅' : 'No ❌';
            showResult('ageResult', `Age: ${age} years | Can vote: ${canVote}`);
        }
    </script>

</body>
</html>
```

> **Code Explanation:**
> This lab experiment demonstrates four interactive JavaScript mini-applications. Here is what each function does:
>
> **`showResult(id, message)` — Helper Function (used by all 4 apps):**
> - `document.getElementById(id)` — Finds the HTML element with the given `id`
> - `.textContent = message` — Sets the text content of that element to the result message
> - `.style.display = 'block'` — Makes the result `<div>` visible (it was hidden with `display: none` in CSS)
>
> **`greet()` — Personalized Greeting:**
> - `document.getElementById('nameInput').value.trim()` — Gets the text typed in the name input field and removes extra spaces
> - `if (name === '')` — Checks if the user left the field empty; if yes, shows an error message and `return` stops the function
> - `new Date().getHours()` — Gets the current hour (0–23) from the system clock
> - `if/else if/else` — Determines the greeting: Morning (before 12), Afternoon (12–17), or Evening (after 17)
> - Template literal `` `${greeting}, ${name}!` `` — Combines the greeting and name into one message string
>
> **`calculate()` — Simple Calculator:**
> - `parseFloat(...)` — Converts the text input into a decimal number (e.g., `"10.5"` → `10.5`)
> - `isNaN(a) || isNaN(b)` — Checks if either input is Not-a-Number (invalid input)
> - `switch (op)` — Checks which operator was selected (`+`, `-`, `*`, `/`) and performs that operation
> - `if (b === 0)` — Prevents division by zero (which would give `Infinity`)
> - Template literal `` `${a} ${op} ${b} = ${result}` `` — Displays the full equation with the answer
>
> **`checkGrade()` — Grade Checker:**
> - `parseInt(...)` — Converts input to a whole number (e.g., `"75"` → `75`)
> - `isNaN(marks) || marks < 0 || marks > 100` — Validates that marks are a number between 0 and 100
> - Chain of `if/else if/else` — Assigns a grade based on the marks range (90+ = A+, 75+ = A, etc.)
> - Each grade includes an emoji for visual feedback (`🏆`, `⭐`, `👍`, `📝`, `❌`)
>
> **`calcAge()` — Age Calculator:**
> - `new Date().getFullYear()` — Gets the current year (e.g., `2026`)
> - `currentYear - birthYear` — Calculates approximate age
> - `age >= 18 ? 'Yes ✅' : 'No ❌'` — Ternary operator checks if the person is old enough to vote in India (18+)
> - `showResult(...)` — Displays both the calculated age and voting eligibility

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| JavaScript | Adds interactivity to web pages |
| `let` / `const` | Modern variable declarations |
| Data types | String, Number, Boolean, null, undefined, Array, Object |
| `===` | Always use strict equality |
| `if/else` | Conditional logic |
| Output | `alert()`, `console.log()`, `document.getElementById()` |

---

*Day 26 of 55 | Unit 5 — JavaScript | Web Technology (25BCA060)*
