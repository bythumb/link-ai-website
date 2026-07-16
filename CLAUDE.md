# Link Website — Agent Instructions

## Project Overview
This is the Link.ai / Link Brokerages website — a real estate tech platform. The design language is premium, modern, and turquoise-accented.

## Design System
**Always read `LINK-DESIGN-SYSTEM.md` before creating or modifying any page, section, or component.** It contains the complete specification: colors, typography, spacing, components, animations, responsive rules, and full code templates.

Quick reference: `link-style-guide.txt`

## Key Design Rules (Non-Negotiable)
- Font: **Mont** (400/600/700/800/900) — files in `reference/webflow-export/fonts/`
- Primary accent: **#5AE9D7** (turquoise) — only for CTAs and interactive elements
- Headings: **#131F4A** (navy), body text: **#636B87**
- Page bg: **#FBFCFF**, card borders: **2px solid #DDE5F3**
- All elements have border-radius (sections: 1.39rem, cards: 1.11rem, buttons: 0.83rem)
- Sections use `margin: 0.56rem` for gaps between rounded blocks
- Fluid typography: `html { font-size: 1vw }` between 992–1600px
- Shadows are always soft and multi-layered, never dark

## Project Structure
```
link-style-guide.txt              # Quick color/font/spacing reference
LINK-DESIGN-SYSTEM.md             # Complete design system (READ THIS)
prototypes/                       # Working HTML prototypes
  hero-section-sample.html        # Hero section with full design tokens
  template-sections-v2.html       # Section component library
  full-site-prototype.html        # Full page layout reference
reference/webflow-export/         # Original Webflow export
  css/link-ai-staging.webflow.css # Main CSS (4800+ lines, authoritative)
  fonts/Mont-*.woff2              # Font files (5 weights)
  images/                         # All images (SVG + WebP)
specs/website-builder-spec.md     # GrapesJS builder specification
```

## When Building New Pages
1. Read `LINK-DESIGN-SYSTEM.md` sections 1–6 for tokens
2. Pick section patterns from section 8
3. Use the starter template from section 14
4. Check responsive rules from section 10
5. Follow the Do's and Don'ts in section 13
