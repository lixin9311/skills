---
name: tech-report-html
description: Turn a technical markdown doc (spike notes, design proposal, architecture overview, status report, audit) into a polished single-page HTML report. Use when the user asks for "rich visualization", "polished/presentable HTML version", "shareable HTML report", "convert this spike/proposal/design to HTML", or wants a hero-styled single-file deliverable with cards, callouts, comparison tables, Mermaid diagrams, and a TOC. Output is one self-contained `.html` file with embedded CSS and Mermaid loaded via CDN — no build step, no external assets to vendor.
---

# tech-report-html

A reusable style for shareable single-page technical reports. The
deliverable is one `.html` file the user can open locally or share via
link. Embedded CSS, Mermaid via CDN, no framework.

## When to use this skill

Invoke when the user says any of:

- "make this into HTML" / "give me an HTML version"
- "rich visualization" / "polished report" / "shareable HTML"
- "convert this spike/design/architecture doc to HTML"
- "with diagrams + cards + comparison tables"
- "single-page report"

Don't use it for:

- Long-form docs that belong in markdown (README, contribution guide).
- Multi-page sites (this skill is single-page only).
- Documents the user will edit further in markdown — keep them in `.md`.

## Workflow

1. **Read the source.** Whatever markdown / notes the user is converting.
2. **Copy `template.html`** (sibling of this `SKILL.md`) as the starting
   skeleton. It contains the full CSS, a placeholder hero, an empty TOC,
   and the Mermaid init script.
3. **Fill in the hero** — eyebrow + title + one-sentence lede + meta strip
   (repo, branch, author, date).
4. **Add a TL;DR row** — 2–4 cards summarizing the document's main beats.
5. **Add a verdict callout** if there's a clear conclusion (`.callout.ok`
   for "feasible / approved", `.callout.warn` for "needs decision",
   `.callout.info` for "recommended approach").
6. **Populate the TOC** — list each numbered top-level section.
7. **Write sections in order** using the components below. Each
   `<h2>` gets a `<span class="num">N</span>` numbered badge.
8. **Footer.**

Output one file, named with `.html` extension. Open it locally to verify
diagrams render before declaring done.

## Style identity (don't deviate without reason)

| Element | Value |
| --- | --- |
| Background | `#fafafa` |
| Surface (cards, tables) | `#ffffff` |
| Text primary | `#1f2937` |
| Text muted | `#6b7280` |
| Border | `#e5e7eb` |
| Accent (orange, primary highlight) | `#fc6d26` |
| Accent-2 (purple, links + secondary) | `#6e49cb` |
| Success | `#16a34a` |
| Warning | `#d97706` |
| Code block bg | `#1e1e2e` |
| Code block text | `#cdd6f4` |
| Body font | system sans (`-apple-system, BlinkMacSystemFont, "Segoe UI", …`) |
| Code font | `"JetBrains Mono", "Fira Code", Menlo, Consolas` |
| Max container width | 1100px |
| Hero | full-width gradient `linear-gradient(135deg, #1f1f2e 0%, #2d1b4e 100%)` + 4px accent border-bottom |

These are baked into `template.html`. Don't introduce Tailwind, Bootstrap,
or any other framework. Don't change the palette unless the user
explicitly asks.

## Components (snippets)

### Hero

```html
<header class="hero">
  <div class="inner">
    <div class="eyebrow">Spike Report</div>
    <h1>Document Title Goes Here</h1>
    <p class="lede">One-sentence lede that tells the reader what they're looking at.</p>
    <div class="meta">
      <span><strong>Repo:</strong> path/to/repo</span>
      <span><strong>Branch:</strong> feature-branch</span>
      <span><strong>Author:</strong> Name &lt;email@…&gt;</span>
      <span><strong>Date:</strong> 2026-05-20</span>
    </div>
  </div>
</header>
```

### TL;DR cards row

```html
<div class="tldr">
  <div class="card">
    <div class="label">Phase 1</div>
    <div class="title">Short title</div>
    <p class="body">One or two sentence summary.</p>
  </div>
  <div class="card green">…</div>
  <div class="card purple">…</div>
  <div class="card amber">…</div>
</div>
```

`.card` has variants: default (orange accent), `.green`, `.purple`,
`.amber`. Use them to colour-code phases or categories.

### Callouts

```html
<div class="callout">…</div>        <!-- default orange -->
<div class="callout ok">…</div>     <!-- green: positive verdict -->
<div class="callout info">…</div>   <!-- purple: recommendation -->
<div class="callout warn">…</div>   <!-- yellow: caveat/gotcha -->
```

Lead with `<strong>One-line headline</strong>` inside the callout, then
the body.

### Pills (status chips)

```html
<span class="pill ok">resolved</span>
<span class="pill warn">verify</span>
<span class="pill info">design</span>
<span class="pill muted">low priority</span>
```

Use inside table cells / list items for compact status labels.

### Comparison table

```html
<table class="compare">
  <thead>
    <tr>
      <th style="width:18%">Column A</th>
      <th style="width:35%">Column B</th>
      <th style="width:35%">Column C</th>
      <th style="width:12%">Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Row label</td>
      <td>Value B</td>
      <td>Value C</td>
      <td><span class="pill ok">Equiv</span></td>
    </tr>
  </tbody>
</table>
```

First column is bold by default. Last row's bottom border is removed
automatically.

### Numbered section heading

```html
<h2 id="phase1"><span class="num">1</span>Phase title</h2>
```

The TOC links to `#phase1` etc.

### Mermaid diagram

```html
<div class="diagram">
  <pre class="mermaid">
flowchart LR
    classDef pin fill:#fff7ed,stroke:#fc6d26,color:#1f1f1f,stroke-width:2px
    classDef ok fill:#f0fdf4,stroke:#16a34a,color:#1f1f1f,stroke-width:2px

    A["Step A"]:::pin --> B["Step B"]:::ok
  </pre>
  <div class="diagram-caption">One-line caption explaining the diagram.</div>
</div>
```

Always declare `classDef` styles using the page palette
(`#fc6d26`/`#6e49cb`/`#16a34a`/`#d97706`/`#be185d` for primary colours)
so diagrams stay visually consistent.

Mermaid types known to render well in this template:
- `flowchart LR` / `flowchart TB` for process flows
- `gantt` for plans / sequencing
- Subgraphs for grouped containers (e.g. cluster + sidecar topology)

### Code block

```html
<pre><code># comment
some --flag arg
piped | command</code></pre>
```

HTML-escape inside `<pre><code>`:

- `&` → `&amp;`
- `<` → `&lt;`
- `>` → `&gt;`

Inline code uses plain `<code>` (subtle background, purple text).

### TOC

```html
<div class="toc">
  <h4>Contents</h4>
  <ol>
    <li><a href="#phase1">Phase 1 — title</a></li>
    <li><a href="#phase2">Phase 2 — title</a></li>
    …
  </ol>
</div>
```

Renders as two columns if 8+ entries; collapses to one column on mobile.

### Footer

```html
<footer>
  Spike notes companion to <a href="…">…</a> · project · branch <code>name</code>
</footer>
```

## Tips & gotchas

- **HTML-escape inside `<pre>`.** Forgetting to escape `&` is the
  number-one rendering bug. Watch for `&&`, `2>&1`, ampersands in URLs.
- **Pipe in table cells.** Markdown-style tables would break on `|` inside
  inline code; HTML `<table>` is safe, but if you copy from a markdown
  table that had `\|` escaped, drop the backslash.
- **One file.** The deliverable is one HTML file; don't split CSS into
  a separate stylesheet "for cleanliness". The whole point is shareability.
- **Mermaid CDN.** `template.html` loads Mermaid from jsdelivr. Don't
  vendor it locally unless the user is air-gapped.
- **Emojis.** Don't add emojis to the report unless the user explicitly
  asks. The visual signature is numbered badges + pills + accent colours,
  not emoji.
- **Diagrams render after JS runs.** When you open the file locally to
  verify, give Mermaid a moment; if a diagram doesn't appear, check the
  console for the actual error (most often: a stray `|` or `<` inside a
  node label).
- **Stay opinionated.** Don't ask the user for theme choices, font
  preferences, or palette tweaks. The style is the value-add — apply it.

## Don't do these

- Don't write a multi-page navigation system.
- Don't reach for a CSS framework.
- Don't reformat the user's prose to fit the template — fit the template
  to the content. If a section is short, it's short.
- Don't auto-translate jargon. If the source says "envsubst", the report
  says "envsubst", not "environment variable substitution".
- Don't pad the report to make it look longer. Cards and callouts are
  for *real* highlights, not for visual filler.
