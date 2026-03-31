# Dummy Content — Form Validation Rules

Use the following field specifications and validation rules to build your form.

---

## Form Title
**Student Registration Form — Mandsaur University**

## Form Fields and Validation Rules

| Field | Input Type | Validation Rule | Error Message |
|-------|-----------|----------------|---------------|
| Full Name | text | Required, only letters and spaces, min 3 characters | "Please enter a valid name (letters only, at least 3 characters)" |
| Age | number | Required, must be between 1 and 150 | "Age must be between 1 and 150" |
| Email | email | Required, must contain @ and . (basic email pattern) | "Please enter a valid email address" |
| Phone | tel | Required, must be exactly 10 digits (Indian mobile) | "Phone number must be 10 digits" |
| Pincode | text | Required, must be exactly 6 digits | "Pincode must be 6 digits" |
| Password | password | Required, minimum 8 characters | "Password must be at least 8 characters long" |
| Confirm Password | password | Must match Password field | "Passwords do not match" |
| Course | select | Required, must select a value (not the default) | "Please select a course" |

## Dropdown Options for Course
- -- Select Course -- (default, invalid)
- BCA (Bachelor of Computer Applications)
- BBA (Bachelor of Business Administration)
- B.Com (Bachelor of Commerce)
- B.Sc CS (Bachelor of Science — Computer Science)
- MCA (Master of Computer Applications)

## Sample Valid Data (for testing)
- Name: Anjali Verma
- Age: 19
- Email: anjali.verma@example.com
- Phone: 9876543210
- Pincode: 458001
- Password: Mandsaur@123
- Confirm Password: Mandsaur@123
- Course: BCA

## Sample Invalid Data (for testing)
- Name: A (too short)
- Age: 200 (out of range)
- Email: anjali@com (missing dot)
- Phone: 12345 (too short)
- Pincode: 4580 (too short)
- Password: 1234 (too short)
- Confirm Password: different123
- Course: -- Select Course --
