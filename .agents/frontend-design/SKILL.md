---
name: frontend-design
description: 'Baseline design philosophy for distinctive, production-grade UI. Use when visual quality and aesthetic direction matter — components, pages, layouts, styling. Extends the frontend skill — all frontend rules still apply.'
license: MIT
---

> Read `.agents/software-principles/SKILL.md` and `.agents/frontend/SKILL.md` first.

## Design Thinking

Before writing code, commit to a clear aesthetic direction:

1. **Purpose** — What problem does this UI solve? Who uses it?
2. **Tone** — Pick one and commit: minimal · editorial · playful · brutalist · luxury · retro-futuristic · organic · industrial · soft · geometric. Intentionality over intensity.
3. **Differentiation** — What is the one thing a user will remember about this UI?

No two designs should look the same. Avoid converging on safe, predictable choices.

## Typography

Choose characterful, distinctive fonts — never Inter, Roboto, Arial, or system fonts as a default. Pair a display font with a refined body font. Font choice drives the entire aesthetic — treat it as the first design decision.

## Color & Composition

- Apply **60-30-10**: dominant (60%) · secondary (30%) · accent (10%)
- Map every color to a semantic role (background, surface, foreground, muted, primary, accent, destructive, success) — never use raw hex ad hoc
- Contrast minimum WCAG AA: normal text ≥ 4.5:1, UI components ≥ 3:1
- Commit to light or dark — don't default to neutral gray
- Asymmetry, overlap, diagonal flow, grid-breaking elements over predictable grid layouts
- Generous negative space OR controlled density — pick one and execute it fully

## Spatial Design (8-Point Grid)

All spacing, sizing, and layout values must be **multiples of 4px**. Prefer 8px increments; use 4px only when 8px is too large. **Never use odd numbers.**

- Applies to: padding, margin, gap, width, height, border-radius, icon size
- Min touch target: 44×44px
- Border radius: pick one scale per project (`4 · 8 · 12 · 16 · 24 · 9999px`) and use it consistently

## UI/UX Principles

- **Hierarchy** — one primary CTA per view; use size + contrast + weight to signal importance
- **Proximity** — related elements closer together than unrelated ones
- **Consistency** — same component, same appearance everywhere; one icon style (outlined vs filled), never mixed
- **Four states** — every interactive element must handle: ideal · loading (skeleton > spinner) · empty (with actionable next step) · error (specific + recoverable)
- **Accessibility** — keyboard-navigable with visible focus ring; `aria-label` on icon-only buttons; wrap animations in `prefers-reduced-motion`
- **Responsive** — mobile-first; fluid type/spacing with `clamp()`

## Motion

Focus on high-impact moments: page load with staggered reveals, surprising hover states, scroll-triggered transitions. One orchestrated entrance creates more delight than scattered micro-interactions everywhere. Use whatever motion tool fits the project — CSS animations, Framer Motion, GSAP, Three.js, etc. Match tool complexity to the aesthetic vision.

## Visual Details

Add depth — gradient meshes, noise textures, geometric patterns, layered transparencies, dramatic shadows, decorative borders. Match the effect to the tone. Do not add texture that contradicts the chosen aesthetic direction.

## Never Do

- Generic AI aesthetics: Inter/Roboto/Arial, purple-on-white gradients, cookie-cutter card layouts
- Aesthetic choices that contradict the committed tone
- Converge on the same aesthetic across different designs
- `outline: none` without a visible focus replacement
- Color as the sole indicator of meaning or state
