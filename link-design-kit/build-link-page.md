# Build Link-Style Page

## Skill Description
Use this skill when asked to create a new page, section, or component that matches the Link.ai design language. This skill ensures visual consistency with the Link brand.

## Instructions

Before building anything, read the full design system at `/Users/bythumb/Desktop/claude_code_folder/websites/link_website/LINK-DESIGN-SYSTEM.md`.

Then follow these mandatory rules:

### Required Setup
1. Include all CSS custom properties from Section 2.1 of the design system
2. Include the fluid font-size scaling (`html { font-size: 1vw }` between 992-1600px)
3. Load Mont font via `@font-face` (5 weights: 400, 600, 700, 800, 900) — font files are in `reference/webflow-export/fonts/`
4. Include the global base reset styles from Section 11

### Color Rules
- Page background: `#fbfcff` (var(--neutral-50))
- Cards: white with `2px solid #dde5f3` border
- Primary accent: `#5ae9d7` (turquoise) — ONLY for CTAs, links, interactive elements
- Headings: `#131f4a` (navy)
- Body text: `#636b87` (neutral-500)
- Muted text: `#9197ae` (neutral-400)
- Dark sections: `linear-gradient(24deg, #131f4a, #022b6d)` with white text
- Tags/badges: `#ff7958` (orange)

### Typography Rules
- Font: `Mont, Arial, sans-serif`
- H1: 5rem/600, H2: 4rem/600, H3: 2.78rem/600, body: 1.111rem/400
- Line-height: headings ~120%, body 162%
- All headings use weight 600 (SemiBold), body uses 400

### Spacing Rules
- Section padding: `4.17rem 6.94rem` (desktop), `3.75rem 1rem` (mobile)
- Section margin: `0.56rem` (creates gaps between rounded sections)
- Section border-radius: `1.39rem`
- Card padding: `2.22rem`, border-radius: `1.11rem`
- Card grid gap: `1.67rem`
- Button border-radius: `0.83rem`

### Button Rules
- Primary: turquoise bg, dark text, white 1px border, radial gradient glow on hover
- Secondary: white bg, navy text, gray border, subtle shadow, lighter bg on hover
- Both: `padding: 1.11rem 1.67rem`, `font-size: 1rem`, weight 600, `transition: all 0.35s`

### Shadow Rules
- Cards: `box-shadow: 0 1px 2px rgba(229,232,235,0.8)`
- Elevated: `0 12px 24px -8px rgba(0,0,0,0.06)` (hover state)
- Never use dark or heavy shadows

### Responsive Rules
- 4+ column grids collapse to 2-col at 991px, 1-col at 767px
- Headings scale down at each breakpoint
- Flex rows stack vertically on mobile
- Section padding reduces on mobile

### Section Patterns
Every section follows one of these patterns (refer to design system Section 8 for full templates):
- Hero Section (image bg + gradient overlay + centered text)
- Card Grid (3-col, 4-col, or 5-col)
- Feature Row (2-col text + visual, reversible)
- Feature List (items with turquoise left border + numbers)
- Comparison Cards (with highlighted "best" card)
- Dark CTA Banner (gradient bg + white text)
- Trust Bar / Marquee (infinite scroll logos)
- Values Grid (icon circles + titles)

### Reference Files
- Full design system: `LINK-DESIGN-SYSTEM.md`
- Style cheat sheet: `link-style-guide.txt`
- Hero example: `prototypes/hero-section-sample.html`
- Section examples: `prototypes/template-sections-v2.html`
- Full site reference: `prototypes/full-site-prototype.html`
- Main CSS reference: `reference/webflow-export/css/link-ai-staging.webflow.css`
- Font files: `reference/webflow-export/fonts/Mont-*.woff2`
