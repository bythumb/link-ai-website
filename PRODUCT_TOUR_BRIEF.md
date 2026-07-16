# Product Tour — Video Brief

**For:** Claude designer agent (or any video/motion designer)
**Deliverable:** 20–40 second product tour video, screen-recording style with zoom/pan camera moves
**Tone:** Confident, calm, modern SaaS — no stock-music energy drops or corporate cheese
**Aspect ratio:** 16:9, 1920×1080 (or 1080×1920 if repurposed for social)

---

## 0. READ THESE FILES FIRST (source of truth)

Do **not** rely on the prose descriptions below alone — the actual component code is authoritative. Read the source files for each scene before mocking anything up. My descriptions may drift; the code will not.

**Design tokens & rules:**
- `app/globals.css` — all color tokens (`--color-brand-*`, `--color-ap-*`), radius, shadow definitions
- `AGENTS.md` — brand accent rule (one teal-500 per page, palette anchors)
- `CLAUDE.md` — UI patterns, shared component list

**Per-scene source files:**

| Scene | Route file | Key component(s) |
|---|---|---|
| 1. Dashboard workflow launcher | `app/(app)/dashboard/page.tsx` | `components/feature/dashboard/DashboardWorkflowLauncherLight.tsx`, `components/feature/dashboard/DashboardWorkflowHub.tsx` |
| 2. Workflow detail | `app/(app)/workflows/[workflowId]/page.tsx` | `lib/workflows.ts` (WORKFLOWS + STEP_ICONS data) |
| 3. Listing Copilot | `app/(app)/listing-copilot/page.tsx` | `components/feature/ListingCopilot/ListingCard.tsx`, `components/feature/ListingCopilot/ListingsPanel.tsx`, `components/feature/ListingCopilot/AddListingDialog.tsx` |
| 4. Valuations (hero: "New Valuation" button) | `app/(app)/valuations/page.tsx` | `components/feature/Valuation/ValuationCard.tsx`, `components/feature/Valuation/ValuationsPanel.tsx`, `components/feature/Valuation/CreateValuationDialog.tsx` |
| 5a. Net Sheet card | `app/(app)/sellers/page.tsx` | `components/feature/sellers/PropertyCard.tsx` (NetSheetCard variant), `components/feature/sellers/NetSheetsPanel.tsx` |
| 5b. Pre-Qual | `app/(app)/prequal/page.tsx`, `app/(app)/prequal/[sessionId]/` | `components/feature/prequal/PreQualFlow.tsx` |
| 6. Saved Searches | `app/(app)/saved-searches/page.tsx` | (page-local components) |
| Shared card patterns | — | `components/common/PageHeader.tsx`, `components/common/EmptyState.tsx`, `components/common/FilterChips.tsx` |
| Dashboard KPI strip (context) | `app/(app)/dashboard/page.tsx` | `components/feature/dashboard/KpiCard.tsx`, `components/feature/dashboard/KpiGrid.tsx`, `components/feature/dashboard/CapturedSection.tsx`, `components/feature/dashboard/MiniStatCard.tsx` |

**How to use this:** for each scene below, open the listed file(s), copy the actual JSX/Tailwind classes into your storyboard frames. If a class differs between the code and my description in Section 3, **trust the code**.

---

## 1. Product context

**Link.ai** is a real estate agent platform: agents capture leads with branded links (pre-quals, home valuations, net sheets, buyer searches), manage listings with an AI copilot, and run a dashboard of KPIs + workflows.

**Value prop in one line:** "Every tool a modern real estate agent needs — capture, qualify, and close from a single dashboard."

---

## 2. Design system snapshot

The video must feel native to the actual product. Do not invent visuals — match these tokens exactly.

### Colors
- **Teal (primary):** `#5AE9D7` (teal-500), light `#9EF8EC` (teal-300), dark `#1CCFB8` (teal-600)
- **Neutrals:** white cards, `border-gray-200`, text hierarchy `text-gray-900 → text-gray-400 → text-gray-300`
- **Dark hero (dashboard launcher):** background `#05070a`, floating teal orb glow (radial `rgba(90, 233, 215, 0.4)`), photo backdrop with dark scrim `linear-gradient(180deg, rgba(5,7,10,0.55) 0%, rgba(5,7,10,0.85) 100%)`
- **Status colors:** active/success teal-500, pending brand-orange `#FF7958`, blue `#045FF0`, red `#FF5858`
- **Rule:** exactly one teal-500 element per page (the primary CTA). Everything else is gray-scale.

### Typography
- Titles: `text-2xl font-semibold` or `text-3xl font-bold` (KPI numbers)
- Body: `text-sm` / `text-xs` for supporting labels, `uppercase tracking-wide` for eyebrows
- Font: default system stack + Geist mono for numeric accents

### Card language ("image cards")
This is the visual signature the tour should hammer:
```
┌────────────────────────────────┐
│ ██████ hero photo/map (h-44) ██│   ← group-hover:scale-105 transition
│                                │
│ ● Status pill (top-3 left-3)   │   ← rounded-full uppercase text-[10px]
│                                │
│                          ⋯     │   ← more menu (top-2.5 right-2.5)
│ $650k              ← overlay   │   ← bottom-left overlay w/ gradient
│ estimated value                │
├────────────────────────────────┤
│ (avatar) Client Name           │
│ 123 Main St, Austin TX         │
│ Monthly updates active         │
└────────────────────────────────┘
   rounded-2xl border-gray-200
```

### Motion vocabulary (already in the app)
- Cards: `hover:shadow-md` + `group-hover:scale-105` on hero image (500ms ease)
- Page transitions: `motion.div` opacity + y-translate (from `motion/react`)
- Teal orb on dashboard: slow radial pulse, 12s ease
- Progress bars fill left-to-right on mount (500ms)

The video should echo these — no zoom bursts, no whip pans. Camera moves should feel like a designer walking through Figma.

---

## 3. Shot list (30-second target — trim/extend as needed)

Total budget: 6 scenes @ ~5s each, plus 1s intro title and 2s outro. All transitions are soft crossfades unless noted.

### Scene 0 — Title card (0:00 → 0:02)
- **Screen:** Solid teal `#5AE9D7` background OR the dashboard dark hero background (photo + teal orb) with a small white lockup lower-left
- **Text:** "link.ai — every tool, one dashboard" (or brand tagline)
- **Motion:** wordmark fades in, held for 1s, then dissolve to Scene 1

### Scene 1 — Dashboard workflow launcher (0:02 → 0:07)
- **Route:** `/dashboard` (top of page)
- **Look:** Full-bleed dark background (`#05070a`) with a blurred photo of a modern home + a glowing teal orb top-right. Foreground: "WORKFLOW" eyebrow, then large white heading "Welcome back, [first name]", then a row of glass-morphism workflow cards (Seller Lead, Buyer Lead, New Listing, Get Leads) with a thin teal progress bar.
- **Camera:** Start wide (whole hero visible), then slow push in 20% toward the 4 workflow cards
- **Zoom target:** the "Seller Lead" card — highlight the teal progress bar at the bottom of that card
- **Voiceover / caption (optional):** "Start with a workflow"

### Scene 2 — Workflow detail view (0:07 → 0:12)
- **Route:** `/workflows/seller-lead` (dark glass detail page)
- **Look:** Same dark background + orb. Centered content column, max-w-xl. Progress bar animates from 0% → 33% as we enter. Below: a glass card showing the active step ("Listing Presentation") with a teal step icon in a rounded square, title "Step 1 of 3", and a teal primary CTA button "Go to Listing Presentations"
- **Camera:** Push in on the active step card, then a specific zoom (150%) on the teal CTA button
- **Emphasis:** the button gets a subtle 1.02× scale pulse on the zoom-in
- **Caption:** "Guided steps, one click at a time"

### Scene 3 — Listing Copilot (0:12 → 0:17)
- **Route:** `/listing-copilot`
- **Look:** Bright white background. PageHeader "Listing Copilot" + subtitle at top. Below: a 3-column grid of listing image cards (rounded-2xl, h-44 hero of a property photo — use a real-looking suburban home photo). Each card shows: status pill (teal "Active"), more menu ⋯, price `$789,000`, specs `4 bd · 3 ba · 2,400 sqft`, address, an "FAQ" chip badge on the right, and a teal "Website" pill at the bottom.
- **Camera:** Start showing 3 cards, then zoom into the middle card, then specifically frame the small teal "Website" pill button
- **Emphasis:** cursor hovers the "Create listing link" item in the ⋯ dropdown briefly (dropdown opens then closes)
- **Caption:** "AI copilot for every listing"

### Scene 4 — Valuations page (0:17 → 0:22)
- **Route:** `/valuations`
- **Look:** PageHeader "Valuations" with a prominent teal "New Valuation" button in the top-right. Below: grid of ValuationCard image cards — each with a Google Static Map as the hero (teal marker pin), a "$650,000 · estimated value" overlay in the bottom-left corner of the hero (white text on dark gradient), a teal "ACTIVE" pill top-left, avatar + client name + address, and "Monthly updates active" subtitle.
- **Camera:** Wide shot of the page, then a fast (but smooth) zoom to the "New Valuation" button in the header — this is a hero moment
- **Emphasis:** the "New Valuation" button briefly ghost-clicks (scale down 0.97 → back to 1.0) but does NOT open a modal — the tour keeps flowing
- **Caption:** "Monthly value updates that keep you top-of-mind"

### Scene 5 — Net Sheet + Pre-Qual (0:22 → 0:27)
- Split into two ~2.5s micro-shots inside one scene, connected by a horizontal wipe/slide.
- **5a — Net Sheet card focus (0:22 → 0:24.5):** Zoom in on a single net-sheet card (image card style with the property map hero). The `$487,320` net-proceeds number overlay in the bottom-left of the hero is the star — camera lands on that number. Below: "net to seller" caption in `text-white/60 text-[10px]`.
- **5b — Pre-Qual link (0:24.5 → 0:27):** Slide horizontally to the pre-qual builder OR a lead detail view showing a completed pre-qual result — with the "Buying power: $625,000" or similar large teal number, a "Qualified" status pill (teal-50 bg, teal-600 text), and a lock icon + "FCRA-compliant" microcopy for trust.
- **Caption:** "Instant clarity for sellers and buyers"

### Scene 6 — Saved Searches (0:27 → 0:32)
- **Route:** `/saved-searches`
- **Look:** A list or grid of subscriber cards — each with a small map thumbnail or property tile row, subscriber name, criteria pill (e.g., "3 bd · $500k–$700k · Austin TX"), an "Active" toggle in teal.
- **Camera:** Slow left-to-right pan across the list showing 4–5 subscribers, then a slight zoom on one row highlighting the teal active toggle
- **Caption:** "Never let a lead go cold"

### Scene 7 — Outro (0:32 → 0:35)
- **Screen:** Fade to the dashboard hero background (dark + teal orb) with white centered lockup
- **Text:**
  - Line 1 (large, `text-3xl font-bold`): "link.ai"
  - Line 2 (small, `text-white/70`): "Capture. Qualify. Close."
  - Line 3 (tiny, `text-teal-300 text-xs uppercase tracking-widest`): a URL or CTA like "book a demo"
- **Motion:** wordmark scales in from 0.95 → 1.0 with 200ms ease, teal orb continues pulsing in the background

---

## 4. Camera / motion rules

- **Zooms:** slow, ease-out, 800–1200ms. No jump cuts within a scene.
- **Cursor:** show a subtle white cursor with a soft glow — moves in curved paths, not straight lines. Slight easing before landing on a target.
- **Highlights:** when zooming to a button, add a 1px teal glow ring around it (`box-shadow: 0 0 0 3px rgba(90,233,215,0.4)`) for the moment it's focal.
- **Transitions between routes:** 300ms cross-dissolve, no swooshes.
- **Never:** shake, glitch effects, particle overlays, "swipe reveals" — this brand is calm.

## 5. Audio (optional)

- Ambient minimal electronic pad, no drums, no vocal hooks. Something like Nils Frahm or minimal Tycho.
- Subtle UI ticks on button emphasis (a single soft "click" when a CTA is hero'd — max 1 per scene)
- No voiceover in the primary cut; captions carry the message. Provide a VO-friendly script alt if wanted.

## 6. Screens to source screenshots from

To avoid inventing UI, capture real screenshots from a running dev server:

```bash
bun dev
```

Then capture at 2880×1800 (Retina):
1. `/dashboard` — full hero at top-of-page (Scene 1)
2. `/workflows/seller-lead` — the dark detail page mid-progress (Scene 2)
3. `/listing-copilot` — after adding 3 listings (Scene 3)
4. `/valuations` — with 3–6 active valuations, "New Valuation" button clearly visible (Scene 4)
5. `/sellers` filtered to Net Sheets tab — one card focused (Scene 5a)
6. `/prequal` or `/leads/[id]` for the pre-qual result view (Scene 5b)
7. `/saved-searches` — with 4+ subscribers listed (Scene 6)

Use anonymized demo data (fake names, generic addresses) — don't ship real client data in marketing.

## 7. Deliverables the designer agent should produce

Ideal output from a designer/video agent:
1. A **Figma or After Effects storyboard** with each scene as a frame + notes on the camera move
2. **A short animated mockup** (even a rough Lottie/Rive prototype) for the two hero moments (workflow launcher + "New Valuation" button zoom)
3. **A finalized 30s MP4 export** at 1920×1080, 30fps, ≤ 10MB

## 8. What NOT to do

- Don't fabricate UI that isn't in the actual product (no "AI chat bubbles" floating, no fictional analytics overlays)
- Don't use teal on secondary elements — one teal CTA per shot, everything else neutral
- Don't voice-over unless the client asks — captions are cleaner for a 30s product tour
- Don't cut faster than one scene per 3.5 seconds — this is a considered product, not a hype reel
