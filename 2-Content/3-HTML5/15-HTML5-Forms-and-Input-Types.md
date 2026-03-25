# Day 15 — HTML5 Forms & New Input Types

🔬 **Type:** Theory + Practical
🕐 **Duration:** 2 Hours
📚 **Unit:** 3 — HTML5
🧪 **Lab Experiments:** 12, 13

---

## Learning Objectives

By the end of this session, students will be able to:
- Create HTML forms with various input types
- Use HTML5-specific input types (date, email, range, color, etc.)
- Add form elements like radio buttons, checkboxes, dropdowns
- Apply basic HTML5 form validation attributes

---

## 1. HTML Forms Basics

> **Analogy:** An HTML form is like a **paper application form** at a bank. It has fields for name, address, date of birth, checkboxes for services, and a submit button. The form collects information and sends it somewhere for processing.

### Basic Form Structure

```html
<form action="/submit" method="POST">
    <label for="name">Name:</label>
    <input type="text" id="name" name="name">
    
    <button type="submit">Submit</button>
</form>
```

| Attribute | Purpose |
|-----------|---------|
| `action` | URL where form data is sent |
| `method` | `GET` (in URL) or `POST` (in body) |

---

## 2. Input Types

### Classic Input Types

```html
<input type="text" placeholder="Enter name">         <!-- Single-line text -->
<input type="password" placeholder="Enter password">  <!-- Hidden characters -->
<input type="email" placeholder="email@example.com">  <!-- Email validation -->
<input type="number" min="1" max="150">               <!-- Numbers only -->
<input type="tel" placeholder="Phone number">         <!-- Phone number -->
<input type="url" placeholder="https://...">          <!-- URL validation -->
```

### HTML5 New Input Types

```html
<input type="date">          <!-- Date picker -->
<input type="time">          <!-- Time picker -->
<input type="datetime-local"> <!-- Date + Time -->
<input type="month">         <!-- Month picker -->
<input type="week">          <!-- Week picker -->
<input type="color">         <!-- Color picker -->
<input type="range" min="0" max="100"> <!-- Slider -->
<input type="search">        <!-- Search box -->
<input type="file">          <!-- File upload -->
<input type="hidden">        <!-- Hidden field -->
```

### Multi-line Text

```html
<textarea rows="5" cols="40" placeholder="Enter your message..."></textarea>
```

---

## 3. Form Elements

### Radio Buttons (Choose ONE)

```html
<p>Gender:</p>
<input type="radio" id="male" name="gender" value="male">
<label for="male">Male</label>

<input type="radio" id="female" name="gender" value="female">
<label for="female">Female</label>

<input type="radio" id="other" name="gender" value="other">
<label for="other">Other</label>
```

> **Key:** All radio buttons in a group must share the same `name` attribute.

### Checkboxes (Choose MULTIPLE)

```html
<p>Hobbies:</p>
<input type="checkbox" id="reading" name="hobbies" value="reading">
<label for="reading">Reading</label>

<input type="checkbox" id="coding" name="hobbies" value="coding">
<label for="coding">Coding</label>

<input type="checkbox" id="gaming" name="hobbies" value="gaming">
<label for="gaming">Gaming</label>
```

### Dropdown (`<select>`)

```html
<label for="subject">Favorite Subject:</label>
<select id="subject" name="subject">
    <option value="" disabled selected>-- Choose --</option>
    <option value="web">Web Technology</option>
    <option value="ds">Data Structures</option>
    <option value="math">Mathematics</option>
</select>
```

### Datalist (Searchable Dropdown)

```html
<label for="city">City:</label>
<input list="cities" id="city" name="city">
<datalist id="cities">
    <option value="Mandsaur">
    <option value="Indore">
    <option value="Bhopal">
    <option value="Mumbai">
</datalist>
```

---

## 4. HTML5 Validation Attributes

```html
<input type="text" required>                    <!-- Must fill -->
<input type="text" minlength="3" maxlength="50"> <!-- Length limits -->
<input type="number" min="1" max="150">          <!-- Number range -->
<input type="text" pattern="[A-Za-z]+" title="Letters only"> <!-- Regex -->
<input type="email" required>                    <!-- Must be valid email -->
```

| Attribute | Purpose |
|-----------|---------|
| `required` | Field must be filled |
| `minlength` / `maxlength` | Text length constraints |
| `min` / `max` | Number range constraints |
| `pattern` | Regex pattern to match |
| `placeholder` | Hint text in empty field |
| `autofocus` | Auto-focus on page load |
| `readonly` | Can see but not edit |
| `disabled` | Grayed out, can't interact |

---

## Practical Session

### 🧪 Lab Experiment 12: User Information Form

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Information Form</title>
</head>
<body style="font-family: Arial; max-width: 600px; margin: 30px auto;">

    <h1>Student Registration Form</h1>
    <hr>

    <form action="#" method="POST">
        
        <p>
            <label for="fullname"><strong>Full Name:</strong></label><br>
            <input type="text" id="fullname" name="fullname" 
                   placeholder="Enter your full name" required 
                   style="width: 100%; padding: 8px; margin-top: 5px;">
        </p>

        <p>
            <label for="age"><strong>Age:</strong></label><br>
            <input type="number" id="age" name="age" 
                   min="16" max="60" placeholder="e.g., 20" required
                   style="width: 100%; padding: 8px; margin-top: 5px;">
        </p>

        <p>
            <label for="address"><strong>Address:</strong></label><br>
            <textarea id="address" name="address" rows="3" 
                      placeholder="Enter your address" required
                      style="width: 100%; padding: 8px; margin-top: 5px;"></textarea>
        </p>

        <p>
            <label for="subject"><strong>Favorite Subject:</strong></label><br>
            <select id="subject" name="subject" required
                    style="width: 100%; padding: 8px; margin-top: 5px;">
                <option value="" disabled selected>-- Select Subject --</option>
                <option value="Web Technology">Web Technology</option>
                <option value="Data Structures">Data Structures</option>
                <option value="Mathematics">Mathematics</option>
                <option value="English">English</option>
            </select>
        </p>

        <p>
            <label for="movie"><strong>Favorite Movie:</strong></label><br>
            <input type="text" id="movie" name="movie" 
                   placeholder="e.g., 3 Idiots"
                   style="width: 100%; padding: 8px; margin-top: 5px;">
        </p>

        <p>
            <label for="singer"><strong>Favorite Singer:</strong></label><br>
            <input type="text" id="singer" name="singer" 
                   placeholder="e.g., Arijit Singh"
                   style="width: 100%; padding: 8px; margin-top: 5px;">
        </p>

        <p>
            <button type="submit" 
                    style="padding: 10px 30px; background: #1565C0; color: white; border: none; cursor: pointer;">
                Submit
            </button>
            <button type="reset" style="padding: 10px 30px;">Reset</button>
        </p>

    </form>

</body>
</html>
```

---

### 🧪 Lab Experiment 13: Advanced Form Elements

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Advanced Form</title>
</head>
<body style="font-family: Arial; max-width: 600px; margin: 30px auto;">

    <h1>Complete Registration Form</h1>
    <hr>

    <form action="#" method="POST">

        <!-- Text fields from Lab 12 -->
        <fieldset>
            <legend><strong>Personal Information</strong></legend>
            
            <p>
                <label for="name">Name:</label><br>
                <input type="text" id="name" name="name" required 
                       style="width:100%; padding:8px;">
            </p>
            <p>
                <label for="email">Email:</label><br>
                <input type="email" id="email" name="email" required 
                       placeholder="you@example.com" style="width:100%; padding:8px;">
            </p>
            <p>
                <label for="pwd">Password:</label><br>
                <input type="password" id="pwd" name="password" 
                       minlength="8" required style="width:100%; padding:8px;">
            </p>
            <p>
                <label for="dob">Date of Birth:</label><br>
                <input type="date" id="dob" name="dob" style="padding:8px;">
            </p>
        </fieldset>

        <br>

        <!-- Radio Buttons -->
        <fieldset>
            <legend><strong>Gender</strong></legend>
            <input type="radio" id="male" name="gender" value="male" required>
            <label for="male">Male</label> &nbsp;
            <input type="radio" id="female" name="gender" value="female">
            <label for="female">Female</label> &nbsp;
            <input type="radio" id="other" name="gender" value="other">
            <label for="other">Other</label>
        </fieldset>

        <br>

        <!-- Checkboxes -->
        <fieldset>
            <legend><strong>Interests</strong></legend>
            <input type="checkbox" id="web" name="interests" value="web">
            <label for="web">Web Development</label><br>
            <input type="checkbox" id="app" name="interests" value="app">
            <label for="app">App Development</label><br>
            <input type="checkbox" id="ai" name="interests" value="ai">
            <label for="ai">Artificial Intelligence</label><br>
            <input type="checkbox" id="game" name="interests" value="game">
            <label for="game">Game Development</label>
        </fieldset>

        <br>

        <!-- HTML5 Special Inputs -->
        <fieldset>
            <legend><strong>Preferences</strong></legend>
            <p>
                <label for="fcolor">Favorite Color:</label>
                <input type="color" id="fcolor" name="fcolor" value="#1565C0">
            </p>
            <p>
                <label for="skill">Skill Level (1-10):</label>
                <input type="range" id="skill" name="skill" min="1" max="10" value="5">
            </p>
            <p>
                <label for="website">Personal Website:</label><br>
                <input type="url" id="website" name="website" 
                       placeholder="https://yoursite.com" style="width:100%; padding:8px;">
            </p>
        </fieldset>

        <br>
        <p>
            <button type="submit" style="padding:12px 40px; background:#4CAF50; color:white; border:none; cursor:pointer; font-size:16px;">
                ✅ Submit
            </button>
            <button type="reset" style="padding:12px 40px; font-size:16px;">🔄 Reset</button>
        </p>
    </form>

</body>
</html>
```

---

## Summary

| Element | Purpose |
|---------|---------|
| `<form>` | Container for form elements |
| `<input type="...">` | Various input types |
| `<textarea>` | Multi-line text input |
| `<select>` + `<option>` | Dropdown menu |
| `<datalist>` | Searchable suggestions |
| `<fieldset>` + `<legend>` | Group related form fields |
| `<label>` | Accessible label for inputs |
| `required`, `pattern`, `min/max` | HTML5 validation |

---

*Day 15 of 55 | Unit 3 — HTML5 | Web Technology (25BCA060)*
