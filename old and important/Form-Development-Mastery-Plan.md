# Form Development Mastery Plan

## From Basic Forms to Complex Form Architecture

> **Prerequisites**: HTML, CSS, JavaScript (ES6+), React basics (useState, useEffect, useRef, useContext)

---

## 🎯 How This Plan Works

| Component             | Purpose                                      |
| --------------------- | -------------------------------------------- |
| **Concepts**          | Theory you must understand                   |
| **Exercises**         | Hands-on coding tasks (do ALL of them)       |
| **Mini Project**      | Apply learned concepts in real scenario      |
| **Checkpoint**        | Self-assessment before moving on             |
| **Library Exercises** | Same task done with different form libraries |

---

# 🟢 PHASE 1: VANILLA FORMS FOUNDATION (Week 1-2)

---

## Section 1: Pure HTML + JavaScript Forms

### Concepts to Master

- HTML form elements (input, select, textarea, radio, checkbox, datalist, fieldset)
- Form submission (default behavior, `event.preventDefault()`)
- `FormData` API
- Input types (text, email, password, number, date, file, range, color, tel, url, hidden)
- HTML5 built-in validation attributes (required, minlength, maxlength, pattern, min, max, step)
- Constraint Validation API (`checkValidity()`, `setCustomValidity()`, `reportValidity()`)
- `input`, `change`, `blur`, `focus`, `submit`, `reset` events
- Accessibility: labels, aria attributes, focus management, error announcements

---

### Exercises (Complete All)

**Exercise 1.1: Registration Form with Native Validation** ⭐

```
Build a registration form with:
- Full Name (required, min 3 chars)
- Email (required, valid email pattern)
- Password (required, min 8 chars, must contain uppercase, lowercase, number, special char)
- Confirm Password (must match password)
- Date of Birth (required, must be 18+)
- Phone (optional, valid BD/US phone pattern)
- Submit button

Requirements:
- Use ONLY HTML5 validation attributes (no JS validation yet)
- Style valid/invalid states with :valid/:invalid/:focus pseudo-classes
- Show browser's default validation messages
- Prevent form submission if invalid
```

**Exercise 1.2: Custom Validation Messages** ⭐⭐

```
Take Exercise 1.1 and upgrade:
- Replace ALL browser default messages with custom Bengali/English messages
- Use setCustomValidity() and reportValidity()
- Show validation on blur (not just on submit)
- Show real-time password strength meter (weak/medium/strong)
- Show real-time character count for name field
- Highlight fields with red border on error, green on valid
- Show error messages below each field (not browser tooltip)
```

**Exercise 1.3: FormData API Deep Dive** ⭐⭐

```
Create a "Job Application" form:
- Personal Info: Name, Email, Phone
- Address: Street, City, State, Zip, Country (dropdown)
- Experience: Company Name, Role, Duration (add multiple dynamically)
- Skills: Checkboxes (JavaScript, Python, React, Node, SQL, etc.)
- Resume: File upload (PDF only, max 5MB)
- Cover Letter: Textarea

Requirements:
- On submit, use FormData to collect ALL data
- Convert FormData to plain object and display as JSON
- Convert FormData to URL-encoded string
- Log FormData entries using for...of
- Handle file separately - show file name, size, type
- Add "Add More Experience" button to dynamically add experience fields
```

**Exercise 1.4: Multi-Step Form (Wizard)** ⭐⭐⭐

```
Build a 4-step form wizard WITHOUT any framework:

Step 1: Personal Info
  - Name, Email, Phone, Avatar upload (with preview)

Step 2: Address Info
  - Street, City, State, Zip
  - "Same as permanent address" checkbox that copies fields

Step 3: Preferences
  - Favorite color (color picker)
  - Notification preference (radio: email/sms/both/none)
  - Interests (multi-select checkboxes)
  - Bio (textarea with max 500 chars, show remaining)

Step 4: Review & Submit
  - Show all collected data in a summary card
  - Show uploaded avatar preview
  - "Edit" button per section that goes back to that step
  - Final submit button

Requirements:
- Progress bar showing current step
- "Next" and "Previous" navigation
- Validate each step before allowing "Next"
- Data persists across steps (don't lose data going back)
- Save draft to localStorage on each step change
- Restore from localStorage on page reload
- Keyboard navigation (Enter = Next, Escape = Previous)
- Smooth CSS transitions between steps
```

**Exercise 1.5: Dynamic Form Builder** ⭐⭐⭐⭐

```
Build a form that builds OTHER forms:

Part A - Form Builder:
- User adds fields by clicking "Add Field"
- For each field, user configures:
  - Field type (text, email, number, select, checkbox, radio, textarea, date)
  - Label text
  - Placeholder text
  - Required or not
  - Validation rules (min/max length, pattern, custom regex)
  - Options (for select/radio/checkbox)
- User can reorder fields (drag and drop or up/down buttons)
- User can delete fields
- User can duplicate fields
- "Preview" button shows the generated form
- "Export JSON" button exports form config as JSON
- "Import JSON" button loads a form config

Part B - Form Renderer:
- Takes the JSON config and renders a fully functional form
- All validations work
- Submit collects all data and shows as JSON

No framework. Pure HTML + CSS + JavaScript.
```

**Exercise 1.6: Inline Editable Table** ⭐⭐⭐

```
Build a data table where each cell is editable:

- Display a table of 10 users (name, email, role, status)
- Click any cell to turn it into an input/select
- Press Enter or click outside to save
- Press Escape to cancel edit
- "Role" column is a dropdown (Admin, User, Moderator)
- "Status" column is a toggle (Active/Inactive)
- Track all changes (highlight modified cells in yellow)
- "Save All Changes" button that collects modified data
- "Undo" button that reverts last change
- Validate: email must be valid, name can't be empty

No framework. Pure JavaScript.
```

### Mini Project: Survey Form Application ⭐⭐⭐⭐

```
Build a complete survey creation and response system:

Admin Panel:
- Create surveys with title and description
- Add different question types:
  - Short text
  - Long text (textarea)
  - Single choice (radio)
  - Multiple choice (checkbox)
  - Rating (1-5 stars)
  - Linear scale (1-10)
  - Date
  - File upload
- Set questions as required/optional
- Reorder questions
- Preview survey
- Save survey to localStorage

Response Panel:
- Load survey from localStorage
- Render all questions dynamically
- Validate required fields
- Submit responses
- Store responses in localStorage

Results Panel:
- Show response count
- Show summary for each question
- Bar chart for choice questions (pure CSS, no chart library)
- Average for rating/scale questions
- List view for text responses

All in Vanilla HTML + CSS + JavaScript. No frameworks.
```

### Checkpoint 1

- [ ] Can I build any form with HTML5 validation?
- [ ] Can I use FormData API confidently?
- [ ] Do I understand all form events (input, change, blur, submit)?
- [ ] Can I build multi-step forms from scratch?
- [ ] Can I dynamically add/remove form fields?
- [ ] Can I handle file uploads and previews?
- [ ] Can I save/restore form state from localStorage?

---

# 🟡 PHASE 2: REACT FORMS - CONTROLLED & UNCONTROLLED (Week 3-4)

---

## Section 2: React Native Forms (No Library)

### Concepts to Master

- Controlled vs Uncontrolled components
- `useState` for form state
- `useRef` for uncontrolled inputs
- Handling onChange for different input types
- Form submission in React
- Lifting state up for shared form data
- Custom form hooks
- Error state management
- Debouncing user input
- Optimistic UI updates

---

### Exercises (Complete All)

**Exercise 2.1: Controlled Form Basics** ⭐

```
Build a "Contact Us" form (fully controlled):
- Name, Email, Subject (dropdown), Message (textarea)
- Priority (radio: Low, Medium, High)
- Attach file (checkbox to show/hide file input)
- Character count for message (max 1000)

Requirements:
- Every field managed with useState
- Display live form state as JSON below the form (for debugging)
- Clear form on successful submit
- Disable submit button until all required fields filled
- Show "Submitting..." state on submit (simulate API with setTimeout)
```

**Exercise 2.2: Uncontrolled Form with useRef** ⭐⭐

```
Rebuild Exercise 2.1 using ONLY useRef (uncontrolled):
- No useState for any form field
- Use ref.current.value to read values on submit
- Use FormData on the form element to collect all data
- Compare: When is uncontrolled better? Document in comments.

Then build a HYBRID form:
- Some fields controlled (real-time validation needed)
- Some fields uncontrolled (no real-time feedback needed)
- Document WHY each field is controlled or uncontrolled
```

**Exercise 2.3: Custom useForm Hook** ⭐⭐⭐

```
Create a reusable custom hook: useForm()

const { values, errors, touched, handleChange, handleBlur, handleSubmit, resetForm, setFieldValue, isSubmitting, isValid } = useForm({
  initialValues: { name: '', email: '', age: '' },
  validate: (values) => {
    const errors = {};
    if (!values.name) errors.name = 'Name is required';
    if (!values.email) errors.email = 'Email is required';
    return errors;
  },
  onSubmit: async (values) => {
    await api.submit(values);
  }
});

Requirements:
- Track touched fields (show errors only after user touches field)
- Support async validation
- Support field-level validation
- Support form-level validation
- isValid computed from errors
- isSubmitting state during async submit
- resetForm() resets to initialValues
- setFieldValue() for programmatic updates
- Dirty check (has form changed from initial values?)

Test with 3 different forms to prove reusability.
```

**Exercise 2.4: Complex Nested Form State** ⭐⭐⭐

```
Build an "Employee Onboarding" form with deeply nested state:

State structure:
{
  personal: {
    firstName: '',
    lastName: '',
    dob: '',
    gender: '',
    photo: null
  },
  contact: {
    email: '',
    phone: '',
    address: {
      street: '',
      city: '',
      state: '',
      zip: '',
      country: ''
    }
  },
  employment: {
    position: '',
    department: '',
    startDate: '',
    salary: '',
    type: '' // full-time, part-time, contract
  },
  education: [
    { degree: '', institution: '', year: '', gpa: '' }
  ],
  skills: [''],
  documents: {
    resume: null,
    idProof: null,
    certificates: []
  },
  emergencyContact: {
    name: '',
    relation: '',
    phone: ''
  }
}

Requirements:
- Handle deeply nested state updates WITHOUT mutating state
- "Add Education" button adds new education entry
- "Remove Education" button removes entry (min 1)
- "Add Skill" button adds new skill input
- Skills have autocomplete suggestions from a predefined list
- File uploads with preview for photo, resume, ID
- Multiple file upload for certificates
- Address autocomplete (fake data, just demonstrate the pattern)
- Conditional fields: if type = "contract", show "Contract End Date"
- Form completion percentage shown in progress bar
- All sections collapsible/expandable
```

**Exercise 2.5: Dependent Dropdowns (Cascading Select)** ⭐⭐⭐

```
Build a location selector with dependent/cascading dropdowns:

- Country → State → City → Area
- When Country changes, State resets and loads relevant states
- When State changes, City resets and loads relevant cities
- When City changes, Area loads relevant areas

Additional:
- Loading spinner while "fetching" data (simulate with setTimeout)
- Search/filter within dropdown options
- "Other" option that shows a text input
- Remember last selection in sessionStorage
- Build this as a reusable <CascadingSelect> component

Then build another cascading example:
- Category → Subcategory → Product
- (Prove the component is truly reusable)
```

**Exercise 2.6: Real-time Search Form with Debouncing** ⭐⭐

```
Build a search interface:
- Search input that filters results as you type
- Debounce input by 300ms (build custom useDebounce hook)
- Show "Searching..." while debounce timer is active
- Advanced filters panel:
  - Category (multi-select checkboxes)
  - Price range (two inputs: min and max)
  - Rating (star selector)
  - Sort by (dropdown)
  - Date range (from-to date inputs)
- "Apply Filters" and "Clear All" buttons
- URL sync: all filters reflected in URL query params
- Back/Forward button properly restores filter state
- Show active filters as removable chips/tags
- Result count updates in real-time
```

**Exercise 2.7: Form with Array Fields** ⭐⭐⭐

```
Build an "Invoice Generator" form:

Header:
- Invoice Number (auto-generated)
- Date
- Due Date
- From: Company Name, Address, Email
- To: Client Name, Address, Email

Line Items (dynamic array):
- Item Description
- Quantity (number)
- Unit Price (number)
- Tax % (number)
- Discount % (number)
- Line Total (auto-calculated: qty * price - discount + tax)

Each line item:
- "Remove" button (min 1 line item)
- "Duplicate" button
- Drag to reorder

Footer:
- Subtotal (auto-calculated)
- Tax Total (auto-calculated)
- Discount Total (auto-calculated)
- Grand Total (auto-calculated)
- Notes (textarea)
- Payment Terms (dropdown)

Requirements:
- All calculations happen in real-time
- Format numbers as currency
- "Add Line Item" button
- "Clear All Items" button (with confirmation)
- "Save Draft" to localStorage
- "Generate PDF" (just create a print-friendly view)
```

**Exercise 2.8: Accessible Form Component Library** ⭐⭐⭐⭐

```
Build a reusable form component library with FULL accessibility:

Components to build:
1. <FormField> - wrapper with label, input, error message, helper text
2. <TextInput> - with all input types support
3. <TextArea> - with auto-resize and character count
4. <Select> - custom styled but accessible dropdown
5. <RadioGroup> - grouped radio buttons
6. <CheckboxGroup> - grouped checkboxes
7. <Toggle> - switch/toggle component
8. <DatePicker> - custom date picker (keyboard navigable)
9. <FileUpload> - drag and drop + click to upload
10. <RangeSlider> - custom range with min/max labels
11. <Combobox> - searchable dropdown with keyboard nav
12. <TagInput> - input that creates tags/chips

Each component MUST have:
- Proper aria-labels, aria-describedby, aria-invalid
- Keyboard navigation (Tab, Enter, Space, Arrow keys, Escape)
- Screen reader announcements for errors
- Focus management (focus trap in dropdowns)
- Disabled state
- Loading state
- Error state
- Helper text
- Required indicator

Test with a screen reader (or Chrome accessibility audit).
Use these components to build a complete registration form.
```

### Mini Project: Multi-page Job Application Portal ⭐⭐⭐⭐

```
Build a complete job application system in React (no form library):

Page 1: Job Listing
- Display 10+ jobs with title, company, location, salary range
- Filter by: location, job type, salary range, experience level
- Search by title/company
- Click a job to go to application form

Page 2: Application Form (multi-step wizard)
Step 1: Personal Information
  - Photo upload with crop/resize preview
  - Full legal name, preferred name
  - Email, Phone (with country code selector)
  - LinkedIn URL, Portfolio URL, GitHub URL (optional)
  - Current location

Step 2: Education & Experience
  - Education entries (dynamic, add/remove)
  - Work experience entries (dynamic, add/remove)
  - For each experience:
    - Company, Title, Start Date, End Date (or "Current")
    - Description (rich text - just use textarea with markdown preview)
  - Skills with proficiency level (slider for each)

Step 3: Application Details
  - Cover letter (textarea, min 100 words, max 500 words, show count)
  - Salary expectation (range selector)
  - Available start date
  - References (add 2-3 dynamically)
  - "How did you hear about us?" (dropdown + "Other" text)

Step 4: Documents
  - Resume upload (PDF/DOC, max 5MB)
  - Certificates (multiple files)
  - Portfolio files (multiple, optional)
  - Show file list with remove option

Step 5: Review & Submit
  - Show complete summary
  - "Edit" links for each section
  - Terms & Conditions checkbox
  - Submit with loading state
  - Success page with application ID

Requirements:
- All validation with clear error messages
- Progress indicator
- Auto-save draft every 30 seconds
- Restore draft on return
- "Save & Exit" button
- Responsive design (mobile-friendly)
- Form completion percentage
- Conditional fields
- At least 2 custom hooks for form logic
```

### Checkpoint 2

- [ ] Do I understand controlled vs uncontrolled components deeply?
- [ ] Can I build a custom useForm hook?
- [ ] Can I handle deeply nested form state?
- [ ] Can I build dynamic array fields?
- [ ] Can I handle dependent dropdowns?
- [ ] Can I make forms fully accessible?
- [ ] Can I build multi-step forms in React?

---

# 🟠 PHASE 3: REACT HOOK FORM (Week 5-7)

---

## Section 3: React Hook Form (RHF) Mastery

### Concepts to Master

- Why React Hook Form? (performance, re-renders)
- `useForm()` hook configuration
- `register()` function
- `handleSubmit()` pattern
- `watch()` vs `getValues()`
- `setValue()`, `trigger()`, `reset()`
- Error handling with `formState.errors`
- `useFieldArray` for dynamic fields
- `useFormContext` and `FormProvider`
- `useWatch()` for optimized watching
- `Controller` component for third-party UI components
- Resolver pattern (Yup, Zod, Joi integration)
- Form validation modes: onChange, onBlur, onSubmit, onTouched, all
- Performance: why RHF minimizes re-renders
- DevTools integration

### Setup

```bash
npm install react-hook-form @hookform/resolvers @hookform/devtools
npm install zod yup joi # validation libraries
```

---

### Exercises (Complete All)

**Exercise 3.1: RHF Basics - Registration Form** ⭐

```
Rebuild the registration form using React Hook Form:
- Name, Email, Password, Confirm Password, DOB, Phone
- Use register() for all fields
- Use handleSubmit() for form submission
- Display errors using formState.errors
- Use validation mode: "onTouched"

Compare with your vanilla React version:
- Count re-renders (use React DevTools Profiler)
- Count lines of code
- Document the differences
```

**Exercise 3.2: Validation with Zod Schema** ⭐⭐

```
Create a Zod schema for a "Create Account" form:

const schema = z.object({
  username: z.string()
    .min(3, 'Min 3 characters')
    .max(20, 'Max 20 characters')
    .regex(/^[a-zA-Z0-9_]+$/, 'Only alphanumeric and underscore'),
  email: z.string().email('Invalid email'),
  password: z.string()
    .min(8, 'Min 8 characters')
    .regex(/[A-Z]/, 'Need uppercase')
    .regex(/[0-9]/, 'Need number')
    .regex(/[^A-Za-z0-9]/, 'Need special character'),
  confirmPassword: z.string(),
  age: z.number().min(18).max(100),
  website: z.string().url().optional().or(z.literal('')),
  role: z.enum(['user', 'admin', 'moderator']),
  terms: z.literal(true, { errorMap: () => ({ message: 'Must accept terms' }) }),
  preferences: z.object({
    newsletter: z.boolean(),
    theme: z.enum(['light', 'dark', 'system']),
    language: z.string()
  })
}).refine(data => data.password === data.confirmPassword, {
  message: 'Passwords do not match',
  path: ['confirmPassword']
});

Requirements:
- Use @hookform/resolvers/zod
- Show all validation errors
- Add async validation: check if username is taken (simulate API)
- Add cross-field validation (password !== username)
- Create and test at least 5 different Zod schemas for practice
```

**Exercise 3.3: Validation with Yup Schema** ⭐⭐

```
Take the SAME form from Exercise 3.2 and implement with Yup:

const schema = yup.object({
  username: yup.string().required().min(3).max(20),
  email: yup.string().email().required(),
  // ... same validations as Zod version
});

Requirements:
- Use @hookform/resolvers/yup
- Implement ALL the same validations
- Compare Zod vs Yup:
  - Syntax differences
  - TypeScript inference (Zod is better?)
  - Bundle size
  - Error message customization
  - Document comparison in comments
```

**Exercise 3.4: useFieldArray - Dynamic Invoice** ⭐⭐⭐

```
Rebuild the Invoice Generator from Exercise 2.7 using React Hook Form:

const { fields, append, remove, move, insert, prepend, swap, update } = useFieldArray({
  control,
  name: 'lineItems'
});

Requirements:
- All line items managed with useFieldArray
- "Add Item" uses append()
- "Remove" uses remove()
- "Move Up/Down" uses move()
- "Insert Above" uses insert()
- "Duplicate" uses insert() with copied values
- Real-time calculation with watch()
- Validate: min 1 line item, max 50
- Validate each line item (qty > 0, price > 0)
- Nested field arrays: each line item has sub-items

Add tax configuration:
- Tax rates array (also useFieldArray)
- Each line item can select which tax rates apply

Compare code complexity with vanilla React version.
```

**Exercise 3.5: Controller Component - Third Party UI** ⭐⭐⭐

```
Build a form using React Hook Form + Material UI (or any UI library):

If using MUI:
npm install @mui/material @emotion/react @emotion/styled

Form fields:
- TextField → use register() directly
- Select → use Controller
- Autocomplete → use Controller
- DatePicker → use Controller
- Slider → use Controller
- Switch → use Controller
- Rating → use Controller
- RadioGroup → use Controller
- Checkbox → use Controller

Requirements:
- Every MUI component properly integrated with RHF
- All validations working through RHF (not MUI validation)
- Error messages displayed using MUI's error/helperText props
- Form values fully typed with TypeScript
- Submit shows all collected data

If no MUI, use React-Select, React-DatePicker, or any custom components.
Key learning: WHEN and WHY to use Controller vs register()
```

**Exercise 3.6: FormProvider & useFormContext** ⭐⭐⭐

```
Build a large form split across multiple components:

<FormProvider {...methods}>
  <form onSubmit={handleSubmit(onSubmit)}>
    <PersonalInfoSection />    // separate component file
    <AddressSection />         // separate component file
    <EmploymentSection />      // separate component file
    <EducationSection />       // separate component file (with useFieldArray)
    <DocumentsSection />       // separate component file
    <FormActions />            // submit, reset, save draft
  </form>
</FormProvider>

Each section component:
- Uses useFormContext() to access form methods
- Has its own validation
- Can show/hide based on other fields (useWatch)
- Has its own error summary

Requirements:
- NO prop drilling for form methods
- Each section is in its own file
- Each section can be tested independently
- "Section Complete" indicator when all fields in section are valid
- Collapsible sections
- Section-level validation errors summary
- Global form error summary at top
```

**Exercise 3.7: Async Operations & Server Integration** ⭐⭐⭐

```
Build a "User Profile Settings" form with real API patterns:

Features:
1. Load existing data from API (simulate):
   - Use reset() to populate form with fetched data
   - Show loading skeleton while fetching

2. Async field validation:
   - Username availability check (debounced, 500ms)
   - Email uniqueness check
   - Show loading spinner next to field during check
   - Show ✓ or ✗ based on result

3. Auto-save:
   - Debounced auto-save on every change (2 seconds)
   - Show "Saving...", "Saved", "Error saving" status
   - Don't auto-save if form has validation errors

4. Optimistic updates:
   - Show success immediately, revert on API error
   - Handle race conditions (user submits while auto-save is running)

5. Dirty tracking:
   - formState.isDirty to show "Unsaved changes" warning
   - Warn user on page navigation if dirty (useBeforeUnload)
   - "Discard Changes" button that resets to last saved state

6. Partial submit:
   - "Save Section" buttons that submit only that section
   - Different API endpoints for different sections
```

**Exercise 3.8: Complex Conditional Logic** ⭐⭐⭐⭐

```
Build an "Insurance Quote" form with heavy conditional logic:

Step 1: Insurance Type
  - Auto / Home / Life / Health / Travel

Step 2: (Changes completely based on Step 1)

If AUTO:
  - Vehicle Make → Model → Year (cascading)
  - VIN number (validate format)
  - Usage (personal/commercial)
  - Annual mileage (range slider)
  - If commercial → additional business fields
  - Number of drivers (for each: name, age, license years, accidents)
  - Coverage type (liability/comprehensive/collision)

If HOME:
  - Property type (house/apartment/condo)
  - Year built
  - Square footage
  - If house → lot size, number of floors, garage
  - If apartment → floor number, building security
  - Alarm system (yes/no → if yes, type of alarm)
  - Previous claims (dynamic list)

If HEALTH:
  - Plan type (individual/family)
  - If family → add family members (dynamic)
  - Pre-existing conditions (multi-select + "Other" text)
  - Current medications
  - Preferred hospital/doctor

If TRAVEL:
  - Destination country (multi-select)
  - Travel dates
  - If dates > 30 days → "Long-term travel" additional fields
  - Adventure sports (checkbox → additional premium warning)
  - Existing health coverage (yes/no → details)

Requirements:
- Use useWatch() for conditional rendering
- Schema validation changes based on selected type
- Dynamic Zod schema (different schema per insurance type)
- unregister() fields when they become hidden
- Don't lose data if user goes back and changes type (warn first)
- Quote calculation preview (fake formula)
- All with React Hook Form
```

**Exercise 3.9: Performance Optimization** ⭐⭐⭐

```
Build a form with 100+ fields and optimize it:

Scenario: "Product Catalog Entry" form with:
- 20 text fields
- 10 select fields
- 10 checkbox groups (5 options each = 50 checkboxes)
- 5 file uploads
- 15 number fields
- Dynamic array with up to 20 items

Requirements:
- Use React.memo on sections
- Use useWatch() instead of watch() where possible
- Use shouldUnregister for conditional fields
- Profile re-renders with React DevTools
- Create two versions:
  1. Naive (everything re-renders)
  2. Optimized (minimal re-renders)
- Document render counts for both versions
- Show before/after performance metrics

Key learning: Why RHF is fast and how to keep it fast
```

**Exercise 3.10: RHF DevTools & Debugging** ⭐⭐

```
Take any form from above and add RHF DevTools:

import { DevTool } from "@hookform/devtools";

<DevTool control={control} />

Tasks:
- Watch all form values change in real-time
- Observe which fields are touched/dirty
- See validation errors as they appear
- Test trigger() to manually validate fields
- Test setValue() with different options:
  - shouldValidate: true/false
  - shouldDirty: true/false
  - shouldTouch: true/false
- Test reset() with different configurations
- Test getValues() vs watch() behavior difference
- Document what each DevTools panel shows
```

### Mini Project: E-Commerce Checkout System ⭐⭐⭐⭐⭐

```
Build a complete checkout flow using React Hook Form:

Page 1: Shopping Cart
- Product list with quantity selectors
- "Proceed to Checkout" button

Page 2: Checkout (multi-step form)

Step 1: Customer Information
  - Returning customer? Auto-fill from email lookup (simulate)
  - Guest checkout option
  - Name, Email, Phone

Step 2: Shipping Address
  - Address form with country → state → city cascade
  - "Save this address" checkbox
  - Multiple saved addresses (radio to select)
  - "Add new address" option
  - Address validation (simulate API)

Step 3: Billing
  - "Same as shipping" checkbox
  - If different, show billing address form
  - Billing name

Step 4: Payment
  - Payment method: Credit Card / Debit Card / UPI / Net Banking / COD

  If Credit/Debit Card:
    - Card Number (formatted: XXXX XXXX XXXX XXXX, Luhn validation)
    - Card Holder Name
    - Expiry (MM/YY format, must be future date)
    - CVV (3-4 digits, masked input)
    - Card type auto-detection (Visa/Mastercard/Amex) from number

  If UPI:
    - UPI ID (validate format: xxx@bank)

  If Net Banking:
    - Bank selection (searchable dropdown)

  If COD:
    - Just confirmation

Step 5: Order Review
  - Complete order summary
  - Shipping cost calculation
  - Tax calculation
  - Discount code input (validate against fake codes)
  - Editable quantities
  - Total with breakdown
  - Place Order button

Step 6: Confirmation
  - Order number
  - Estimated delivery
  - Order details
  - "Continue Shopping" button

Requirements:
- React Hook Form with Zod validation
- useFieldArray for cart items
- FormProvider across steps
- Auto-save progress
- Handle "Back to cart" (restore cart state)
- Loading states for all async operations
- Error boundaries for payment failures
- Responsive design
- Input masking (credit card, phone, expiry)
- Real-time order total calculation
```

### Checkpoint 3

- [ ] Can I use register(), handleSubmit(), watch(), setValue() confidently?
- [ ] Can I write Zod and Yup schemas for complex validation?
- [ ] Can I use useFieldArray for dynamic fields?
- [ ] Can I use Controller for third-party components?
- [ ] Can I split forms with FormProvider/useFormContext?
- [ ] Can I handle async validation and auto-save?
- [ ] Can I optimize form performance?
- [ ] Can I build complex conditional forms?

---

# 🔴 PHASE 4: FORMIK (Week 8-9)

---

## Section 4: Formik Deep Dive

### Concepts to Master

- Formik vs React Hook Form (philosophy differences)
- `<Formik>` component
- `useFormik()` hook
- `<Form>`, `<Field>`, `<ErrorMessage>` components
- `<FieldArray>` component
- `validationSchema` (Yup integration)
- `validate` function
- `touched`, `dirty`, `isValid`, `isSubmitting`
- `setFieldValue`, `setFieldTouched`, `setFieldError`
- Fast Field component (`<FastField>`)
- Formik context (`useFormikContext`)
- Render prop pattern vs hook pattern

### Setup

```bash
npm install formik yup
```

---

### Exercises (Complete All)

**Exercise 4.1: Formik Basics** ⭐

```
Build the registration form with Formik (3 different ways):

Version A: Component Approach
<Formik initialValues={...} onSubmit={...}>
  <Form>
    <Field name="email" />
    <ErrorMessage name="email" />
  </Form>
</Formik>

Version B: useFormik Hook
const formik = useFormik({
  initialValues: {...},
  onSubmit: (values) => {...}
});
<form onSubmit={formik.handleSubmit}>
  <input {...formik.getFieldProps('email')} />
</form>

Version C: Render Props
<Formik>
  {({ values, errors, handleChange }) => (
    <Form>...</Form>
  )}
</Formik>

Compare all 3:
- Which is most readable?
- Which gives most control?
- When to use which?
```

**Exercise 4.2: Formik + Yup Deep Validation** ⭐⭐

```
Build a "Company Registration" form with complex Yup schema:

const schema = yup.object({
  company: yup.object({
    name: yup.string().required(),
    type: yup.string().oneOf(['LLC', 'Corp', 'Partnership', 'Sole Proprietor']),
    taxId: yup.string().when('type', {
      is: (type) => type !== 'Sole Proprietor',
      then: (schema) => schema.required('Tax ID required for business entities'),
      otherwise: (schema) => schema.notRequired()
    }),
    employees: yup.number().min(1).max(100000),
    revenue: yup.number().when('employees', {
      is: (emp) => emp > 50,
      then: (schema) => schema.required('Revenue required for companies with 50+ employees')
    })
  }),
  contacts: yup.array().of(
    yup.object({
      name: yup.string().required(),
      email: yup.string().email().required(),
      role: yup.string().required(),
      isPrimary: yup.boolean()
    })
  ).min(1, 'At least one contact required')
  .test('has-primary', 'Must have exactly one primary contact',
    (contacts) => contacts?.filter(c => c.isPrimary).length === 1
  ),
  address: yup.object({
    street: yup.string().required(),
    city: yup.string().required(),
    state: yup.string().required(),
    zip: yup.string().matches(/^\d{5}(-\d{4})?$/, 'Invalid ZIP')
  })
});

Requirements:
- All conditional validations working
- Array validation with custom tests
- Cross-field validation
- Nested object validation
- Custom error messages
- Async validation (company name uniqueness)
```

**Exercise 4.3: Formik FieldArray** ⭐⭐⭐

```
Build a "Team Builder" form:

- Team Name, Description
- Team Members (FieldArray):
  - Name, Email, Role (dropdown)
  - Skills (nested FieldArray inside member!)
  - Each skill: name, proficiency (1-5)
  - "Add Skill" per member
  - "Remove Skill" per member
- "Add Member" button
- "Remove Member" button (min 2 members)

Requirements:
- Nested FieldArray (skills inside members inside form)
- Validate each level independently
- Show member count and total skill count
- "Auto-assign roles" button (sets roles based on skills)
- Drag to reorder members (or up/down buttons)
- "Import from CSV" button (parse CSV, populate FieldArray)
```

**Exercise 4.4: FastField Optimization** ⭐⭐

```
Build a form with 50+ fields and compare:

Version A: All <Field> components
Version B: All <FastField> components

Measure:
- Re-renders per keystroke (React DevTools)
- Performance difference
- When does FastField NOT work? (demonstrate breaking case)

Document:
- FastField rules: field must be independent, no cross-field validation
- When it re-renders vs when it doesn't
```

**Exercise 4.5: Formik vs RHF - Same Form, Both Libraries** ⭐⭐⭐

```
Take the E-Commerce Checkout from Phase 3 Mini Project.
Rebuild the ENTIRE thing with Formik.

Then create a comparison document:

| Feature              | React Hook Form      | Formik              |
|---------------------|--------------------  |---------------------|
| Bundle Size         |                      |                     |
| Re-renders          |                      |                     |
| TypeScript Support  |                      |                     |
| Learning Curve      |                      |                     |
| Array Fields        |                      |                     |
| Validation          |                      |                     |
| Performance (50+ fields) |                 |                     |
| Code Readability    |                      |                     |
| Community/Ecosystem |                      |                     |
| DevTools            |                      |                     |

Include actual numbers and code comparisons.
```

### Checkpoint 4

- [ ] Can I use Formik with both component and hook patterns?
- [ ] Can I write complex Yup schemas?
- [ ] Can I use FieldArray with nested arrays?
- [ ] Do I understand FastField optimization?
- [ ] Can I compare Formik vs RHF objectively?

---

# 🟣 PHASE 5: OTHER FORM LIBRARIES & PATTERNS (Week 10-11)

---

## Section 5A: TanStack Form (formerly React Form)

### Setup

```bash
npm install @tanstack/react-form @tanstack/zod-form-adapter zod
```

### Exercises

**Exercise 5A.1: TanStack Form Basics** ⭐⭐

```
Build a user profile form with TanStack Form:

import { useForm } from '@tanstack/react-form'
import { zodValidator } from '@tanstack/zod-form-adapter'

const form = useForm({
  defaultValues: { name: '', email: '', bio: '' },
  onSubmit: async ({ value }) => { ... },
  validatorAdapter: zodValidator()
})

Requirements:
- Field-level validation
- Form-level validation
- Async validation (email uniqueness)
- Linked/dependent fields
- Show validation on blur
- TypeScript fully typed

Compare with RHF:
- API differences
- Philosophy differences
- Performance differences
```

**Exercise 5A.2: TanStack Form Advanced** ⭐⭐⭐

```
Build the Invoice Generator with TanStack Form:
- Dynamic line items using form array API
- Nested validation
- Cross-field computation
- Side effects on field changes
- Compare complexity with RHF useFieldArray
```

---

## Section 5B: React Final Form

### Setup

```bash
npm install final-form react-final-form
```

### Exercises

**Exercise 5B.1: React Final Form Basics** ⭐⭐

```
Build a subscription preferences form:

import { Form, Field } from 'react-final-form'

<Form
  onSubmit={onSubmit}
  validate={validate}
  render={({ handleSubmit, form, submitting, pristine, values }) => (
    <form onSubmit={handleSubmit}>
      <Field name="email" component="input" />
    </form>
  )}
/>

Features:
- Subscription-based re-rendering (only re-render what changes)
- Field-level subscriptions
- Form Spy for cross-field effects
- Decorator pattern for calculated fields
- Compare with RHF and Formik render counts
```

**Exercise 5B.2: Final Form with Decorators & Mutators** ⭐⭐⭐

```
Build a mortgage calculator form:
- Loan Amount, Interest Rate, Term (years)
- Calculated fields: Monthly Payment, Total Interest, Total Payment
- Use final-form-calculate decorator for auto-computation
- Use mutators for complex field updates
- Compare this decorator pattern with RHF's watch()
```

---

## Section 5C: Validation Library Deep Dives

### Exercises

**Exercise 5C.1: Zod Advanced Patterns** ⭐⭐⭐

```
Master these Zod patterns (create a form for each):

1. Discriminated Unions:
const schema = z.discriminatedUnion('type', [
  z.object({ type: z.literal('personal'), firstName: z.string(), lastName: z.string() }),
  z.object({ type: z.literal('business'), companyName: z.string(), taxId: z.string() })
]);

2. Recursive schemas (tree/nested comments):
const commentSchema: z.ZodType<Comment> = z.object({
  text: z.string(),
  replies: z.lazy(() => commentSchema.array())
});

3. Transform & Preprocess:
const schema = z.object({
  price: z.string().transform(val => parseFloat(val)),
  email: z.string().transform(val => val.toLowerCase().trim()),
  tags: z.string().transform(val => val.split(',').map(t => t.trim()))
});

4. Custom error maps:
z.setErrorMap((issue, ctx) => {
  // Custom Bengali/English error messages
});

5. Schema composition (merge, extend, pick, omit, partial):
const baseUser = z.object({ name: z.string(), email: z.string() });
const createUser = baseUser.extend({ password: z.string() });
const updateUser = baseUser.partial();
const loginUser = baseUser.pick({ email: true }).extend({ password: z.string() });

Build forms that use EACH of these patterns.
```

**Exercise 5C.2: Joi Validation** ⭐⭐

```
npm install joi

Build the same registration form with Joi validation:

const schema = Joi.object({
  username: Joi.string().alphanum().min(3).max(30).required(),
  email: Joi.string().email().required(),
  password: Joi.string().pattern(new RegExp('^[a-zA-Z0-9]{3,30}$')),
  repeat_password: Joi.ref('password'),
  birth_year: Joi.number().integer().min(1900).max(2013),
  type: Joi.string().valid('personal', 'business'),
  companyName: Joi.when('type', {
    is: 'business',
    then: Joi.string().required(),
    otherwise: Joi.string().allow('')
  })
})
.with('password', 'repeat_password');

Note: Joi is HEAVY for frontend. Document when to use Joi vs Zod vs Yup.
Create a comparison table with pros/cons of each.
```

**Exercise 5C.3: Valibot (Lightweight Alternative)** ⭐⭐

```
npm install valibot

Build the same form with Valibot:

import * as v from 'valibot';

const Schema = v.object({
  email: v.pipe(v.string(), v.email()),
  password: v.pipe(v.string(), v.minLength(8)),
  age: v.pipe(v.number(), v.minValue(18))
});

Compare bundle size: Valibot vs Zod vs Yup vs Joi
Document tree-shaking differences.
```

---

## Section 5D: Input Masking & Formatting

### Exercises

**Exercise 5D.1: Input Masking** ⭐⭐

```
npm install react-input-mask    # or imask or cleave.js

Build a form with masked inputs:
- Phone: (XXX) XXX-XXXX
- Credit Card: XXXX XXXX XXXX XXXX
- Expiry: MM/YY
- SSN: XXX-XX-XXXX
- Time: HH:MM AM/PM
- IP Address: XXX.XXX.XXX.XXX
- Currency: $XX,XXX.XX

Integrate masked inputs with:
1. React Hook Form (using Controller)
2. Formik (using custom component)

Bonus: Build your OWN input mask utility without any library.
```

**Exercise 5D.2: Rich Form Inputs** ⭐⭐⭐

```
Build a form with these custom input types (integrate with RHF):

1. Color Picker (custom, not native)
2. Star Rating (1-5, half stars)
3. Tag Input (type and press enter to add tags)
4. Markdown Editor (textarea with preview)
5. Code Editor (syntax highlighted textarea - use simple approach)
6. Signature Pad (draw signature with mouse/touch)
7. OTP Input (4-6 separate boxes, auto-focus next)
8. Phone Input (country code dropdown + number)
9. Address Autocomplete (with Google Maps or fake data)
10. Image Cropper (upload → crop → preview)

Each input must:
- Work as controlled component
- Integrate with React Hook Form via Controller
- Have proper validation
- Be accessible
- Handle focus/blur properly
```

---

# ⚫ PHASE 6: ADVANCED PATTERNS & ARCHITECTURE (Week 12-14)

---

## Section 6: Complex Form Patterns

**Exercise 6.1: Form State Machine** ⭐⭐⭐⭐

```
Build a form with XState or useReducer state machine:

States:
- idle → filling → validating → submitting → success/error → idle

Each state defines:
- Which fields are enabled/disabled
- Which buttons are visible
- What happens on submit
- Error recovery behavior

Use case: Multi-step application form where:
- Step transitions are state machine events
- Can't skip steps (enforced by machine)
- Can go back (machine remembers state)
- Timeout handling (session expires)
- Error recovery (retry from failed step)

Implement with useReducer first, then with XState if desired.
```

**Exercise 6.2: Form with Undo/Redo** ⭐⭐⭐

```
Build a form that supports Undo/Redo:

Requirements:
- Every field change recorded in history stack
- Ctrl+Z to undo last change
- Ctrl+Y to redo
- History panel showing all changes with timestamps
- Click any history entry to restore that state
- "Compare with original" button showing diff
- Max 50 history entries (drop oldest)

Implement as a custom hook: useUndoableForm()
Integrate with React Hook Form.
```

**Exercise 6.3: Collaborative Form (Multiplayer)** ⭐⭐⭐⭐

```
Build a form that multiple users can edit simultaneously:

- Open form in 2 browser tabs (simulate 2 users)
- Use BroadcastChannel API for cross-tab communication
- Show other user's cursor position (which field they're editing)
- Real-time sync of field values across tabs
- Conflict resolution: last-write-wins or merge
- Show "User 2 is editing this field" indicator
- Lock field while another user is typing (optional)

This simulates Google Docs-style collaboration for forms.
```

**Exercise 6.4: Form Analytics & Interaction Tracking** ⭐⭐⭐

```
Build a form analytics system that tracks:

- Time spent on each field
- Field visit order
- Number of corrections per field
- Fields that cause users to abandon form
- Total form completion time
- Drop-off point (which step users leave)
- Error rate per field
- Paste events (detect pasted content)
- Rage clicks (multiple rapid clicks = frustration)

Display analytics dashboard with:
- Completion funnel
- Average time per field (bar chart)
- Error hotspots (heatmap-style)
- Abandonment rate per step

Integrate with any form (RHF or vanilla) as a reusable wrapper/hook.
```

**Exercise 6.5: Schema-Driven Form Generator** ⭐⭐⭐⭐⭐

```
Build a production-quality form generator system:

Input: JSON Schema or custom schema
Output: Fully functional, validated React form

Schema format:
{
  "title": "User Registration",
  "sections": [
    {
      "title": "Personal Info",
      "fields": [
        {
          "name": "fullName",
          "type": "text",
          "label": "Full Name",
          "required": true,
          "validation": { "minLength": 2, "maxLength": 100 },
          "placeholder": "Enter your name"
        },
        {
          "name": "email",
          "type": "email",
          "label": "Email",
          "required": true,
          "asyncValidation": { "endpoint": "/api/check-email", "debounce": 500 }
        },
        {
          "name": "country",
          "type": "select",
          "label": "Country",
          "options": "dynamic:countries",
          "dependents": ["state", "city"]
        },
        {
          "name": "profilePhoto",
          "type": "file",
          "label": "Profile Photo",
          "accept": "image/*",
          "maxSize": "5MB",
          "preview": true
        }
      ],
      "conditions": {
        "visible": "always"
      }
    }
  ],
  "submitAction": { "method": "POST", "url": "/api/register" }
}

Requirements:
- Parse schema and render form dynamically
- Support ALL field types (text, email, select, checkbox, radio, file, date, number, textarea, rich-text, etc.)
- Dynamic validation from schema (build Zod schema from JSON)
- Conditional visibility (field A depends on field B's value)
- Cascading dropdowns from schema
- Array fields from schema
- Async validation from schema
- Custom components registry (plug in custom field renderers)
- Section-based layout
- Multi-step from schema
- Generate TypeScript types from schema
- Schema editor UI (visual builder that outputs the JSON)
- Preview mode
- Submit handling based on schema

This is a CAPSTONE project. Use React Hook Form + Zod under the hood.
```

**Exercise 6.6: Form Testing Mastery** ⭐⭐⭐

```
Write comprehensive tests for your forms:

Setup:
npm install -D @testing-library/react @testing-library/jest-dom @testing-library/user-event vitest

Test categories:

1. Unit Tests:
   - Validation functions (Zod/Yup schemas)
   - Form utility hooks
   - Input formatters/parsers

2. Integration Tests:
   - Form renders all fields
   - Validation errors appear on invalid input
   - Validation errors clear on valid input
   - Form submits with correct data
   - Error states display properly
   - Loading states work
   - Conditional fields show/hide

3. Interaction Tests:
   - Tab through fields in correct order
   - Enter key submits form
   - Escape key cancels/closes
   - Dynamic fields add/remove correctly
   - File upload works
   - Paste handling

4. Accessibility Tests:
   - All fields have labels
   - Error messages linked with aria-describedby
   - Focus management correct
   - Screen reader announcements

Write tests for at least 3 forms from previous exercises.
Aim for 80%+ coverage.
```

### FINAL CAPSTONE PROJECT: Full-Stack Form Platform ⭐⭐⭐⭐⭐

```
Build a "FormBuilder Pro" - a form building platform like Google Forms/Typeform:

Frontend (React + React Hook Form + Zod):

1. Dashboard
   - List all created forms
   - Create new form
   - Duplicate form
   - Delete form
   - View responses

2. Form Builder (Drag & Drop)
   - Left panel: field types to drag
   - Center: form preview/canvas
   - Right panel: field properties editor
   - Support 15+ field types
   - Conditional logic builder (if field X = value, show/hide field Y)
   - Validation rule builder
   - Theme/style customization
   - Multi-page/section support
   - Preview mode (desktop/mobile toggle)
   - Save/publish form

3. Form Renderer
   - Public URL for each form (/form/:id)
   - Renders form from saved schema
   - Full validation
   - File uploads
   - Progress bar for multi-step
   - Thank you page customization
   - Response limit option

4. Response Viewer
   - Table view of all responses
   - Individual response detail
   - Export to CSV/JSON
   - Basic charts (response distribution)
   - Filter/search responses

5. Settings
   - Form title, description
   - Open/close form
   - Response limit
   - Email notifications
   - Custom success message
   - Form embed code generator

Backend (Node.js + Express + MongoDB/PostgreSQL):
- CRUD APIs for forms
- CRUD APIs for responses
- File upload handling
- User authentication
- Form access control (public/private)

This is a complete production-level application.
Spend 2-3 weeks on this if needed.
```

---

## 📊 Library Comparison Quick Reference

| Feature        | Vanilla React  | React Hook Form  | Formik      | TanStack Form | Final Form |
| -------------- | -------------- | ---------------- | ----------- | ------------- | ---------- |
| Bundle Size    | 0KB            | ~9KB             | ~13KB       | ~12KB         | ~8KB       |
| Re-renders     | Many           | Minimal          | Many        | Minimal       | Minimal    |
| TypeScript     | Manual         | Excellent        | Good        | Excellent     | Good       |
| Learning Curve | Low            | Medium           | Medium      | Medium-High   | Medium     |
| Array Fields   | Manual         | useFieldArray    | FieldArray  | Built-in      | Built-in   |
| Validation     | Manual         | Resolver pattern | Yup default | Built-in      | Built-in   |
| Performance    | Depends        | Best             | Okay        | Great         | Great      |
| Community      | N/A            | Largest          | Large       | Growing       | Small      |
| DevTools       | React DevTools | Official DevTool | No          | No            | No         |
| Recommendation | Learning       | Production ✅    | Legacy      | Modern        | Niche      |

---

## 📋 Validation Library Comparison

| Feature          | Zod             | Yup             | Joi             | Valibot          |
| ---------------- | --------------- | --------------- | --------------- | ---------------- |
| Bundle Size      | ~14KB           | ~18KB           | ~40KB+          | ~1-5KB           |
| TypeScript       | First-class ✅  | Decent          | Weak            | First-class      |
| Tree-shaking     | No              | No              | No              | Yes ✅           |
| Schema Inference | Yes ✅          | Partial         | No              | Yes              |
| Ecosystem        | Large           | Large           | Large (backend) | Growing          |
| Best For         | Fullstack TS    | Legacy/Formik   | Backend         | Bundle-conscious |
| Recommendation   | Go-to choice ✅ | If using Formik | Backend only    | If size matters  |

---

## 🗓️ Study Schedule Summary

| Week  | Topic                             | Key Deliverable              |
| ----- | --------------------------------- | ---------------------------- |
| 1-2   | Vanilla HTML + JS Forms           | Survey Form Application      |
| 3-4   | React Native Forms                | Job Application Portal       |
| 5-7   | React Hook Form + Zod/Yup         | E-Commerce Checkout System   |
| 8-9   | Formik                            | Formik vs RHF Comparison     |
| 10-11 | Other Libraries + Advanced Inputs | Rich Input Component Library |
| 12-14 | Advanced Patterns + Capstone      | FormBuilder Pro Platform     |

---

## 💡 Tips for Success

1. **Do every exercise** - Don't skip even if it seems easy
2. **Build before reading docs** - Struggle first, then look up solutions
3. **Compare libraries** - Same form in different libraries teaches the most
4. **Focus on accessibility** - Forms without a11y are broken forms
5. **Test your forms** - If you can't test it, you don't understand it
6. **Read the source code** - Look at RHF/Formik internals to understand HOW they work
7. **Performance matters** - Profile re-renders, bundle size, and user experience
8. **TypeScript everything** - Forms are where TypeScript shines the most
9. **Mobile-first** - Always test forms on mobile viewport
10. **Real-world patterns** - Study forms on sites like Stripe, GitHub, Shopify

---

> **After completing this plan, you will be able to build ANY form in ANY complexity with ANY library confidently. Form development will never intimidate you again.**
