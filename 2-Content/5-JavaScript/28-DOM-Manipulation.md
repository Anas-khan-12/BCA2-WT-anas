# Day 28 — DOM Manipulation

🔬 **Type:** Theory + Practical
🕐 **Duration:** 2 Hours
📚 **Unit:** 5 — JavaScript
🧪 **Lab Experiments:** 19, 20

---

## Learning Objectives

By the end of this session, students will be able to:
- Explain the DOM (Document Object Model)
- Select and modify HTML elements using JavaScript
- Create, append, and remove elements dynamically
- Change styles and classes with JavaScript

---

## 1. What is the DOM?

> **Analogy:** The DOM is like a **family tree** of your HTML page. Each element is a node — `<html>` is the grandparent, `<body>` is the parent, and `<div>`, `<p>`, `<h1>` are children. JavaScript can walk through this tree and change any family member.

```
document
  └── html
       ├── head
       │    └── title
       └── body
            ├── h1
            ├── p
            └── div
                 ├── p
                 └── img
```

The browser reads your HTML and creates this tree structure in memory. JavaScript interacts with this in-memory tree — not the HTML file directly.

> **Code Explanation:**
> - The diagram above shows the **DOM tree** — a visual map of how the browser organizes your HTML page in memory
> - `document` is the root node (the topmost point) — it represents the entire web page and is your entry point for all JavaScript DOM operations
> - `html` is the child of `document` — it corresponds to the `<html>` tag that wraps everything
> - `html` has two children: `head` (contains metadata like `<title>`, `<meta>`, `<link>`) and `body` (contains all the visible content)
> - Inside `body`, you see `h1`, `p`, and `div` as **sibling elements** — they share the same parent (`body`) and sit at the same level
> - The `div` has its own children: another `p` and an `img` — these are nested one level deeper in the tree
> - JavaScript uses this tree structure to **find** (select), **change** (modify), **add** (create), or **remove** (delete) any element on the page

---

## 2. Selecting Elements

| Method | Returns | Use Case |
|--------|---------|----------|
| `getElementById('id')` | Single element | When element has unique ID |
| `querySelector('.class')` | First match | CSS selector — most versatile |
| `querySelectorAll('p')` | NodeList (all matches) | Multiple elements |
| `getElementsByClassName('cls')` | HTMLCollection | All with that class |
| `getElementsByTagName('p')` | HTMLCollection | All elements of that tag |

```javascript
// By ID
const header = document.getElementById('header');

// By CSS selector (first match)
const firstCard = document.querySelector('.card');

// All matches
const allCards = document.querySelectorAll('.card');
allCards.forEach(card => {
    console.log(card.textContent);
});
```

> **Code Explanation:**
> - `document.getElementById('header')` — searches the entire page for an element with `id="header"` and returns it. If no such element exists, it returns `null`. IDs must be unique on a page, so this always returns at most one element
> - `document.querySelector('.card')` — uses a CSS selector to find the **first** element with the class `card`. You can use any valid CSS selector here — `'#id'`, `'.class'`, `'div > p'`, `'input[type="text"]'`, etc.
> - `document.querySelectorAll('.card')` — returns a **NodeList** (an array-like collection) of ALL elements that match the CSS selector `.card`. Unlike `querySelector`, this returns every match, not just the first
> - `.forEach(card => {...})` — loops through each element in the NodeList. The arrow function `card => {...}` is executed once for each matched element, where `card` represents the current element in that iteration
> - `card.textContent` — retrieves the text content of each card element (without any HTML tags). `console.log()` prints it to the browser's developer console (press F12 to see it)

---

## 3. Modifying Elements

### Changing Content

```javascript
const title = document.getElementById('title');

title.textContent = 'New Title';          // Plain text (safe from XSS)
title.innerHTML = '<em>New Title</em>';   // Parses HTML (use carefully)
```

> **Code Explanation:**
> - `document.getElementById('title')` — searches the page for the element with `id="title"` and stores a reference to it in the variable `title`
> - `title.textContent = 'New Title'` — replaces the text inside the element with plain text. Even if you write HTML tags here (like `'<b>Hello</b>'`), they will display as literal text on the page, not as bold text. This is **safe from XSS attacks** (Cross-Site Scripting — a security vulnerability where attackers inject malicious code)
> - `title.innerHTML = '<em>New Title</em>'` — replaces the content and **parses any HTML tags** inside the string. The `<em>` tag will actually render as italicized text on the page. Use `innerHTML` with caution — never insert user-provided input directly into `innerHTML` as it could allow malicious script injection

### Changing Attributes

```javascript
// Select the first image element on the page
const img = document.querySelector('img');
img.setAttribute('src', 'new-photo.jpg');   // Change the image source
img.setAttribute('alt', 'Updated photo');   // Update alt text for accessibility

// Select the first link element on the page
const link = document.querySelector('a');
console.log(link.getAttribute('href'));     // Read and print the link's URL
link.removeAttribute('target');             // Remove the target attribute entirely
```

> **Code Explanation:**
> - `document.querySelector('img')` — selects the **first** `<img>` element found on the page using a CSS selector
> - `img.setAttribute('src', 'new-photo.jpg')` — changes (or creates) the `src` attribute to point to `new-photo.jpg`. If `src` already exists, its value is updated; if it doesn't exist, it is created
> - `img.setAttribute('alt', 'Updated photo')` — sets the `alt` text, which is displayed when the image cannot load and is read by screen readers for accessibility
> - `document.querySelector('a')` — selects the first `<a>` (anchor/link) element on the page
> - `link.getAttribute('href')` — reads and returns the current value of the `href` attribute (the URL the link points to). `console.log()` prints it to the browser console
> - `link.removeAttribute('target')` — completely removes the `target` attribute from the link. For example, if the link had `target="_blank"` (open in new tab), removing it makes the link open in the same tab

#### Common Attribute Operations

| Method | What It Does | Example |
|--------|-------------|---------|
| `setAttribute(name, value)` | Sets/creates an attribute | `img.setAttribute('src', 'photo.jpg')` |
| `getAttribute(name)` | Gets the value of an attribute | `link.getAttribute('href')` |
| `removeAttribute(name)` | Removes an attribute entirely | `link.removeAttribute('target')` |
| `hasAttribute(name)` | Checks if attribute exists (true/false) | `input.hasAttribute('required')` |

```javascript
// Working with a student ID card element
var idCard = document.getElementById('studentCard');

// Set attributes — store student information on the element
idCard.setAttribute('data-name', 'Rahul Kumar');
idCard.setAttribute('data-roll', 'BCA2025-042');
idCard.setAttribute('data-branch', 'BCA');

// Get attributes — read the stored information
var studentName = idCard.getAttribute('data-name');
console.log(studentName);  // "Rahul Kumar"

// Check if attribute exists before using it
if (idCard.hasAttribute('data-roll')) {
    console.log("Roll number is set");
}

// Remove attribute — delete information from the element
idCard.removeAttribute('data-branch');
```

> **Code Explanation:**
> - `document.getElementById('studentCard')` — selects the HTML element with `id="studentCard"` and stores it in the variable `idCard`
> - `setAttribute('data-name', 'Rahul Kumar')` — creates a custom attribute called `data-name` on the element with the value `'Rahul Kumar'`. If `data-name` already exists, its value is updated
> - `setAttribute('data-roll', 'BCA2025-042')` — stores the student's roll number as another custom attribute on the same element
> - `getAttribute('data-name')` — retrieves the value of the `data-name` attribute, returning the string `'Rahul Kumar'`. This value is then stored in the variable `studentName`
> - `hasAttribute('data-roll')` — returns `true` because we previously set `data-roll` on this element. This is useful for checking whether an attribute exists before trying to read it, which prevents errors
> - `removeAttribute('data-branch')` — completely removes the `data-branch` attribute from the element. After this, `idCard.hasAttribute('data-branch')` would return `false`

> **💡 Data Attributes:** Attributes starting with `data-` are called **custom data attributes**. They let you store extra information on HTML elements that JavaScript can read. For example, `data-roll="BCA2025-042"` stores the roll number directly on the element itself. You can name them anything you want after `data-` (like `data-city`, `data-semester`, `data-phone`, etc.). They are very useful when you need to attach data to elements without using hidden inputs or global variables.

### Changing Styles

```javascript
const box = document.getElementById('box');    // Select element by ID
box.style.backgroundColor = '#1565C0';         // CSS: background-color → JS: backgroundColor
box.style.color = 'white';                     // Change text color
box.style.padding = '20px';                    // Add 20px inner spacing
box.style.borderRadius = '10px';               // CSS: border-radius → JS: borderRadius
```

> **Code Explanation:**
> - `document.getElementById('box')` — selects the HTML element with `id="box"`
> - `box.style.backgroundColor = '#1565C0'` — sets the background color to blue. Notice that the CSS property `background-color` becomes `backgroundColor` in JavaScript — the hyphen is removed and the next word is capitalized. This naming convention is called **camelCase**
> - `box.style.color = 'white'` — changes the text color to white
> - `box.style.padding = '20px'` — adds 20 pixels of inner spacing (padding) on all sides
> - `box.style.borderRadius = '10px'` — rounds the corners of the element by 10 pixels. CSS `border-radius` becomes `borderRadius` in JavaScript
> - All these changes are applied as **inline styles** — equivalent to writing `style="background-color: #1565C0; color: white; ..."` directly on the HTML element. Inline styles have high priority and override most CSS rules

### Changing Classes

```javascript
const el = document.getElementById('box');

el.classList.add('active');         // Add class
el.classList.remove('hidden');      // Remove class
el.classList.toggle('dark-mode');   // Toggle (add/remove)
el.classList.contains('active');    // Check: true/false
el.classList.replace('old', 'new'); // Replace class
```

> **Code Explanation:**
> - `document.getElementById('box')` — selects the element with `id="box"` and stores it in `el`
> - `classList.add('active')` — adds the CSS class `active` to the element. This is like writing `class="active"` in HTML. If the class already exists, nothing happens
> - `classList.remove('hidden')` — removes the class `hidden` from the element. If the class doesn't exist, nothing happens (no error)
> - `classList.toggle('dark-mode')` — if the `dark-mode` class is currently on the element, it removes it; if it's missing, it adds it. This is perfect for on/off switches like dark mode buttons or menu toggles
> - `classList.contains('active')` — returns `true` if the element currently has the class `active`, otherwise returns `false`. Useful for checking state before performing an action
> - `classList.replace('old', 'new')` — swaps the class `old` with the class `new` in a single step, which is cleaner than doing `remove('old')` followed by `add('new')`

---

## 4. Creating & Removing Elements

### Creating Elements

```javascript
// Create a new paragraph
const newP = document.createElement('p');
newP.textContent = 'This was added by JavaScript!';
newP.classList.add('highlight');

// Append to body
document.body.appendChild(newP);

// Insert before a specific element
const container = document.getElementById('container');
const firstChild = container.firstElementChild;
container.insertBefore(newP, firstChild);
```

> **Code Explanation:**
> - `document.createElement('p')` — creates a brand new `<p>` element in memory. Important: it is NOT visible on the page yet — it only exists in JavaScript's memory until you add it to the DOM
> - `newP.textContent = '...'` — sets the text content inside the newly created paragraph
> - `newP.classList.add('highlight')` — adds the CSS class `highlight` to the new element (for styling purposes)
> - `document.body.appendChild(newP)` — adds the new paragraph as the **last child** of `<body>`, making it appear at the very bottom of the page. `appendChild()` always adds at the end
> - `document.getElementById('container')` — selects the container element where we want to insert the paragraph
> - `container.firstElementChild` — gets the first child element inside the container
> - `container.insertBefore(newP, firstChild)` — inserts the new paragraph **before** the first child, so it appears at the **top** of the container instead of the bottom. The syntax is `parent.insertBefore(newElement, referenceElement)`

### Removing Elements

```javascript
const oldElement = document.getElementById('remove-me');  // Find the element to remove
oldElement.remove();  // Delete it from the page permanently
```

> **Code Explanation:**
> - `document.getElementById('remove-me')` — searches the page for the element with `id="remove-me"` and stores a reference to it in the variable `oldElement`
> - `.remove()` — permanently deletes that element from the DOM. The element disappears from the page instantly. There is no built-in undo — if you need the element back, you would have to create it again using `createElement()` and `appendChild()`

---

## 5. Event Delegation

> **Analogy:** Imagine a college notice board. Instead of telling each student individually about a new announcement, you pin one notice on the board and all students read it. Event delegation works the same way — instead of adding event listeners to every child element, you add ONE listener to the parent and let events "bubble up".

```html
<!-- Without delegation — BAD (one listener per button) -->
<ul id="studentList">
    <li><button class="delete-btn">Delete Rahul</button></li>
    <li><button class="delete-btn">Delete Priya</button></li>
    <li><button class="delete-btn">Delete Amit</button></li>
</ul>
```

```javascript
// ❌ Without delegation — must add listener to EACH button
// If you add a new student later, the new button won't have a listener!
var buttons = document.querySelectorAll('.delete-btn');
buttons.forEach(function(btn) {
    btn.addEventListener('click', function() {
        // 'this' refers to the button that was clicked
        this.parentElement.remove();  // Remove the parent <li>
    });
});

// ✅ With delegation — ONE listener on the parent handles ALL buttons
// Even new buttons added later will work automatically!
document.getElementById('studentList').addEventListener('click', function(event) {
    // event.target is the actual element that was clicked
    // Check if the clicked element is a delete button
    if (event.target.classList.contains('delete-btn')) {
        // Remove the parent <li> of the clicked button
        event.target.parentElement.remove();
    }
});
```

> **Code Explanation:**
> - **Without delegation (❌ BAD approach):**
>   - `document.querySelectorAll('.delete-btn')` — selects ALL buttons with the class `delete-btn` that currently exist on the page
>   - `.forEach(function(btn) {...})` — loops through each button and attaches a separate click event listener to it
>   - `this.parentElement.remove()` — inside the listener, `this` refers to the specific button that was clicked. `.parentElement` navigates up to its parent `<li>` element, and `.remove()` deletes that `<li>` from the list
>   - **Problem:** If you later add a new student (a new `<li>` with a button) using JavaScript, the new button will NOT have a listener because listeners were only attached to buttons that existed when the code ran
> - **With delegation (✅ GOOD approach):**
>   - `document.getElementById('studentList')` — selects the parent `<ul>` element and attaches just ONE click listener to it
>   - **How event bubbling works:** When you click a button, the click event starts at the button, then "bubbles up" through its ancestors: button → `<li>` → `<ul>`. The listener on `<ul>` catches it
>   - `event.target` — this property tells you the exact element where the click originated (the actual button), even though the listener is on the `<ul>`
>   - `event.target.classList.contains('delete-btn')` — checks if the clicked element has the class `delete-btn`. This is important because clicking on empty space inside the `<ul>` should NOT delete anything
>   - `event.target.parentElement.remove()` — navigates from the clicked button up to its parent `<li>` and removes it
>   - **Key advantage:** Any new buttons added to the list later will automatically work because the listener is on the parent `<ul>`, not on individual buttons

---

## 6. DOM Traversal — Navigating the Family Tree

> **Analogy:** Just like you can navigate a family tree — find someone's parent, children, or siblings — you can navigate the DOM tree using special properties.

| Property | Returns | Example |
|----------|---------|---------|
| `parentElement` | The parent element | `child.parentElement` |
| `children` | All child elements (HTMLCollection) | `parent.children` |
| `firstElementChild` | First child element | `parent.firstElementChild` |
| `lastElementChild` | Last child element | `parent.lastElementChild` |
| `nextElementSibling` | Next sibling element | `el.nextElementSibling` |
| `previousElementSibling` | Previous sibling element | `el.previousElementSibling` |
| `closest('selector')` | Nearest ancestor matching selector | `el.closest('.card')` |

```html
<div id="classRoom">
    <p id="student1">Rahul</p>
    <p id="student2">Priya</p>
    <p id="student3">Amit</p>
</div>
```

```javascript
// Navigate the DOM tree
var priya = document.getElementById('student2');

// Go to parent
console.log(priya.parentElement.id);         // "classRoom"

// Go to siblings
console.log(priya.previousElementSibling.textContent);  // "Rahul"
console.log(priya.nextElementSibling.textContent);       // "Amit"

// Access children from parent
var classRoom = document.getElementById('classRoom');
console.log(classRoom.children.length);           // 3
console.log(classRoom.firstElementChild.textContent);  // "Rahul"
console.log(classRoom.lastElementChild.textContent);   // "Amit"

// Loop through all children
for (var i = 0; i < classRoom.children.length; i++) {
    console.log(classRoom.children[i].textContent);
}
// Output: "Rahul", "Priya", "Amit"
```

> **Code Explanation:**
> - `document.getElementById('student2')` — selects the `<p>` element containing "Priya"
> - **Parent navigation:** `priya.parentElement` returns the `<div id="classRoom">` — the element that directly contains Priya's `<p>` tag. Adding `.id` gives us the string `"classRoom"`
> - **Sibling navigation:** `previousElementSibling` moves to the element immediately before Priya in the DOM (Rahul's `<p>`), and `nextElementSibling` moves to the element immediately after (Amit's `<p>`)
> - **Children access:** `classRoom.children` returns an HTMLCollection (array-like list) of all direct child elements — all three `<p>` tags. `.length` gives the count (3)
> - `firstElementChild` returns the first child element (Rahul's `<p>`), and `lastElementChild` returns the last (Amit's `<p>`)
> - **Looping through children:** A standard `for` loop iterates over `classRoom.children` using index `i` from 0 to 2, printing each student's name using `.textContent` — outputs "Rahul", "Priya", "Amit" in order

---

## 7. Performance Tips for DOM Manipulation

### Cache DOM Queries

> **Analogy:** Imagine you need to check your Aadhaar number multiple times. Would you visit the Aadhaar office every time, or would you save a photocopy at home? Similarly, store DOM elements in variables instead of looking them up every time.

```javascript
// ❌ BAD — searches the DOM every single time
document.getElementById('output').textContent = "Hello";
document.getElementById('output').style.color = "blue";
document.getElementById('output').style.fontSize = "20px";
document.getElementById('output').classList.add('active');

// ✅ GOOD — search once, reuse the variable
var output = document.getElementById('output');  // Search DOM only ONCE
output.textContent = "Hello";
output.style.color = "blue";
output.style.fontSize = "20px";
output.classList.add('active');
```

> **Code Explanation:**
> - **BAD approach (❌):** `document.getElementById('output')` is called 4 separate times. Each call forces the browser to search through the entire DOM tree to find the element with `id="output"` — this is wasteful and slow, especially on large pages with hundreds of elements.
> - **GOOD approach (✅):** `document.getElementById('output')` is called only ONCE, and the result is stored in the variable `output`. All 4 subsequent operations (`.textContent`, `.style.color`, `.style.fontSize`, `.classList.add`) use this variable directly — no repeated DOM searching.
> - Both approaches produce the exact same visual result on the page, but the second approach is faster because it avoids 3 unnecessary DOM lookups.

> **💡 Why does this matter?** Every time you call `document.getElementById()`, the browser has to search through the entire DOM tree. If you call it 4 times, that's 4 searches. By storing the element in a variable, you search once and reuse the reference — this makes your code faster, especially on complex pages.

---

## Practical Session

### 🧪 Lab Experiment 19: DOM Manipulation

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DOM Manipulation</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: 'Segoe UI', sans-serif; background: #f4f4f4; padding: 30px; }
        .container { max-width: 700px; margin: 0 auto; }
        h1 { color: #1565C0; text-align: center; margin-bottom: 25px; }
        
        .card { background: white; border-radius: 10px; padding: 25px;
                margin-bottom: 20px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); }
        
        .btn { padding: 8px 20px; border: none; border-radius: 5px;
               cursor: pointer; color: white; font-size: 14px; margin: 3px; }
        .btn-blue { background: #1565C0; }
        .btn-green { background: #4CAF50; }
        .btn-red { background: #E91E63; }
        .btn-orange { background: #FF9800; }
        
        #targetBox {
            padding: 30px; text-align: center; font-size: 18px;
            border: 2px dashed #ccc; margin: 15px 0; border-radius: 10px;
            transition: all 0.3s ease;
        }
        
        .dark-mode { background: #333 !important; color: #0f0 !important; }
        
        #todoList { list-style: none; }
        #todoList li {
            padding: 10px; margin: 5px 0; background: #f9f9f9;
            border-radius: 5px; display: flex; justify-content: space-between;
            align-items: center;
        }
        #todoList li .delete-btn {
            background: #E91E63; color: white; border: none;
            padding: 4px 10px; border-radius: 3px; cursor: pointer;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>DOM Manipulation Lab</h1>

    <!-- 1. Modify Styles -->
    <div class="card">
        <h3>1. Change Element Styles</h3>
        <div id="targetBox">I'm a target box — click buttons to change me!</div>
        <button class="btn btn-blue" onclick="changeColor()">Change Color</button>
        <button class="btn btn-green" onclick="changeSize()">Change Size</button>
        <button class="btn btn-orange" onclick="toggleDark()">Toggle Dark Mode</button>
        <button class="btn btn-red" onclick="resetBox()">Reset</button>
    </div>

    <!-- 2. Dynamic Content -->
    <div class="card">
        <h3>2. Dynamic Content</h3>
        <p id="dynamicText">This text will change.</p>
        <button class="btn btn-blue" onclick="changeText()">Change Text</button>
        <button class="btn btn-green" onclick="addElement()">Add Paragraph</button>
    </div>

    <!-- 3. Mini Todo List -->
    <div class="card">
        <h3>3. Dynamic Todo List</h3>
        <div style="display:flex; gap:10px; margin-bottom:10px;">
            <input type="text" id="todoInput" placeholder="Enter a task..." 
                   style="flex:1; padding:10px; border:1px solid #ddd; border-radius:5px;">
            <button class="btn btn-green" onclick="addTodo()">Add</button>
        </div>
        <ul id="todoList"></ul>
    </div>
</div>

<script>
    const colors = ['#1565C0', '#E91E63', '#4CAF50', '#FF9800', '#9C27B0'];
    let colorIndex = 0;

    // 1. Style Manipulation
    function changeColor() {
        const box = document.getElementById('targetBox');
        colorIndex = (colorIndex + 1) % colors.length;
        box.style.backgroundColor = colors[colorIndex];
        box.style.color = 'white';
        box.textContent = `Color: ${colors[colorIndex]}`;
    }

    function changeSize() {
        const box = document.getElementById('targetBox');
        const currentSize = parseInt(box.style.fontSize) || 18;
        box.style.fontSize = (currentSize < 36 ? currentSize + 4 : 18) + 'px';
    }

    function toggleDark() {
        document.getElementById('targetBox').classList.toggle('dark-mode');
    }

    function resetBox() {
        const box = document.getElementById('targetBox');
        box.style = '';
        box.className = '';
        box.textContent = "I'm a target box — click buttons to change me!";
        colorIndex = 0;
    }

    // 2. Dynamic Content
    function changeText() {
        const el = document.getElementById('dynamicText');
        const messages = [
            'JavaScript is powerful! 💪',
            'DOM manipulation is easy! 🎯',
            'Keep learning! 📚',
            'You are doing great! ⭐'
        ];
        el.textContent = messages[Math.floor(Math.random() * messages.length)];
    }

    function addElement() {
        const p = document.createElement('p');
        p.textContent = `New paragraph added at ${new Date().toLocaleTimeString()}`;
        p.style.color = '#1565C0';
        p.style.padding = '5px';
        document.getElementById('dynamicText').parentElement.appendChild(p);
    }

    // 3. Todo List
    function addTodo() {
        const input = document.getElementById('todoInput');
        const text = input.value.trim();
        if (!text) return;

        const li = document.createElement('li');
        li.innerHTML = `<span>${text}</span>`;

        const deleteBtn = document.createElement('button');
        deleteBtn.textContent = '✕';
        deleteBtn.className = 'delete-btn';
        deleteBtn.addEventListener('click', function() {
            li.remove();
        });
        li.appendChild(deleteBtn);

        document.getElementById('todoList').appendChild(li);
        input.value = '';
        input.focus();
    }

    // Allow Enter key to add todo
    document.getElementById('todoInput').addEventListener('keydown', function(e) {
        if (e.key === 'Enter') addTodo();
    });
</script>

</body>
</html>
```

> **Code Explanation:**
> - **CSS Styles:**
>   - `.card` — white rounded boxes (`border-radius: 10px`) with a subtle shadow (`box-shadow`) for each section of the lab
>   - `.btn` — base button style with padding, no border, rounded corners, white text, and a pointer cursor on hover
>   - `.btn-blue`, `.btn-green`, `.btn-red`, `.btn-orange` — different background colors for each button type
>   - `#targetBox` — the main demo element with a dashed border and `transition: all 0.3s ease` which makes all style changes animate smoothly over 0.3 seconds
>   - `.dark-mode` — uses `!important` to force dark background (`#333`) and green text (`#0f0`) even when inline styles are present
>   - `#todoList li` — uses `display: flex` with `justify-content: space-between` to push the task text to the left and the delete button to the right
> - **JavaScript — Style Manipulation Functions:**
>   - **`colors` array** — stores 5 hex color values: blue, pink, green, orange, purple
>   - **`colorIndex`** — tracks which color is currently applied (starts at 0)
>   - **`changeColor()`** — `colorIndex = (colorIndex + 1) % colors.length` advances to the next color. The `%` (modulo) operator ensures it wraps back to 0 after reaching the last color (e.g., 5 % 5 = 0). Then sets the box's background color, text color, and displays the hex value
>   - **`changeSize()`** — `parseInt(box.style.fontSize) || 18` reads the current font size as a number (defaults to 18 if not set). Increases by 4px each click up to 36px, then resets to 18px using a ternary operator
>   - **`toggleDark()`** — `classList.toggle('dark-mode')` adds the class if it's missing, or removes it if it's present — one line creates a toggle switch
>   - **`resetBox()`** — `box.style = ''` clears ALL inline styles, `box.className = ''` removes ALL CSS classes, then restores the original text and resets `colorIndex` to 0
> - **JavaScript — Dynamic Content Functions:**
>   - **`changeText()`** — creates an array of 4 motivational messages, then picks a random one using `Math.floor(Math.random() * messages.length)`. `Math.random()` gives a decimal between 0 and 1, multiplying by array length and flooring gives a valid random index
>   - **`addElement()`** — `document.createElement('p')` creates a new paragraph element. `new Date().toLocaleTimeString()` gets the current time in the user's local format (e.g., "2:30:45 PM"). The new paragraph is appended to the card using `parentElement.appendChild(p)` — `parentElement` navigates up to the containing `.card` div
> - **JavaScript — Todo List Functions:**
>   - **`addTodo()`** — reads and trims the input value. If empty, `return` exits the function early. Creates a `<li>` element and sets its HTML content with the task text inside a `<span>`. Creates a delete button with `addEventListener('click', function() { li.remove(); })` — clicking the ✕ button removes the entire `<li>`. Finally, appends the `<li>` to the `<ul>` and clears the input field with `input.focus()` to keep the cursor ready for the next task
>   - **Enter key listener** — `addEventListener('keydown', function(e) {...})` listens for every key press on the input field. `e.key === 'Enter'` checks if the pressed key was Enter, and if so, calls `addTodo()` — this lets users add tasks without clicking the button

---

### 🧪 Lab Experiment 20: Dynamic Table Generation

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dynamic Table</title>
    <style>
        * { box-sizing: border-box; }
        body { font-family: 'Segoe UI', sans-serif; background: #f4f4f4; padding: 30px; }
        .container { max-width: 600px; margin: 0 auto; background: white;
                     border-radius: 10px; padding: 25px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); }
        h1 { color: #1565C0; text-align: center; }
        input { padding: 10px; border: 1px solid #ddd; border-radius: 5px; width: 100%; margin: 5px 0; }
        button { padding: 10px 25px; background: #4CAF50; color: white;
                 border: none; border-radius: 5px; cursor: pointer; margin: 5px 0; }
        table { width: 100%; border-collapse: collapse; margin-top: 15px; }
        th { background: #1565C0; color: white; padding: 10px; }
        td { padding: 10px; border: 1px solid #ddd; text-align: center; }
        tr:nth-child(even) { background: #f9f9f9; }
    </style>
</head>
<body>

<div class="container">
    <h1>Student Records (Dynamic Table)</h1>
    
    <input type="text" id="sName" placeholder="Student Name">
    <input type="text" id="sRoll" placeholder="Roll Number">
    <input type="number" id="sMarks" placeholder="Marks" min="0" max="100">
    <button onclick="addStudent()">Add Student</button>

    <table id="studentTable">
        <thead>
            <tr><th>#</th><th>Name</th><th>Roll No</th><th>Marks</th><th>Grade</th></tr>
        </thead>
        <tbody id="tableBody"></tbody>
    </table>
</div>

<script>
    let count = 0;

    function getGrade(marks) {
        if (marks >= 90) return 'A+';
        if (marks >= 75) return 'A';
        if (marks >= 60) return 'B';
        if (marks >= 40) return 'C';
        return 'F';
    }

    function addStudent() {
        const name = document.getElementById('sName').value.trim();
        const roll = document.getElementById('sRoll').value.trim();
        const marks = parseInt(document.getElementById('sMarks').value);

        if (!name || !roll || isNaN(marks)) {
            alert('Please fill all fields!');
            return;
        }

        count++;
        const grade = getGrade(marks);
        const row = document.createElement('tr');
        row.innerHTML = `<td>${count}</td><td>${name}</td><td>${roll}</td><td>${marks}</td><td>${grade}</td>`;
        document.getElementById('tableBody').appendChild(row);

        // Clear inputs
        document.getElementById('sName').value = '';
        document.getElementById('sRoll').value = '';
        document.getElementById('sMarks').value = '';
        document.getElementById('sName').focus();
    }
</script>

</body>
</html>
```

> **Code Explanation:**
> - **`let count = 0;`** — a counter variable that tracks the serial number of each student added to the table. It starts at 0 and increases by 1 each time a student is added.
> - **`getGrade(marks)` function** — takes a student's marks as input and returns the corresponding grade:
>   - 90 and above → `'A+'`, 75–89 → `'A'`, 60–74 → `'B'`, 40–59 → `'C'`, below 40 → `'F'`
>   - The `if` conditions are checked top to bottom — the first one that matches returns the grade immediately
> - **`addStudent()` function — step by step:**
>   - `document.getElementById('sName').value.trim()` — reads the student's name from the input field. `.value` gets whatever the user typed, and `.trim()` removes any extra spaces from the beginning and end
>   - `document.getElementById('sRoll').value.trim()` — reads the roll number the same way
>   - `parseInt(document.getElementById('sMarks').value)` — reads the marks input and converts it from a string (all input values are strings) to an integer using `parseInt()`
>   - `if (!name || !roll || isNaN(marks))` — validation check: `!name` is true if name is empty, `!roll` is true if roll is empty, `isNaN(marks)` is true if marks is not a valid number. If ANY of these are true, an alert is shown and the function returns early
>   - `count++` — increments the serial number counter by 1
>   - `const grade = getGrade(marks)` — calls the grade function to calculate the grade
>   - `document.createElement('tr')` — creates a new `<tr>` (table row) element in memory
>   - `` row.innerHTML = `<td>${count}</td>...` `` — fills the row with 5 table cells: serial number, name, roll number, marks, and grade. Template literals (`` ` ` ``) allow embedding variables with `${}`
>   - `document.getElementById('tableBody').appendChild(row)` — adds the newly created row as the last child of the `<tbody>` element, making it appear at the bottom of the table
>   - The last 4 lines clear all three input fields by setting `.value = ''` and move the cursor back to the name field with `.focus()` so the user can quickly enter the next student

---

## Summary

| Method | Purpose |
|--------|---------|
| `getElementById()` | Select by ID |
| `querySelector()` | Select by CSS selector |
| `textContent` / `innerHTML` | Change content |
| `style.property` | Change inline style |
| `classList.add/remove/toggle` | Change CSS classes |
| `createElement()` + `appendChild()` | Add new elements |
| `element.remove()` | Remove an element |

---

*Day 28 of 55 | Unit 5 — JavaScript | Web Technology (25BCA060)*
