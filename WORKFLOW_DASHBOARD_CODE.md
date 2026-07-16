# Workflow Cards & Dashboard — Source Code Bundle

**For:** a design agent replicating the dashboard hero, workflow carousel, workflow mini-cards, workflow detail page, and KPI/captured strip.

**Stack this code assumes:**
- **React 19** + **Next.js 16** (App Router, `"use client"` directives)
- **Tailwind CSS 4** — no `tailwind.config.js`, tokens live in `app/globals.css` as CSS variables (see Section 1)
- **`motion/react`** (not `framer-motion`) — API is identical
- **`lucide-react`** for icons
- **shadcn/ui** `Button` component (Radix Nova preset)
- **Convex** (`useQuery` from `convex/react`) — only used to hydrate live data; the visual structure works with stub data

**What you can safely ignore or stub:**
- Any `useQuery(api.xxx.yyy)` call — replace with static mock data
- Imports from `@/convex/*`, `@/components/links/*` (dialogs) — not needed for visual replication
- `import { cn } from "@/lib/utils"` — this is just `clsx` + `tailwind-merge`; replace with `clsx` or template strings

**External deps you'll need on npm:** `motion`, `lucide-react`, `clsx`, `class-variance-authority`, `@radix-ui/react-slot` (for Button)

---

## 1. Design tokens (from `app/globals.css`)

These are the CSS variables the entire UI reads from. Recreate them in your mockup exactly.

```css
html, body {
  background-color: #FAFCFF;
}

@theme {
  /* ── Brand teal palette ─────────────────────── */
  /* main: #5AE9D7 (teal-300), primary CTA: teal-500 */
  --color-teal-50:  #f0fefb;
  --color-teal-100: #d5faf5;
  --color-teal-200: #b5f5ee;
  --color-teal-300: #9EF8EC;
  --color-teal-400: #72eede;
  --color-teal-500: #1CCFB8;   /* primary CTA */
  --color-teal-600: #15a695;
  --color-teal-700: #15a695;

  /* ── Brand neutrals (blue-tinted gray scale) ── */
  --color-gray-50:  #FAFCFF;   /* page bg */
  --color-gray-100: #E9EEF7;   /* card borders, hairlines */
  --color-gray-200: #DDE5F3;
  --color-gray-300: #CCD0E0;
  --color-gray-400: #9197AE;   /* muted text */
  --color-gray-500: #636B87;
  --color-gray-600: #4E546A;
  --color-gray-700: #333B57;
  --color-gray-800: #202537;
  --color-gray-900: #131720;   /* primary text */

  /* ── Brand accents ──────────────────────────── */
  --color-brand-orange:       #FF7958;
  --color-brand-orange-light: #FFEFEB;
  --color-brand-blue:         #045FF0;
  --color-brand-blue-light:   #EBF3FF;
  --color-brand-red:          #FF5858;
  --color-brand-red-light:    #FFEBEB;
  --color-brand-purple:       #9B81FF;   /* AI accents */
  --color-brand-purple-light: #F0ECFF;

  /* ── Card shadows ───────────────────────────── */
  --shadow-card:       0 2px 16px -2px rgba(0, 0, 0, 0.05);
  --shadow-card-hover: 0 6px 24px -4px rgba(0, 0, 0, 0.08);
}
```

## 2. Brand rules (from `AGENTS.md`)

**One `teal-500` element per page.** That's the primary CTA (e.g. "Start workflow", the big teal button on the workflow detail page). Everything else uses gray-scale or lighter teal (`teal-50`, `teal-100`, `teal-300`).

**Anchors:** main `#5AE9D7` (teal-300), light `#9EF8EC` (teal-300), dark `#1CCFB8` (teal-500).

Never use blue, purple, orange, or red for primary actions — only for destructive/status states.

---

## 3. Workflow data model (`lib/workflows.ts`)

The data that drives every workflow card. Icons come from `lucide-react`.

```tsx
import type { LucideIcon } from "lucide-react";
import {
  Users, Home, Building2, Globe, Search,
  SearchCheck, ShieldCheck, Route, FileText, Presentation,
  Megaphone, Crosshair, Target, Link2, HelpCircle,
} from "lucide-react";

export type WorkflowStep = {
  id: string;
  title: string;
  description: string;
  cta: string;
} & (
  | { href: string; linkType?: never }
  | { linkType: string; href?: never }
);

export type Workflow = {
  id: string;
  title: string;
  blurb: string;
  icon: LucideIcon;
  steps: WorkflowStep[];
};

export const WORKFLOWS: Workflow[] = [
  {
    id: "seller-lead",
    title: "Seller Lead",
    blurb: "Convert a seller inquiry into a client with a presentation, valuation drip, and net sheet.",
    icon: Home,
    steps: [
      { id: "listing-presentation", title: "Listing Presentation",  description: "Present your marketing plan and track record in a polished, shareable format that wins the listing appointment.", cta: "Go to Listing Presentations", href: "/listing-presentations" },
      { id: "valuation-drip",       title: "Create Valuation Drip", description: "Enroll the seller in monthly home value updates — keeps you top-of-mind and builds trust long before they're ready to list.", cta: "Go to Valuations", href: "/valuations" },
      { id: "net-sheet",            title: "Create a Net Sheet",    description: "Give your seller a clear picture of what they'll walk away with — builds trust and removes price objections.", cta: "Create Net Sheet", linkType: "net_sheet" },
    ],
  },
  {
    id: "buyer-lead",
    title: "Buyer Lead",
    blurb: "Qualify, match, and guide your buyer from first contact to offer-ready.",
    icon: Users,
    steps: [
      { id: "saved-search",     title: "Set Up Saved Search",     description: "Pre-load their criteria so they get notified the moment a matching listing hits the market — keeps you top-of-mind.", cta: "Go to Saved Searches", href: "/saved-searches" },
      { id: "pre-qual",         title: "Send a Pre-Qual Link",    description: "Send it to your buyer — they answer a few questions and you instantly get their buying power and affordability range.", cta: "Create Pre-Qual Link", linkType: "advanced_pre_qual" },
      { id: "home-search-link", title: "Share a Home Search Link", description: "Create a pre-filtered search link so clients start browsing the right homes immediately.", cta: "Create Search Link", linkType: "buyer_search" },
      { id: "buyer-tour",       title: "Set Up Buyer Tour",       description: "Create a branded, interactive tour guide they can follow on showing day — ratings and notes come back to you in real time.", cta: "Go to Buyer Tours", href: "/buyer-tours" },
    ],
  },
  {
    id: "new-listing",
    title: "New Listing",
    blurb: "Set up your listing, build an FAQ page, and promote it across every channel.",
    icon: Building2,
    steps: [
      { id: "listing-copilot", title: "Create Listing Copilot", description: "Add the listing to your system and set up an AI-powered copilot that answers buyer questions around the clock.", cta: "Go to Listing Copilot", href: "/listing-copilot" },
      { id: "spw",             title: "Create SPW",             description: "Launch a dedicated property website with photos, details, and your branding — the hub everything links back to.", cta: "Create Property Website", linkType: "single_property" },
      { id: "add-faqs",        title: "Add FAQs",               description: "Add the most common buyer questions so your copilot can answer them instantly — fewer calls, more conversions.", cta: "Go to Listing Copilot", href: "/listing-copilot" },
      { id: "promote-listing", title: "Promote Listing",        description: "Push the listing across social, Meta ads, Google ads, and circle prospecting — all from one place.", cta: "Go to Promote", href: "/promote" },
    ],
  },
  {
    id: "get-leads",
    title: "Get Leads",
    blurb: "Fill your pipeline with targeted campaigns — funnels, prospecting, and paid ads.",
    icon: Target,
    steps: [
      { id: "build-funnel",        title: "Build a Funnel",       description: "Create a capture link — home valuation, buyer search, pre-qual, or generic — and start driving traffic to it.", cta: "Go to Links", href: "/links" },
      { id: "circle-prospecting",  title: "Circle Prospecting",   description: "Target homeowners around a listing with mailers and digital ads — the fastest way to generate seller leads in a neighborhood.", cta: "Go to Circle Prospecting", href: "/circle-prospecting" },
      { id: "meta-ads",            title: "Meta Ads",             description: "Run targeted Facebook and Instagram campaigns to drive buyers and sellers to your capture links.", cta: "Go to Meta Ads", href: "/meta-ads" },
      { id: "google-ads",          title: "Google Ads",           description: "Capture high-intent buyers and sellers searching for homes in your market with Google search campaigns.", cta: "Go to Google Ads", href: "/google-ads" },
    ],
  },
];

// Step icons — used inside the workflow detail page's active-step card
export const STEP_ICONS: Record<string, React.ElementType> = {
  "listing-presentation": Presentation,
  "valuation-drip": Home,
  "net-sheet": FileText,
  "saved-search": SearchCheck,
  "pre-qual": ShieldCheck,
  "home-search-link": Search,
  "buyer-tour": Route,
  "listing-copilot": Building2,
  "spw": Globe,
  "add-faqs": HelpCircle,
  "promote-listing": Megaphone,
  "build-funnel": Link2,
  "circle-prospecting": Crosshair,
  "meta-ads": Target,
  "google-ads": Search,
};
```

---

## 4. Dashboard hero — workflow carousel (`DashboardWorkflowLauncherLight.tsx`)

Full-width section at the top of the dashboard. Light background (`bg-gray-50`), decorative teal glow in top-left, a "Workflows / Dashboard" pill toggle, and a 3D-ish carousel of tall cards (380×440) with left/right previews at 82% scale.

**Key visual elements:**
- Background: `bg-gray-50` with a `600×600` radial teal glow (`rgba(90,233,215,0.12)`) blurred 40px in top-left
- Toggle pill: `bg-gray-200 rounded-full p-1`, active tab `bg-white shadow-sm`
- Cards: `rounded-2xl` `w-[380px]` `h-[440px]` `p-8`. Active card `bg-white shadow-lg border border-gray-200`. Off-focus cards `bg-gray-50/80 border-gray-100 shadow-sm scale-82`.
- Dot indicators below: active dot `w-5 h-1.5 bg-teal-500 rounded-full`, inactive `w-1.5 h-1.5 bg-gray-300`.
- Icon square inside card: `w-12 h-12 rounded-xl bg-teal-50` with `text-teal-600` icon
- Primary CTA: `bg-teal-500 hover:bg-teal-500/90 text-white w-full` with `<ArrowRight size={15} />` after label
- Progress badge inside card (when >0 done): `text-xs text-teal-600 bg-teal-50 border border-teal-100 rounded-full px-3 py-1`
- Card enter/switch animation: `motion.div` with `type: "tween"`, ease `[0.2, 0.8, 0.2, 1]`, duration `0.55s`

```tsx
"use client";

import { useRouter } from "next/navigation";
import { motion } from "motion/react";
import { ArrowRight, CheckCircle2, Users, Activity } from "lucide-react";
import { WORKFLOWS, getProgress } from "@/lib/workflows";
import { useState, useEffect, useRef, useCallback } from "react";
import { useQuery } from "convex/react";
import { api } from "@/convex/_generated/api";
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";

type WorkflowCard = {
  kind: "workflow";
  id: string;
  title: string;
  blurb: string;
  icon: React.ComponentType<{ size?: number; className?: string }>;
  steps: number;
  cta: string;
  href: string;
  done: number;
};

type DataCard = {
  kind: "data";
  id: string;
  tag: string;
  title: string;
  body: string;
  icon: React.ComponentType<{ size?: number; className?: string }>;
  cta: string;
  href: string;
};

type Card = WorkflowCard | DataCard;

const CARD_TRANSITION = {
  type: "tween" as const,
  ease: [0.2, 0.8, 0.2, 1] as [number, number, number, number],
  duration: 0.55,
};

export function DashboardWorkflowLauncherLight({
  view,
  onViewChange,
}: {
  view: "plays" | "dashboard";
  onViewChange: (v: "plays" | "dashboard") => void;
}) {
  const router = useRouter();
  const [index, setIndex] = useState(0);
  const [progress, setProgress] = useState<Record<string, number>>({});
  const kpis = useQuery(api.dashboard.getKpis, {});
  const activity = useQuery(api.dashboard.getRecentActivity, { limit: 1 });
  const dragStart = useRef(0);

  useEffect(() => {
    const p: Record<string, number> = {};
    WORKFLOWS.forEach((wf) => {
      p[wf.id] = getProgress(wf.id).length;
    });
    setProgress(p);
  }, []);

  const cards: Card[] = [
    ...WORKFLOWS.map((wf) => ({
      kind: "workflow" as const,
      id: wf.id,
      title: wf.title,
      blurb: wf.blurb,
      icon: wf.icon,
      steps: wf.steps.length,
      cta: "Start workflow",
      href: `/workflows/${wf.id}`,
      done: progress[wf.id] ?? 0,
    })),
    // Data cards (contextual "action needed" cards) — optional, stub for mockup
    ...(kpis && kpis.activeLeads > 0
      ? [{
          kind: "data" as const,
          id: "leads-action",
          tag: "Action needed",
          title: `${kpis.activeLeads} active lead${kpis.activeLeads !== 1 ? "s" : ""}`,
          body: "You have leads in your pipeline waiting for follow-up. Keep them moving.",
          icon: Users,
          cta: "View leads",
          href: "/leads",
        }]
      : []),
    ...(activity && activity.length > 0
      ? [{
          kind: "data" as const,
          id: "activity-action",
          tag: "Recent activity",
          title: activity[0].leadName ?? "New activity",
          body: `${activity[0].eventType.replace(/_/g, " ")} — take action while the lead is warm.`,
          icon: Activity,
          cta: "View lead",
          href: "/leads",
        }]
      : []),
  ];

  const total = cards.length;
  const go = useCallback(
    (newIndex: number) => setIndex(((newIndex % total) + total) % total),
    [total],
  );

  return (
    <section
      className="relative w-full flex flex-col items-center justify-center bg-gray-50 overflow-hidden"
      style={{
        minHeight: view === "plays" ? "calc(100vh - 2rem)" : "auto",
        paddingTop: view === "dashboard" ? "36px" : "0",
      }}
    >
      {/* Subtle teal glow decoration */}
      <div
        aria-hidden="true"
        className="pointer-events-none absolute top-0 left-0 select-none"
        style={{
          width: 600,
          height: 600,
          background: "radial-gradient(circle at 30% 30%, rgba(90,233,215,0.12) 0%, transparent 65%)",
          filter: "blur(40px)",
        }}
      />

      {/* Eyebrow label */}
      <p className="relative z-10 text-[11px] font-semibold uppercase tracking-[0.2em] text-gray-400 mb-4">
        Your next steps
      </p>

      {/* Workflows / Dashboard view toggle */}
      <div className="relative z-10 flex bg-gray-200 rounded-full p-1 mb-8">
        {(["plays", "dashboard"] as const).map((v) => (
          <button
            key={v}
            onClick={() => onViewChange(v)}
            className={cn(
              "px-4 py-1.5 rounded-full text-xs font-semibold transition-all",
              view === v
                ? "bg-white text-gray-900 shadow-sm"
                : "text-gray-500 hover:text-gray-700",
            )}
          >
            {v === "plays" ? "Workflows" : "Dashboard"}
          </button>
        ))}
      </div>

      {view === "plays" && (
        <>
          {/* Carousel stage */}
          <div
            className="relative z-10 w-full"
            style={{ height: 500 }}
            onPointerDown={(e) => { dragStart.current = e.clientX; }}
            onPointerUp={(e) => {
              const dx = e.clientX - dragStart.current;
              if (Math.abs(dx) > 40) {
                if (dx < 0) go(index + 1);
                else go(index - 1);
              }
            }}
          >
            {cards.map((c, i) => {
              let offset = i - index;
              if (offset > Math.floor(total / 2)) offset -= total;
              if (offset < -Math.ceil(total / 2)) offset += total;

              const isActive = offset === 0;
              const isVisible = Math.abs(offset) === 1;

              const animValues = {
                x: offset * 360,
                y: Math.abs(offset) * 16,
                scale: isActive ? 1 : isVisible ? 0.82 : 0.5,
                opacity: isActive || isVisible ? 1 : 0,
              };

              return (
                <motion.div
                  key={c.id}
                  initial={animValues}
                  animate={animValues}
                  transition={CARD_TRANSITION}
                  className={cn(
                    "absolute left-1/2 -ml-[190px] w-[380px]",
                    isActive ? "z-10" : "z-[1]",
                    !isActive && !isVisible && "pointer-events-none",
                    !isActive && isVisible && "cursor-pointer",
                  )}
                  onClick={!isActive && isVisible ? () => go(i) : undefined}
                >
                  <div
                    className={cn(
                      "rounded-2xl h-[440px] flex flex-col p-8 select-none",
                      isActive
                        ? "bg-white shadow-lg border border-gray-200"
                        : "bg-gray-50/80 border border-gray-100 shadow-sm",
                    )}
                  >
                    {c.kind === "workflow" ? (
                      <WorkflowCardContent card={c} isActive={isActive} onAction={() => router.push(c.href)} />
                    ) : (
                      <DataCardContent card={c} isActive={isActive} onAction={() => router.push(c.href)} />
                    )}
                  </div>
                </motion.div>
              );
            })}
          </div>

          {/* Step count */}
          <p className="relative z-10 mt-6 text-xs text-gray-400">
            {index + 1} of {total}
          </p>

          {/* Dot indicators */}
          <div className="relative z-10 flex gap-2 mt-3 mb-10">
            {cards.map((c, i) => (
              <button
                key={c.id}
                onClick={() => go(i)}
                className={`rounded-full transition-all duration-300 ${
                  i === index
                    ? "w-5 h-1.5 bg-teal-500"
                    : "w-1.5 h-1.5 bg-gray-300 hover:bg-gray-400"
                }`}
              />
            ))}
          </div>

          {/* Scroll hint */}
          <div className="absolute bottom-6 left-1/2 -translate-x-1/2 z-10 flex flex-col items-center gap-1 opacity-40">
            <div className="w-px h-6 bg-gray-400 rounded-full" />
            <p className="text-[10px] font-medium uppercase tracking-widest text-gray-400">
              scroll
            </p>
          </div>
        </>
      )}
    </section>
  );
}

function WorkflowCardContent({ card, isActive, onAction }: {
  card: WorkflowCard; isActive: boolean; onAction: () => void;
}) {
  const Icon = card.icon;
  return (
    <div className="flex flex-col h-full">
      <div className="w-12 h-12 rounded-xl bg-teal-50 flex items-center justify-center mb-5">
        <Icon size={22} className="text-teal-600" />
      </div>
      <p className="text-xl font-bold text-gray-900 leading-tight mb-2">{card.title}</p>
      <p className="text-sm text-gray-500 leading-relaxed">{card.blurb}</p>
      <div className="flex-1" />
      {card.done > 0 && (
        <div className="flex items-center gap-2 mb-4">
          {card.done === card.steps && (
            <CheckCircle2 size={13} className="text-teal-500 shrink-0" />
          )}
          <span className="text-xs font-medium text-teal-600 bg-teal-50 border border-teal-100 rounded-full px-3 py-1">
            {card.done}/{card.steps} steps done
          </span>
        </div>
      )}
      <Button
        onClick={onAction}
        className={
          isActive
            ? "bg-teal-500 hover:bg-teal-500/90 text-white gap-2 w-full"
            : "bg-gray-100 hover:bg-gray-200 text-gray-400 gap-2 w-full"
        }
      >
        {card.cta}
        <ArrowRight size={15} />
      </Button>
    </div>
  );
}

function DataCardContent({ card, isActive, onAction }: {
  card: DataCard; isActive: boolean; onAction: () => void;
}) {
  const Icon = card.icon;
  return (
    <div className="flex flex-col h-full">
      <p className="text-[10px] font-semibold uppercase tracking-widest text-gray-400 mb-4">
        {card.tag}
      </p>
      <div className="w-12 h-12 rounded-xl bg-gray-100 flex items-center justify-center mb-5">
        <Icon size={22} className="text-gray-500" />
      </div>
      <p className="text-xl font-bold text-gray-900 leading-tight mb-2">{card.title}</p>
      <p className="text-sm text-gray-500 leading-relaxed">{card.body}</p>
      <div className="flex-1" />
      <Button
        onClick={onAction}
        className={
          isActive
            ? "bg-teal-500 hover:bg-teal-500/90 text-white gap-2 w-full"
            : "bg-gray-100 hover:bg-gray-200 text-gray-400 gap-2 w-full"
        }
      >
        {card.cta}
        <ArrowRight size={15} />
      </Button>
    </div>
  );
}
```

---

## 5. Dashboard page composition (`app/(app)/dashboard/page.tsx`)

The main dashboard layout. Two views (`plays` = the carousel, `dashboard` = the metrics view). The `dashboard` view stacks: header greeting → 2×2 workflow mini-cards → KPI grid (5 cards) → captured + activity row (4/5 + 1/5 split).

```tsx
"use client";

import { useState, useEffect } from "react";
import { useRouter } from "next/navigation";
import { useQuery } from "convex/react";
import { api } from "@/convex/_generated/api";
import { ArrowRight, CheckCircle2 } from "lucide-react";
import { DashboardHeader } from "@/components/feature/dashboard/DashboardHeader";
import { DashboardGetStarted } from "@/components/feature/dashboard/DashboardGetStarted";
import { DashboardWorkflowLauncherLight } from "@/components/feature/dashboard/DashboardWorkflowLauncherLight";
import { KpiGrid } from "@/components/feature/dashboard/KpiGrid";
import { CapturedSection } from "@/components/feature/dashboard/CapturedSection";
import { RecentActivity } from "@/components/feature/dashboard/RecentActivity";
import { SubscribersSection } from "@/components/feature/dashboard/SubscribersSection";
import { WORKFLOWS, getProgress } from "@/lib/workflows";

export default function DashboardPage() {
  const agent = useQuery(api.agents.getCurrentAgent);
  const firstName = agent?.name?.split(" ")[0] ?? "there";
  const [view, setView] = useState<"plays" | "dashboard">("dashboard");

  return (
    <div className="flex flex-col">
      <DashboardGetStarted />
      <DashboardWorkflowLauncherLight view={view} onViewChange={setView} />

      {view === "dashboard" && (
        <div className="p-4 md:p-6">
          <DashboardHeader agentName={firstName} />
          <WorkflowMiniCards />
          <KpiGrid />
          <div className="grid grid-cols-1 lg:grid-cols-5 gap-3 mt-3">
            <div className="lg:col-span-4">
              <CapturedSection />
            </div>
            <RecentActivity />
          </div>
          <SubscribersSection />
        </div>
      )}
    </div>
  );
}

// The 2×2 grid of small workflow cards under the greeting
function WorkflowMiniCards() {
  const router = useRouter();
  const [progress, setProgress] = useState<Record<string, number>>({});

  useEffect(() => {
    const p: Record<string, number> = {};
    WORKFLOWS.forEach((wf) => { p[wf.id] = getProgress(wf.id).length; });
    setProgress(p);
  }, []);

  return (
    <div className="mb-6">
      <p className="text-[10px] font-semibold uppercase tracking-[0.18em] text-gray-400 mb-3">
        Workflows
      </p>
      <div className="grid grid-cols-1 sm:grid-cols-2 gap-3">
        {WORKFLOWS.map((wf) => {
          const done = progress[wf.id] ?? 0;
          const total = wf.steps.length;
          const Icon = wf.icon;
          return (
            <button
              key={wf.id}
              onClick={() => router.push(`/workflows/${wf.id}`)}
              className="bg-white border border-gray-200 rounded-xl p-4 text-left hover:shadow-md hover:border-teal-200 transition-all group"
            >
              <div className="flex items-start justify-between mb-3">
                <div className="w-9 h-9 rounded-lg bg-teal-50 flex items-center justify-center shrink-0">
                  <Icon size={16} className="text-teal-600" />
                </div>
                {done === total && total > 0 && (
                  <CheckCircle2 size={14} className="text-teal-400 shrink-0 mt-0.5" />
                )}
              </div>
              <p className="text-sm font-semibold text-gray-900 leading-tight mb-1">
                {wf.title}
              </p>
              <p className="text-xs text-gray-400 leading-snug line-clamp-2 mb-3">
                {wf.blurb}
              </p>
              <div className="flex items-center justify-between">
                <div className="flex items-center gap-1.5">
                  <div className="h-1 w-16 rounded-full bg-gray-100 overflow-hidden">
                    <div
                      className="h-full rounded-full bg-teal-400"
                      style={{ width: total > 0 ? `${(done / total) * 100}%` : "0%" }}
                    />
                  </div>
                  <span className="text-[10px] text-gray-400 tabular-nums">
                    {done}/{total}
                  </span>
                </div>
                <ArrowRight size={12} className="text-gray-300 group-hover:text-teal-500 transition-colors" />
              </div>
            </button>
          );
        })}
      </div>
    </div>
  );
}
```

---

## 6. Workflow detail page (`app/(app)/workflows/[workflowId]/page.tsx`)

The page you land on when you click a workflow card. Shows title + blurb, an animated teal progress bar (fills on mount), status pills for each step (done ✓ / active ○ / locked 🔒), a large glossy active-step card with a step icon and primary CTA, and a stack of dimmed completed-step rows below.

**Key visual elements:**
- Container: `bg-gray-50` with `600×600` radial teal glow (`rgba(90,233,215,0.1)`) in top-left, 40px blur
- Progress bar: `h-1.5 rounded-full` gray track, `bg-teal-500` fill animated with `motion.div` (0.5s easeOut)
- Step pills (`flex flex-wrap gap-2`):
  - Active: `bg-[rgb(240,253,250)] border-[rgb(153,246,228)] text-teal-700` + `Circle` icon
  - Done: `bg-gray-50 border-gray-100 text-gray-400` + `CheckCircle2` icon
  - Locked: `bg-gray-50 border-gray-100 text-gray-300` + `Lock` icon
- Active step card: `rounded-2xl p-8 bg-white border-gray-200 shadow-sm`, centered `max-w-xl mx-auto`
- Step icon square: `w-14 h-14 rounded-2xl bg-teal-50` with `text-teal-600` icon (26px)
- Primary CTA: `bg-teal-500 hover:bg-teal-500/90 text-white w-full h-12 text-base gap-2` with `ArrowRight` after label
- Secondary link: "Skip this step" — `text-xs text-gray-400 hover:text-gray-600` centered under CTA
- Completed-step rows: `rounded-xl px-4 py-3 bg-gray-50 border-gray-100 flex items-center gap-3`, title `line-through text-gray-400`
- Active step card enter/exit: `motion.div` with `initial={{ opacity: 0, y: 20 }}` `animate={{ opacity: 1, y: 0 }}` `exit={{ opacity: 0, y: -12 }}`, `duration: 0.4`, ease `[0.2, 0.8, 0.2, 1]`

```tsx
"use client";

import { useState, useEffect } from "react";
import { useParams, useRouter } from "next/navigation";
import { ArrowRight, CheckCircle2, Circle, Lock, RotateCcw } from "lucide-react";
import { motion, AnimatePresence } from "motion/react";
import { Button } from "@/components/ui/button";
import {
  getWorkflow, getProgress, saveProgress, resetProgress,
  STEP_ICONS, type WorkflowStep,
} from "@/lib/workflows";

export default function WorkflowPage() {
  const params = useParams();
  const router = useRouter();
  const workflowId = params.workflowId as string;
  const workflow = getWorkflow(workflowId);

  const [{ completedSteps, mounted }, setClientState] = useState<{
    completedSteps: string[]; mounted: boolean;
  }>({ completedSteps: [], mounted: false });

  useEffect(() => {
    setClientState({
      mounted: true,
      completedSteps: workflowId ? getProgress(workflowId) : [],
    });
  }, [workflowId]);

  useEffect(() => {
    if (!workflow) router.replace("/dashboard");
  }, [workflow, router]);

  if (!workflow) return null;

  const steps = workflow.steps;
  const activeStep = steps.find((s) => !completedSteps.includes(s.id));
  const allDone = completedSteps.length === steps.length;
  const progressPct = Math.round((completedSteps.length / steps.length) * 100);

  function markDone(stepId: string) {
    setClientState((prev) => {
      if (prev.completedSteps.includes(stepId)) return prev;
      const next = [...prev.completedSteps, stepId];
      saveProgress(workflowId, next);
      return { ...prev, completedSteps: next };
    });
  }

  function handleStepAction(step: WorkflowStep) {
    if (step.href) {
      markDone(step.id);
      window.open(step.href, "_blank");
    } else if (step.linkType) {
      // Opens a "Create Link" dialog — stub for mockup
    }
  }

  function handleReset() {
    resetProgress(workflowId);
    setClientState((prev) => ({ ...prev, completedSteps: [] }));
  }

  function stepStatus(step: WorkflowStep): "done" | "active" | "locked" {
    if (completedSteps.includes(step.id)) return "done";
    if (step === activeStep) return "active";
    return "locked";
  }

  return (
    <div className="relative flex-1 flex flex-col overflow-hidden bg-gray-50" style={{ minHeight: "100%" }}>
      {/* Subtle teal glow */}
      <div
        aria-hidden="true"
        className="pointer-events-none absolute top-0 left-0 select-none"
        style={{
          width: 600,
          height: 600,
          background: "radial-gradient(circle at 30% 30%, rgba(90,233,215,0.1) 0%, transparent 65%)",
          filter: "blur(40px)",
        }}
      />

      {/* Title + reset */}
      <div className="relative z-10 flex items-start justify-between px-6 pt-8 pb-5">
        <div>
          <p className="text-[10px] font-semibold uppercase tracking-[0.2em] mb-1 text-gray-400">Workflow</p>
          <p className="text-lg font-bold leading-tight text-gray-900">{workflow.title}</p>
          <p className="text-sm leading-snug mt-0.5 max-w-sm text-gray-500">{workflow.blurb}</p>
        </div>
        {mounted && completedSteps.length > 0 && (
          <button
            onClick={handleReset}
            className="flex items-center gap-1.5 text-xs transition-colors mt-1 shrink-0 ml-4 text-gray-400 hover:text-gray-700"
          >
            <RotateCcw size={11} />
            Reset
          </button>
        )}
      </div>

      {/* Progress bar + step counter */}
      <div className="relative z-10 px-6 mb-5">
        <div className="flex items-center gap-3 mb-3">
          <div className="flex-1 h-1.5 rounded-full overflow-hidden" style={{ background: "rgb(229,231,235)" }}>
            <motion.div
              className="h-full rounded-full bg-teal-500"
              initial={{ width: 0 }}
              animate={{ width: mounted ? `${progressPct}%` : "0%" }}
              transition={{ duration: 0.5, ease: "easeOut" }}
            />
          </div>
          <span className="text-xs font-medium tabular-nums shrink-0 text-gray-400">
            {completedSteps.length} / {steps.length}
          </span>
        </div>

        {/* Step pills */}
        <div className="flex flex-wrap gap-2">
          {steps.map((step, i) => {
            const status = stepStatus(step);
            return (
              <div
                key={step.id}
                className={
                  status === "done"   ? "flex items-center gap-1.5 rounded-full px-3 py-1 text-xs font-medium text-gray-400"
                  : status === "active" ? "flex items-center gap-1.5 rounded-full px-3 py-1 text-xs font-medium text-teal-700"
                  : "flex items-center gap-1.5 rounded-full px-3 py-1 text-xs font-medium text-gray-300"
                }
                style={
                  status === "active"
                    ? { background: "rgb(240,253,250)", border: "1px solid rgb(153,246,228)" }
                    : { background: "rgb(249,250,251)", border: "1px solid rgb(229,231,235)" }
                }
              >
                {status === "done" ? <CheckCircle2 size={10} />
                  : status === "active" ? <Circle size={10} />
                  : <Lock size={10} />}
                {i + 1}. {step.title}
              </div>
            );
          })}
        </div>
      </div>

      {/* Centered content */}
      <div className="relative z-10 flex-1 flex flex-col px-4 pb-12">
        <div className="w-full max-w-xl mx-auto space-y-3">

          {/* All done state */}
          {allDone && (
            <div className="rounded-2xl p-10 text-center bg-white border border-gray-200 shadow-sm">
              <CheckCircle2 size={40} className="mx-auto mb-4 text-teal-500" />
              <p className="text-2xl font-bold mb-2 text-gray-900">All steps complete</p>
              <p className="text-sm max-w-xs mx-auto text-gray-500">
                Great work. You can reset this workflow and run it again any time.
              </p>
            </div>
          )}

          {/* Active step card */}
          <AnimatePresence mode="wait">
            {activeStep && (
              <motion.div
                key={activeStep.id}
                initial={{ opacity: 0, y: 20 }}
                animate={{ opacity: 1, y: 0 }}
                exit={{ opacity: 0, y: -12 }}
                transition={{ duration: 0.4, ease: [0.2, 0.8, 0.2, 1] }}
              >
                <div className="rounded-2xl p-8 bg-white border border-gray-200 shadow-sm">
                  <p className="text-[10px] font-semibold uppercase tracking-widest mb-5 text-gray-400">
                    Step {steps.indexOf(activeStep) + 1} of {steps.length}
                  </p>
                  {(() => {
                    const Icon = STEP_ICONS[activeStep.id];
                    return Icon ? (
                      <div className="w-14 h-14 rounded-2xl flex items-center justify-center mb-6 bg-teal-50">
                        <Icon size={26} className="text-teal-600" />
                      </div>
                    ) : null;
                  })()}
                  <h2 className="text-2xl font-bold leading-tight mb-3 text-gray-900">{activeStep.title}</h2>
                  <p className="text-base leading-relaxed mb-8 text-gray-500">{activeStep.description}</p>
                  <Button
                    size="lg"
                    onClick={() => handleStepAction(activeStep)}
                    className="bg-teal-500 hover:bg-teal-500/90 text-white gap-2 w-full h-12 text-base"
                  >
                    {activeStep.cta}
                    <ArrowRight size={16} />
                  </Button>
                  <button
                    onClick={() => markDone(activeStep.id)}
                    className="mt-3 w-full text-center text-xs text-gray-400 hover:text-gray-600 transition-colors"
                  >
                    Skip this step
                  </button>
                </div>
              </motion.div>
            )}
          </AnimatePresence>

          {/* Completed steps stack */}
          {mounted && completedSteps.length > 0 && (
            <div className="space-y-2 pt-1">
              {steps.filter((s) => completedSteps.includes(s.id)).map((step) => {
                const Icon = STEP_ICONS[step.id] ?? CheckCircle2;
                return (
                  <div
                    key={step.id}
                    className="flex items-center gap-3 rounded-xl px-4 py-3 bg-gray-50 border border-gray-100"
                  >
                    <CheckCircle2 size={13} className="shrink-0 text-teal-400/60" />
                    <Icon size={12} className="shrink-0 text-gray-300" />
                    <span className="text-sm line-through text-gray-400">{step.title}</span>
                  </div>
                );
              })}
            </div>
          )}
        </div>
      </div>
    </div>
  );
}
```

---

## 7. KPI grid (`KpiCard.tsx` + `KpiGrid.tsx`)

Row of 5 identical white cards under the workflow mini-cards. Each shows a label, big number, and subtitle. Optional HoverCard preview (Radix) shows a mini-list on hover.

### `KpiCard.tsx`
```tsx
import Link from "next/link";
import type { ReactNode } from "react";
import { cn } from "@/lib/utils";
import { HoverCard, HoverCardContent, HoverCardTrigger } from "@/components/ui/hover-card";

interface KpiCardProps {
  label: string;
  value: string | number;
  trend: number;
  subtitle: string;
  isPlaceholder?: boolean;
  href?: string;
  preview?: ReactNode;
}

export function KpiCard({ label, value, trend, subtitle, isPlaceholder = false, href, preview }: KpiCardProps) {
  const isPositive = trend > 0;

  const content = (
    <>
      <div className="flex items-center justify-between">
        <p className="text-xs font-semibold text-gray-400 uppercase tracking-wide">{label}</p>
        {trend !== 0 ? (
          <span className={cn("text-xs font-semibold", isPositive ? "text-brand-green" : "text-brand-red")}>
            {isPositive ? "+" : ""}{trend}%
          </span>
        ) : (
          <span className="text-xs text-gray-300">—</span>
        )}
      </div>
      {isPlaceholder ? (
        <p className="text-3xl font-bold text-gray-300 leading-none">—</p>
      ) : (
        <p className="text-3xl font-bold text-gray-900 leading-none">{value}</p>
      )}
      <p className="text-xs text-gray-400">{subtitle}</p>
    </>
  );

  const baseClassName = "bg-white rounded-xl border border-gray-100 p-5 flex flex-col gap-3 shadow-card";

  const card = href ? (
    <Link href={href} className={cn(baseClassName, "transition-all hover:border-gray-200 hover:shadow-card-hover")}>
      {content}
    </Link>
  ) : (
    <div className={baseClassName}>{content}</div>
  );

  if (!preview) return card;

  return (
    <HoverCard openDelay={120} closeDelay={80}>
      <HoverCardTrigger asChild>{card}</HoverCardTrigger>
      <HoverCardContent align="start" className="w-72 p-0 overflow-hidden">
        <div className="px-3 py-2 border-b border-gray-100 bg-gray-50/60">
          <p className="text-xs font-semibold text-gray-400 uppercase tracking-wide">{label}</p>
        </div>
        <div className="p-2">{preview}</div>
      </HoverCardContent>
    </HoverCard>
  );
}
```

### `KpiGrid.tsx` (composition)
```tsx
// The 5 KPIs: Active Leads · Engaged Leads · Active Links · Link Views · Saved Searches
<div className="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-5 gap-3">
  {items.map((item) => <KpiCard key={item.label} {...item} />)}
</div>
```

Sample item shape:
```ts
{ label: "Active Leads", value: 12, trend: 0, subtitle: "Pre-qual + net sheet", href: "/leads" }
```

---

## 8. Captured this period strip (`CapturedSection.tsx` + `MiniStatCard.tsx`)

A white card that sits under the KPI grid. Contains a header + "View all leads" link, then a horizontal stat strip (icon + number + label × 4, divided by hairlines), then a chart section with a Select + segmented granularity pills. Spans 4 of 5 columns in the dashboard layout.

### `MiniStatCard.tsx`
```tsx
import type { LucideIcon } from "lucide-react";

interface MiniStatCardProps {
  icon: LucideIcon;
  label: string;
  value: number;
  subtitle: string;
}

export function MiniStatCard({ icon: Icon, label, value }: MiniStatCardProps) {
  return (
    <div className="flex items-center gap-3 px-4 py-3 flex-1 min-w-0">
      <div className="w-7 h-7 rounded-lg flex items-center justify-center bg-white border border-gray-100 shrink-0">
        <Icon size={13} className="text-gray-400" />
      </div>
      <div className="min-w-0">
        <p className="text-lg font-bold text-gray-900 leading-none">{value}</p>
        <p className="text-[10px] text-gray-400 uppercase tracking-wide mt-0.5 truncate">{label}</p>
      </div>
    </div>
  );
}
```

### Strip container (from `CapturedSection.tsx`)
```tsx
<div className="bg-white rounded-xl border border-gray-100 p-6 shadow-card">
  <div className="flex items-center justify-between mb-4">
    <h2 className="text-sm font-semibold text-gray-900">Captured this period</h2>
    <Link href="/leads" className="flex items-center gap-1 text-xs text-teal-600 hover:text-teal-700 font-medium transition-colors">
      View all leads
      <ArrowUpRight size={12} />
    </Link>
  </div>

  {/* 4-slot stat strip — divided container, not individual cards */}
  <div className="grid grid-cols-2 sm:grid-cols-4 divide-x divide-y sm:divide-y-0 divide-gray-100 bg-gray-50 rounded-xl border border-gray-100 mb-5 overflow-hidden">
    {miniStats.map((stat) => <MiniStatCard key={stat.label} {...stat} />)}
  </div>

  {/* Chart controls + chart component */}
  <div className="rounded-lg border border-gray-100 p-4">…</div>
</div>
```

Stat items:
```ts
[
  { icon: Shield,   label: "Pre-Quals",     value: 4, subtitle: "Last 30 days" },
  { icon: FileText, label: "Net Sheets",    value: 2, subtitle: "Last 30 days" },
  { icon: Home,     label: "Valuations",    value: 7, subtitle: "Last 30 days" },
  { icon: Bookmark, label: "Saved Searches", value: 12, subtitle: "Active subscribers" },
]
```

---

## 9. Reference: the shadcn `Button` component

The Button used everywhere is shadcn's default. If your mockup doesn't have it, you can approximate with:

```tsx
// Primary CTA
<button className="inline-flex items-center justify-center gap-2 rounded-md text-sm font-medium h-9 px-4 bg-teal-500 hover:bg-teal-500/90 text-white transition-colors">
  {children}
</button>

// Ghost/disabled variant used for inactive carousel card CTAs
<button className="inline-flex items-center justify-center gap-2 rounded-md text-sm font-medium h-9 px-4 bg-gray-100 hover:bg-gray-200 text-gray-400 transition-colors">
  {children}
</button>
```

---

## 10. What the designer should produce

1. **Figma frames** for each screen (dashboard hero carousel, dashboard metrics view, workflow detail page)
2. **Prototype interactions**: the carousel drag, the pill toggle, the progress bar fill animation, the active-step card enter/exit
3. **Component library** in Figma or code — reusable "Workflow Card", "Mini Workflow Card", "KPI Card", "Mini Stat", "Step Pill"

**Fidelity target:** pixel-accurate to the code above. All spacing, radii, and colors are already defined — don't invent variations.
