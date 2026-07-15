# HTML Technical Guide — Style & Output Instructions

> Use this document as the definitive reference when generating HTML technical guides in the same style as the *AI Agents Architecture Guide*. Follow every specification precisely — do not substitute, simplify, or deviate unless explicitly told to.

---

## 1. File & Structure

- **Single self-contained `.html` file.** All CSS and SVG inline. No external JS. No separate stylesheets.
- **Google Fonts** loaded via `<link>` in `<head>` — the only external dependency allowed.
- **Document order:**
  1. `<head>` with charset, viewport, title, Google Fonts link, `<style>` block
  2. `<div class="hero">` — full-viewport opening screen
  3. `<main>` — numbered sections
  4. `<footer>`

---

## 2. Typography

### Font Pairing (exact)
```
Display / Body:  Fraunces — Google Fonts
                 Weights: 300 (body), 700 (headings)
                 Styles: normal + italic
                 Optical size axis: 9..144
                 Import string:
                 family=Fraunces:ital,opsz,wght@0,9..144,300;0,9..144,700;1,9..144,300

Monospace / UI:  Space Mono — Google Fonts
                 Weights: 400, 700
                 Styles: normal + italic
                 Import string:
                 family=Space+Mono:ital,wght@0,400;0,700;1,400
```

### Usage Rules
| Element | Font | Size | Weight | Color |
|---|---|---|---|---|
| `body` | Fraunces | — | 300 | `--text` |
| `h1` (hero) | Fraunces | `clamp(3rem, 8vw, 7rem)` | 700 | see hero section |
| `h2` (section) | Fraunces | `2.2rem` | 700 | `--text` |
| `h4` (subsection) | Fraunces | `1rem` | 700 | `--text` |
| `p` | Fraunces | — | 300 | `--text-muted` |
| `p strong` | Fraunces | — | 700 | `--text` |
| badges, labels, TOC, table headers, section numbers, `.code-label`, footer | Space Mono | 0.65–0.75rem | 400/700 | varies |
| `code`, `pre` | Space Mono | `0.8rem` | 400 | varies |

- `body` `line-height: 1.7`
- `letter-spacing: -0.02em` on all headings
- `letter-spacing: 0.1em` on all monospace labels (badges, section nums, table headers)
- `text-transform: uppercase` on all monospace labels

---

## 3. Color System

### CSS Custom Properties (copy verbatim into `:root`)
```css
:root {
  --bg:         #0a0a0f;   /* near-black, slightly blue-tinted — page background */
  --surface:    #12121a;   /* card / panel background */
  --surface2:   #1a1a26;   /* table header background, slightly lighter surface */
  --border:     #2a2a3e;   /* all borders — subtle, blue-tinted dark */
  --accent:     #7c3aed;   /* PRIMARY accent — violet/purple */
  --accent2:    #06b6d4;   /* SECONDARY accent — cyan */
  --accent3:    #f59e0b;   /* TERTIARY accent — amber */
  --accent4:    #10b981;   /* QUATERNARY accent — emerald green */
  --text:       #e2e8f0;   /* primary text — near-white with slight blue */
  --text-muted: #94a3b8;   /* secondary text — slate-400 */
  --text-dim:   #64748b;   /* tertiary text — slate-500, comments, placeholders */
  --code-bg:    #0d0d18;   /* code block background — darker than --bg */
  --red:        #ef4444;   /* danger / negative tags */
  --pink:       #ec4899;   /* sixth accent — pink */
}
```

### Accent Color Assignments (consistent across guides)
- **Purple `#7c3aed`** → primary structure (section nums, TOC hover, card borders)
- **Cyan `#06b6d4`** → secondary/info (badge text, code labels, arrow markers, list bullets `→`)
- **Amber `#f59e0b`** → tertiary/caution (number literals in code, amber callouts)
- **Green `#10b981`** → success/positive (green callouts, green tags)
- **Red `#ef4444`** → danger/negative (red tags in tables)
- **Pink `#ec4899`** → sixth category (pink cards, feedback/critic concepts)

### Inline opacity patterns
- Hero radial glows: `rgba(accent, 0.08–0.15)` — never higher
- Card `::before` top-border: solid accent, 2px height
- Callout backgrounds: `rgba(accent-rgb, 0.08)`
- Tag backgrounds: `rgba(accent-rgb, 0.15)`
- Grid lines in hero: `rgba(124,58,237,0.06)`
- Row hover: `rgba(255,255,255,0.02)`

---

## 4. Hero Section

### Structure
```html
<div class="hero">
  <div class="hero-grid"></div>   <!-- grid overlay, CSS-only -->
  <div class="hero-content">
    <span class="badge">LABEL · Context · Date</span>
    <h1>
      <span class="line1">First word — plain white</span>
      <span class="line2">Second word — outlined (text-stroke)</span>
      <span class="line3">Third word — gradient fill</span>
    </h1>
    <p class="hero-desc">Italic subtitle in text-muted.</p>
    <div class="toc-grid">
      <a href="#section-id" class="toc-item">
        <span class="toc-num">01</span> Section Name
      </a>
      <!-- repeat for each section -->
    </div>
  </div>
</div>
```

### Hero CSS Rules (exact)
```css
.hero {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 4rem 2rem;
  position: relative;
  overflow: hidden;
}

/* Background glow — 3 radial gradients at different positions */
.hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background: 
    radial-gradient(ellipse 80% 60% at 20% 50%, rgba(124,58,237,0.15) 0%, transparent 60%),
    radial-gradient(ellipse 60% 80% at 80% 30%, rgba(6,182,212,0.1)  0%, transparent 60%),
    radial-gradient(ellipse 40% 40% at 60% 80%, rgba(245,158,11,0.08) 0%, transparent 50%);
  pointer-events: none;
}

/* Grid overlay — fades at edges via mask */
.hero-grid {
  position: absolute;
  inset: 0;
  background-image: 
    linear-gradient(rgba(124,58,237,0.06) 1px, transparent 1px),
    linear-gradient(90deg, rgba(124,58,237,0.06) 1px, transparent 1px);
  background-size: 60px 60px;
  mask-image: radial-gradient(ellipse 80% 80% at 50% 50%, black 20%, transparent 80%);
}
```

### H1 Three-Line Treatment (exact)
```css
.hero h1 {
  font-size: clamp(3rem, 8vw, 7rem);
  font-weight: 700;
  line-height: 1;
  margin-bottom: 1.5rem;
  letter-spacing: -0.02em;
}
.hero h1 .line1 { color: var(--text); }                          /* plain */
.hero h1 .line2 {                                                /* outlined */
  color: transparent;
  -webkit-text-stroke: 1px var(--accent);
  display: block;
}
.hero h1 .line3 {                                                /* gradient */
  background: linear-gradient(90deg, var(--accent), var(--accent2));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  display: block;
}
```

### Badge (exact)
```css
.badge {
  display: inline-block;
  font-family: 'Space Mono', monospace;
  font-size: 0.7rem;
  color: var(--accent2);
  border: 1px solid rgba(6,182,212,0.3);
  padding: 0.3rem 0.8rem;
  border-radius: 2px;
  margin-bottom: 1.5rem;
  letter-spacing: 0.15em;
  text-transform: uppercase;
}
```

### TOC Grid (exact)
```css
.toc-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 0.75rem;
  max-width: 900px;
}
.toc-item {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  padding: 0.6rem 1rem;
  background: rgba(255,255,255,0.03);
  border: 1px solid var(--border);
  border-radius: 4px;
  text-decoration: none;
  color: var(--text-muted);
  font-family: 'Space Mono', monospace;
  font-size: 0.72rem;
  transition: all 0.2s;
  letter-spacing: 0.05em;
}
.toc-item:hover {
  background: rgba(124,58,237,0.1);
  border-color: var(--accent);
  color: var(--text);
}
.toc-num { color: var(--accent); font-weight: 700; }
```

---

## 5. Section Layout

### Section HTML Template
```html
<section class="section" id="section-id">
  <div class="section-header">
    <span class="section-num">// 01</span>
    <h2>Section Title</h2>
  </div>
  <!-- content -->
</section>
```

### Section CSS (exact)
```css
main {
  max-width: 1100px;
  margin: 0 auto;
  padding: 2rem;
}
.section {
  margin-bottom: 6rem;
  scroll-margin-top: 2rem;
}
.section-header {
  display: flex;
  align-items: baseline;
  gap: 1rem;
  margin-bottom: 2.5rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid var(--border);
}
.section-num {
  font-family: 'Space Mono', monospace;
  font-size: 0.75rem;
  color: var(--accent);
  opacity: 0.7;
  letter-spacing: 0.1em;
  white-space: nowrap;
}
.section h2 {
  font-size: 2.2rem;
  font-weight: 700;
  letter-spacing: -0.02em;
  line-height: 1.1;
}
```

### Section Number Format
- Use `// 01`, `// 02`, ... `// 08` (two digits, space Mono, comment-style prefix `//`)

### Subsection Headers
- Use `<h4>` for in-section subheadings
- CSS: `font-size: 1rem; color: var(--text); margin: 1.5rem 0 0.5rem; font-weight: 700;`
- No `h3` used at section level (reserved for card titles only)

---

## 6. Component Reference

### 6.1 Cards (`.card`)

**Colors available:** `.purple`, `.cyan`, `.amber`, `.green`, `.red`, `.pink`

The colored class controls a 2px top border (`::before` pseudo-element).

```html
<div class="card-grid">
  <div class="card purple">
    <span class="card-icon">🧠</span>
    <h3>Card Title</h3>
    <p>Description text goes here.</p>
  </div>
</div>
```

```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1rem;
  margin: 1.5rem 0;
}
.card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 1.5rem;
  position: relative;
  overflow: hidden;
}
.card::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 2px;
}
.card h3 { font-size: 1.05rem; font-weight: 700; margin-bottom: 0.5rem; color: var(--text); }
.card p  { font-size: 0.9rem; margin: 0; }
.card-icon { font-size: 1.5rem; margin-bottom: 0.75rem; display: block; }
```

- Use 3 or 6 cards per grid (fills 2-column or 3-column evenly at most viewport sizes)
- Each card gets one emoji icon, one short title, one 2–3 sentence description
- Rotate through the 6 colors in order: purple → cyan → amber → green → red → pink

---

### 6.2 Code Blocks (`<pre>`)

```html
<pre><code><span class="code-label">Python · Description of what this shows</span><span class="kw">import</span> anthropic
...</code></pre>
```

**Syntax highlighting spans (CSS classes):**

| Class | Color | Used For |
|---|---|---|
| `.kw` | `#c084fc` | Keywords (`import`, `def`, `class`, `return`, `if`, `for`, `await`) |
| `.fn` | `#60a5fa` | Function names at call site |
| `.str` | `#86efac` | String literals |
| `.cm` | `--text-dim` + italic | Comments |
| `.num` | `--accent3` (`#f59e0b`) | Number literals |
| `.cls` | `#fbbf24` | Class names |
| `.op` | `--accent2` (`#06b6d4`) | Operators, special symbols |
| `.var` | `#f9a8d4` | Variables, `self` |

**Code block CSS (exact):**
```css
pre {
  background: var(--code-bg);   /* #0d0d18 */
  border: 1px solid var(--border);
  border-radius: 6px;
  padding: 1.5rem;
  overflow-x: auto;
  margin: 1.5rem 0;
  position: relative;
}
code {
  font-family: 'Space Mono', monospace;
  font-size: 0.8rem;
  line-height: 1.6;
}
.code-label {
  font-family: 'Space Mono', monospace;
  font-size: 0.65rem;
  color: var(--accent2);
  letter-spacing: 0.1em;
  text-transform: uppercase;
  display: block;
  border-bottom: 1px solid var(--border);
  padding-bottom: 0.5rem;
  margin-bottom: 1rem;
}
```

**Rules:**
- `.code-label` is **always** the very first element inside `<code>`, immediately before the first line of code with **no newline** between them
- Label format: `Language · Short description` (e.g., `Python · Tool Definitions + Agent Loop`)
- Highlight every keyword, function call, string, number, class, and comment — do not leave code unstyled
- Inline comments use `<span class="cm"># comment text</span>` and appear after code on the same line

---

### 6.3 Callouts (`.callout`)

```html
<div class="callout">           <!-- default: purple -->
<div class="callout cyan">
<div class="callout amber">
<div class="callout green">
```

```css
.callout {
  background: rgba(124,58,237,0.08);
  border-left: 3px solid var(--accent);
  padding: 1rem 1.5rem;
  margin: 1.5rem 0;
  border-radius: 0 6px 6px 0;
}
.callout p { margin: 0; font-size: 0.9rem; }
```

- One `<p>` inside
- Start with `<strong>Label:</strong>` for context (e.g., `<strong>Key mental model:</strong>`)
- Use inline `<span class="hl">`, `<span class="hl-amber">`, `<span class="hl-green">`, `<span class="hl-purple">` to highlight specific terms within the text

---

### 6.4 Tables

```html
<div class="table-wrap">
  <table>
    <tr><th>Col A</th><th>Col B</th></tr>
    <tr>
      <td><strong>Row label</strong></td>
      <td><span class="tag tag-green">Yes</span></td>
    </tr>
  </table>
</div>
```

**Tag colors in table cells:**

| Class | Background | Text |
|---|---|---|
| `.tag-green` | `rgba(16,185,129,0.15)` | `#10b981` |
| `.tag-amber` | `rgba(245,158,11,0.15)` | `#f59e0b` |
| `.tag-red` | `rgba(239,68,68,0.15)` | `#ef4444` |
| `.tag-blue` | `rgba(96,165,250,0.15)` | `#60a5fa` |
| `.tag-purple` | `rgba(124,58,237,0.15)` | `#c084fc` |

```css
.table-wrap { overflow-x: auto; margin: 1.5rem 0; }
table { width: 100%; border-collapse: collapse; font-size: 0.87rem; }
th {
  background: var(--surface2);
  padding: 0.75rem 1rem;
  text-align: left;
  font-family: 'Space Mono', monospace;
  font-size: 0.7rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text-muted);
  border-bottom: 1px solid var(--border);
}
td {
  padding: 0.75rem 1rem;
  border-bottom: 1px solid rgba(42,42,62,0.5);
  color: var(--text-muted);
  vertical-align: top;
}
tr:hover td { background: rgba(255,255,255,0.02); }
td strong { color: var(--text); }
td .tag {
  display: inline-block;
  font-family: 'Space Mono', monospace;
  font-size: 0.65rem;
  padding: 0.15rem 0.4rem;
  border-radius: 2px;
  margin: 0.1rem;
}
```

---

### 6.5 SVG Diagrams (`.diagram`)

```html
<div class="diagram">
  <div class="diagram-title">Diagram Title Here</div>
  <svg viewBox="0 0 W H" width="100%" style="max-width:Wpx;display:block;margin:0 auto;">
    <!-- nodes, arrows, labels -->
  </svg>
</div>
```

**SVG design rules:**
- All SVG text: `font-family: 'Space Mono', monospace` (set globally via `svg text { font-family: ... }`)
- Node rectangles: `fill="#1a1a26"` (surface2), `stroke` = accent color for the concept, `stroke-width="1.5"` (important nodes) or `"1"` (secondary)
- Node dimensions: typically `width=60–165`, `height=26–50`, `rx=4–6`
- Node label text: `font-size="9"` or `"10"`, `text-anchor="middle"`, `fill` = the node's accent color (lighter tint like `#c084fc`, `#67e8f9`, `#fcd34d`, `#6ee7b7`)
- Sublabels / descriptions under node title: `font-size="9"`, `fill="#94a3b8"` (text-muted)
- All arrows use `<defs>` `<marker>` elements with triangular paths — define one per color used
- Solid arrows: `stroke-width="1.5"–"2"`, no dash
- Dashed arrows (feedback loops, optional paths): `stroke-dasharray="4,2"` or `"5,3"`
- Background color behind SVG: `var(--surface)` via `.diagram`
- Diagram must be responsive: `width="100%"` + `viewBox` always set + `max-width` inline style

**SVG marker pattern (copy):**
```html
<defs>
  <marker id="arr" markerWidth="6" markerHeight="6" refX="5" refY="3" orient="auto">
    <path d="M0,0 L6,3 L0,6 Z" fill="#COLOR"/>
  </marker>
</defs>
<!-- Usage: -->
<path d="M x1 y1 L x2 y2" stroke="#COLOR" stroke-width="2" marker-end="url(#arr)"/>
```

Use a **unique marker `id`** per arrow color (`#arr`, `#arr2`, `#arr3`...).

---

### 6.6 Industry / Feature Cards (`.industry-card`)

Used for rich cards with icon + title + subtitle + body + bulleted list.

```html
<div class="industry-grid">
  <div class="industry-card">
    <div class="industry-header">
      <div class="industry-icon" style="background:rgba(R,G,B,0.15)">🏥</div>
      <div>
        <h3>Industry Name</h3>
        <small>Tag · Tag · Tag</small>
      </div>
    </div>
    <div class="industry-body">
      <p>Lead sentence.</p>
      <ul class="use-cases">
        <li>Use case one with enough detail to be useful</li>
        <li>Use case two</li>
      </ul>
    </div>
  </div>
</div>
```

- Grid: `repeat(auto-fill, minmax(320px, 1fr))`, gap `1.25rem`
- Icon box: 44×44px, `border-radius: 8px`, `background` = tinted rgba of a unique accent per industry
- `<small>` uses Space Mono for sub-tagging
- List uses `→` pseudo-element in `--accent2` cyan color
- 4–6 items per list

---

## 7. Global Utilities

### Inline highlight spans (for use inside callouts and prose)
```css
.hl        { color: var(--accent2); }   /* cyan  */
.hl-amber  { color: var(--accent3); }   /* amber */
.hl-green  { color: var(--accent4); }   /* green */
.hl-purple { color: #c084fc; }          /* purple */
```

### Two-column layout (for side-by-side content)
```css
.two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 1.5rem; }
@media (max-width: 700px) { .two-col { grid-template-columns: 1fr; } }
```

### Custom scrollbar
```css
::-webkit-scrollbar { width: 6px; height: 6px; }
::-webkit-scrollbar-track { background: var(--bg); }
::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }
```

---

## 8. Footer

```html
<footer>
  <p>Guide Title · Built with Claude · Month Year</p>
  <p style="margin-top:0.5rem;color:#1a1a26">Keyword summary line</p>
</footer>
```

```css
footer {
  border-top: 1px solid var(--border);
  padding: 2rem;
  text-align: center;
  color: var(--text-dim);
  font-family: 'Space Mono', monospace;
  font-size: 0.7rem;
  letter-spacing: 0.05em;
}
```

Note: the second `<p>` in the footer uses `color:#1a1a26` — rendering it nearly invisible (same as `--surface`). It's a watermark-style SEO/meta line.

---

## 9. Content Rules

### Section count and depth
- **7–9 numbered sections** total per guide
- Section order template: Core Concepts → Detailed Technical Topic → Implementation/Code → Comparison → Advanced Topic → Deployment/Operations → Use Cases → Best Practices
- Always end with a Best Practices section (cards format)

### Writing style
- Section intro: 2–3 sentences prose, `<p>` tags, `color: var(--text-muted)`
- `<strong>` inside `<p>` for emphasized terms → renders in `--text` (near-white)
- Italics used sparingly: hero subtitle, key mental models
- No bullet `<ul>/<li>` in prose — use cards or industry-cards instead
- Only use `<ul class="use-cases">` inside industry cards

### Code blocks per section
- At least 1 fully highlighted code block per technical section
- Code must be complete enough to run (or nearly so) — no `...` placeholder-only examples
- Each block has a `.code-label` describing language and purpose
- Code line length: keep under ~90 chars; break long strings across lines

### Diagrams per guide
- Minimum **2 SVG diagrams** in the full guide
- Always: one high-level concept diagram (e.g., loop, pipeline) + one more complex topology/architecture diagram
- Diagrams use multiple accent colors to distinguish node types — never monochrome

### Tables
- Use for comparisons with 4+ rows and 4+ columns
- First column is always `<strong>Name/Label</strong>`
- Use `.tag` spans for categorical values (yes/no, difficulty levels, ratings)

### Callouts
- 1 callout per section maximum
- Purpose: distill the most important takeaway or decision rule
- Always placed at the **end** of a section, after code and diagrams

---

## 10. Complete CSS Reset Block

Add this to the very top of `<style>`:
```css
* { margin: 0; padding: 0; box-sizing: border-box; }
html { scroll-behavior: smooth; }
body {
  background: var(--bg);
  color: var(--text);
  font-family: 'Fraunces', serif;
  font-weight: 300;
  line-height: 1.7;
  overflow-x: hidden;
}
p { margin-bottom: 1rem; color: var(--text-muted); }
p strong { color: var(--text); font-weight: 700; }
h4 { font-size: 1rem; color: var(--text); margin: 1.5rem 0 0.5rem; font-weight: 700; }
svg text { font-family: 'Space Mono', monospace; }
```

---

## 11. Quick Checklist Before Output

- [ ] Google Fonts `<link>` includes both Fraunces and Space Mono with all weights/axes
- [ ] `:root` has all 12 CSS variables defined exactly
- [ ] Hero has: grid overlay div, badge, h1 with three `<span>` lines, italic desc, TOC grid
- [ ] Every section has `class="section"` + `id` + `.section-header` with `// 0N` number
- [ ] All code blocks have `.code-label` as first child of `<code>`, no newline before first code line
- [ ] All code syntax is highlighted with `.kw`, `.fn`, `.str`, `.cm`, `.num`, `.cls`, `.op`, `.var`
- [ ] At least 2 SVG diagrams with `viewBox`, `width="100%"`, `max-width` inline, and `<defs>` markers
- [ ] Cards rotate through all 6 colors in order
- [ ] Tables use `.table-wrap` wrapper and `.tag` spans for categorical values
- [ ] Custom scrollbar CSS included
- [ ] Footer has the invisible second line (`color:#1a1a26`)
- [ ] No external JS, no separate CSS file, no Tailwind, no frameworks
