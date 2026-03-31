# Day 30 — JavaScript Events, Alerts & Dynamic Content

🔬 **Type:** Theory + Practical
🕐 **Duration:** 2 Hours
📚 **Unit:** 5 — JavaScript
🧪 **Lab Experiments:** 17 (contd.), Review of 18-21

---

## Learning Objectives

By the end of this session, students will be able to:
- Handle various JavaScript events
- Use alert, confirm, and prompt dialogs
- Manipulate dates with the Date object
- Build interactive dynamic pages combining all JS concepts

---

## 1. JavaScript Events

> **Analogy:** Events are like **doorbells** — when someone presses the button (user interaction), it triggers an action (a function runs).

### Common Events

| Event | Triggers When |
|-------|--------------|
| `click` | Element is clicked |
| `dblclick` | Element is double-clicked |
| `mouseover` | Mouse enters element |
| `mouseout` | Mouse leaves element |
| `keydown` | Key is pressed |
| `keyup` | Key is released |
| `focus` | Input receives focus |
| `blur` | Input loses focus |
| `change` | Select/checkbox value changes |
| `submit` | Form is submitted |
| `load` | Page finishes loading |
| `scroll` | User scrolls |

### Adding Event Listeners

```javascript
// Method 1: Inline (avoid)
// <button onclick="doSomething()">Click</button>

// Method 2: Property
document.getElementById('btn').onclick = function() {
    alert('Clicked!');
};

// Method 3: addEventListener (recommended)
document.getElementById('btn').addEventListener('click', function() {
    alert('Clicked!');
});
```

> **Code Explanation:**
> - **Method 1 — Inline (avoid):** `<button onclick="doSomething()">` — Puts JavaScript directly in HTML attributes. This mixes HTML and JS, making code harder to maintain. Avoid this method in real projects.
> - **Method 2 — Property:** `element.onclick = function() {...}` — Assigns a function to the element's `onclick` property. Simple but limited — you can only attach ONE handler per event per element. A second assignment overwrites the first.
> - **Method 3 — addEventListener (recommended):** `element.addEventListener('click', function() {...})` — The modern, preferred way. You can attach MULTIPLE handlers to the same event on the same element, and it supports advanced features like capturing and removal.
> - `document.getElementById('btn')` — Finds the HTML element with `id="btn"` in the DOM
> - `function() { alert('Clicked!'); }` — An anonymous (unnamed) function that runs when the click event fires

### The Event Object

```javascript
document.addEventListener('keydown', function(event) {     // Listen for key presses on entire page
    console.log(`Key pressed: ${event.key}`);              // e.g., "a", "Enter", "Shift"
    console.log(`Key code: ${event.code}`);                // e.g., "KeyA", "Enter", "ShiftLeft"
    
    if (event.key === 'Enter') {                           // Check for a specific key
        console.log('Enter was pressed!');
    }
});

document.getElementById('box').addEventListener('click', function(event) {  // Listen for clicks on #box
    console.log(`Clicked at X: ${event.clientX}, Y: ${event.clientY}`);     // Mouse coordinates in window
});
```

> **Code Explanation:**
> - `document.addEventListener('keydown', function(event) {...})` — Listens for any key press on the entire page (not just one element)
> - `event` — The Event Object is automatically passed to the handler function; it contains all details about what happened
> - `event.key` — The character or name of the key pressed (e.g., "a", "Enter", "ArrowUp", "Shift")
> - `event.code` — The physical key on the keyboard (e.g., "KeyA", "Enter", "ArrowUp") — useful for game controls where physical position matters
> - `if (event.key === 'Enter')` — Checks if the specific key pressed was the Enter key
> - `document.getElementById('box').addEventListener('click', function(event) {...})` — Listens for clicks only on the element with `id="box"`
> - `event.clientX` / `event.clientY` — The X (horizontal) and Y (vertical) coordinates of the mouse click relative to the browser window

### Event Bubbling and Capturing — How Events Travel Through the DOM

> **Analogy:** Imagine dropping a stone in a pond — the ripples travel outward from the center. Event **bubbling** is similar: when you click a button inside a div inside the body, the click event first fires on the button, then "bubbles up" to the div, then to the body, then to the document. **Capturing** is the reverse — the event travels from the outermost element down to the target.

```
Event Flow (3 Phases):

CAPTURING PHASE (top → down)          BUBBLING PHASE (down → top)
    document                               document
       ↓                                      ↑
     <body>                                 <body>
       ↓                                      ↑
      <div>                                  <div>
       ↓                                      ↑
    <button>  ← TARGET PHASE →           <button>
    (click happens here)
```

**Example — Event Bubbling (Default Behavior):**

```html
<div id="outer" style="padding: 30px; background: #E3F2FD;">
    Outer Div
    <div id="inner" style="padding: 30px; background: #BBDEFB;">
        Inner Div
        <button id="btn">Click Me</button>
    </div>
</div>
```

```javascript
// By default, events BUBBLE (from target up to document)
document.getElementById('outer').addEventListener('click', function() {
    console.log('1. Outer div clicked');  // Fires THIRD (bubbling)
});

document.getElementById('inner').addEventListener('click', function() {
    console.log('2. Inner div clicked');  // Fires SECOND (bubbling)
});

document.getElementById('btn').addEventListener('click', function() {
    console.log('3. Button clicked');     // Fires FIRST (target)
});

// When you click the button, console shows:
// "3. Button clicked"     (target — where click happened)
// "2. Inner div clicked"  (bubbles up to inner div)
// "1. Outer div clicked"  (bubbles up to outer div)
```

> **Code Explanation:**
> - Three `addEventListener` calls are set up on three nested elements: outer div → inner div → button
> - When the button is clicked, the event fires on the **target** (button) FIRST
> - Then the event **bubbles up** — it triggers the inner div's handler, then the outer div's handler
> - **Bubbling order:** Button (target) → Inner Div → Outer Div → body → document
> - This is the DEFAULT behavior — you don't need to do anything special to enable bubbling
> - Even though you only clicked the button, ALL parent elements' click handlers also fire!

**Capturing Phase — Top Down Instead of Bottom Up:**

```javascript
// To use CAPTURING instead, pass 'true' as third argument
document.getElementById('outer').addEventListener('click', function() {
    console.log('Outer (capturing)');
}, true);  // true = capture phase

// Capturing order is REVERSED — from top down to target
// Outer (capturing) → Inner → Button (target)
```

> **Code Explanation:**
> - `addEventListener('click', function, true)` — The third argument `true` switches from bubbling (default) to **capturing** mode
> - In capturing mode, the event travels from the **outermost** element DOWN to the target
> - **Capturing order:** document → body → Outer Div → Inner Div → Button (target)
> - This is the opposite of bubbling — parent handlers fire BEFORE the target's handler
> - **When to use:** Capturing is rarely needed in practice; bubbling (default) handles most real-world cases

### Stopping Event Propagation

Sometimes you want a click on a button to NOT trigger the parent's click handler. Use `event.stopPropagation()`.

```javascript
document.getElementById('outer').addEventListener('click', function() {
    console.log('Outer clicked');
    // This changes background of outer div
    this.style.backgroundColor = '#C8E6C9';  // 'this' refers to the outer div
});

document.getElementById('btn').addEventListener('click', function(event) {
    event.stopPropagation();  // Stop! Don't let this click bubble up to parent
    console.log('Only button clicked — outer will NOT react');
    // Without stopPropagation(), clicking the button would ALSO
    // trigger the outer div's click handler
});
```

> **Code Explanation:**
> - `this.style.backgroundColor = '#C8E6C9'` — Changes the outer div's background to light green when clicked; `this` refers to the element the handler is attached to
> - `event.stopPropagation()` — Prevents the click event from bubbling up to any parent elements
> - Without `stopPropagation()`: clicking the button fires BOTH the button's handler AND the outer div's handler (because of bubbling)
> - With `stopPropagation()`: clicking the button fires ONLY the button's handler — the outer div never knows the click happened
>
> **When to use `stopPropagation()`:** Use it when you have nested clickable elements and you want a click on the inner element to NOT trigger the outer element's handler. For example, a "Delete" button inside a card — clicking Delete should not also trigger the card's click event.

---

## 2. Dialog Boxes

### Alert (Information)

```javascript
alert('Welcome to the website!');  // Shows a popup message with OK button
```

> **Code Explanation:**
> - `alert('Welcome to the website!')` — Displays a popup dialog box with the message "Welcome to the website!" and an OK button
> - The `alert()` function takes one argument — the message string to display to the user
> - `alert()` PAUSES JavaScript execution — no other code runs until the user clicks OK
> - **Note:** Alert boxes are "blocking" — they freeze the page until dismissed. Use them sparingly for important messages only.

### Confirm (Yes/No Question)

```javascript
const answer = confirm('Are you sure you want to delete?');  // Shows OK/Cancel dialog
if (answer) {                                                 // true = user clicked OK
    console.log('User clicked OK — deleting...');
} else {                                                      // false = user clicked Cancel
    console.log('User clicked Cancel — keeping...');
}
```

> **Code Explanation:**
> - `confirm('Are you sure you want to delete?')` — Shows a dialog box with the question and two buttons: OK and Cancel
> - Returns `true` if the user clicks OK, `false` if they click Cancel
> - `const answer = confirm(...)` — Stores the boolean result (`true` or `false`) in the variable `answer`
> - `if (answer)` — Since `answer` is `true` or `false`, this runs the deletion code when the user confirmed (clicked OK)
> - `else` — Runs the cancellation code when the user declined (clicked Cancel)
> - **Use case:** Always use `confirm()` before destructive actions like deleting data, submitting a form, or navigating away from unsaved changes

### Prompt (Get Input)

```javascript
const name = prompt('What is your name?', 'Student');  // 'Student' is the default value
if (name !== null && name.trim() !== '') {              // Check: not cancelled AND not empty
    alert(`Hello, ${name}!`);                           // Greet the user by name
}
```

> **Code Explanation:**
> - `prompt('What is your name?', 'Student')` — Shows an input dialog with the message "What is your name?" and a default value "Student" pre-filled in the input box
> - The user can type a new value, accept the default by clicking OK, or click Cancel
> - `prompt()` returns the typed string if the user clicks OK, or `null` if the user clicks Cancel
> - `name !== null` — Checks that the user didn't click Cancel (Cancel returns `null`)
> - `name.trim() !== ''` — Checks that the user didn't just enter spaces or leave it empty
> - Both conditions use `&&` (AND) — BOTH must be true for the greeting to show
> - `` alert(`Hello, ${name}!`) `` — If input is valid, shows a greeting using a template literal (backtick string with `${variable}`)

---

## 3. Date Object

```javascript
const now = new Date();

console.log(now.getFullYear());     // 2026
console.log(now.getMonth());        // 0-11 (April = 3)
console.log(now.getDate());         // Day of month (1-31)
console.log(now.getDay());          // Day of week (0=Sun, 6=Sat)
console.log(now.getHours());        // 0-23
console.log(now.getMinutes());      // 0-59
console.log(now.getSeconds());      // 0-59

// Format date
console.log(now.toLocaleDateString('en-IN'));  // "28/4/2026"
console.log(now.toLocaleTimeString('en-IN'));  // "10:30:00 am"
```

> **Code Explanation:**
> - `const now = new Date()` — Creates a Date object containing the current date and time at the moment it's called
> - `now.getFullYear()` — Returns the 4-digit year (e.g., 2026)
> - `now.getMonth()` — Returns month as 0–11 (January = 0, April = 3) — **remember to add 1 for human-readable display!**
> - `now.getDate()` — Returns the day of the month (1–31)
> - `now.getDay()` — Returns the day of the week as a number (0 = Sunday, 1 = Monday, ..., 6 = Saturday)
> - `now.getHours()` — Returns hours in 24-hour format (0–23)
> - `now.getMinutes()` / `now.getSeconds()` — Returns minutes and seconds (0–59)
> - `now.toLocaleDateString('en-IN')` — Formats the date in Indian English style: "28/4/2026" (DD/M/YYYY)
> - `now.toLocaleTimeString('en-IN')` — Formats the time in Indian English style: "10:30:00 am"
> - The `'en-IN'` locale is important — it formats dates and times the way they're written in India

### Timers

```javascript
// Run after 3 seconds (once)
setTimeout(() => {
    alert('3 seconds passed!');
}, 3000);

// Run every 1 second (repeating)
const timer = setInterval(() => {
    console.log(new Date().toLocaleTimeString());
}, 1000);

// Stop the interval
clearInterval(timer);
```

> **Code Explanation:**
> - `setTimeout(callback, 3000)` — Executes the callback function ONCE after a 3000ms (3-second) delay
> - The arrow function `() => { alert('3 seconds passed!'); }` runs after the delay and shows an alert
> - `setInterval(callback, 1000)` — Executes the callback function REPEATEDLY every 1000ms (1 second)
> - `const timer = setInterval(...)` — Stores the interval ID in a variable so we can stop it later
> - `new Date().toLocaleTimeString()` — Gets the current time as a formatted string each time the interval runs
> - `clearInterval(timer)` — Stops the repeating interval using the stored ID; without this, the timer runs forever

---

## 4. Throttling and Debouncing — Controlling Event Frequency

### The Problem

Some events fire VERY rapidly — `mousemove`, `scroll`, `resize`, and `keyup` can fire hundreds of times per second. If your event handler does heavy work (like searching a database or updating the DOM), this can freeze the browser.

> **Analogy:** Imagine you're a waiter at a busy restaurant in Indore. If you run to the kitchen every time a customer even THINKS about ordering, you'll exhaust yourself. Instead, you use two strategies:
> - **Debouncing:** Wait until the customer STOPS talking, then take the order (like a search bar — wait until the user stops typing)
> - **Throttling:** Take orders at most once every 5 minutes, no matter how many requests come in (like a scroll event — update at most once per 100ms)

### Debouncing — Wait Until User Stops

```javascript
// Debouncing — wait until user STOPS typing for 500ms before searching
var searchInput = document.getElementById('searchBox');
var debounceTimer;  // Stores the timer ID so we can cancel it

searchInput.addEventListener('keyup', function() {
    // Clear the previous timer (user is still typing)
    clearTimeout(debounceTimer);
    
    // Set a new timer — only search after 500ms of no typing
    debounceTimer = setTimeout(function() {
        var query = searchInput.value.trim();  // Get trimmed input value
        console.log('Searching for: ' + query);
        // In real app: fetch search results from server
    }, 500);  // 500ms delay before searching
});

// Without debouncing: typing "Mandsaur" triggers 8 searches (M, Ma, Man, Mand...)
// With debouncing: only 1 search happens — after user stops typing
```

> **Code Explanation:**
> - `var debounceTimer` — A variable to store the timeout ID so we can cancel it on each keystroke
> - `searchInput.addEventListener('keyup', function() {...})` — Fires every time a key is released in the search box
> - `clearTimeout(debounceTimer)` — Cancels the previous timer every time a new key is pressed (resets the waiting period)
> - `debounceTimer = setTimeout(function() {...}, 500)` — Sets a new timer: "if no more keys are pressed for 500ms, THEN run the search"
> - `searchInput.value.trim()` — Gets the text from the search box and removes extra leading/trailing spaces
> - **How it works:** Each keystroke cancels the previous timer and starts a new one. Only when the user pauses for 500ms does the search actually execute. Typing "Mandsaur" quickly triggers only 1 search instead of 8!

### Throttling — Limit How Often a Function Runs

```javascript
// Throttling — run at most once every 200ms during scroll
var isThrottled = false;  // Flag to track if we're in "cooldown"

window.addEventListener('scroll', function() {
    if (isThrottled) return;  // Skip if we already ran recently
    
    isThrottled = true;  // Lock — prevent running again too soon
    
    console.log('Scroll position: ' + window.scrollY);
    // Do your scroll-related work here
    
    setTimeout(function() {
        isThrottled = false;  // Unlock after 200ms — ready to run again
    }, 200);  // 200ms cooldown period
});

// Without throttling: scroll handler fires 100+ times per second
// With throttling: fires at most 5 times per second (every 200ms)
```

> **Code Explanation:**
> - `var isThrottled = false` — A boolean flag that acts as a "lock" to control how often the function executes
> - `if (isThrottled) return` — If the function ran recently (lock is ON), skip this scroll event entirely
> - `isThrottled = true` — After running, turn the lock ON so no more executions happen during cooldown
> - `setTimeout(function() { isThrottled = false; }, 200)` — After 200ms, turn the lock OFF so the function can run again
> - `window.scrollY` — The current vertical scroll position in pixels from the top of the page
> - **How it works:** The first scroll event runs normally and sets the lock. For the next 200ms, all scroll events are ignored. After 200ms, the lock resets and one more event can run. This limits execution to at most 5 times per second.

### Comparison Table

| Technique | When It Runs | Best For |
|-----------|-------------|----------|
| No control | Every single event | Simple clicks, hovers |
| **Debouncing** | After user STOPS for X ms | Search boxes, form validation, auto-save |
| **Throttling** | At most once every X ms | Scroll events, resize events, mousemove |

---

## 5. Removing Event Listeners — Proper Cleanup

Sometimes you need to REMOVE an event listener — for example, after a button is clicked once, or when a component is no longer visible.

```javascript
// Define the handler as a named function (NOT anonymous)
function handleClick() {
    alert('Button was clicked!');
    // Remove the listener after first click — button becomes "one-time use"
    document.getElementById('oneTimeBtn').removeEventListener('click', handleClick);
    document.getElementById('oneTimeBtn').textContent = 'Already Clicked';  // Change button text
    document.getElementById('oneTimeBtn').disabled = true;                  // Disable the button
}

// Add the listener
document.getElementById('oneTimeBtn').addEventListener('click', handleClick);
```

> **Code Explanation:**
> - `function handleClick()` — Defines a NAMED function (not anonymous) — this is crucial because you need the name to remove it later
> - `alert('Button was clicked!')` — Shows an alert when the button is clicked for the first time
> - `removeEventListener('click', handleClick)` — Removes the click listener by passing the SAME function reference that was added
> - `.textContent = 'Already Clicked'` — Changes the button text to indicate it has been used
> - `.disabled = true` — Disables the button so it appears greyed out and cannot be clicked again
> - `addEventListener('click', handleClick)` — Attaches the named function as the click handler initially
> - **Key concept:** After the first click, the function removes itself — making it a "one-time use" button

> **⚠️ Important Rule:** You can only remove a listener if you used a **named function**. Anonymous functions cannot be removed!

```javascript
// ❌ CANNOT remove — anonymous function has no reference
document.getElementById('btn').addEventListener('click', function() {
    alert('Hello!');
});
// document.getElementById('btn').removeEventListener('click', ???);  // No way to reference it!

// ✅ CAN remove — named function
function sayHello() {
    alert('Hello!');
}
document.getElementById('btn').addEventListener('click', sayHello);      // Add listener
document.getElementById('btn').removeEventListener('click', sayHello);   // Remove listener — Works!
```

> **Code Explanation:**
> - **Anonymous function (❌):** `function() { alert('Hello!'); }` has no name — you cannot pass it to `removeEventListener` because there's no way to reference the exact same function
> - **Named function (✅):** `function sayHello()` has a name — you can pass `sayHello` to both `addEventListener` and `removeEventListener`
> - `removeEventListener('click', sayHello)` — Works because `sayHello` is the exact same function reference that was originally added
> - **Rule of thumb:** If you ever need to remove a listener later, ALWAYS use a named function instead of an anonymous one

---

## Practical Session: Interactive Dashboard

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive JS Dashboard</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: 'Segoe UI', sans-serif; background: #f0f0f0; }
        
        header {
            background: linear-gradient(135deg, #1565C0, #0D47A1);
            color: white; text-align: center; padding: 20px;
        }
        header h1 { font-size: 1.8em; }
        #clock { font-size: 2.5em; font-weight: 300; margin-top: 5px; }
        #date { opacity: 0.8; }

        .container { max-width: 800px; margin: 20px auto; padding: 0 20px; }
        
        .card {
            background: white; border-radius: 10px; padding: 25px;
            margin-bottom: 20px; box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }
        .card h3 { color: #1565C0; margin-bottom: 12px; }
        
        input { padding: 10px; border: 1px solid #ddd; border-radius: 5px;
                font-size: 15px; width: 100%; margin-bottom: 10px; }
        
        .btn {
            padding: 10px 20px; border: none; border-radius: 5px;
            cursor: pointer; color: white; font-size: 14px; margin: 3px;
        }
        .btn-blue { background: #1565C0; }
        .btn-green { background: #4CAF50; }
        .btn-red { background: #E91E63; }

        .counter-display {
            font-size: 4em; text-align: center; padding: 20px;
            color: #1565C0; font-weight: bold;
        }

        .event-log {
            background: #f9f9f9; padding: 10px; border-radius: 5px;
            max-height: 200px; overflow-y: auto; font-family: monospace;
            font-size: 13px;
        }
        .event-log p { padding: 3px 0; border-bottom: 1px solid #eee; }

        .typing-area {
            width: 100%; padding: 10px; border: 2px solid #ddd;
            border-radius: 5px; font-size: 15px; min-height: 80px;
            resize: vertical;
        }
        #charCount { color: #777; font-size: 14px; margin-top: 5px; }
        #keyOutput { margin-top: 10px; font-size: 1.5em; text-align: center;
                     padding: 15px; background: #f0f0f0; border-radius: 5px; }
    </style>
</head>
<body>

    <header>
        <h1>JavaScript Dashboard</h1>
        <div id="clock">--:--:--</div>
        <div id="date">Loading...</div>
    </header>

    <div class="container">

        <!-- 1. Counter with Events -->
        <div class="card">
            <h3>1. Counter (Click Events)</h3>
            <div class="counter-display" id="counter">0</div>
            <div style="text-align: center;">
                <button class="btn btn-red" id="decBtn">− Decrease</button>
                <button class="btn btn-blue" id="resetBtn">Reset</button>
                <button class="btn btn-green" id="incBtn">+ Increase</button>
            </div>
        </div>

        <!-- 2. Keyboard Events -->
        <div class="card">
            <h3>2. Keyboard Events</h3>
            <textarea class="typing-area" id="typingArea" placeholder="Start typing here..."></textarea>
            <div id="charCount">Characters: 0 | Words: 0</div>
            <div id="keyOutput">Press any key...</div>
        </div>

        <!-- 3. Mouse Events -->
        <div class="card">
            <h3>3. Mouse Event Logger</h3>
            <div id="mouseBox" 
                 style="height: 150px; background: linear-gradient(135deg, #667eea, #764ba2);
                        border-radius: 10px; display: flex; align-items: center;
                        justify-content: center; color: white; font-size: 1.2em; cursor: crosshair;">
                Move your mouse here!
            </div>
            <div class="event-log" id="eventLog"></div>
        </div>

        <!-- 4. Countdown Timer -->
        <div class="card">
            <h3>4. Countdown Timer</h3>
            <input type="number" id="timerInput" placeholder="Enter seconds (e.g., 10)" min="1" max="3600">
            <button class="btn btn-green" id="startTimer">▶ Start</button>
            <button class="btn btn-red" id="stopTimer">■ Stop</button>
            <div style="text-align: center; font-size: 3em; padding: 20px; color: #1565C0; font-weight: bold;"
                 id="timerDisplay">00:00</div>
        </div>
    </div>

    <script>
        // ===== LIVE CLOCK =====
        function updateClock() {
            const now = new Date();
            document.getElementById('clock').textContent = now.toLocaleTimeString('en-IN');
            
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };  // Date display format
            document.getElementById('date').textContent = now.toLocaleDateString('en-IN', options);
        }
        setInterval(updateClock, 1000);  // Update clock every 1 second
        updateClock();                   // Call once immediately so clock shows right away

        // ===== 1. COUNTER =====
        let count = 0;
        const counterEl = document.getElementById('counter');
        
        document.getElementById('incBtn').addEventListener('click', () => {
            count++;
            counterEl.textContent = count;
        });
        document.getElementById('decBtn').addEventListener('click', () => {
            count--;
            counterEl.textContent = count;
        });
        document.getElementById('resetBtn').addEventListener('click', () => {
            count = 0;
            counterEl.textContent = count;
        });

        // ===== 2. KEYBOARD EVENTS =====
        const typingArea = document.getElementById('typingArea');
        
        typingArea.addEventListener('input', () => {
            const text = typingArea.value;
            const chars = text.length;
            const words = text.trim() === '' ? 0 : text.trim().split(/\s+/).length;  // Count words by splitting on whitespace
            document.getElementById('charCount').textContent = `Characters: ${chars} | Words: ${words}`;
        });
        
        typingArea.addEventListener('keydown', (e) => {
            document.getElementById('keyOutput').textContent = 
                `Key: "${e.key}" | Code: ${e.code}`;
        });

        // ===== 3. MOUSE EVENTS =====
        const mouseBox = document.getElementById('mouseBox');
        const eventLog = document.getElementById('eventLog');
        
        function logEvent(msg) {
            const p = document.createElement('p');
            p.textContent = `[${new Date().toLocaleTimeString()}] ${msg}`;
            eventLog.prepend(p);                                          // Add newest event at top
            if (eventLog.children.length > 20) eventLog.lastChild.remove();  // Keep max 20 log entries
        }
        
        mouseBox.addEventListener('mousemove', (e) => {
            mouseBox.textContent = `X: ${e.offsetX}, Y: ${e.offsetY}`;
        });
        mouseBox.addEventListener('click', (e) => logEvent(`Click at (${e.offsetX}, ${e.offsetY})`));
        mouseBox.addEventListener('dblclick', () => logEvent('Double-click!'));
        mouseBox.addEventListener('mouseenter', () => logEvent('Mouse entered'));
        mouseBox.addEventListener('mouseleave', () => logEvent('Mouse left'));

        // ===== 4. COUNTDOWN TIMER =====
        let timerInterval = null;
        
        document.getElementById('startTimer').addEventListener('click', () => {
            clearInterval(timerInterval);
            let seconds = parseInt(document.getElementById('timerInput').value);   // Convert input to integer
            if (isNaN(seconds) || seconds <= 0) { alert('Enter a valid number!'); return; }  // Validate input
            
            function updateTimer() {
                const mins = String(Math.floor(seconds / 60)).padStart(2, '0');  // Convert to MM format
                const secs = String(seconds % 60).padStart(2, '0');               // Convert to SS format
                document.getElementById('timerDisplay').textContent = `${mins}:${secs}`;
                
                if (seconds <= 0) {
                    clearInterval(timerInterval);
                    document.getElementById('timerDisplay').textContent = "⏰ TIME UP!";
                    return;
                }
                seconds--;
            }
            updateTimer();
            timerInterval = setInterval(updateTimer, 1000);
        });
        
        document.getElementById('stopTimer').addEventListener('click', () => {
            clearInterval(timerInterval);
        });
    </script>

</body>
</html>
```

> **Code Explanation:**
> The Interactive Dashboard demonstrates all JavaScript concepts from this unit working together:
>
> **Live Clock Section (Lines 260–268):**
> - `function updateClock()` — Creates a function that gets the current date/time using `new Date()`
> - `now.toLocaleTimeString('en-IN')` — Formats time in Indian English format (e.g., "10:30:00 am")
> - `now.toLocaleDateString('en-IN', options)` — Formats date with weekday, year, month, and day in Indian format (e.g., "Tuesday, 28 April 2026")
> - `setInterval(updateClock, 1000)` — Calls `updateClock` every 1000ms (1 second) to keep the clock ticking live
> - `updateClock()` — Called once immediately so the clock shows right away (without waiting 1 second)
>
> **Counter Section (Lines 271–285):**
> - `let count = 0` — Initializes a counter variable to track the current count
> - `document.getElementById('incBtn').addEventListener('click', ...)` — Listens for clicks on the "Increase" button
> - `count++` / `count--` — Increments or decrements the counter by 1
> - `counterEl.textContent = count` — Updates the displayed number on screen after each click
> - The Reset button sets `count = 0` and updates the display back to zero
>
> **Keyboard Events Section (Lines 288–300):**
> - `typingArea.addEventListener('input', ...)` — Fires every time the textarea content changes (typing, pasting, deleting)
> - `text.trim().split(/\s+/).length` — Splits text by whitespace (spaces, tabs, newlines) to count words; `trim()` removes leading/trailing spaces first
> - `text.trim() === '' ? 0 : ...` — If the text is empty after trimming, word count is 0 (prevents counting empty string as 1 word)
> - `typingArea.addEventListener('keydown', ...)` — Fires when any key is pressed down in the textarea
> - `e.key` shows the character (e.g., "a"), `e.code` shows the physical key (e.g., "KeyA")
>
> **Mouse Events Section (Lines 303–319):**
> - `logEvent(msg)` — Helper function that creates a `<p>` element with a timestamp and prepends it to the event log
> - `eventLog.prepend(p)` — Adds new events at the TOP of the log (newest first), unlike `append` which adds at bottom
> - `if (eventLog.children.length > 20) eventLog.lastChild.remove()` — Keeps only 20 entries to prevent the log from growing forever
> - `e.offsetX, e.offsetY` — Mouse coordinates relative to the element (not the page), useful for tracking position within the box
> - Five different mouse events are tracked: `mousemove` (position), `click` (single), `dblclick` (double), `mouseenter` (enter), `mouseleave` (leave)
>
> **Countdown Timer Section (Lines 322–347):**
> - `let timerInterval = null` — Stores the interval ID so we can stop it later using `clearInterval`
> - `clearInterval(timerInterval)` — Clears any existing timer before starting a new one (prevents multiple timers running simultaneously)
> - `parseInt(document.getElementById('timerInput').value)` — Reads user input and converts string to integer
> - `isNaN(seconds) || seconds <= 0` — Validates that the input is a valid positive number; shows alert if not
> - `Math.floor(seconds / 60)` — Calculates whole minutes from total seconds (e.g., 125 seconds → 2 minutes)
> - `seconds % 60` — Gets remaining seconds after extracting minutes (e.g., 125 % 60 → 5 seconds)
> - `String(...).padStart(2, '0')` — Pads single digits with leading zero (e.g., "5" → "05") for MM:SS format
> - When `seconds <= 0`, the timer stops and displays "⏰ TIME UP!"
> - The Stop button calls `clearInterval(timerInterval)` to pause the countdown at any time

---

## Summary

| Concept | Key Syntax |
|---------|-----------|
| addEventListener | `el.addEventListener('click', fn)` |
| Event object | `event.key`, `event.clientX` |
| alert / confirm / prompt | User dialogs |
| Date | `new Date()`, `.getHours()`, `.toLocaleString()` |
| setTimeout | One-time delay |
| setInterval | Repeating timer |

**🎉 Unit 5 Complete!** Starting Day 31, we begin **Unit 6: Bootstrap** — a CSS framework that makes responsive design fast and easy.

---

*Day 30 of 55 | Unit 5 — JavaScript | Web Technology (25BCA060)*
