# Simple Omelette Recipe — Front-end Build

A responsive recipe card built from a provided style guide and design mockups
(mobile: 375px, desktop: 1440px). Built with plain, semantic HTML and CSS
only — no framework, no build step.

## Project structure

```
recipe-page/
├── index.html
├── styles.css
├── README.md
└── assets/
    └── images/
        └── image-omelette.jpg
```

## How it was built, step by step

### 1. Read the style guide first

Before writing any markup, the tokens in `style-guide.md` were pulled out
and turned into CSS custom properties: the five colors (white, three stone
shades, brown, two rose shades), the two font families (Young Serif for
headings, Outfit for body text), and the two reference widths (375px mobile,
1440px desktop). Keeping these as `:root` variables means every color or
font change happens in one place.

### 2. Structured the content semantically

Rather than wrapping everything in generic `<div>`s, each part of the page
uses the HTML element that actually describes it:

| Content                                                | Element                                                            |
| ------------------------------------------------------ | ------------------------------------------------------------------ |
| The whole recipe card                                  | `<main>`                                                           |
| Recipe title and intro copy                            | `<header>` inside an `<article>`                                   |
| Preparation time, ingredients, instructions, nutrition | `<section>`, each with its own `<h2>` linked via `aria-labelledby` |
| The ingredient list                                    | `<ul>`, unordered — order doesn't matter                           |
| The cooking steps                                      | `<ol>`, ordered — sequence matters                                 |
| The nutrition facts                                    | `<table>` with `<th scope="row">` labels                           |

Using `<ol>` for the instructions and `<ul>` for the ingredients is
deliberate: one is a sequence, the other is not, and screen readers announce
them differently.

### 3. Laid the page out with Flexbox

No grid was needed for a single-column card, so Flexbox handles every layout
job:

- `body` uses `display: flex` to center the card on larger screens.
- `.recipe-card__body` is a flex column, so the vertical spacing between
  sections comes from a single `gap` value instead of repeated margins.
- Each list (`.prep-time__list`, `.ingredients__list`, `.instructions__list`)
  is a flex column so its own item spacing is independent of the section
  around it.
- List markers (bullets, numbers) are drawn with `::before` pseudo-elements
  rather than default `list-style`, so their color can match the section's
  accent (rose for prep time, brown for ingredients and instructions).

### 4. Checked accessibility basics

- Every section heading is programmatically tied to its section with
  `aria-labelledby`.
- The nutrition table uses `<th scope="row">` so a screen reader announces
  "Calories, 277kcal" as a pair, not two unrelated cells.
- The one true image (the finished omelette) has descriptive `alt` text.
- A `.visually-hidden` utility class hides the table `<caption>` visually
  while keeping it available to assistive technology.

## Fonts

Loaded from Google Fonts in `index.html`:

- **Young Serif** (400) — headings only
- **Outfit** (400, 600, 700) — body copy, labels, table values
