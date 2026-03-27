---
name: rikai
description: >
  Turn any codebase into a deeply interactive, single-page HTML learning experience for vibe coders — people who build with AI but want to understand what they built. Trigger whenever someone says "explain this codebase," "help me understand this project," "make a course from this repo," "teach me how this code works," "I want to understand the architecture," "turn this into something I can learn from," or shares a GitHub link and wants to understand it. Also trigger when someone says they feel lost in their own project, wants to debug more effectively, or wants to steer AI coding tools better. Rikai (理解, Japanese for "understanding") produces a single HTML file — no setup, no dependencies beyond Google Fonts — that teaches the codebase through storytelling, animated flows, interactive quizzes, and side-by-side code/English panels.
---

# Rikai — Codebase Understanding for Vibe Coders

> 理解 (rikai) — Japanese for "understanding, comprehension"

Rikai transforms any codebase into a cinematic, single-page learning experience. The learner already built (or found) something that works. Rikai answers: *"What exactly did I build — and how does it actually work?"*

---

## Who You're Teaching

**The vibe coder**: builds software by directing AI in natural language. May have never read their own codebase. Practical, fast-moving, curious — but not trying to become a software engineer. They need:

1. **Vocabulary** — precise technical terms so they can give better instructions to AI ("put this in the service layer" vs "the thing that does the logic")
2. **Architecture intuition** — where things live and why, so they can navigate and extend
3. **Debugging eyes** — pattern recognition for when something smells wrong
4. **AI steering power** — knowing enough to correct AI mistakes and unblock loops

**Assume zero prior CS knowledge.** Every term needs a plain-language definition, embedded inline (tooltips), not in a separate glossary. Write like a brilliant friend explaining over coffee, not a professor lecturing from a podium.

---

## Phase 1 — Deep Read

Before writing a single line of HTML, thoroughly understand the codebase.

### What to extract

**Architecture map**
- Main actors: every significant file, module, or service — what it *owns* (its responsibility), what it *knows about* (its dependencies), and what it *talks to*
- The dependency graph: who imports who, which direction data flows, what's at the center vs the edges
- Entry points: where does execution begin? Where does a user request enter the system?

**The living story**
- Pick the single most representative user action (e.g., "user clicks Submit") and trace it fully — frontend → validation → API call → database → response → UI update. This becomes your spine.
- Secondary flows worth tracing: auth, error paths, background jobs

**Clever engineering**
- Caching strategies, lazy loading, memoization
- Error handling philosophy (fail fast? silent fallback?)
- Unusual patterns or strong opinions baked into the architecture

**Friction points**
- Files with unusually high churn (many changes in git history = complex/fragile)
- Long functions, deeply nested code, or commented-out blocks (debt signals)
- Missing error handling or TODO comments

**Tech stack rationale**
- For each major dependency, what problem does it solve? Why this one and not an alternative?

### Codebase entry sequence

1. `README.md` — product description, setup instructions, architecture diagrams if present
2. `package.json` / `pyproject.toml` / `Cargo.toml` / equivalent — full dependency map
3. Main entry point (`main.py`, `index.ts`, `app.js`, `cmd/...`) — execution start
4. Config files (`.env.example`, `config/`) — environment, feature flags, secrets shape
5. Core business logic — the files that would make the app stop working if deleted
6. API/route definitions — the public contract
7. Data models/schemas — the shape of data at rest
8. Tests (if any) — often the best documentation of intent

> **Rule:** Figure out what the app does yourself. Never ask the user to explain the product. They may not know either — that's why they're using Rikai.

---

## Phase 2 — Curriculum Architecture

Design 5–8 modules. The structure is always a zoom-in:

```
Wide (experience) → Narrow (implementation)
↓
Module 1: The Big Picture — what does this app do, who uses it, what's the core value?
Module 2: The Journey — trace one user action from click to response, end-to-end
Module 3: The Cast — who are the main "characters"? What does each one own?
Module 4: The Conversations — how do the pieces talk to each other?
Module 5: The Outside World — external APIs, databases, third-party services
Module 6: The Safety Net — error handling, validation, edge cases
Module 7: The Clever Bits — interesting patterns, architecture decisions, trade-offs
Module 8: (if needed) The Big Picture Revisited — now that you know how it works, what would you change?
```

Adapt ruthlessly. A simple CLI tool might need 4 modules. A microservices app might need 8. Every module must answer "why does this help me build better with AI?" — if it doesn't, cut it or reframe it.

### Module anatomy

Each module contains:
- **A hook** — one sentence that explains why this module matters for the vibe coder (practical framing)
- **3–6 screens** — each screen teaches exactly one idea
- **One code ↔ English translation** (minimum, often more)
- **One hero visual** — the dominant teaching element (flow animation, architecture diagram, etc.)
- **One quiz** (at end of module) — scenario-based, never definitional
- **Tooltips** on every technical term, first use per module
- **One metaphor** per new concept — must be original, never recycle, never "restaurant/kitchen"

### Required elements across the full course (not optional)

| Element | Minimum count | Purpose |
|---|---|---|
| Code ↔ English Translation | 1 per module | Core comprehension tool |
| Animated data flow | 1 per course | Show how data moves through the system |
| Group chat animation | 1 per course | Make component communication feel human |
| Scenario quiz | 1 per module | Test application, not memory |
| Glossary tooltips | All technical terms, first use per module | Vocabulary acquisition |

> **Do not present the curriculum for review. Design it, then build it.** The learner wants to understand their code, not approve a plan.

---

## Phase 3 — Build

Generate a single, self-contained HTML file. For large codebases, build module by module and append — don't try to generate everything at once.

### Technical constraints

```
✓ Single HTML file (no external JS/CSS files, no build step)
✓ Only external dependency: Google Fonts CDN
✓ scroll-snap-type: y proximity (NEVER mandatory — traps users)
✓ min-height: 100dvh (100vh fallback)
✓ All JS in IIFE, passive scroll listeners, rAF throttling
✓ white-space: pre-wrap on ALL code (no horizontal scroll ever)
✓ Tooltips: position: fixed, appended to document.body (never clipped by overflow:hidden)
✓ Touch support on all drag interactions
✓ Keyboard nav: arrow keys between modules
✓ ARIA roles on interactive elements
✓ GPU animations only: transform + opacity (never width/height/top/left)
```

### Read reference files when building

- `references/design.md` — full CSS design system (tokens, typography, components)
- `references/interactions.md` — implementation patterns for every interactive element

Read these **before** writing CSS or interactive elements. They contain complete, working code — don't reinvent.

---

## Phase 4 — Content Rules

These are what separate Rikai from a generic tutorial generator. Internalize them.

### The 50% visual rule
Every screen must be at least 50% visual. If you have written 3+ sentences in a row, stop and convert the next piece of information into a diagram, card, animation, or code block.

**Conversion table:**

| If you're about to write... | Convert it to... |
|---|---|
| A list of 3+ items | Icon cards with one-liner descriptions |
| A sequence of steps | Numbered step cards or flow diagram |
| "Component A talks to B" | Animated data flow or group chat |
| "File X does Y, file Z does W" | Annotated file tree |
| What a piece of code does | Code ↔ English translation block |
| Two competing approaches | Side-by-side comparison columns |
| A warning or insight | Callout box (aha!/warning/tip) |

### Code snippets — strict rules
- **Use real code from the codebase, verbatim.** Never modify, simplify, or "clean up."
- Instead of trimming code, **find** naturally short (5–15 line) snippets that illustrate the concept.
- Every codebase has compact, self-contained moments — seek those out.
- Trust: the learner should be able to open the real file and see the exact code they learned from.

### Metaphors — rules and anti-patterns
Every new technical concept gets one metaphor. Rules:
1. The metaphor must be **organically fitting** — it should feel *inevitable* for that concept
2. **Never recycle** — each module uses different metaphors
3. **Banned forever:** restaurant, kitchen, chef, waiter, recipe (overused to the point of meaninglessness)
4. **Good metaphor targets:** the concept's *mechanism*, not just its *purpose*

Examples of strong metaphors:
- Event loop → air traffic controller (single runway, coordinates many planes, no parallel landings)
- Database index → back-of-book index (find the page without reading every page)
- Middleware → customs at an airport (every person passes through, can be stopped or waved through)
- Cache → sticky note on your monitor (you could look it up, but it's faster right there)
- JWT token → a stamped wristband at a concert (you proved your identity once at the gate)
- Promises → a restaurant buzzer (you get on with life; it alerts you when ready)

### Quizzes — application over memory
Quiz types (in order of value):
1. **Scenario** — "You want to add feature X. Which files would you touch and why?"
2. **Debugging** — "A user reports Y is broken. Where would you look first?"
3. **Architecture decision** — "You're adding a new integration. Frontend or backend? Why?"
4. **Trace** — "User does X. Walk me through what happens."

**Never quiz:**
- Definitions ("What does API stand for?")
- File names ("Which file handles routing?")
- Syntax ("What's the correct way to write a fetch?")
- Anything answerable by scrolling up

**Quiz tone:** wrong answers get a *teaching moment*, not just "incorrect." Right answers reinforce the *principle*, not just confirm correctness. No scores. No "you got X/Y." It's thinking practice, not an exam.

### Tooltips — be aggressive
If there is even a 1% chance a non-technical person doesn't know a term, tooltip it. This includes:
- All acronyms (API, CLI, SDK, JWT, REPL, JSON, ORM, CDN...)
- All programming concepts (function, class, module, variable, callback, middleware...)
- All infrastructure terms (env variable, namespace, entry point, path, pip...)
- Software names they might not know (Prisma, Vite, Fastify, Zod...)

**The vocabulary IS the learning.** The tooltip should teach the term in a way the learner can USE it — not just understand it. Format: short plain-English definition, then optionally "When talking to AI, you'd say: ..."

**Tooltip implementation: must use `position: fixed` + `getBoundingClientRect()` + append to `document.body`.** This is not optional. Any other approach clips tooltips inside overflow containers.

---

## Phase 5 — Quality Checklist

Before presenting the course, verify each item:

**Content**
- [ ] Every technical term has a tooltip on first use (per module)
- [ ] No text block is more than 3 sentences
- [ ] Every screen is at least 50% visual
- [ ] No code snippet has been modified from the original
- [ ] No metaphor appears more than once
- [ ] No "restaurant" or "kitchen" metaphors anywhere
- [ ] Every quiz question tests application, not memory

**Interactivity**
- [ ] At least one animated data flow in the course
- [ ] At least one group chat animation in the course
- [ ] At least one quiz per module
- [ ] At least one code ↔ English translation per module

**Technical**
- [ ] `scroll-snap-type: y proximity` (not mandatory)
- [ ] No horizontal scrollbars on any code block
- [ ] Tooltips use `position: fixed` and append to `body`
- [ ] All JS wrapped in IIFE
- [ ] Keyboard arrow navigation works
- [ ] Looks good on mobile (test at 375px width)
- [ ] File is completely self-contained

---

## Failure Modes — What Goes Wrong

**Tooltip clipping** — The #1 bug. Tooltips use `position: absolute` inside an `overflow: hidden` container → they disappear. Always `position: fixed` + `document.body`. Always.

**Text avalanche** — Prose paragraphs describing what code does instead of showing a code ↔ English block. Any description longer than 2 sentences should be converted to a visual.

**Clone metaphors** — Using "restaurant" or reusing any metaphor from an earlier module. Each concept deserves its own metaphor.

**Sanitized code** — Showing cleaned-up code instead of real code from the file. The learner loses trust when the "real" file looks different.

**Memory quizzes** — Asking what API stands for or which file handles routing. Test thinking, not recall.

**Mandatory scroll-snap** — Trapping users inside long modules. `proximity` always.

**Module soup** — Writing all modules in one long pass causes later modules to be thin and rushed. Build one module → verify → next.

**Fake technical depth** — Explaining what a function *is called* instead of what it *does* and *why*. Every explanation should end with a practical implication: "This means when you're directing AI to add feature X, you'd tell it to..."

---

## Rikai vs Generic Tutorials

| Generic tutorial | Rikai |
|---|---|
| Explains concepts in abstract | Traces concepts through the learner's actual code |
| Assumes you want to code | Assumes you want to *understand and direct* |
| Starts with syntax | Starts with the user experience |
| Glossary at the end | Tooltips inline, where the term appears |
| Tests definition recall | Tests architectural reasoning |
| Uses any metaphor available | Hunts for the one metaphor that makes the concept inevitable |
| Shows cleaned-up example code | Shows real code, verbatim, with English translation |
| One-size curriculum | Curriculum shaped around this specific codebase's personality |