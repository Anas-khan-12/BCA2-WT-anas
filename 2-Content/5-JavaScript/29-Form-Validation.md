# Day 29 — JavaScript Form Validation

🔬 **Type:** Theory + Practical
🕐 **Duration:** 2 Hours
📚 **Unit:** 5 — JavaScript
🧪 **Lab Experiment:** 21

---

## Learning Objectives

By the end of this session, students will be able to:
- Validate form fields using JavaScript
- Display real-time error messages
- Use regular expressions for pattern matching
- Combine HTML5 validation with JavaScript validation

---

## 1. Why Client-Side Validation?

> **Analogy:** Client-side validation is like the **security check at the airport entrance**. It catches obvious issues (no ticket, wrong ID) before you even reach the counter. But the airline counter (server) still verifies everything — client-side validation improves UX but doesn't replace server-side validation.

| Type | Where | Purpose |
|------|-------|---------|
| Client-side (JS) | Browser | Instant feedback, better UX |
| Server-side | Server | Security, data integrity |
| Both needed | Always | Defense in depth |

---

## 2. Basic Form Validation

```javascript
function validateForm() {
    // Get form field values — .trim() removes extra spaces from both ends
    const name = document.getElementById('name').value.trim();
    const email = document.getElementById('email').value.trim();
    const password = document.getElementById('password').value;
    
    // Check empty fields
    if (name === '') {
        alert('Name is required!');
        return false;   // Stop validation — don't submit the form
    }
    
    // Check email format using a regex pattern
    const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailPattern.test(email)) {
        alert('Enter a valid email!');
        return false;
    }
    
    // Check password length (minimum 8 characters)
    if (password.length < 8) {
        alert('Password must be at least 8 characters!');
        return false;
    }
    
    return true; // All checks passed — form is valid, allow submission
}
```

> **Code Explanation:**
> - `document.getElementById('name')` — Finds the HTML input element with `id="name"` in the page.
> - `.value` — Gets whatever text the user has typed in that input field.
> - `.trim()` — Removes extra spaces from both ends of the string (e.g., `" Rahul "` becomes `"Rahul"`). This prevents users from submitting just spaces.
> - `if (name === '')` — Checks if the name field is empty after trimming. The `===` is strict equality (checks both value and type).
> - `return false` — Immediately stops the function and returns `false`, which tells the form "validation failed — don't submit."
> - `const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/` — Creates a regex (regular expression) pattern for email validation. The pattern is explained in detail in Section 3 below.
> - `emailPattern.test(email)` — The `.test()` method checks if the email string matches the regex pattern. Returns `true` if it matches, `false` if it doesn't.
> - `!emailPattern.test(email)` — The `!` (NOT operator) flips the result: "if the email does NOT match the valid pattern, show an error."
> - `password.length < 8` — The `.length` property returns the number of characters in the string. This checks if the password is shorter than 8 characters.
> - `return true` — If all three checks pass without returning `false`, we reach this line and return `true`, meaning "the form is valid, go ahead and submit."

---

## 3. Regular Expressions (Regex)

| Pattern | Matches | Use |
|---------|---------|-----|
| `/^[A-Za-z ]+$/` | Letters and spaces only | Names |
| `/^[^\s@]+@[^\s@]+\.[^\s@]+$/` | Standard email format | Email |
| `/^\d{10}$/` | Exactly 10 digits | Phone number |
| `/^(?=.*[A-Z])(?=.*\d).{8,}$/` | 1 uppercase + 1 digit + min 8 chars | Strong password |
| `/^\d{6}$/` | Exactly 6 digits | PIN code |

### Regex Characters Explained — A Visual Guide

> **Analogy:** Regular expressions are like giving the security guard at a college gate a checklist: "Only allow people whose name starts with a letter, is 2-50 characters long, and contains only letters and spaces." The guard checks each person against this pattern.

| Character | Meaning | Example | Matches |
|-----------|---------|---------|---------|
| `^` | Start of string | `/^Hello/` | "Hello World" ✅, "Say Hello" ❌ |
| `$` | End of string | `/World$/` | "Hello World" ✅, "World Cup" ❌ |
| `.` | Any single character | `/h.t/` | "hat", "hit", "hot" ✅ |
| `*` | Zero or more of previous | `/ab*/` | "a", "ab", "abbb" ✅ |
| `+` | One or more of previous | `/ab+/` | "ab", "abbb" ✅, "a" ❌ |
| `?` | Zero or one of previous (optional) | `/colou?r/` | "color", "colour" ✅ |
| `[]` | Character set — match any ONE inside | `/[aeiou]/` | any vowel ✅ |
| `[^]` | Negated set — match any NOT inside | `/[^0-9]/` | any non-digit ✅ |
| `\d` | Any digit (0-9) | `/\d{3}/` | "123" ✅ |
| `\s` | Any whitespace (space, tab) | `/\s/` | " " ✅ |
| `\w` | Word character (letter, digit, _) | `/\w+/` | "hello_123" ✅ |
| `{n}` | Exactly n repetitions | `/\d{10}/` | exactly 10 digits |
| `{n,m}` | Between n and m repetitions | `/\w{2,50}/` | 2 to 50 word chars |
| `()` | Grouping | `/(ab)+/` | "ab", "abab" ✅ |
| `\|` | OR operator | `/cat\|dog/` | "cat" or "dog" ✅ |
| `(?=...)` | Lookahead — must be followed by | `/(?=.*[A-Z])/` | has uppercase somewhere |

### Breaking Down Each Regex Pattern Used in This Lesson

**Pattern 1: Name Validation — `/^[A-Za-z ]+$/`**

```
/^[A-Za-z ]+$/

^           → Start of string
[A-Za-z ]   → Allow: uppercase letters (A-Z), lowercase letters (a-z), and space
+           → One or more of the above characters
$           → End of string

✅ Matches: "Rahul Kumar", "Priya"
❌ Rejects: "Rahul123", "Priya@gmail", ""
```

**Pattern 2: Email Validation — `/^[^\s@]+@[^\s@]+\.[^\s@]+$/`**

```
/^[^\s@]+@[^\s@]+\.[^\s@]+$/

^           → Start of string
[^\s@]+     → One or more characters that are NOT whitespace or @  (the username part)
@           → Literal @ symbol
[^\s@]+     → One or more characters that are NOT whitespace or @  (the domain name)
\.          → Literal dot (. is escaped because . normally means "any character")
[^\s@]+     → One or more characters that are NOT whitespace or @  (the extension like "com")
$           → End of string

✅ Matches: "rahul@gmail.com", "priya.sharma@university.ac.in"
❌ Rejects: "rahul@", "@gmail.com", "rahul @gmail.com"
```

**Pattern 3: Phone Number — `/^\d{10}$/`**

```
/^\d{10}$/

^       → Start of string
\d      → Any digit (0-9)
{10}    → Exactly 10 of the previous character
$       → End of string

✅ Matches: "9876543210"
❌ Rejects: "98765", "98765432100", "phone1234"
```

**Pattern 4: Strong Password — `/^(?=.*[A-Z])(?=.*\d).{8,}$/`**

```
/^(?=.*[A-Z])(?=.*\d).{8,}$/

^               → Start of string
(?=.*[A-Z])     → Lookahead: somewhere in the string, there must be at least 1 uppercase letter
  (?=           → Start lookahead (peek ahead without consuming characters)
  .*            → Any characters (zero or more)
  [A-Z]         → Followed by an uppercase letter
  )             → End lookahead
(?=.*\d)        → Lookahead: somewhere in the string, there must be at least 1 digit
  (?=           → Start lookahead
  .*            → Any characters
  \d            → Followed by a digit
  )             → End lookahead
.{8,}           → At least 8 characters of any type
$               → End of string

✅ Matches: "Rahul@123", "SecurePass1"
❌ Rejects: "rahul123" (no uppercase), "RAHUL" (no digit), "Ra1" (too short)
```

**Pattern 5: PIN Code — `/^\d{6}$/`**

```
/^\d{6}$/

^       → Start of string
\d      → Any digit (0-9)
{6}     → Exactly 6 of the previous character
$       → End of string

✅ Matches: "458001" (Mandsaur PIN), "452001" (Indore PIN)
❌ Rejects: "4580", "45800100", "ABCDEF"
```

```javascript
// Regex pattern: only letters (A-Z, a-z) and spaces, between 2 and 50 characters long
const nameRegex = /^[A-Za-z ]{2,50}$/;
console.log(nameRegex.test("Rahul Kumar"));  // true — only letters and spaces, 11 chars
console.log(nameRegex.test("123"));          // false — digits are not in [A-Za-z ]
```

> **Code Explanation:**
> - `const nameRegex = /^[A-Za-z ]{2,50}$/` — Creates a regex pattern that allows only uppercase letters (A-Z), lowercase letters (a-z), and spaces, with a minimum of 2 and maximum of 50 characters.
> - `.test("Rahul Kumar")` — The `.test()` method checks if the given string matches the regex pattern. It returns `true` or `false`.
> - `"Rahul Kumar"` → returns `true` because it contains only letters and spaces (11 characters, which is between 2 and 50).
> - `"123"` → returns `false` because digits (`1`, `2`, `3`) are not in the allowed character set `[A-Za-z ]`.
> - The `.test()` method is the most common way to validate user input against a regex pattern in JavaScript.

---

## 4. Real-Time Validation

Instead of showing alert boxes, display inline error messages:

```javascript
function showError(inputId, message) {
    // Find the error <div> by appending 'Error' to the input's ID (e.g., 'email' → 'emailError')
    const errorEl = document.getElementById(inputId + 'Error');
    errorEl.textContent = message;          // Set the error message text
    errorEl.style.display = 'block';        // Make the error message visible
    document.getElementById(inputId).style.borderColor = '#E91E63';  // Red border on input
}

function clearError(inputId) {
    const errorEl = document.getElementById(inputId + 'Error');  // Find the error <div>
    errorEl.textContent = '';                // Clear the error text
    errorEl.style.display = 'none';          // Hide the error message
    document.getElementById(inputId).style.borderColor = '#4CAF50';  // Green border (valid)
}
```

> **Code Explanation:**
> - `showError(inputId, message)` — A reusable function to display an error for **any** form field:
>   - `document.getElementById(inputId + 'Error')` — Finds the error `<div>` by combining the input's ID with the word `'Error'` (e.g., for input `"email"`, it finds the element with `id="emailError"`)
>   - `errorEl.textContent = message` — Sets the visible error text (e.g., "Enter a valid email")
>   - `errorEl.style.display = 'block'` — Makes the error div visible (it was hidden with `display: none` in CSS)
>   - `style.borderColor = '#E91E63'` — Changes the input's border to pink/red to visually indicate an error
> - `clearError(inputId)` — The opposite function, called when the user fixes their input:
>   - Sets `textContent` to empty string `''` to remove the error text
>   - Sets `display` to `'none'` to hide the error div again
>   - Changes border to green (`#4CAF50`) to indicate the field is now valid
> - **Why is this approach better than `alert()`?** — Alert boxes block the entire page, are annoying, and show only one error at a time. Inline errors let users see all problems at once without disruption.

---

## 4.1 Common Validation Mistakes

### What Makes an Email Valid/Invalid?

| Input | Valid? | Why |
|-------|--------|-----|
| `rahul@gmail.com` | ✅ | Correct format: username + @ + domain + .com |
| `priya.sharma@university.ac.in` | ✅ | Subdomains (ac.in) are perfectly fine |
| `user+tag@email.com` | ✅ | `+` is allowed in the username part |
| `rahul@` | ❌ | Missing domain after @ |
| `@gmail.com` | ❌ | Missing username before @ |
| `rahul@gmail` | ❌ | Missing extension (.com, .in, etc.) |
| `rahul @gmail.com` | ❌ | Space in email is not allowed |
| `rahul@@gmail.com` | ❌ | Double @ symbol |

### Common Validation Mistakes Students Make

| Mistake | Problem | Fix |
|---------|---------|-----|
| Not trimming input | `" Rahul "` passes but has leading/trailing spaces | Use `.trim()` before validation |
| Only checking empty string | `"   "` (just spaces) passes the empty check | Use `.trim()` first, then check length |
| Not validating on server | Client-side validation can be bypassed using browser DevTools | Always validate on both client AND server side |
| Too strict email regex | Rejects valid emails like `user+tag@email.com` | Use a simple regex, don't try to be RFC-perfect |
| Not checking password match | Users type different passwords in both fields | Always add a confirm password field and compare |
| Forgetting `event.preventDefault()` | Form submits and page reloads before validation runs | Always call `event.preventDefault()` first in your submit handler |

---

## 4.2 Understanding `event.preventDefault()`

> **Analogy:** When you submit a form, the browser's default behavior is to reload the page and send data to the server — like a student who automatically submits their answer sheet when time is up. `event.preventDefault()` is like telling the student "Wait! Let me check your answers first before submitting."

```javascript
// WITHOUT preventDefault — form submits and page reloads immediately
document.getElementById('myForm').addEventListener('submit', function(event) {
    console.log("This message appears for a split second, then page reloads!");
    // Browser reloads the page — you can't validate anything!
});

// WITH preventDefault — stop the default behavior, validate first
document.getElementById('myForm').addEventListener('submit', function(event) {
    event.preventDefault();  // Stop the page from reloading!
    
    // Now we have time to validate the form
    var name = document.getElementById('name').value.trim();
    if (name === '') {
        alert('Please enter your name!');
        return;  // Stop here — don't submit
    }
    
    // If validation passes, we can submit manually
    alert('Form is valid! Submitting...');
    // this.submit();  // Uncomment this line to actually submit the form
});
```

> **Code Explanation:**
> - **First example (without `preventDefault`):** The browser's default behavior runs — the page reloads instantly when the form is submitted. Any `console.log()` or validation code you write executes for a fraction of a second, then disappears because the page refreshes. You never get to validate anything!
> - **Second example (with `preventDefault`):**
>   - `event.preventDefault()` — Tells the browser "don't do your default action (reloading the page)." The page stays as-is.
>   - Now we can safely run validation code — checking if the name is empty, if the email is valid, etc.
>   - `return` — If validation fails, we stop the function here. The form doesn't submit.
>   - `this.submit()` — If we want to actually submit after validation passes, we call the form's `.submit()` method manually. This is commented out in the example so you can test without actually submitting.
> - The `event` parameter is automatically passed by the browser when a `submit` event fires. It contains information about the event and methods like `preventDefault()` to control it.

### Other Common Uses of `preventDefault()`

| Event | Default Behavior | Why Prevent It |
|-------|-----------------|----------------|
| `submit` (form) | Reloads page and sends data to server | Validate form data before submitting |
| `click` (link `<a>`) | Navigates to the `href` URL | Handle navigation with JavaScript (e.g., single-page apps) |
| `keydown` | Types the key character in the input | Block certain keys (e.g., prevent non-numeric input in phone field) |
| `contextmenu` (right-click) | Shows the browser's right-click menu | Show a custom context menu instead |

---

## Practical Session

### 🧪 Lab Experiment 21: Complete Form Validation

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Form Validation</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: 'Segoe UI', sans-serif; background: #f4f4f4; padding: 30px; }
        
        .form-container {
            max-width: 500px; margin: 0 auto; background: white;
            border-radius: 12px; padding: 30px; box-shadow: 0 4px 20px rgba(0,0,0,0.1);
        }
        h1 { color: #1565C0; text-align: center; margin-bottom: 25px; }
        
        .form-group { margin-bottom: 18px; }
        label { display: block; font-weight: 600; margin-bottom: 5px; color: #333; }
        
        input, select, textarea {
            width: 100%; padding: 10px 12px; border: 2px solid #ddd;
            border-radius: 6px; font-size: 15px; transition: border-color 0.3s;
        }
        input:focus, select:focus, textarea:focus {
            outline: none; border-color: #1565C0;
        }
        
        .error-msg {
            color: #E91E63; font-size: 13px; margin-top: 4px; display: none;
        }
        .valid { border-color: #4CAF50 !important; }
        .invalid { border-color: #E91E63 !important; }
        
        .password-strength {
            height: 5px; border-radius: 3px; margin-top: 5px; transition: all 0.3s;
        }
        .strength-weak { background: #E91E63; width: 33%; }
        .strength-medium { background: #FF9800; width: 66%; }
        .strength-strong { background: #4CAF50; width: 100%; }
        
        .btn-submit {
            width: 100%; padding: 12px; background: #1565C0; color: white;
            border: none; border-radius: 6px; font-size: 16px; cursor: pointer;
            transition: background 0.3s;
        }
        .btn-submit:hover { background: #0D47A1; }
        .btn-submit:disabled { background: #ccc; cursor: not-allowed; }
        
        .success-msg {
            text-align: center; color: #4CAF50; font-weight: bold;
            padding: 15px; display: none;
        }
    </style>
</head>
<body>

<div class="form-container">
    <h1>Registration Form</h1>
    
    <form id="regForm" onsubmit="return handleSubmit(event)">
        
        <div class="form-group">
            <label for="fullname">Full Name</label>
            <input type="text" id="fullname" placeholder="e.g., Rahul Kumar"
                   oninput="validateName()">
            <div class="error-msg" id="fullnameError"></div>
        </div>

        <div class="form-group">
            <label for="email">Email Address</label>
            <input type="email" id="email" placeholder="e.g., rahul@example.com"
                   oninput="validateEmail()">
            <div class="error-msg" id="emailError"></div>
        </div>

        <div class="form-group">
            <label for="phone">Phone Number</label>
            <input type="tel" id="phone" placeholder="e.g., 9876543210" maxlength="10"
                   oninput="validatePhone()">
            <div class="error-msg" id="phoneError"></div>
        </div>

        <div class="form-group">
            <label for="password">Password</label>
            <input type="password" id="password" placeholder="Min 8 chars, 1 uppercase, 1 digit"
                   oninput="validatePassword()">
            <div class="password-strength" id="strengthBar"></div>
            <div class="error-msg" id="passwordError"></div>
        </div>

        <div class="form-group">
            <label for="confirmPassword">Confirm Password</label>
            <input type="password" id="confirmPassword" placeholder="Re-enter password"
                   oninput="validateConfirm()">
            <div class="error-msg" id="confirmPasswordError"></div>
        </div>

        <div class="form-group">
            <label for="dob">Date of Birth</label>
            <input type="date" id="dob" onchange="validateDOB()">
            <div class="error-msg" id="dobError"></div>
        </div>

        <div class="form-group">
            <label for="gender">Gender</label>
            <select id="gender" onchange="validateGender()">
                <option value="">-- Select --</option>
                <option value="male">Male</option>
                <option value="female">Female</option>
                <option value="other">Other</option>
            </select>
            <div class="error-msg" id="genderError"></div>
        </div>

        <button type="submit" class="btn-submit">Register</button>
    </form>

    <div class="success-msg" id="successMsg">
        ✅ Registration Successful! Welcome aboard!
    </div>
</div>

<script>
    // Utility functions — reused by all validators to show/hide error messages
    function showError(id, msg) {
        document.getElementById(id + 'Error').textContent = msg;     // Set error text
        document.getElementById(id + 'Error').style.display = 'block'; // Show the error div
        document.getElementById(id).classList.add('invalid');        // Add red border class
        document.getElementById(id).classList.remove('valid');       // Remove green border class
    }
    function clearError(id) {
        document.getElementById(id + 'Error').style.display = 'none'; // Hide the error div
        document.getElementById(id).classList.remove('invalid');     // Remove red border
        document.getElementById(id).classList.add('valid');          // Add green border
    }

    // Validators
    function validateName() {
        const val = document.getElementById('fullname').value.trim();
        if (val.length < 2) { showError('fullname', 'Name must be at least 2 characters'); return false; }
        if (!/^[A-Za-z ]+$/.test(val)) { showError('fullname', 'Name can only contain letters and spaces'); return false; }
        clearError('fullname'); return true;
    }

    function validateEmail() {
        const val = document.getElementById('email').value.trim();
        if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(val)) { showError('email', 'Enter a valid email address'); return false; }
        clearError('email'); return true;
    }

    function validatePhone() {
        const val = document.getElementById('phone').value.trim();
        if (!/^\d{10}$/.test(val)) { showError('phone', 'Phone must be exactly 10 digits'); return false; }
        clearError('phone'); return true;
    }

    function validatePassword() {
        const val = document.getElementById('password').value;
        const bar = document.getElementById('strengthBar');  // The colored strength indicator bar
        
        // Step 1: Check minimum length
        if (val.length < 8) {
            showError('password', 'Password must be at least 8 characters');
            bar.className = 'password-strength strength-weak';  // Red bar, 33% width
            return false;
        }
        
        // Step 2: Calculate strength score (0 to 4)
        let strength = 0;
        if (/[A-Z]/.test(val)) strength++;       // Has uppercase letter?
        if (/[a-z]/.test(val)) strength++;       // Has lowercase letter?
        if (/[0-9]/.test(val)) strength++;       // Has digit?
        if (/[^A-Za-z0-9]/.test(val)) strength++;  // Has special character (@, #, ! etc.)?
        
        // Step 3: Update the visual strength bar based on score
        if (strength <= 2) bar.className = 'password-strength strength-medium';  // Orange, 66%
        else bar.className = 'password-strength strength-strong';                // Green, 100%
        
        // Step 4: Strict requirement — must have at least 1 uppercase AND 1 digit
        if (!/(?=.*[A-Z])(?=.*\d)/.test(val)) {
            showError('password', 'Must include at least 1 uppercase letter and 1 digit');
            return false;
        }
        clearError('password'); return true;
    }

    function validateConfirm() {
        const pwd = document.getElementById('password').value;
        const confirm = document.getElementById('confirmPassword').value;
        if (confirm !== pwd) { showError('confirmPassword', 'Passwords do not match'); return false; }
        clearError('confirmPassword'); return true;
    }

    function validateDOB() {
        const val = document.getElementById('dob').value;
        if (!val) { showError('dob', 'Date of birth is required'); return false; }
        const age = new Date().getFullYear() - new Date(val).getFullYear();
        if (age < 16 || age > 100) { showError('dob', 'Age must be between 16 and 100'); return false; }
        clearError('dob'); return true;
    }

    function validateGender() {
        const val = document.getElementById('gender').value;
        if (!val) { showError('gender', 'Please select a gender'); return false; }
        clearError('gender'); return true;
    }

    // Form Submit — called when the user clicks the Register button
    function handleSubmit(e) {
        e.preventDefault();  // Stop the browser from reloading the page

        // Using & (bitwise AND) instead of && (logical AND) so ALL validators run
        // && would stop at the first false — only showing one error at a time
        // & evaluates ALL functions — showing ALL errors at once (better UX)
        const isValid = validateName() & validateEmail() & validatePhone() &
                        validatePassword() & validateConfirm() & validateDOB() & validateGender();
        
        if (isValid) {
            document.getElementById('regForm').style.display = 'none';       // Hide the form
            document.getElementById('successMsg').style.display = 'block';   // Show success message
        }
        return false;  // Prevent default form submission in all cases
    }
</script>

</body>
</html>
```

> **Code Explanation:**
>
> **HTML Structure:**
> - `<form id="regForm" onsubmit="return handleSubmit(event)">` — When the form is submitted, it calls `handleSubmit()` and passes the submit event object. The `return` keyword ensures that if the function returns `false`, the form won't submit.
> - Each form field is wrapped in a `<div class="form-group">` containing three elements: a `<label>`, an `<input>` (or `<select>`), and a hidden `<div class="error-msg">` for showing errors.
> - `oninput="validateName()"` — Calls the validation function **every time the user types a character**, giving real-time feedback (not just on submit).
> - `maxlength="10"` on the phone input — An HTML attribute that prevents typing more than 10 characters in the browser itself.
>
> **CSS Highlights:**
> - `.error-msg { display: none; }` — Error messages are hidden by default using CSS.
> - `.valid` and `.invalid` — Two CSS classes that change the input border color: green (`#4CAF50`) for valid, pink/red (`#E91E63`) for invalid.
> - `.password-strength` — A thin colored bar (`height: 5px`) that visually shows password strength. Its width and color change based on the password score.
> - `.btn-submit:disabled { background: #ccc; cursor: not-allowed; }` — Styles a disabled button with grey background and a "not allowed" cursor icon.
> - `transition: border-color 0.3s` — Creates a smooth 0.3-second animation when border colors change (instead of an instant jump).
>
> **JavaScript — Utility Functions (`showError` & `clearError`):**
> - `showError(id, msg)` — Displays an error for a specific field:
>   - Finds the error `<div>` by appending `'Error'` to the field's ID (e.g., `'fullname'` → `'fullnameError'`).
>   - Sets the error text with `.textContent = msg`.
>   - Makes the error visible with `.style.display = 'block'`.
>   - Adds the `invalid` CSS class (red border) and removes `valid` class using `classList`.
> - `clearError(id)` — Hides the error and marks the field as valid:
>   - Hides the error `<div>` with `.style.display = 'none'`.
>   - Removes `invalid` class and adds `valid` class (green border).
>
> **JavaScript — `validateName()`:**
> - Gets the trimmed value from the `fullname` input.
> - First checks: is the name at least 2 characters? If not → show error.
> - Then checks: does it match `/^[A-Za-z ]+$/`? This regex allows only letters and spaces. If "Rahul123" is entered, it fails because of the digits.
> - If both checks pass → `clearError('fullname')` and return `true`.
>
> **JavaScript — `validateEmail()`:**
> - Uses regex `/^[^\s@]+@[^\s@]+\.[^\s@]+$/` to verify: text before `@`, text after `@`, a dot, and text after the dot.
> - Example: `"rahul@gmail.com"` ✅, `"rahul@"` ❌ (nothing after @).
>
> **JavaScript — `validatePhone()`:**
> - Uses regex `/^\d{10}$/` — exactly 10 digits, nothing else.
> - Example: `"9876543210"` ✅, `"98765"` ❌ (only 5 digits).
>
> **JavaScript — `validatePassword()`:**
> - **Step 1:** Checks minimum length of 8 characters. If too short → show error and set strength bar to "weak" (red, 33% width).
> - **Step 2:** Calculates a strength score (0 to 4) by testing for four categories:
>   - `/[A-Z]/` — Has at least one uppercase letter? (+1)
>   - `/[a-z]/` — Has at least one lowercase letter? (+1)
>   - `/[0-9]/` — Has at least one digit? (+1)
>   - `/[^A-Za-z0-9]/` — Has at least one special character (like @, #, !)? (+1)
> - **Step 3:** Updates the visual strength bar: score ≤ 2 = medium (orange, 66%), score ≥ 3 = strong (green, 100%).
> - **Step 4:** Strictly requires at least 1 uppercase AND 1 digit using `/(?=.*[A-Z])(?=.*\d)/`.
>
> **JavaScript — `validateConfirm()`:**
> - Compares the confirm password field with the original password field.
> - Uses `!==` (strict not-equal) — if they don't match exactly, shows an error.
>
> **JavaScript — `validateDOB()`:**
> - Checks if a date was selected (empty `value` means no selection).
> - Calculates approximate age: `current year - birth year`.
> - Validates age is between 16 and 100 (reasonable for a student registration form).
>
> **JavaScript — `validateGender()`:**
> - Checks if the `<select>` dropdown has a non-empty value.
> - The default option `<option value="">-- Select --</option>` has an empty `value`, so if the user hasn't selected anything, `val` is `""` which is falsy.
>
> **JavaScript — `handleSubmit(e)` (Most Important!):**
> - `e.preventDefault()` — Stops the browser from reloading the page (default form submit behavior).
> - Calls ALL seven validation functions using `&` (bitwise AND) instead of `&&` (logical AND).
> - **Why `&` instead of `&&`?** — With `&&` (logical AND), JavaScript **stops at the first `false`** (short-circuit evaluation). So if `validateName()` returns `false`, the remaining functions like `validateEmail()`, `validatePhone()` etc. would never run, and only the name error would show. With `&` (bitwise AND), JavaScript **evaluates ALL functions** regardless of individual results, so ALL errors are displayed at once — much better user experience!
> - If all validations pass (`isValid` is truthy), the form is hidden and a success message is shown.
> - `return false` at the end prevents any form submission in all cases (since we handle success display manually).

---

## Summary

| Concept | Example |
|---------|---------|
| Basic validation | Check empty, length, format |
| Regex patterns | `/^[A-Za-z ]+$/`, `/^\d{10}$/` |
| Real-time validation | `oninput` event + inline errors |
| Password strength | Check uppercase, digits, symbols |
| Submit prevention | `event.preventDefault()` |

---

*Day 29 of 55 | Unit 5 — JavaScript | Web Technology (25BCA060)*
