# Day 22 — CSS Box Model

📖 **Type:** Theory Session
🕐 **Duration:** 2 Hours
📚 **Unit:** 4 — CSS

---

## Learning Objectives

By the end of this session, students will be able to:
- Explain the CSS Box Model (content, padding, border, margin)
- Calculate total element dimensions
- Use `box-sizing: border-box` for predictable sizing
- Control spacing with margin and padding
- Understand margin collapse

---

## 1. The CSS Box Model

> **Analogy:** Think of a **gift box**. The **gift** inside is the content. The **bubble wrap** around the gift is padding. The **cardboard box** is the border. The **space between boxes** on the shelf is the margin.

```
┌──────────────────────────── Margin ───────────────────────────┐
│                                                               │
│  ┌────────────────────── Border ───────────────────────────┐  │
│  │                                                         │  │
│  │  ┌──────────────── Padding ────────────────────────┐    │  │
│  │  │                                                 │    │  │
│  │  │  ┌──────────── Content ────────────────────┐    │    │  │
│  │  │  │                                         │    │    │  │
│  │  │  │    Your text, images, etc.              │    │    │  │
│  │  │  │    (width × height)                     │    │    │  │
│  │  │  │                                         │    │    │  │
│  │  │  └─────────────────────────────────────────┘    │    │  │
│  │  │                                                 │    │  │
│  │  └─────────────────────────────────────────────────┘    │  │
│  │                                                         │  │
│  └─────────────────────────────────────────────────────────┘  │
│                                                               │
└───────────────────────────────────────────────────────────────┘
```

| Layer | Property | Role |
|-------|----------|------|
| **Content** | `width`, `height` | The actual content area |
| **Padding** | `padding` | Space between content and border |
| **Border** | `border` | The visible boundary |
| **Margin** | `margin` | Space outside the border |

---

## 2. Calculating Total Size

### Default Behavior (`content-box`)

```css
.box {
    width: 200px;
    padding: 20px;
    border: 5px solid black;
    margin: 10px;
}
```

**Total width on screen:**
- Content: 200px
- Padding: 20px × 2 (left + right) = 40px
- Border: 5px × 2 = 10px
- **Total visible width = 200 + 40 + 10 = 250px**
- Margin adds 10px × 2 = 20px of space around it

### The Fix: `border-box`

```css
* {
    box-sizing: border-box;
}
```

With `border-box`, the `width` includes padding and border:
- Total visible width = **200px** (padding and border are inside)
- Content area shrinks to: 200 - 40 - 10 = 150px

> **Always use `box-sizing: border-box`** — it makes layout math much simpler.

---

## 3. Padding

```css
/* All sides */
.box { padding: 20px; }

/* Vertical | Horizontal */
.box { padding: 20px 40px; }

/* Top | Horizontal | Bottom */
.box { padding: 10px 20px 30px; }

/* Top | Right | Bottom | Left (clockwise) */
.box { padding: 10px 20px 30px 40px; }

/* Individual sides */
.box {
    padding-top: 10px;
    padding-right: 20px;
    padding-bottom: 30px;
    padding-left: 40px;
}
```

---

## 4. Margin

```css
/* Same shorthand patterns as padding */
.box { margin: 20px; }
.box { margin: 20px auto; }    /* Center horizontally */
.box { margin: 10px 20px 30px 40px; }

/* Individual sides */
.box { margin-top: 20px; }

/* Negative margins (pull elements closer) */
.overlap { margin-top: -10px; }
```

### Centering a Block Element

```css
.container {
    width: 800px;
    margin: 0 auto;    /* top/bottom: 0, left/right: auto (centered) */
}
```

### Margin Collapse

When two vertical margins touch, they **collapse** — only the larger one is used.

```css
.box1 { margin-bottom: 30px; }
.box2 { margin-top: 20px; }
/* The gap between them is 30px (not 50px) */
```

---

## 5. Visual Demonstration

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSS Box Model Demo</title>
    <style>
        * { box-sizing: border-box; }
        body {
            font-family: Arial, sans-serif;
            background: #f0f0f0;
            padding: 20px;
        }
        
        h1 { text-align: center; color: #1565C0; }
        
        /* Box Model Visualization */
        .box-demo {
            max-width: 600px;
            margin: 30px auto;
            text-align: center;
        }
        
        .margin-area {
            background: #FFCDD2;
            padding: 20px;
            display: inline-block;
        }
        .border-area {
            background: #FFF9C4;
            border: 8px solid #F57F17;
            padding: 20px;
        }
        .padding-area {
            background: #C8E6C9;
            padding: 30px;
        }
        .content-area {
            background: #BBDEFB;
            padding: 20px;
            color: #0D47A1;
            font-weight: bold;
        }

        .label {
            font-size: 12px;
            color: #666;
            margin-bottom: 5px;
        }

        /* Comparison */
        .comparison {
            max-width: 600px;
            margin: 40px auto;
        }
        .comparison h2 { color: #333; margin-bottom: 15px; }
        
        .content-box-demo {
            box-sizing: content-box;
            width: 200px;
            padding: 20px;
            border: 5px solid #E91E63;
            background: #FCE4EC;
            margin: 10px 0;
        }
        .border-box-demo {
            box-sizing: border-box;
            width: 200px;
            padding: 20px;
            border: 5px solid #4CAF50;
            background: #E8F5E9;
            margin: 10px 0;
        }
    </style>
</head>
<body>

    <h1>The CSS Box Model</h1>

    <div class="box-demo">
        <div class="margin-area">
            <p class="label">← Margin (space outside) →</p>
            <div class="border-area">
                <p class="label">← Border →</p>
                <div class="padding-area">
                    <p class="label">← Padding →</p>
                    <div class="content-area">
                        Content Area<br>(width × height)
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="comparison">
        <h2>content-box vs border-box</h2>
        
        <div class="content-box-demo">
            <strong>content-box</strong><br>
            width: 200px<br>
            Total: 200 + 40 + 10 = <strong>250px</strong>
        </div>

        <div class="border-box-demo">
            <strong>border-box</strong><br>
            width: 200px<br>
            Total: <strong>200px</strong> (padding inside)
        </div>
    </div>

</body>
</html>
```

---

## Practice Exercise

1. Create three boxes side by side:
   - Each 30% width with `border-box`
   - Different padding values
   - Different border styles
   - Centered on the page using `margin: 0 auto`

2. Experiment with margin collapse by creating stacked boxes.

---

## Summary

| Property | What It Controls |
|----------|-----------------|
| `width` / `height` | Content area size |
| `padding` | Space inside border |
| `border` | Visible boundary |
| `margin` | Space outside border |
| `box-sizing: border-box` | Width includes padding + border |
| `margin: 0 auto` | Center a block element |

---

*Day 22 of 55 | Unit 4 — CSS | Web Technology (25BCA060)*
