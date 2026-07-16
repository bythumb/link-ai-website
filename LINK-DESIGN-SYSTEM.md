# Link.ai Design System — Complete Agent Reference

> **Purpose**: This document contains everything an AI agent needs to build websites and apps that are visually indistinguishable from link.ai. It covers design tokens, typography, components, layout patterns, animations, responsive behavior, and code templates.

---

## 1. DESIGN PHILOSOPHY

Link.ai follows a **premium, modern, tech-forward** aesthetic:

- **Clean and airy** — pale blue-gray backgrounds, generous whitespace, minimal ornamentation
- **Institutional yet approachable** — professional enough for brokers/agents, friendly enough for consumers
- **Rounded everywhere** — no sharp corners on any element; border-radius scales with element size
- **Depth through shadows** — soft multi-layer shadows create hierarchy, never dark or harsh
- **Typography-driven** — the Mont font family at multiple weights creates clear visual hierarchy
- **Turquoise as the action color** — every primary CTA, link hover, and interactive accent is turquoise
- **Sections as cards** — full-width sections have rounded corners with small outer margins, making the whole page feel like stacked cards

---

## 2. COLOR SYSTEM

### 2.1 CSS Custom Properties (copy exactly)

```css
:root {
  /* Neutral Scale */
  --neutral-50:    #fbfcff;   /* Page background */
  --neutral-100:   #e9eef7;   /* Alternating section backgrounds */
  --neutral-200:   #dde5f3;   /* Card borders, dividers */
  --neutral-300:   #ccd0e0;   /* Input borders, secondary borders */
  --neutral-400:   #9197ae;   /* Muted text, placeholders, captions */
  --neutral-500:   #636b87;   /* Secondary body text */
  --neutral-600:   #4e546a;   /* Medium-weight body text */
  --neutral-700:   #333b57;   /* Darker body text */
  --neutral-800:   #202537;   /* Primary text (headings, button labels) */
  --neutral-900:   #131720;   /* Very dark (rarely used) */
  --neutral-1000:  #040506;   /* Near black (rarely used) */

  /* Brand Colors */
  --turquoise:     #5ae9d7;   /* Primary accent — CTAs, links, interactive */
  --navy:          #131f4a;   /* Dark blue — headings, dark sections */
  --blue:          #022b6d;   /* Secondary blue — gradients, CTA sections */
  --orange:        #ff7958;   /* Alert/accent — tags, badges, highlights */
}
```

### 2.2 Color Usage Rules

| Context | Color | Hex |
|---------|-------|-----|
| Page background | neutral-50 | `#fbfcff` |
| Card/section background (white) | white | `#ffffff` |
| Card/section background (gray) | neutral-100 | `#e9eef7` |
| Card border | neutral-200 | `#dde5f3` |
| Input border (default) | neutral-300 | `#ccd0e0` |
| Input border (focus) | turquoise variant | `#1ccfb8` |
| Primary heading text | navy | `#131f4a` |
| Primary dark text | neutral-800 | `#202537` |
| Body text | neutral-500 | `#636b87` |
| Muted/caption text | neutral-400 | `#9197ae` |
| Text on dark backgrounds | white | `#ffffff` |
| Body text on dark backgrounds | white at 72% opacity | `rgba(255,255,255,0.72)` |
| Link hover color | turquoise hover | `#1ccfb8` |
| Primary button background | turquoise | `#5ae9d7` |
| Primary button text | neutral-800 | `#202537` |
| Tags/badges accent | orange | `#ff7958` |
| Dark CTA gradient start | navy | `#131f4a` |
| Dark CTA gradient end | blue | `#022b6d` |

### 2.3 Key Gradients

```css
/* Dark CTA section background */
background: linear-gradient(24deg, #131f4a, #022b6d);

/* Text gradient (blue-to-turquoise) */
background-image: linear-gradient(31deg, #022b6d, #5ae9d7);
-webkit-background-clip: text;
-webkit-text-fill-color: transparent;

/* Card dark variants */
.dark   { background: #131f4a; }
.teal   { background: linear-gradient(135deg, #0a4a7a, #022b6d); }
.deep   { background: linear-gradient(135deg, #0d1c40, #131f4a); }

/* Button hover glow */
background: radial-gradient(100% 100% at 50% 0%,
  rgba(255,255,255,0.50) 0%,
  rgba(255,255,255,0.00) 79.54%), #5ae9d7;

/* Hero image overlay */
background-image:
  linear-gradient(rgba(0,0,0,0.08) 45%, transparent),
  linear-gradient(rgba(20,31,74,0.53) 34%, transparent),
  url('your-image.webp');
```

---

## 3. TYPOGRAPHY

### 3.1 Font Family

```css
font-family: Mont, Arial, sans-serif;
```

**Mont** is a custom font loaded via `@font-face` in 5 weights:

```css
@font-face { font-family: Mont; src: url('fonts/Mont-Regular.woff2') format('woff2'); font-weight: 400; font-display: swap; }
@font-face { font-family: Mont; src: url('fonts/Mont-SemiBold.woff2') format('woff2'); font-weight: 600; font-display: swap; }
@font-face { font-family: Mont; src: url('fonts/Mont-Bold.woff2') format('woff2'); font-weight: 700; font-display: swap; }
@font-face { font-family: Mont; src: url('fonts/Mont-Heavy.woff2') format('woff2'); font-weight: 800; font-display: swap; }
@font-face { font-family: Mont; src: url('fonts/Mont-Black.woff2') format('woff2'); font-weight: 900; font-display: swap; }
```

> **Fallback**: If Mont is unavailable, use `Montserrat` from Google Fonts as the closest match, then `Arial, sans-serif`.

### 3.2 Type Scale

| Class | Size | Weight | Line Height | Letter Spacing | Use |
|-------|------|--------|-------------|----------------|-----|
| `.heading-1` | 5rem | 600 | 112% | 0.05rem | Hero headlines |
| `.heading-2` | 4rem | 600 | 119% | 0.04rem | Section headlines |
| `.heading-3` | 2.78–3.33rem | 600 | 120% | 0.03rem | Sub-section headlines |
| `.heading-4` | 1.875rem | 600 | 2.5rem | — | Card titles, features |
| `.heading-5` | 1.389rem | 600 | 150% | — | Small headings |
| `.heading-hero` | 4.44rem | 600 | 118% | -0.044rem | Hero-specific headline |
| `.body-m` | 1.111rem | 400 | 162% | — | Primary body text |
| `.body-s` | varies | 400 | 150% | — | Small body text |
| `.body-hero` | 1.111rem | 400 | 162% | — | Hero subtitle (white) |
| `.caption` | 0.833rem | 400 | 150% | — | Captions, metadata |
| `.tag` | 0.833rem | 600–700 | 133% | 0.017rem | Tags, badges (uppercase) |

### 3.3 Fluid Scaling

```css
html { font-size: 1rem; } /* Default: 16px base */

/* Between 992px and 1600px, font-size scales with viewport */
@media screen and (max-width: 1600px) and (min-width: 992px) {
  html { font-size: 1vw; }
}
/* Between 768px and 991px, same fluid scaling */
@media screen and (max-width: 991px) and (min-width: 768px) {
  html { font-size: 1vw; }
}
```

This means all `rem` values scale proportionally with the viewport between 992–1600px, creating fluid responsive typography without breakpoint jumps.

### 3.4 Text Color Classes

```css
.heading-3 { color: var(--navy); }           /* #131f4a */
.body-m { color: var(--navy); }              /* #131f4a */
.body-m.muted { color: var(--neutral-600); } /* #4e546a */
/* On dark backgrounds: color: #fff or rgba(255,255,255,0.72) */
```

---

## 4. SPACING SYSTEM

All spacing uses `rem` units (which scale with the fluid font-size).

### 4.1 Common Spacing Values

| Value | Pixels (at 16px base) | Usage |
|-------|----------------------|-------|
| 0.28rem | ~4.5px | Tag padding, checkbox spacing |
| 0.56rem | ~9px | Small gaps, badge spacing, section margins |
| 0.83rem | ~13px | Button border-radius, small padding |
| 1.11rem | ~18px | Card gap, moderate padding, heading gaps |
| 1.39rem | ~22px | Section border-radius, divider margins |
| 1.67rem | ~27px | Card grid gap, pill badge radius |
| 2.22rem | ~36px | Card inner padding, footer spacing |
| 2.78rem | ~44px | Card inner padding (larger variant) |
| 4.17rem | ~67px | Section top/bottom padding |
| 4.44rem | ~71px | Large feature gaps |
| 5.56rem | ~89px | Hero container top padding |
| 6.94rem | ~111px | Section horizontal padding (desktop) |

### 4.2 Section Layout Pattern

```css
/* White section */
.section-white {
  background-color: #fff;
  border-radius: 1.39rem;
  margin: 0.56rem;
  padding: 4.17rem 6.94rem;
}

/* Gray section */
.section-gray {
  background: var(--neutral-100);
  border-radius: 1.39rem;
  margin: 0 0.56rem;
  padding: 4.17rem 6.94rem;
}

/* Dark CTA section */
.section-dark {
  background: linear-gradient(24deg, #131f4a, #022b6d);
  border-radius: 1.39rem;
  margin: 0.56rem;
  padding: 4.17rem 6.94rem;
  color: #fff;
}
```

---

## 5. BORDER RADIUS SCALE

| Size | Value | Applied To |
|------|-------|-----------|
| XS | 0.28rem | Tags, checkboxes, tiny badges |
| S | 0.56rem | Nav links, small badges, hero tags |
| M | 0.83rem | Buttons, inputs, small cards |
| L | 1.11rem | Cards, card-like containers |
| XL | 1.39rem | Full-width sections, large wrappers |
| Pill | 1.67rem | Pill-shaped badges |
| Circle | 50% or 100px | Avatar frames, icon circles, nav arrows |

---

## 6. SHADOW SYSTEM

```css
/* Primary card shadow (use on all cards) */
box-shadow:
  0 40px 64px -12px rgba(0, 0, 0, 0.02),
  0 32px 48px -8px rgba(0, 0, 0, 0.02),
  0 12px 12px -8px rgba(229, 232, 235, 0.50);

/* Simpler card shadow (lighter variant) */
box-shadow: 0 1px 2px rgba(229, 232, 235, 0.8);

/* Combined for inputs */
box-shadow:
  0 1px 2px rgba(229, 232, 235, 0.8),
  0 12px 12px -8px rgba(229, 232, 235, 0.6);

/* Button shadow (subtle) */
box-shadow: 0 1px 2px rgba(227, 228, 229, 0.25);

/* Featured/elevated card */
box-shadow: 0 4px 24px rgba(19, 31, 74, 0.12);

/* Hover card elevation */
box-shadow: 0 12px 24px -8px rgba(0, 0, 0, 0.06);

/* Large elevation (rare) */
box-shadow: 0 8px 28px rgba(0, 0, 0, 0.08);
```

**Key rule**: Shadows are always soft, layered, and use desaturated blue-gray tones — never pure black at high opacity.

---

## 7. COMPONENT LIBRARY

### 7.1 Buttons

```css
/* PRIMARY BUTTON (turquoise) */
.btn-primary {
  background-color: var(--turquoise);     /* #5ae9d7 */
  color: var(--neutral-800);              /* #202537 */
  border: 1px solid #fff;
  border-radius: 0.83rem;
  padding: 0.9rem 1.94rem 0.76rem;       /* or: 1.11rem 1.67rem */
  font-family: Mont, Arial, sans-serif;
  font-size: 1rem;
  font-weight: 600;
  text-decoration: none;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: all 0.35s;
}
.btn-primary:hover {
  background: radial-gradient(100% 100% at 50% 0%,
    rgba(255,255,255,0.50) 0%,
    rgba(255,255,255,0.00) 79.54%), #5ae9d7;
}

/* SECONDARY BUTTON (white) */
.btn-secondary {
  background-color: #fff;
  color: var(--navy);                     /* #131f4a */
  border: 1px solid var(--neutral-300);   /* #ccd0e0 */
  border-radius: 0.83rem;
  padding: 0.9rem 1.94rem 0.76rem;
  font-family: Mont, Arial, sans-serif;
  font-size: 1rem;
  font-weight: 600;
  text-decoration: none;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: all 0.35s ease;
  box-shadow: 0 1px 2px rgba(229,232,235,0.8);
}
.btn-secondary:hover {
  background-color: var(--neutral-50);    /* #fbfcff */
}
```

### 7.2 Cards

```css
/* Standard card */
.card {
  background: #fff;
  border: 2px solid var(--neutral-200);   /* #dde5f3 */
  border-radius: 1.11rem;
  padding: 2.22rem;
  box-shadow: 0 1px 2px rgba(229,232,235,0.8);
  transition: border-color 0.3s, box-shadow 0.3s, transform 0.3s;
}
.card:hover {
  border-color: var(--turquoise);
  box-shadow: 0 12px 24px -8px rgba(0,0,0,0.06);
  transform: translateY(-3px);
}

/* Featured card (stands out) */
.card-featured {
  border: 2px solid var(--turquoise);
  box-shadow: 0 4px 24px rgba(19,31,74,0.12);
}
```

### 7.3 Tags / Badges

```css
/* Orange tag (default) */
.tag {
  color: #fff;
  background-color: var(--orange);        /* #ff7958 */
  padding: 0.35rem 0.69rem 0.21rem;
  font-size: 0.833rem;
  font-weight: 600;
  letter-spacing: 0.017rem;
  text-transform: uppercase;
  border-radius: 0.28rem;
  line-height: 133%;
}

/* Subtle tag (outline) */
.tag-subtle {
  color: var(--neutral-600);
  background-color: var(--neutral-50);
  border: 1px solid var(--neutral-100);
  border-radius: 0.28rem;
  padding: 0.35rem 0.69rem 0.21rem;
  font-size: 0.833rem;
  font-weight: 600;
  letter-spacing: 0.017rem;
  text-transform: uppercase;
}

/* Eyebrow label (used above section headings) */
.eyebrow {
  color: var(--orange);
  font-size: 0.833rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}
```

### 7.4 Form Inputs

```css
.input {
  background-color: var(--neutral-50);    /* #fbfcff */
  color: var(--neutral-800);              /* #202537 */
  border: 1px solid var(--neutral-300);   /* #ccd0e0 */
  border-radius: 1.25rem;
  padding: 1.25rem 1.25rem 1.25rem 1.67rem;
  font-family: Mont, Arial, sans-serif;
  font-size: 1.111rem;
  font-weight: 400;
  line-height: 162%;
  width: 100%;
  outline: none;
  box-shadow: 0 1px 2px rgba(229,232,235,0.8),
              0 12px 12px -8px rgba(229,232,235,0.6);
  transition: border-color 0.2s ease;
}
.input:hover {
  border-color: var(--neutral-300);
}
.input:focus {
  border-color: var(--turquoise);         /* #5ae9d7 or #1ccfb8 */
}
.input::placeholder {
  color: var(--neutral-800);
}
```

### 7.5 Navigation

```css
.nav-link {
  color: var(--navy);                     /* #131f4a */
  font-size: 0.972rem;
  font-weight: 400;
  line-height: 171%;
  transition: all 0.3s ease;
}
.nav-link:hover {
  color: #1ccfb8;
}

/* Nav link with border variant */
.nav-link-bordered {
  border: 1px solid transparent;
  border-radius: 0.28rem;
  padding: 0.42rem 1.11rem 0.28rem;
  transition: all 0.3s ease;
}
.nav-link-bordered:hover {
  border-color: var(--neutral-100);
  background-color: var(--neutral-50);
  box-shadow: 0 1px 2px rgba(227,228,229,0.25);
}
```

### 7.6 Icon Circles

```css
/* Colored icon circle (used in feature grids) */
.icon-circle {
  width: 2.78rem;
  height: 2.78rem;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.icon-circle.teal   { background: rgba(90, 233, 215, 0.15); }
.icon-circle.orange  { background: rgba(255, 121, 88, 0.15); }
.icon-circle.blue    { background: rgba(2, 43, 109, 0.12); }

/* Check icon (turquoise circle with white checkmark) */
.check-icon {
  width: 1.39rem;
  height: 1.39rem;
  background-color: var(--turquoise);
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
}
```

### 7.7 Dividers

```css
hr {
  border: none;
  height: 1px;
  background-color: var(--neutral-100);   /* #e9eef7 */
  margin: 1.39rem 0;
}
```

---

## 8. SECTION PATTERNS (Templates)

### 8.1 Section Header (reusable across all sections)

```html
<div class="section-header" style="display:flex; flex-flow:column; align-items:center;
  text-align:center; gap:1.11rem; max-width:42.222rem; margin:0 auto 3.61rem;">
  <div class="eyebrow">SECTION LABEL</div>
  <h2 class="heading-3">Section Headline<br>With Line Break</h2>
  <p class="body-m muted">Supporting paragraph describing the section content.
    Keep to 1-2 sentences.</p>
</div>
```

### 8.2 Hero Section

```html
<section style="padding:9.56rem .56rem 0; position:relative;">
  <div style="background-image:linear-gradient(rgba(0,0,0,0.08) 45%,transparent),
    linear-gradient(rgba(20,31,74,0.53) 34%,transparent),
    url('hero-image.webp');
    background-position:0 0,0 0,50% 0; background-size:auto,auto,cover;
    border-radius:1.39rem; overflow:hidden;">
    <div style="text-align:center; display:flex; flex-direction:column;
      align-items:center; max-width:99vw; padding:5.56rem 0 2rem; position:relative; z-index:9;">
      <div style="display:flex; flex-flow:column; align-items:center; gap:1.11rem; max-width:57rem;">
        <div class="tag">LABEL</div>
        <h1 style="color:#fff; font-size:4.44rem; font-weight:600; line-height:118%;">
          Hero Headline
        </h1>
        <p style="color:#fff; max-width:38rem; font-size:1.111rem; font-weight:400; line-height:162%;">
          Hero subtitle text goes here
        </p>
      </div>
      <!-- CTA buttons or search form go here -->
    </div>
  </div>
</section>
```

### 8.3 3-Column Card Grid

```html
<section style="background:#fff; border-radius:1.39rem; margin:.56rem; padding:4.17rem 6.94rem;">
  <!-- Section Header -->
  <div style="display:grid; grid-template-columns:repeat(3,1fr); gap:1.67rem;">
    <div class="card">
      <div class="icon-circle teal"><!-- icon SVG --></div>
      <h3 style="color:var(--navy); font-size:1.111rem; font-weight:700;">Card Title</h3>
      <p style="color:var(--neutral-600); font-size:.97rem; line-height:165%;">Card description</p>
    </div>
    <!-- Repeat cards -->
  </div>
</section>
```

### 8.4 Two-Column Feature Row

```html
<section style="background:#fff; border-radius:1.39rem; margin:.56rem; padding:4.17rem 6.94rem;">
  <div style="display:flex; gap:4.44rem; align-items:center;">
    <div style="flex:1;">
      <h2 class="heading-3" style="margin-bottom:1.11rem;">Feature Headline</h2>
      <p class="body-m" style="margin-bottom:2.22rem;">Feature description paragraph.</p>
      <a href="#" class="btn-primary">Call to Action</a>
    </div>
    <div style="flex:1;">
      <!-- Image or feature list -->
    </div>
  </div>
</section>
```

### 8.5 Feature List with Left Border

```html
<div style="display:flex; gap:1.11rem; align-items:flex-start; padding:1.39rem;
  background:#fff; border-radius:.83rem; box-shadow:0 2px 8px rgba(19,31,74,0.06);
  border-left:4px solid var(--turquoise);">
  <div style="font-size:1.67rem; font-weight:800; color:var(--turquoise); line-height:1;">01</div>
  <div>
    <h4 style="color:var(--navy); font-size:1.111rem; font-weight:700; margin-bottom:.28rem;">Feature Title</h4>
    <p style="color:var(--neutral-500); font-size:.97rem; line-height:160%;">Feature description</p>
  </div>
</div>
```

### 8.6 Comparison Cards (4-column)

```html
<section style="padding:0 .56rem;">
  <div style="background:var(--neutral-100); border-radius:1.39rem; padding:4.17rem 6.94rem;">
    <!-- Section Header -->
    <div style="display:grid; grid-template-columns:repeat(4,1fr); gap:1.11rem;">
      <!-- Competitor card -->
      <div style="border-radius:1.11rem; padding:2.22rem 1.67rem; text-align:center;
        background:var(--neutral-50); border:2px solid transparent;">
        <div style="font-size:.97rem; font-weight:700; color:var(--neutral-500);">Competitor</div>
        <div style="font-size:2.78rem; font-weight:800; color:var(--neutral-500);">70/30</div>
      </div>
      <!-- Highlighted (Link) card -->
      <div style="border-radius:1.11rem; padding:2.22rem 1.67rem; text-align:center;
        background:var(--navy); border:2px solid var(--turquoise); position:relative;">
        <div style="position:absolute; top:-0.9rem; left:50%; transform:translateX(-50%);
          background:var(--turquoise); color:var(--navy); font-size:.69rem; font-weight:800;
          padding:.21rem .83rem; border-radius:3rem;">BEST VALUE</div>
        <div style="font-size:.97rem; font-weight:700; color:var(--turquoise);">Link</div>
        <div style="font-size:2.78rem; font-weight:800; color:#fff;">90/10</div>
      </div>
    </div>
  </div>
</section>
```

### 8.7 Dark CTA Banner

```html
<section style="background:linear-gradient(24deg,#131f4a,#022b6d); border-radius:1.39rem;
  margin:.56rem; padding:4.17rem 6.94rem; text-align:center;">
  <h2 style="color:#fff; font-size:2.78rem; font-weight:600; line-height:120%; margin-bottom:1.11rem;">
    CTA Headline
  </h2>
  <p style="color:rgba(255,255,255,0.72); font-size:1.111rem; line-height:162%; max-width:38rem;
    margin:0 auto 2.22rem;">CTA supporting text.</p>
  <a href="#" class="btn-primary">Get Started</a>
</section>
```

### 8.8 Infinite Marquee / Trust Bar

```html
<div style="display:flex; flex-flow:column; gap:.62rem; overflow:hidden;">
  <p style="color:#fff; text-align:center; font-size:1.111rem; font-weight:400;">As seen on</p>
  <div style="display:flex; gap:4.8rem; width:max-content; animation:marquee 220s linear infinite;">
    <div style="display:flex; gap:4.8rem; flex:none;">
      <img src="logo1.webp" style="opacity:.54; filter:brightness(200%); width:9rem;" alt="">
      <!-- Repeat logos -->
    </div>
    <!-- Duplicate set for seamless loop -->
    <div style="display:flex; gap:4.8rem; flex:none;">
      <!-- Same logos repeated -->
    </div>
  </div>
</div>

<style>
@keyframes marquee {
  0%   { transform: translateX(0); }
  100% { transform: translateX(-80%); }
}
</style>
```

### 8.9 Values / Icon Grid (5-column)

```html
<div style="display:grid; grid-template-columns:repeat(5,1fr); gap:1.11rem;">
  <div style="background:var(--neutral-50); border:1px solid var(--neutral-200);
    border-radius:1.11rem; padding:2.22rem 1.39rem; text-align:center;
    transition:transform .25s, box-shadow .25s;">
    <div style="width:3.11rem; height:3.11rem; border-radius:50%;
      background:rgba(90,233,215,0.12); display:flex; align-items:center;
      justify-content:center; margin:0 auto 1.11rem;">
      <!-- Icon SVG -->
    </div>
    <h3 style="color:var(--navy); font-size:.97rem; font-weight:700; margin-bottom:.56rem;">Value Title</h3>
    <p style="color:var(--neutral-500); font-size:.83rem; line-height:160%;">Value description</p>
  </div>
  <!-- Repeat -->
</div>
```

---

## 9. ANIMATIONS & TRANSITIONS

### 9.1 Standard Transition Durations

| Duration | Easing | Usage |
|----------|--------|-------|
| 0.2s | ease | Input focus, close buttons |
| 0.25s | ease | Footer links, value card hover |
| 0.3s | ease | Nav links, tags, hero tags |
| 0.35s | ease | Buttons (primary/secondary), main interactions |

### 9.2 Hover Effects

```css
/* Card hover — lift + glow */
.card:hover {
  border-color: var(--turquoise);
  box-shadow: 0 12px 24px -8px rgba(0,0,0,0.06);
  transform: translateY(-3px);
}

/* Value card hover */
.value-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 28px rgba(0,0,0,0.08);
}

/* Link hover */
a:hover { color: #1ccfb8; }

/* Button hover — radial glow on turquoise */
.btn-primary:hover {
  background: radial-gradient(100% 100% at 50% 0%,
    rgba(255,255,255,0.5) 0%, rgba(255,255,255,0) 79.54%), #5ae9d7;
}
```

### 9.3 Keyframe Animations

```css
/* Infinite horizontal marquee */
@keyframes marquee {
  0%   { transform: translateX(0); }
  100% { transform: translateX(-80%); }
}

/* Reverse marquee */
@keyframes marquee2 {
  0%   { transform: translateX(-80%); }
  100% { transform: translateX(0); }
}

/* Fade in */
@keyframes fadeInOut {
  0%   { opacity: 0; }
  100% { opacity: 1; }
}

/* Loading spinner */
@keyframes spin {
  0%   { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
```

---

## 10. RESPONSIVE DESIGN

### 10.1 Breakpoints

| Name | Width | Key Changes |
|------|-------|-------------|
| Desktop | 992px+ | Full layout, 1vw fluid scaling (992–1600px) |
| Tablet | 768–991px | Grids reduce columns, fluid scaling continues |
| Mobile | 480–767px | Single column, stacked layouts, reduced padding |
| Small mobile | <479px | Further size reductions, simplified grids |

### 10.2 Responsive Rules

```css
/* Tablet */
@media screen and (max-width: 991px) {
  /* 4-col → 2-col, 5-col → 3-col */
  .grid-4 { grid-template-columns: repeat(2, 1fr); }
  .grid-5 { grid-template-columns: repeat(3, 1fr); }
}

/* Mobile */
@media screen and (max-width: 767px) {
  /* All sections: reduce padding */
  .section { padding: 3.75rem 1rem; margin: 0.5rem 0; }

  /* Headings: scale down */
  .heading-3 { font-size: 2.5rem; }
  .heading-hero { font-size: 3rem; }

  /* Grids: collapse to 1-col (some to 2-col) */
  .grid-3 { grid-template-columns: 1fr; }
  .grid-2 { grid-template-columns: 1fr; }

  /* Flex rows: stack vertically */
  .feature-row { flex-flow: column; gap: 2.5rem; }
}

/* Small mobile */
@media screen and (max-width: 479px) {
  .heading-3 { font-size: 2rem; }
  .heading-hero { font-size: 2.5rem; }
  .grid-4 { grid-template-columns: 1fr; }
  .grid-5 { grid-template-columns: 1fr; }
}
```

### 10.3 Visibility Classes

```css
.hide-desktop          { /* hidden above 992px */ }
.hide-tablet           { /* hidden 768–991px */ }
.hide-mobile-landscape { /* hidden 480–767px */ }
.hide-mobile           { /* hidden below 479px */ }
```

---

## 11. GLOBAL BASE STYLES

Every page should start with this base:

```css
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
  background-color: var(--neutral-50);         /* #fbfcff */
  color: #333;
  font-family: Mont, Arial, sans-serif;
  font-size: 14px;
  font-weight: 600;
  line-height: 20px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-rendering: optimizeLegibility;
}

a { color: inherit; text-decoration: none; }
img { max-width: 100%; height: auto; display: block; }
```

---

## 12. ASSET GUIDELINES

### Images
- Use **WebP** format for photos (`.webp`)
- Use **SVG** for icons and logos
- Hero backgrounds: `background-size: cover; background-position: 50% 0;`
- Always include `loading="lazy"` on below-fold images
- Marquee logos: `opacity: 0.54; filter: brightness(200%);` on dark backgrounds

### Icons
- SVG inline icons, stroke-based (not filled), 24x24 viewBox
- Stroke colors match the icon circle tint:
  - Teal icons: `stroke="#5AE9D7"`
  - Orange icons: `stroke="#FF7958"`
  - Blue icons: `stroke="#022B6D"`
- Stroke width: 1.5–2px
- Linecap: `round`, linejoin: `round`

### Custom Scrollbar (optional enhancement)

```css
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: var(--neutral-100); }
::-webkit-scrollbar-thumb { background: var(--turquoise); border-radius: 3px; }
```

---

## 13. DO's AND DON'Ts

### DO
- Use the exact hex values from the color system — no approximations
- Apply `border-radius` to everything — sections, cards, buttons, inputs, images
- Keep generous whitespace — sections with 4.17rem vertical padding minimum
- Use `transition: all 0.35s` on all interactive elements
- Apply the multi-layer card shadow on elevated surfaces
- Use turquoise (#5ae9d7) as the only primary action color
- Use the fluid `1vw` font-size scaling between 992–1600px
- Place eyebrow labels above section headings in orange uppercase
- Use 2px solid borders on cards (not 1px) — color: `#dde5f3`

### DON'T
- Use sharp corners on any visible element
- Use dark/heavy shadows — all shadows are soft and desaturated
- Use more than 3 colors in any single section (typically: navy text, gray body, turquoise accent)
- Add excessive decoration — the design relies on typography and whitespace, not ornament
- Use font weights below 400 or above 900
- Use pure black (`#000`) anywhere — the darkest color is `#040506` (neutral-1000), used rarely
- Mix border-radius scales — cards are always 1.11rem, sections always 1.39rem, buttons always 0.83rem
- Use gradients on light sections — gradients are reserved for dark CTA sections and dark card variants

---

## 14. COMPLETE STARTER TEMPLATE

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Page Title — Link</title>

  <style>
    /* Font faces */
    @font-face { font-family: Mont; src: url('fonts/Mont-Regular.woff2') format('woff2'); font-weight: 400; font-display: swap; }
    @font-face { font-family: Mont; src: url('fonts/Mont-SemiBold.woff2') format('woff2'); font-weight: 600; font-display: swap; }
    @font-face { font-family: Mont; src: url('fonts/Mont-Bold.woff2') format('woff2'); font-weight: 700; font-display: swap; }
    @font-face { font-family: Mont; src: url('fonts/Mont-Heavy.woff2') format('woff2'); font-weight: 800; font-display: swap; }
    @font-face { font-family: Mont; src: url('fonts/Mont-Black.woff2') format('woff2'); font-weight: 900; font-display: swap; }

    /* Design tokens */
    :root {
      --neutral-50:   #fbfcff;  --neutral-100: #e9eef7;
      --neutral-200:  #dde5f3;  --neutral-300: #ccd0e0;
      --neutral-400:  #9197ae;  --neutral-500: #636b87;
      --neutral-600:  #4e546a;  --neutral-700: #333b57;
      --neutral-800:  #202537;  --neutral-900: #131720;
      --neutral-1000: #040506;
      --turquoise:    #5ae9d7;
      --navy:         #131f4a;
      --blue:         #022b6d;
      --orange:       #ff7958;
    }

    /* Fluid scaling */
    html { font-size: 1rem; }
    @media screen and (max-width: 1600px) and (min-width: 992px) { html { font-size: 1vw; } }
    @media screen and (max-width: 991px) and (min-width: 768px) { html { font-size: 1vw; } }

    /* Reset */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      background-color: var(--neutral-50);
      color: #333;
      font-family: Mont, Arial, sans-serif;
      font-size: 14px;
      font-weight: 600;
      line-height: 20px;
      -webkit-font-smoothing: antialiased;
    }
    a { color: inherit; text-decoration: none; }
    img { max-width: 100%; height: auto; display: block; }

    /* Typography */
    .heading-2 { color: var(--navy); font-size: 4rem; font-weight: 600; line-height: 119%; letter-spacing: .04rem; }
    .heading-3 { color: var(--navy); font-size: 2.78rem; font-weight: 600; line-height: 120%; letter-spacing: .03rem; }
    .heading-5 { color: var(--navy); font-size: 1.389rem; font-weight: 600; line-height: 150%; }
    .body-m { color: var(--navy); font-size: 1.111rem; font-weight: 400; line-height: 162%; }
    .body-m.muted { color: var(--neutral-600); }
    .eyebrow { color: var(--orange); font-size: .833rem; font-weight: 700; letter-spacing: .12em; text-transform: uppercase; }

    /* Buttons */
    .btn-primary {
      background-color: var(--turquoise); color: var(--neutral-800); border: 1px solid #fff;
      border-radius: .83rem; padding: 1.11rem 1.67rem; font-family: Mont, Arial, sans-serif;
      font-size: 1rem; font-weight: 600; display: inline-flex; justify-content: center;
      align-items: center; cursor: pointer; transition: all .35s; text-decoration: none;
    }
    .btn-primary:hover {
      background: radial-gradient(100% 100% at 50% 0%, rgba(255,255,255,0.5) 0%, rgba(255,255,255,0) 79.54%), #5ae9d7;
    }
    .btn-secondary {
      border: 1px solid var(--neutral-300); background-color: #fff; color: var(--navy);
      border-radius: .83rem; padding: 1.11rem 1.67rem; font-family: Mont, Arial, sans-serif;
      font-size: 1rem; font-weight: 600; display: inline-flex; justify-content: center;
      align-items: center; cursor: pointer; transition: all .35s ease;
      box-shadow: 0 1px 2px rgba(229,232,235,0.8); text-decoration: none;
    }
    .btn-secondary:hover { background-color: var(--neutral-50); }

    /* Cards */
    .card {
      background: #fff; border: 2px solid var(--neutral-200); border-radius: 1.11rem;
      padding: 2.22rem; transition: border-color .3s, box-shadow .3s, transform .3s;
    }
    .card:hover { border-color: var(--turquoise); box-shadow: 0 12px 24px -8px rgba(0,0,0,0.06); transform: translateY(-3px); }

    /* Sections */
    .section-white { background: #fff; border-radius: 1.39rem; margin: .56rem; padding: 4.17rem 6.94rem; }
    .section-gray  { background: var(--neutral-100); border-radius: 1.39rem; margin: 0 .56rem; padding: 4.17rem 6.94rem; }
    .section-dark  { background: linear-gradient(24deg, #131f4a, #022b6d); border-radius: 1.39rem; margin: .56rem; padding: 4.17rem 6.94rem; color: #fff; }

    /* Section header */
    .section-header { display: flex; flex-flow: column; align-items: center; text-align: center; gap: 1.11rem; max-width: 42.222rem; margin: 0 auto 3.61rem; }

    /* Container */
    .container { width: 100%; max-width: 90rem; margin: 0 auto; }

    /* Responsive */
    @media screen and (max-width: 767px) {
      .section-white, .section-gray, .section-dark { padding: 3.75rem 1rem; margin: .5rem 0; }
      .heading-3 { font-size: 2.5rem; }
    }
    @media screen and (max-width: 479px) {
      .heading-3 { font-size: 2rem; }
    }
  </style>
</head>
<body>

  <!-- Add sections here using the patterns from the design system -->

</body>
</html>
```

---

## 15. PHOTOGRAPHY & OFF-CENTERED IMAGERY

### 15.1 Photo Categories

Link.ai uses three types of photography, each with a distinct purpose:

| Category | Examples | Use case |
|----------|----------|----------|
| **Lifestyle** | Agents with clients, people at laptops, team in office | Sections about people, trust, human connection |
| **Property** | Luxury home exteriors, staged interiors | Sections about listings, valuation, seller tools |
| **Product UI** | App screenshots, chat interfaces, map/search views | Sections demonstrating specific features |

Never mix all three in a single composition — pick one dominant category per section.

---

### 15.2 The Off-Centered Float Treatment

The "floating photo" pattern places 2–4 images in the gutters of a centered-text hero or feature section. Photos appear slightly outside the main content column, creating an editorial, magazine-like depth without competing with the copy.

**Rules:**
- Photos live in the **side gutters only** — never overlapping the headline or body copy
- Use an even number of photos (2 or 4), balanced left/right
- Each photo is a **different size** (vary height by 15–30px between adjacent photos)
- Photos on the same side should be at **different heights** — never aligned
- The visual center of gravity should still be the text, not the photos

**When to use:**
- Hero sections where the headline is a logo or very short (1–3 words)
- Full-width feature sections with a single strong message
- Campaign landing pages

**When NOT to use:**
- Sections with complex copy or multiple subpoints
- Mobile — always hide floating photos below 1100px; they compress into the text column

---

### 15.3 Rotation

Every floating photo gets a subtle rotation. This breaks the grid rigidity and makes the layout feel alive without being cluttered.

```
Left-side photos:   alternate between -1.5deg and +1.5deg
Right-side photos:  mirror the opposite of their left counterpart
Maximum rotation:   ±2.5deg — beyond this feels accidental, not intentional
Never:              two adjacent photos rotated the same direction
```

```css
/* Pattern for 4-photo float */
.hp1 { transform: rotate(-1.8deg); }   /* left top */
.hp2 { transform: rotate( 1.5deg); }   /* left bottom */
.hp3 { transform: rotate( 2.0deg); }   /* right top */
.hp4 { transform: rotate(-1.5deg); }   /* right bottom */
```

---

### 15.4 Shadows & Border Radius on Photos

```css
/* Standard photo shadow */
box-shadow:
  0 8px 32px rgba(19, 31, 74, 0.08),
  0 2px 6px rgba(0, 0, 0, 0.05);

/* Border radius */
border-radius: 1rem;   /* standard photos */
border-radius: 1.39rem; /* large hero photos */
```

Never use dark or harsh shadows (no `rgba(0,0,0,0.3)+`). The shadow should be barely visible — it creates lift, not drama.

---

### 15.5 Photo Sizing

| Position | Typical width | Typical height |
|----------|--------------|----------------|
| Left top | 185–205px | 215–240px |
| Left bottom | 165–185px | 195–215px |
| Right top | 190–210px | 215–235px |
| Right bottom | 175–195px | 205–225px |

Keep photos taller than wide (portrait orientation) for the floating treatment. Landscape photos flatten the composition.

---

### 15.6 Animation

Floating photos animate in after the core copy, reinforcing that they are context — not the message.

```css
/* Core copy: delay 0.3s–0.7s */
/* Floating photos: delay 0.8s–1.2s, staggered 0.15s apart */

.hp1 { animation: fadeIn .9s ease forwards .80s; }
.hp2 { animation: fadeIn .9s ease forwards .95s; }
.hp3 { animation: fadeIn .9s ease forwards .90s; }
.hp4 { animation: fadeIn .9s ease forwards 1.05s; }
```

Use a simple `fadeIn` (opacity + subtle translateY) — not spring physics or dramatic slides.

---

### 15.7 Do's and Don'ts

**Do:**
- Use real photos of real people and real properties — no stock generics
- Maintain the rounded corner and soft shadow on every photo
- Vary sizes between photos in the same composition
- Test that the layout still works with photos hidden (mobile fallback)

**Don't:**
- Add text or overlays directly on floating photos
- Use filters, duotones, or brand-colored tints on photography
- Place a product screenshot next to a lifestyle photo — they fight each other
- Use more than 4 floating photos in a single section

---

## 16. QUICK REFERENCE CHEAT SHEET

```
COLORS:     turquoise=#5AE9D7  navy=#131F4A  blue=#022B6D  orange=#FF7958
            bg=#FBFCFF  card-border=#DDE5F3  body-text=#636B87
FONT:       Mont (400/600/700/800/900) | fallback: Montserrat, Arial
SIZES:      H1=5rem  H2=4rem  H3=2.78rem  H4=1.875rem  body=1.111rem
RADIUS:     sections=1.39rem  cards=1.11rem  buttons=0.83rem  tags=0.28rem
SPACING:    section-pad=4.17rem  card-pad=2.22rem  grid-gap=1.67rem
SHADOWS:    soft multi-layer, never dark — see shadow system
TRANSITIONS: 0.35s standard, 0.3s nav, 0.2s inputs
SCALING:    html{font-size:1vw} between 992–1600px
```
