# Website Builder — Project Specification

A simplified, Wix-style drag-and-drop website builder for creating branded web pages without writing code.

---

## Overview

A browser-based visual page editor that lets a non-technical user create and edit web pages by dragging pre-built components onto a canvas. The builder produces clean, standalone HTML/CSS files that match the Linked Escrow brand system.

**Built on:** [GrapesJS](https://grapesjs.com/) (open-source web builder framework)
**Why GrapesJS:** Provides drag-and-drop canvas, inline text editing, responsive preview, style manager, and layer manager — all via a single CDN script tag. No React, no npm, no build step required. Exports clean HTML/CSS.

---

## Core Features

### Drag-and-Drop Editor
- Visual canvas where users drag pre-built blocks (sections, text, images, buttons) to compose pages
- Inline text editing — click any text to edit it directly on the canvas
- Component selection, reordering, and deletion
- Undo/redo support (built into GrapesJS)

### Responsive Preview
- Toggle between Desktop (1200px), Tablet (768px), and Mobile (375px) views
- Components adapt automatically based on existing CSS responsive breakpoints

### Style Manager (Limited)
- Edit colors, spacing, font size, alignment on selected components
- Color picker restricted to brand palette only (8 approved colors)
- Font options restricted to Mont family weights

### Page Management
- Create new pages, rename, duplicate, delete
- Auto-save to browser localStorage
- Multi-page support — switch between pages without losing work

### Export
- "Export HTML" button generates a complete, standalone HTML file
- Exported file includes all necessary `<head>` boilerplate (meta tags, font references, CSS links)
- Downloaded file works when opened directly in a browser — no dependencies on the builder

### Import
- Load an existing HTML file into the editor for modification

---

## Tech Stack

| Layer | Choice | Rationale |
|-------|--------|-----------|
| Editor engine | GrapesJS (CDN) | Full editor out of the box, no build step |
| Block library plugin | gjs-blocks-basic (CDN) | Starter blocks: columns, text, image, video |
| Styling | Plain CSS | Builder chrome styled to match brand palette |
| Persistence | localStorage | Simple, no backend needed for single-user tool |
| Export format | Static HTML + CSS | Matches existing site architecture |
| Server | `python3 -m http.server` | Zero-install local dev server |

**No build tools. No npm. No frameworks. Everything loads via CDN or local script tags.**

---

## Folder Structure

```
/builder/
  index.html                # The builder application (single page)
  css/
    builder-ui.css           # Builder chrome styling (navy/turquoise theme)
  js/
    builder-init.js          # GrapesJS initialization and configuration
    blocks.js                # Custom brand block definitions (~25 blocks)
    panels.js                # Toolbar/panel customization
    storage.js               # Save/load/export logic
  templates/
    blank.json               # Empty starter template
    homepage.json            # Pre-built homepage template (optional)
```

---

## Component Block Library

### Section-Level Blocks (full-width page sections)

| Block | Description | Based On |
|-------|-------------|----------|
| Hero Section | Full-width hero with heading, subtext, CTA buttons, optional stats row | `.le-hero` |
| Page Hero | Simpler inner-page hero with heading + subtext | `.le-page-hero` |
| Trust Bar | Horizontal strip of short trust statements with dot indicators | `.le-trust-bar` |
| Card Grid (3-col) | Three cards with icon, title, and body text | `.le-cards-grid` |
| Card Grid (4-col) | Four smaller cards | `.le-cards-grid-4` |
| Steps / Process | Numbered vertical timeline with descriptions | `.le-steps` |
| Feature Row | Two-column layout: text + visual (reversible) | `.le-feature-row` |
| Testimonials | Quote cards with author attribution | `.le-testimonial` |
| CTA Banner | Image background with gradient overlay, heading, buttons | `.le-cta-banner` |
| Stats Row | Big numbers with labels in a grid | `.le-stats` |
| Text Section | Generic section with container, heading, paragraph | `.section.large` |
| Gallery | Offset image collage with centered text | `.le-gallery` |
| Bio Card | Team member card with headshot, name, role, bio | `.le-bio-card` |

### Atomic Blocks (drag inside sections)

| Block | Description | CSS Class |
|-------|-------------|-----------|
| Heading | Text heading (H1-H5 selectable) | `.heading-1` through `.heading-5` |
| Paragraph | Body text | `.body-m`, `.body-s` |
| Button (Primary) | Turquoise filled button | `.btn-primary` |
| Button (Outline) | White/outline button | `.btn-primary-white` |
| Tag / Badge | Small turquoise pill label | `.tag` |
| Eyebrow | Uppercase label above headings | `.le-eyebrow` |
| Image | Responsive image with rounded corners | `<img>` |
| Checklist | Checkmark bullet list | `.le-checklist` |
| Divider | Horizontal separator | `<hr>` |
| Spacer | Adjustable vertical whitespace | margin block |

---

## Brand Integration

The builder canvas loads the existing `custom.css` file directly from the Linked Escrow site. This means:

1. **Every block renders with full brand styling immediately** — no duplicate CSS needed
2. **Font files (Mont family)** are referenced by `custom.css` via relative paths and load automatically
3. **Brand colors** are defined as CSS custom properties in `custom.css` and available to all blocks
4. **Responsive breakpoints** from `custom.css` apply inside the canvas

### Brand Color Palette (restricted in style manager)

| Color | Hex | Usage |
|-------|-----|-------|
| White | `#FFFFFF` | Backgrounds |
| Light Gray | `#F7F9FC` | Alternate section backgrounds |
| Light Turquoise | `#9EF8EC` | Hover states, light accents |
| Turquoise | `#5AE9D7` | Primary accent, buttons, highlights |
| Orange | `#FF7958` | Alert accent |
| Blue | `#022B6D` | Secondary text, links |
| Dark Gray | `#9197AE` | Body text, borders |
| Dark Blue | `#131F4A` | Headings, primary text |

---

## Implementation Phases

### Phase 1: Working Editor Shell
**Goal:** Functional drag-and-drop editor with basic blocks

- Create `index.html` loading GrapesJS + gjs-blocks-basic from CDN
- Configure canvas to inject `custom.css` and Mont fonts
- Style builder UI chrome (sidebar, toolbar) to match brand palette
- Enable localStorage auto-save
- Configure device manager (desktop/tablet/mobile toggle)

**Result:** A working editor where you can drag text, images, and column layouts onto a canvas that renders with brand styling.

### Phase 2: Custom Brand Blocks
**Goal:** All Linked Escrow section types available as drag-and-drop blocks

- Register each section-level block via `editor.BlockManager.add()`
- Each block's `content` property contains the exact HTML pattern from the existing site
- Register atomic blocks (headings, buttons, lists, etc.)
- Organize blocks into sidebar categories: Sections, Content, Buttons, Layout
- Add SVG preview icons for each block

**Result:** User can compose a complete branded page by dragging pre-built sections.

### Phase 3: Export & Page Management
**Goal:** Save work, manage multiple pages, export standalone HTML files

- Export function: wraps `editor.getHtml()` + `editor.getCss()` in a complete HTML document with correct `<head>` boilerplate, triggers browser file download
- Page management UI: list saved pages, create/rename/duplicate/delete
- Import function: read an HTML file and load into editor via `editor.setComponents()`

**Result:** User can create pages, save them, and export production-ready HTML files.

### Phase 4: Polish
**Goal:** Refined, user-friendly experience

- Restrict Style Manager to brand colors and font weights only
- Add full-screen "Preview" mode (hides editor chrome)
- Pre-build a homepage template matching the current `index.html`
- Simplify toolbar: Save, Export, Preview, Templates, Pages buttons
- Lock structural classes on section blocks (prevent accidental class deletion)

**Result:** A polished, brand-safe editing experience.

---

## How to Run

```bash
# From the project root directory
cd ~/Desktop/claude_code_folder/Linked\ Escrow\ Website/
python3 -m http.server 8080

# Open in browser
# http://localhost:8080/builder/
```

---

## How Export Works

When the user clicks "Export HTML", the builder:

1. Calls `editor.getHtml()` to get the canvas HTML content
2. Calls `editor.getCss()` to get any canvas-level style overrides
3. Wraps both in a complete HTML document:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Page Title</title>
  <link href="custom.css" rel="stylesheet">
</head>
<body>
  <!-- editor.getHtml() output here -->
  <style>
    /* editor.getCss() output here */
  </style>
</body>
</html>
```

4. Creates a `Blob` and triggers a browser file download
5. The downloaded HTML file works standalone — open it in any browser

---

## Files to Create (Summary)

| File | Lines (est.) | Purpose |
|------|-------------|---------|
| `builder/index.html` | ~80 | Builder shell, CDN imports, mount point |
| `builder/css/builder-ui.css` | ~150 | Builder chrome theming |
| `builder/js/builder-init.js` | ~120 | GrapesJS config, canvas setup, device manager |
| `builder/js/blocks.js` | ~800 | All custom block definitions (largest file) |
| `builder/js/panels.js` | ~200 | Toolbar buttons, panel layout |
| `builder/js/storage.js` | ~200 | Page management, export, import |
| `builder/templates/blank.json` | ~20 | Empty page starter |

**Total: ~1,570 lines across 7 files**

---

## Key Dependencies

| Dependency | Version | CDN URL |
|------------|---------|---------|
| GrapesJS | latest | `https://unpkg.com/grapesjs` |
| GrapesJS CSS | latest | `https://unpkg.com/grapesjs/dist/css/grapes.min.css` |
| gjs-blocks-basic | latest | `https://unpkg.com/grapesjs-blocks-basic` |

All loaded via CDN `<script>` and `<link>` tags. No installation required.

---

## Risks & Mitigations

| Risk | Mitigation |
|------|------------|
| Canvas CSS conflicts with builder UI | GrapesJS uses an iframe for the canvas — styles are naturally isolated |
| Font paths break in canvas | Serve from project root so relative paths in `custom.css` resolve correctly |
| Exported HTML includes GrapesJS wrapper markup | Use clean export mode; post-process to strip `data-gjs-*` attributes |
| User accidentally deletes structural CSS classes | Lock component types on section blocks — only allow content editing |
| localStorage fills up | ~5-10MB limit is sufficient for dozens of pages; add warning + suggest export at 80% |
| GrapesJS CDN goes down | Pin specific version; optionally download and serve locally as fallback |
