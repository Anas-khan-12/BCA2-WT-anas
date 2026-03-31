# Lab 21: JavaScript — Display a Hidden Div

## Experiment
Write a JavaScript program to display a hidden div.

## Objective
Learn to show and hide HTML elements dynamically using JavaScript by manipulating the `display` CSS property, and implement toggle functionality with button text updates.

## Instructions
1. Open `index.html` and `script.js` in VS Code
2. Use the content from `content.md` for the page and hidden sections
3. Create a page with a visible introduction and a hidden student profile card
4. The profile div should start with `style="display: none;"`
5. In `script.js`, write a `toggleProfile()` function that:
   - Shows the div if it is hidden (`display = "block"`)
   - Hides the div if it is visible (`display = "none"`)
   - Updates the button text between "Show Profile" and "Hide Profile"
6. **Bonus:** Add additional hidden sections (Marks table, Attendance) with their own toggle buttons
7. Test by clicking the button to show/hide the profile card
8. Save and preview in Chrome

## Expected Output
A page with a toggle button that reveals and hides a student profile card. The button text updates based on the current state.

## Grading Criteria
- [ ] Div is initially hidden on page load (3 marks)
- [ ] Button click reveals the hidden div (3 marks)
- [ ] Toggle functionality — show/hide works both ways (3 marks)
- [ ] Smooth appearance with proper layout (3 marks)
- [ ] Clean, well-commented code (3 marks)

**Total: 15 marks**

## Reference
See `2-Content/5-JavaScript/28-DOM-Manipulation.md` in the course repository.
