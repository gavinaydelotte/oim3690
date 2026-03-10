# CSS Study Guide

---

## 1. Selectors

Selectors target which HTML elements your styles apply to.

| Selector | Example | Targets |
|---|---|---|
| Element | `p` | all `<p>` tags |
| Class | `.card` | elements with `class="card"` |
| ID | `#header` | element with `id="header"` |
| Descendant | `div p` | `<p>` anywhere inside a `<div>` |
| Child | `ul > li` | direct `<li>` children of `<ul>` |
| Adjacent sibling | `h1 + p` | `<p>` immediately after `<h1>` |
| Pseudo-class | `a:hover` | `<a>` when hovered |
| Pseudo-element | `p::first-line` | first line of a `<p>` |
| Universal | `*` | every element |
| Attribute | `input[type="text"]` | inputs with a specific attribute |

---

## 2. Specificity

When multiple rules target the same element, the **most specific** rule wins.

**Specificity order (lowest → highest):**

1. Element selectors — `p`, `h1` → **0-0-1**
2. Class / pseudo-class selectors — `.nav`, `:hover` → **0-1-0**
3. ID selectors — `#header` → **1-0-0**
4. Inline styles — `style="..."` → highest (avoid)
5. `!important` — overrides everything (use sparingly)

> **Tip:** If two rules have the same specificity, the one that appears **last** in the CSS wins.

---

## 3. Box Model

Every HTML element is a rectangular box made of four layers (inside out):

```
+---------------------------+
|         margin            |
|  +---------------------+  |
|  |      border         |  |
|  |  +---------------+  |  |
|  |  |    padding    |  |  |
|  |  |  +---------+  |  |  |
|  |  |  | content |  |  |  |
|  |  |  +---------+  |  |  |
|  |  +---------------+  |  |
|  +---------------------+  |
+---------------------------+
```

- **Content** — the actual text or image
- **Padding** — space inside the border (background color fills here)
- **Border** — line around the padding
- **Margin** — space outside the border (always transparent)

**`box-sizing` property:**
- `content-box` (default) — width/height applies to content only; padding and border add on top
- `border-box` — width/height includes padding and border (much easier to work with)

```css
* {
  box-sizing: border-box;
}
```

---

## 4. Display

The `display` property controls how an element is rendered in the page flow.

| Value | Behavior |
|---|---|
| `block` | takes full width, starts on new line (`div`, `p`, `h1`) |
| `inline` | sits in line with text, ignores width/height (`span`, `a`) |
| `inline-block` | inline flow but respects width/height |
| `none` | hides the element and removes it from layout |
| `flex` | enables Flexbox on the container |
| `grid` | enables Grid on the container |

---

## 5. Layout

### Flexbox
Controls how children line up inside a container **in one direction** (row or column).

```css
.container {
  display: flex;
  flex-direction: row;        /* row (default) | column */
  justify-content: center;    /* alignment along main axis */
  align-items: center;        /* alignment along cross axis */
  flex-wrap: wrap;            /* allow items to wrap to next line */
  gap: 16px;                  /* space between items */
}
```

**`justify-content` values:**

| Value | Effect |
|---|---|
| `flex-start` | items packed at the start |
| `flex-end` | items packed at the end |
| `center` | items centered |
| `space-between` | equal space between items, none at edges |
| `space-around` | equal space around each item |
| `space-evenly` | equal space between items and edges |

**Child (flex item) properties:**
```css
.item {
  flex-grow: 1;    /* how much the item grows to fill space */
  flex-shrink: 1;  /* how much the item shrinks when space is tight */
  flex-basis: auto; /* starting size before grow/shrink applies */
  align-self: center; /* overrides align-items for this item only */
}
```

---

### Grid
Defines a **two-dimensional** layout with explicit rows and columns.

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr; /* 3 equal columns */
  grid-template-rows: 200px auto;     /* first row 200px, second fits content */
  gap: 16px;                          /* space between all cells */
  column-gap: 16px;                   /* space between columns only */
  row-gap: 8px;                       /* space between rows only */
}
```

**Shorthand helpers:**
```css
/* repeat(count, size) */
grid-template-columns: repeat(3, 1fr);   /* same as 1fr 1fr 1fr */
grid-template-columns: repeat(4, 200px); /* four 200px columns */

/* minmax(min, max) — column is at least 150px, at most 1fr */
grid-template-columns: repeat(3, minmax(150px, 1fr));
```

**Unit reference:**
| Unit | Meaning |
|---|---|
| `fr` | fraction of remaining space |
| `px` | fixed pixel size |
| `%` | percentage of the container |
| `auto` | size to content |

**Placing items manually:**
```css
.item {
  grid-column: 1 / 3;   /* span from column line 1 to line 3 (2 columns wide) */
  grid-row: 1 / 2;      /* span row line 1 to line 2 (1 row tall) */
}

/* Shorthand */
.item {
  grid-column: span 2;  /* span 2 columns from current position */
}
```

**Named template areas:**
```css
.container {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-columns: 200px 1fr;
}

.header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main    { grid-area: main; }
.footer  { grid-area: footer; }
```

**Aligning grid items:**
```css
.container {
  justify-items: center;   /* horizontal alignment of items in their cell */
  align-items: center;     /* vertical alignment of items in their cell */
}

.item {
  justify-self: end;   /* override for one item */
  align-self: start;
}
```

---

## 6. Forms

### Basic Reset & Styling

Browsers apply different default styles to form elements — always reset first.

```css
input,
textarea,
select {
  box-sizing: border-box;
  width: 100%;
  padding: 8px 12px;
  font-size: 1rem;
  font-family: inherit;       /* match surrounding text */
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: #fff;
}
```

### Focus & Interaction States

```css
input:focus,
textarea:focus,
select:focus {
  outline: none;              /* remove default browser outline */
  border-color: #0066cc;
  box-shadow: 0 0 0 3px rgba(0, 102, 204, 0.2); /* soft glow */
}

input:disabled,
textarea:disabled {
  background-color: #f5f5f5;
  cursor: not-allowed;
  opacity: 0.6;
}

input::placeholder {
  color: #aaa;
  font-style: italic;
}
```

### Buttons

```css
button {
  display: inline-block;
  padding: 10px 20px;
  font-size: 1rem;
  font-family: inherit;
  background-color: #0066cc;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

button:hover {
  background-color: #0052a3;
}

button:active {
  background-color: #003d7a;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
```

### Labels & Layout

```css
label {
  display: block;         /* put label on its own line above input */
  margin-bottom: 4px;
  font-weight: bold;
  font-size: 0.9rem;
}

.form-group {
  margin-bottom: 16px;    /* space between each label+input pair */
}
```

```html
<!-- Recommended HTML structure -->
<div class="form-group">
  <label for="email">Email</label>
  <input type="email" id="email" name="email" placeholder="you@example.com">
</div>
```

### Checkboxes & Radio Buttons

```css
input[type="checkbox"],
input[type="radio"] {
  width: auto;            /* override the 100% width from the reset */
  margin-right: 8px;
  cursor: pointer;
  accent-color: #0066cc;  /* tints the checkbox/radio with your brand color */
}
```

**Key form pseudo-classes:**
| Pseudo-class | When it applies |
|---|---|
| `:focus` | element is focused / active |
| `:hover` | mouse is over the element |
| `:disabled` | input has the `disabled` attribute |
| `:checked` | checkbox or radio is checked |
| `:valid` | input value passes HTML validation |
| `:invalid` | input value fails HTML validation |
| `::placeholder` | styles the placeholder text |

### Form Accessibility

Accessible forms work for everyone — keyboard users, screen reader users, and people with visual impairments.

**1. Always link labels to inputs with `for` / `id`**

```html
<!-- The for attribute must match the input's id -->
<label for="username">Username</label>
<input type="text" id="username" name="username">
```

Never use placeholder text as a substitute for a label — placeholders disappear when the user starts typing.

**2. Never fully remove the focus outline**

```css
/* BAD — keyboard users can't tell what's focused */
input:focus {
  outline: none;
}

/* GOOD — replace it with a custom visible style */
input:focus {
  outline: none;
  border-color: #0066cc;
  box-shadow: 0 0 0 3px rgba(0, 102, 204, 0.35);
}
```

**3. Error states — use color AND an icon/text, not color alone**

Color-blind users can't rely on red alone to identify errors.

```css
.input-error {
  border-color: #cc0000;
  background-color: #fff5f5;
}

.error-message {
  color: #cc0000;
  font-size: 0.85rem;
  margin-top: 4px;
}
```

```html
<div class="form-group">
  <label for="email">Email</label>
  <input type="email" id="email" class="input-error" aria-describedby="email-error">
  <span class="error-message" id="email-error">⚠ Please enter a valid email address.</span>
</div>
```

**4. Use `aria` attributes when needed**

| Attribute | Purpose |
|---|---|
| `aria-required="true"` | tells screen readers the field is required |
| `aria-describedby="id"` | links the input to a hint or error message |
| `aria-invalid="true"` | marks the field as having an error |
| `aria-label="..."` | provides a label when no visible `<label>` exists |

```html
<input
  type="email"
  id="email"
  aria-required="true"
  aria-invalid="true"
  aria-describedby="email-error"
>
```

**5. Sufficient color contrast**

Text and UI elements must have enough contrast to be readable.

| Text size | Minimum contrast ratio |
|---|---|
| Normal text (< 18px) | 4.5 : 1 |
| Large text (≥ 18px or bold ≥ 14px) | 3 : 1 |
| UI components (borders, icons) | 3 : 1 |

> Use a tool like [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/) to verify your colors.

**6. Touch target size**

Buttons and inputs should be large enough to tap on mobile.

```css
button,
input,
select {
  min-height: 44px; /* Apple's recommended minimum tap target */
}
```

---

## 7. Responsive Design

Designing so your page looks good on all screen sizes.

### Viewport Meta Tag (required in HTML)

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

Without this, mobile browsers zoom out and your media queries won't work correctly.

---

### Media Queries

Apply styles **only when a condition is true**, like screen width.

**Basic syntax:**
```css
@media (condition) {
  /* styles that apply when condition is met */
}
```

**Most common conditions:**
```css
/* min-width: applies when screen is AT LEAST this wide */
@media (min-width: 768px) { ... }

/* max-width: applies when screen is NO WIDER than this */
@media (max-width: 767px) { ... }

/* range: between two sizes */
@media (min-width: 576px) and (max-width: 991px) { ... }

/* orientation */
@media (orientation: landscape) { ... }
@media (orientation: portrait)  { ... }
```

**Mobile-first approach (recommended):**
Write default styles for small screens, then add media queries to adjust for larger screens.

```css
/* Mobile default */
.container {
  display: flex;
  flex-direction: column;
  padding: 16px;
}

/* Tablet and up */
@media (min-width: 768px) {
  .container {
    flex-direction: row;
    padding: 32px;
  }
}

/* Desktop and up */
@media (min-width: 1200px) {
  .container {
    max-width: 1140px;
    margin: 0 auto;
  }
}
```

**Common breakpoints:**
| Name | Width | Typical device |
|---|---|---|
| Small | `< 576px` | small phones |
| Medium | `≥ 576px` | large phones |
| Large | `≥ 768px` | tablets |
| XL | `≥ 992px` | laptops |
| XXL | `≥ 1200px` | desktops |

---

### Responsive Units

Instead of fixed `px` values, use relative units that scale naturally:

| Unit | Relative to | Example use |
|---|---|---|
| `%` | parent element | `width: 50%` |
| `vw` | viewport width | `width: 100vw` |
| `vh` | viewport height | `height: 100vh` |
| `rem` | root font size (usually 16px) | `font-size: 1.5rem` |
| `em` | parent font size | `padding: 1em` |

---

### Other Responsive Techniques

```css
/* Images never overflow their container */
img {
  max-width: 100%;
  height: auto;
}

/* Prevent layout from stretching too wide on large screens */
.container {
  max-width: 1200px;
  margin: 0 auto;
}

/* Responsive grid that auto-fills columns */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 16px;
}
```

> `auto-fit` + `minmax` is one of the most useful responsive tricks — columns automatically wrap to the next line when they can't fit, no media query needed.

---

## 8. Transitions

Transitions animate a property smoothly from one value to another when it changes (e.g. on hover).

### Syntax

```css
selector {
  transition: property duration timing-function delay;
}
```

| Part | What it does | Example |
|---|---|---|
| `property` | which CSS property to animate | `background-color`, `all` |
| `duration` | how long the animation takes | `0.3s`, `300ms` |
| `timing-function` | speed curve of the animation | `ease`, `linear`, `ease-in-out` |
| `delay` | wait before starting | `0s`, `0.1s` |

### Basic Example

```css
button {
  background-color: blue;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: darkblue;
}
```

### Transitioning Multiple Properties

```css
/* Comma-separate multiple transitions */
.card {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
}

/* Transition everything at once (less performant) */
.card {
  transition: all 0.3s ease;
}
```

### Timing Functions

| Value | Effect |
|---|---|
| `ease` | starts fast, slows at end (default) |
| `linear` | constant speed throughout |
| `ease-in` | starts slow, ends fast |
| `ease-out` | starts fast, ends slow |
| `ease-in-out` | slow start and end, fast in the middle |

### Commonly Transitioned Properties

```css
.element {
  /* Color & background */
  transition: color 0.2s ease;
  transition: background-color 0.2s ease;
  transition: opacity 0.3s ease;

  /* Size & spacing */
  transition: width 0.3s ease;
  transition: padding 0.2s ease;

  /* Movement & shape */
  transition: transform 0.3s ease;
  transition: border-radius 0.2s ease;

  /* Shadow */
  transition: box-shadow 0.3s ease;
}
```

> **Tip:** `transform` and `opacity` are the most performant properties to animate — they don't trigger layout recalculations. Prefer them over animating `width`, `height`, or `margin`.
