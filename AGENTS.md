# Agent Instructions — Handwritten Assignment Workflow

> **Read this first.** This file is auto-discovered by OpenCode and most agent
> harnesses. The styled version lives in [`AGENT-INSTRUCTIONS.html`](AGENT-INSTRUCTIONS.html)
> — open it for visuals and the full CSS, but **everything required to do the
> work correctly is duplicated here** so a second read is never needed.

Project folder: `E:\Workplace\Assignment`

---

## 1. Files

| File | Status | Purpose |
|---|---|---|
| `wraith-ink-sonata-regular.ttf` | **KEEP** | The handwriting font. Every assignment loads it by relative path. Never delete or rename. |
| `experiment-05.html` | **KEEP** | Reference implementation. Copy its structure and CSS as the template for any new assignment. |
| `AGENT-INSTRUCTIONS.html` | **KEEP** | Human-readable / printable version of these rules. |
| `AGENTS.md` | **KEEP** | This file. |

Older assignments and all source PDFs/DOCX were deleted at the user's request on
18 Oct 2026. If they reappear, treat them as user work — do not delete without asking.

---

## 2. The deliverable

One self-contained HTML page that looks like **blue handwriting on ruled notebook
paper**, with a red margin rule, printing cleanly to A4.

---

## 3. Workflow

1. **Get content as text.** ChatGPT share links will not load — they require
   JavaScript and return only the title. Ask the user to paste Q&A as plain text.
2. **Read source files.** `.docx` = ZIP → unzip `word/document.xml`, strip tags.
   Scanned `.pdf` has no text layer → extract embedded JPEGs and read as images.
3. **Reuse the template** from `experiment-05.html`. Do not restyle from scratch.
4. **Verify** (section 6) before delivering.

---

## 4. Required CSS core

```css
@font-face {
  font-family: 'Wraith Ink';
  src: url('wraith-ink-sonata-regular.ttf') format('truetype');
}
body {
  font-family: 'Wraith Ink', cursive;
  color: #15249b;                 /* blue ink */
  -webkit-print-color-adjust: exact;
  print-color-adjust: exact;      /* rules must survive printing */
}

/* Ruled paper: red margin rule + 32px blue lines */
.sheet {
  width: 210mm;
  background-color: #fffdf7;
  background-image:
    linear-gradient(90deg, transparent 21mm, rgba(230,60,60,.65) 21mm,
                    rgba(230,60,60,.65) 21.6mm, transparent 21.6mm),
    repeating-linear-gradient(to bottom, transparent 0, transparent 31px,
                    rgba(60,140,220,.45) 31px, rgba(60,140,220,.45) 32px);
  padding: 32px 12mm 32px 25mm;   /* all multiples of 32px */
  line-height: 32px;              /* one line of text per rule */
  min-height: 297mm;
}

/* CRITICAL: ink must sit ON the line, not float above it */
.head-row, h2.q, h3, p, li, .title, .code, .trav, .out, .sign, td, th {
  position: relative;
  top: 7px;
}

@page { size: A4; margin: 0; }
@media print {
  body { background: #fff; }
  .toolbar { display: none !important; }
  .sheet { width: auto; margin: 0; box-shadow: none; padding: 12mm 10mm 12mm 24mm; }
}
```

### Why `line-height: 32px` matters
Rules repeat every 32px. Text line-height must equal 32px and sheet padding must
be a multiple of 32px, or every line drifts off its rule. Block spacing must also
be multiples of 32 (`margin: 32px 0 0 0`, never `16px`).

### Why `top: 7px` matters
Handwriting fonts have large ascender space, so the baseline lands above the rule
and the text looks like it floats. Shifting every text block down 7px seats it on
the line. If it looks off on the user's screen, nudge to 9px — never remove it.

### Alignment rule learned 18 Oct 2026
**Every top-level block inside `.sheet` must be an integer multiple of 32px tall.**
A fixed-height element (e.g. a Page No. stamp box) will silently push *everything
below it* out of phase. Give such wrappers an explicit `height: 64px` (or another
32-multiple) and confirm by eye that text below still sits on the rules.

---

## 5. Conventions the user expects

| Element | Rule |
|---|---|
| Page heading | Only `Assignment - 0N`, centred, underlined, large. **No** name/roll/branch/date/subject block — the user had it removed. |
| Date field | Optional `Date - ____ / ____ / 26` top-left + small Page No./Date stamp box top-right. |
| Section headings | Underlined via CSS only (`h2.q`, `h3`). |
| **Bold text** | **NEVER.** The user explicitly rejected `<b>` on labels such as `Aim :`, `Binary Tree :`, `Types of traversal :`. Labels are plain text — distinguish them by position, not weight. Also avoid `font-weight: bold`. |
| Inline underlines | Almost none. Do **not** wrap labels in `<u>`. At most `Ans.`, `Conclusion :`, `Result :`. |
| Length | Match supplied content exactly. Never pad or summarise. |
| Scope | Mirror the reference sample's length — the user rejected an over-long draft once. |
| Equations | Unicode subscripts: `Fe₂O₃ + 3CO → 2Fe + 3CO₂`. Never keep raw `\mathrm{...}` LaTeX. |
| Code blocks | `<pre class="code">`, same font, 32px line-height. Escape `<` as `&lt;`. |

---

## 6. Verify before delivering

1. Font path resolves relative to the HTML file — keep both in the same folder.
2. No raw `<` inside `<pre>` blocks (use `&lt;`).
3. Count `<b>` and `<u>` tags — both should be **0**.
4. Every top-level block in `.sheet` is a multiple of 32px tall; text sits on the rules.
5. If output examples are shown, verify them by hand or by running the code.

---

## 7. Humanistic errors (make it look real)

Light, deliberate imperfection. Too many looks fake; too few looks printed.

| Type | Markup | Examples used |
|---|---|---|
| Struck-out correction | `<span class="cross">typo</span> fixedword` | ~~resorces~~ resources · ~~machinary~~ machinery · ~~similiar~~ similar |
| Ink smudge | `<span class="smudge"></span>` | Once or twice per assignment, near a paragraph end |
| Plain misspelling | write it directly | `seperating`, `occuring`, `tranportation`, `temprature`, `sulpher`, `maintanance`, `managment` |

```css
.cross { text-decoration: line-through; text-decoration-thickness: 2px; }
.smudge {
  display: inline-block; width: 14px; height: 10px;
  background: radial-gradient(ellipse at center, rgba(21,36,155,.55) 0,
              rgba(21,36,155,.25) 60%, transparent 70%);
  border-radius: 50%; vertical-align: baseline; margin: 0 2px;
}
```

**Target:** 2–3 struck corrections · 1–2 smudges · 2–4 plain misspellings.
Never corrupt formulas, code, variable names, or numerical answers.

---

## 8. How the user prints

1. Open in Chrome or Edge.
2. Click **Print Assignment** in the top toolbar.
3. Set **Paper: A4**, **Margins: None**, and enable **Background graphics** —
   without it the ruled lines and red margin will not print.
4. Print or Save as PDF.

---

## Quick start for the next assignment

1. Copy `experiment-05.html` to a new file.
2. Swap the heading to `Assignment - 0N`, replace the body with the new content.
3. Keep the CSS core untouched, apply humanistic errors, run section 6.
