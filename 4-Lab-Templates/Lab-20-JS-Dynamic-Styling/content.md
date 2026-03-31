# Dummy Content — Dynamic Styling

Use the following paragraphs, button labels, and specifications to build your page.

---

## Page Title
**Dynamic Text Styling with JavaScript**

---

## Paragraphs

### Paragraph 1 (id="para1")
Web development is the process of building and maintaining websites. It includes aspects such as web design, web publishing, web programming, and database management. A web developer may work on the front-end, back-end, or both sides of a website.

### Paragraph 2 (id="para2")
HTML stands for HyperText Markup Language. It is the standard markup language for creating web pages. HTML describes the structure of a web page and tells the browser how to display content. HTML elements are the building blocks of HTML pages.

### Paragraph 3 (id="para3")
JavaScript is a programming language that makes websites interactive. It can update content dynamically, control multimedia, animate images, and much more. JavaScript was created by Brendan Eich in 1995 and is now the most popular programming language in the world.

---

## Button Labels and Actions

| Button | Action | JavaScript Property |
|--------|--------|-------------------|
| Bold | Toggle bold on selected paragraph | `element.style.fontWeight` = `"bold"` / `"normal"` |
| Italic | Toggle italic on selected paragraph | `element.style.fontStyle` = `"italic"` / `"normal"` |
| Underline | Toggle underline on selected paragraph | `element.style.textDecoration` = `"underline"` / `"none"` |
| Reset All | Remove all custom styles from all paragraphs | Reset all style properties to default |
| Change Color | Change text color of selected paragraph | `element.style.color` = selected color |

---

## Additional Features (Bonus)

### Color Picker
- **Input Type:** `<input type="color" id="colorPicker" value="#000000">`
- **Usage:** Select a color, then click "Change Color" to apply it to the chosen paragraph

### Font Size Controls
| Button | Action |
|--------|--------|
| Increase Font (A+) | Increase font size of selected paragraph by 2px |
| Decrease Font (A-) | Decrease font size of selected paragraph by 2px |

---

## Paragraph Selection
Students should provide a way to select which paragraph to style. Options include:
- **Radio buttons** with labels "Paragraph 1", "Paragraph 2", "Paragraph 3"
- Or a **dropdown** to select the paragraph
- Or **click on a paragraph** to select it (highlight selected paragraph with a border)
