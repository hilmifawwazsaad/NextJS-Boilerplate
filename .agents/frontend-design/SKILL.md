---
name: frontend-design
description: 'Baseline design philosophy for distinctive, production-grade UI. Use when visual quality and aesthetic direction matter — components, pages, layouts, styling. Extends the frontend skill — all frontend rules still apply.'
license: MIT
---

> Read `.agents/software-principles/SKILL.md` and `.agents/frontend/SKILL.md` first — all their rules apply here.

## Design Thinking

Before writing code, commit to a clear aesthetic direction:

1. **Purpose** — What problem does this UI solve? Who uses it?
2. **Tone** — Pick one and commit: minimal · editorial · playful · brutalist · luxury · retro-futuristic · organic · industrial · soft · geometric. Intentionality over intensity.
3. **Differentiation** — What is the one thing a user will remember about this UI?

No two designs should look the same. Avoid converging on safe, predictable choices.

## Typography

Choose characterful, distinctive fonts — never Inter, Roboto, Arial, or system fonts as a default. Pair a display font with a refined body font. Font choice drives the entire aesthetic — treat it as the first design decision.

## Color & Composition

- Dominant palette + sharp accent outperforms even, timid color distribution
- Commit to light or dark — don't default to neutral gray
- Asymmetry, overlap, diagonal flow, grid-breaking elements over predictable grid layouts
- Generous negative space OR controlled density — pick one and execute it fully

## Motion

Focus on high-impact moments: page load with staggered reveals, surprising hover states, scroll-triggered transitions. One orchestrated entrance creates more delight than scattered micro-interactions everywhere. Use whatever motion tool fits the project — CSS animations, Framer Motion, GSAP, Three.js, etc. Match tool complexity to the aesthetic vision.

## Visual Details

Add depth — gradient meshes, noise textures, geometric patterns, layered transparencies, dramatic shadows, decorative borders. Match the effect to the tone. Do not add texture that contradicts the chosen aesthetic direction.

## Never Do

- Generic AI aesthetics: Inter/Roboto/Arial, purple-on-white gradients, cookie-cutter card layouts
- Aesthetic choices that contradict the committed tone
- Ignore the four states (loading · error · empty · ideal) for the sake of visual polish
- Converge on the same aesthetic across different designs
