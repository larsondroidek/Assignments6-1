# Assignment 6.1 — Solution: Box Model Inspection

## Project Structure
```
box-model/
├── index.html
├── style.css
└── annotated-box-model.png   (annotated screenshot)
```

## index.html
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Box model inspection: content, padding, border and margin, with content-box and border-box." />
    <title>Box Model Inspection</title>
    <link rel="stylesheet" href="style.css" />
  </head>

  <body>
    <header>
      <h1>Box Model Inspection</h1>
      <p>Right-click a box &rarr; <strong>Inspect</strong> &rarr; look at the box model diagram in the <strong>Computed</strong> tab.</p>
    </header>

    <main>
      <!-- Box A: content-box, small padding, thin border, small margin -->
      <section class="box box-a">
        <h2>Box A</h2>
        <p>
          <code>content-box</code><br />
          width 200px + padding 10px + border 4px
        </p>
      </section>

      <!-- Box B: content-box, big padding, thick border, big margin -->
      <section class="box box-b">
        <h2>Box B</h2>
        <p>
          <code>content-box</code><br />
          width 200px + padding 30px + border 8px
        </p>
      </section>

      <!-- Box C: border-box, same numbers as Box B -->
      <section class="box box-c">
        <h2>Box C</h2>
        <p>
          <code>border-box</code><br />
          width 200px includes padding 30px + border 8px
        </p>
      </section>
    </main>
  </body>
</html>
```

## style.css
```css
body {
  font-family: Arial, sans-serif;
  background-color: #f3f4f6;
  color: #1f2937;
  margin: 0;
  padding: 24px;
}

header {
  margin-bottom: 24px;
}

/* Boxes sit side by side; align-items keeps their different heights visible */
main {
  display: flex;
  align-items: flex-start;
  flex-wrap: wrap;
}

/* Shared look (no sizing here, so each box can differ) */
.box {
  background-color: #bfdbfe;          /* shows through the padding area */
  border-style: solid;
  border-color: #1e3a8a;
  width: 200px;
}

.box h2 {
  margin: 0 0 8px;
  font-size: 1.2rem;
  background-color: #ffffff;           /* white = the CONTENT area */
}

.box p {
  margin: 0;
  font-size: 0.9rem;
  background-color: #ffffff;
}

/* Box A — content-box (default): total width = 200 + 2*10 + 2*4 = 228px */
.box-a {
  box-sizing: content-box;
  padding: 10px;
  border-width: 4px;
  margin: 10px;
}

/* Box B — content-box: total width = 200 + 2*30 + 2*8 = 276px */
.box-b {
  box-sizing: content-box;
  padding: 30px;
  border-width: 8px;
  margin: 20px;
}

/* Box C — border-box: width 200px INCLUDES padding and border -> total width = 200px */
.box-c {
  box-sizing: border-box;
  padding: 30px;
  border-width: 8px;
  border-style: dashed;
  margin: 30px;
}
```

## The Box Model
Every HTML element is a rectangular box made of four layers, from inside to outside:

| # | Layer | What it is | CSS property |
|---|-------|------------|--------------|
| 4 | **Content** | The text/images; its size is set by `width` and `height` | `width`, `height` |
| 3 | **Padding** | Space between the content and the border (shows the element's background) | `padding` |
| 2 | **Border** | The line around the padding | `border` |
| 1 | **Margin** | Transparent space outside the border that separates the element from its neighbours | `margin` |

## The Three Boxes
| Box | `box-sizing` | width | padding | border | margin | **Visible box width** (content + padding + border) |
|-----|--------------|-------|---------|--------|--------|-----------------------------------------------------|
| A | `content-box` | 200px | 10px | 4px | 10px | 200 + 2×10 + 2×4 = **228px** |
| B | `content-box` | 200px | 30px | 8px | 20px | 200 + 2×30 + 2×8 = **276px** |
| C | `border-box`  | 200px | 30px | 8px (dashed) | 30px | **200px** (padding and border are inside the width) |

I measured these in a browser (`getBoundingClientRect`): the visible boxes are 228px, 276px and 200px wide.

## `content-box` vs `border-box`
- **`content-box`** (the browser default): `width` sets only the **content**. Padding and border are **added** on top, so the box grows (Box B is 276px wide even though `width: 200px`).
- **`border-box`**: `width` includes **content + padding + border**. The box stays exactly 200px (Box C), and the content area shrinks instead (200 − 60 − 16 = **124px**).
- Box B and Box C have the same padding and border; the only difference is `box-sizing`, so the 76px width difference (276 vs 200) comes entirely from it.
- Most projects use `*, *::before, *::after { box-sizing: border-box; }` because widths become predictable.

## Inspect in Chrome DevTools (for your own screenshot)
1. Open `index.html` with Live Server.
2. Right-click Box B → **Inspect**.
3. In the **Styles** pane look at the rule `.box-b`; in the **Computed** tab you see the **box model diagram**: the outer layer is the **margin (orange)**, then **border (yellow)**, **padding (green)** and the **content (blue)** with its size (`200 × 78`, for example).
4. Hover over the element in the Elements panel — the page highlights each layer with the same colors.
5. Double-click a value in the diagram (for example padding) and type another number to see the box change live.
6. Do the same for Box A and Box C and compare the numbers.
7. Take a screenshot and label **content, padding, border, margin**.

## Annotated Screenshot
`annotated-box-model.png` (attached) is a labelled version of this page: the four layers are color-coded (orange = margin, yellow = border, green = padding, blue = content), numbered 1–4 on Box B, and each box shows its visible width. I created it from the rendered page, so you should still take your own screenshot with the DevTools diagram open, as the assignment asks, and label it the same way.

## Submission
- ✅ `index.html`, `style.css` (attached)
- 📸 Annotated screenshot (attached `annotated-box-model.png` as a model, plus your own DevTools screenshot)
