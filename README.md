# Rikai

> *Turn any codebase into a cinematic learning experience — built for the vibe coder who wants to understand what they built.*

**→ [See a live example: Quasar](https://quasar.nanasi.workers.dev/)**

---

## What is Rikai?

Rikai (理解) is Japanese for **understanding, comprehension**.

It's a Claude skill that transforms any codebase into a single, self-contained HTML learning experience — no setup, no dependencies, no "you need to know Rust first." Just open the file and start understanding.

The learner already built something (or found something). Rikai answers the question they're actually asking:

> *"What exactly did I build — and how does it actually work?"*

---

## Who It's For

**The vibe coder.** Someone who builds software by directing AI in natural language. They may have never read their own codebase. Practical, fast-moving, genuinely curious — but not trying to become a software engineer.

They need three things:

- **Vocabulary** — precise technical terms so they can give better instructions to AI
- **Architecture intuition** — where things live and why, so they can navigate and extend
- **AI steering power** — knowing enough to correct AI's mistakes and escape bug loops

Rikai gives them all three, through storytelling instead of lectures.

---

## What Gets Generated

A single `.html` file. Nothing to install. Open it in any browser.

Inside, the codebase becomes a scrollable course — 5–8 modules that zoom from the user experience down to the implementation details:

| Module | What it teaches |
|--------|----------------|
| **The Big Picture** | What does this app do, and why is it interesting? |
| **The Journey** | Trace one user action from click to database and back |
| **The Cast** | Who are the main "characters"? What does each one own? |
| **The Conversations** | How do the pieces talk to each other? |
| **The Outside World** | External APIs, databases, third-party services |
| **The Safety Net** | Error handling, validation, edge cases |
| **The Clever Bits** | Interesting patterns, architecture decisions, trade-offs |

Every module includes:

- Side-by-side **code ↔ plain English** translations — real code, verbatim, never cleaned up
- **Animated data flows** showing how information travels through the system
- **Group chat animations** — components talking to each other like iMessages
- **Scenario quizzes** that test architectural reasoning, not memorization
- **Glossary tooltips** on every technical term, inline, so the learner never has to leave the page
- **AI Steering Tips** — "now that you know this, here's exactly how to instruct Claude better"

---

## The AI Steering Tip

This is Rikai's signature move.

Every architectural concept maps to a concrete change in *how the learner talks to AI*. Understanding the service layer isn't just interesting — it means the learner can now say:

> *"Add a `calculateDiscount()` method to the OrderService class in `services/order.service.ts`"*

instead of:

> *"add a discount feature"*

That precision is the difference between getting it right on the first try and spending an afternoon in a bug loop. Rikai teaches the vocabulary of software so the learner can wield AI like a precision instrument.

---

## Usage

Point Rikai at any codebase:

```
explain this codebase
```

```
help me understand this project
```
```
make a course from this repo https://github.com/user/repo
```

```
I want to understand the architecture
```

```
Turn this into something I can learn from
```

Rikai reads the code, figures out what the app does, designs a curriculum, and generates the course. No planning docs, no approval step — just the course.

---

## Design Philosophy

The output looks like a **beautiful developer notebook** — warm off-white backgrounds, a single bold accent color, generous whitespace, and dark code blocks with Catppuccin-inspired syntax highlighting.

Every screen is at least 50% visual. If there are more than 3 sentences in a row, something is wrong.

Metaphors are earned, not defaulted to. Every concept gets the metaphor that feels *inevitable* for it — an event loop is an air traffic controller, a JWT token is a concert wristband, zero-copy is reading blueprints instead of building a house. No restaurant analogies. Ever.

Quizzes test whether you can reason, not whether you can remember. No scores. No "you got 4/5." It's a thinking exercise, not an exam.

---

## What Rikai Is Not

- **Not a documentation generator.** It doesn't produce API docs or changelogs.
- **Not a code review tool.** It doesn't find bugs or suggest improvements.
- **Not a tutorial for people learning to code.** The learner isn't trying to write code — they want to *understand and direct* it.
- **Not a generic "explain this" wrapper.** The output is curriculum-shaped, metaphor-driven, and practically grounded in AI steering — not a summary or a walkthrough.

---

## File Structure

```
rikai/
├── SKILL.md                  ← Core instructions: philosophy, phases, content rules
└── references/
    ├── design.md             ← Complete CSS design system, typography, layout tokens
    └── interactions.md       ← Every interactive element with full working code
```

Claude reads `SKILL.md` at skill activation. The reference files are loaded on demand — `design.md` before writing any CSS, `interactions.md` before building interactive elements.

---

---

Built by **[Nanasi](https://x.com/oxnanasi)** · [𝕏](https://x.com/oxnanasi) · [GitHub](https://github.com/spellsaif)

*Inspired by the belief that understanding your tools — really understanding them — is how vibe coding becomes craft.*
