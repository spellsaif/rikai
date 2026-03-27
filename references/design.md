# Rikai Design System

Complete CSS design tokens and component styles. Copy the entire `:root` block into every course. Adapt only `--color-accent` to match the project's personality.

---

## Color Palette

```css
:root {
  /* BACKGROUNDS */
  --bg:             #F9F6F1;   /* warm off-white, like aged paper */
  --bg-alt:         #F4EFE7;   /* slightly darker for alternating modules */
  --bg-code:        #1C1B2E;   /* deep indigo for code blocks */
  --bg-surface:     #FFFFFF;
  --bg-surface-warm:#FDF9F4;

  /* TEXT */
  --text:           #27251F;   /* near-black, warm */
  --text-secondary: #6A6460;
  --text-muted:     #9C9590;
  --text-code:      #CDD6F4;   /* light on dark code bg */

  /* BORDERS */
  --border:         #E4DDD4;
  --border-light:   #EDEBE5;

  /* ACCENT — change this per project */
  /* Options: vermillion #D94F30 | teal #1E7F96 | amber #C9882A | forest #2B7A4B | indigo #5B4FCF */
  --accent:         #D94F30;
  --accent-hover:   #C2401F;
  --accent-light:   #FDEEE9;
  --accent-muted:   #E5806A;

  /* SEMANTIC */
  --success:        #2B7A4B;
  --success-light:  #E6F5ED;
  --error:          #C53030;
  --error-light:    #FDE8E8;
  --info:           #1E7F96;
  --info-light:     #E3F3F7;
  --warning:        #C9882A;
  --warning-light:  #FDF3E0;

  /* ACTOR COLORS — assign one per major component */
  --actor-1: #D94F30;   /* vermillion */
  --actor-2: #1E7F96;   /* teal */
  --actor-3: #7060AA;   /* plum */
  --actor-4: #C9882A;   /* amber */
  --actor-5: #2B7A4B;   /* forest */
}
```

**Picking an accent per project:**
- Web app / SaaS → vermillion `#D94F30`
- Data / analytics → teal `#1E7F96`
- Finance / careful → amber `#C9882A`
- Developer tool / CLI → indigo `#5B4FCF`
- Content / media → forest `#2B7A4B`

**Module alternation:** odd modules use `--bg`, even modules use `--bg-alt`. This creates visual rhythm without design effort.

---

## Typography

```css
:root {
  --font-display: 'Bricolage Grotesque', Georgia, serif;
  --font-body:    'DM Sans', -apple-system, sans-serif;
  --font-mono:    'JetBrains Mono', 'Fira Code', monospace;

  /* Scale — 1.25 ratio */
  --text-xs:   0.75rem;    /* 12px */
  --text-sm:   0.875rem;   /* 14px */
  --text-base: 1rem;       /* 16px */
  --text-lg:   1.125rem;   /* 18px */
  --text-xl:   1.25rem;    /* 20px */
  --text-2xl:  1.5rem;     /* 24px */
  --text-3xl:  1.875rem;   /* 30px */
  --text-4xl:  2.25rem;    /* 36px */
  --text-5xl:  3rem;       /* 48px */
  --text-6xl:  3.75rem;    /* 60px — module numbers */

  --leading-tight:  1.15;
  --leading-snug:   1.3;
  --leading-normal: 1.6;
  --leading-loose:  1.8;
}
```

**Google Fonts (put in `<head>`):**
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,400;12..96,600;12..96,700;12..96,800&family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;1,9..40,400&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
```

**Usage rules:**
- Module numbers: `--text-6xl`, display font, weight 800, `--accent` at 12% opacity
- Module titles: `--text-4xl`, display font, weight 700
- Screen headings: `--text-xl`–`--text-2xl`, display font, weight 600
- Body: `--text-base`–`--text-lg`, body font, `--leading-normal`
- Code: `--text-sm`, mono font
- Labels/badges: `--text-xs`, mono font, uppercase, `letter-spacing: 0.06em`

---

## Spacing & Layout

```css
:root {
  --sp-1:  0.25rem;
  --sp-2:  0.5rem;
  --sp-3:  0.75rem;
  --sp-4:  1rem;
  --sp-5:  1.25rem;
  --sp-6:  1.5rem;
  --sp-8:  2rem;
  --sp-10: 2.5rem;
  --sp-12: 3rem;
  --sp-16: 4rem;
  --sp-20: 5rem;
  --sp-24: 6rem;

  --content-max:   800px;
  --content-wide:  1040px;
  --nav-h:         52px;
  --radius-sm:     8px;
  --radius-md:     12px;
  --radius-lg:     18px;
  --radius-full:   9999px;
}

/* Module layout */
.module {
  min-height: 100dvh;
  min-height: 100vh; /* fallback */
  scroll-snap-align: start;
  padding: var(--sp-20) var(--sp-6) var(--sp-16);
  padding-top: calc(var(--nav-h) + var(--sp-12));
}
.module-content {
  max-width: var(--content-max);
  margin: 0 auto;
}
.module-content.wide {
  max-width: var(--content-wide);
}
```

---

## Shadows

```css
:root {
  /* Warm-tinted (never pure black) */
  --shadow-xs: 0 1px 2px rgba(39, 37, 31, 0.04);
  --shadow-sm: 0 2px 6px rgba(39, 37, 31, 0.07);
  --shadow-md: 0 4px 14px rgba(39, 37, 31, 0.09);
  --shadow-lg: 0 8px 28px rgba(39, 37, 31, 0.11);
  --shadow-xl: 0 16px 52px rgba(39, 37, 31, 0.13);
}
```

---

## Motion

```css
:root {
  --ease-out:    cubic-bezier(0.16, 1, 0.3, 1);
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);
  --dur-fast:    140ms;
  --dur-mid:     280ms;
  --dur-slow:    480ms;
  --stagger:     110ms;
}

/* Scroll-reveal */
.reveal {
  opacity: 0;
  transform: translateY(18px);
  transition: opacity var(--dur-slow) var(--ease-out),
              transform var(--dur-slow) var(--ease-out);
}
.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}
.stagger-children > .reveal {
  transition-delay: calc(var(--stagger-index, 0) * var(--stagger));
}
```

**JS to activate:**
```javascript
// Set stagger indices
document.querySelectorAll('.stagger-children').forEach(parent => {
  [...parent.children].forEach((c, i) => c.style.setProperty('--stagger-index', i));
});

// Intersection observer
const io = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) { e.target.classList.add('visible'); io.unobserve(e.target); }
  });
}, { rootMargin: '0px 0px -8% 0px', threshold: 0.08 });
document.querySelectorAll('.reveal').forEach(el => io.observe(el));
```

**Animation rule: only animate `transform` and `opacity`.** Never animate `width`, `height`, `top`, `left`, or `margin` — they trigger layout recalculation and cause jank.

---

## Navigation

```html
<nav class="nav" role="navigation">
  <div class="progress-bar" id="progress" role="progressbar" aria-valuenow="0"></div>
  <div class="nav-inner">
    <span class="nav-title"><!-- course title --></span>
    <div class="nav-dots" role="tablist">
      <!-- One per module: -->
      <button class="nav-dot" data-target="m1" aria-label="Module 1: Title"
              role="tab" data-tip="Module Name"></button>
    </div>
  </div>
</nav>
```

```css
.nav {
  position: fixed; top: 0; left: 0; right: 0;
  height: var(--nav-h);
  background: rgba(249, 246, 241, 0.92);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border);
  z-index: 100;
}
.progress-bar {
  position: absolute; top: 0; left: 0;
  height: 3px;
  background: var(--accent);
  width: 0%;
  transition: width 100ms linear;
}
.nav-inner {
  max-width: var(--content-wide);
  margin: 0 auto;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 var(--sp-6);
}
.nav-title {
  font-family: var(--font-display);
  font-weight: 700;
  font-size: var(--text-sm);
  color: var(--text);
  letter-spacing: -0.01em;
}
.nav-dots { display: flex; gap: var(--sp-2); }
.nav-dot {
  width: 10px; height: 10px;
  border-radius: var(--radius-full);
  border: 2px solid var(--border);
  background: transparent;
  cursor: pointer;
  transition: border-color var(--dur-fast), background var(--dur-fast), transform var(--dur-fast);
  position: relative;
}
.nav-dot:hover { transform: scale(1.3); border-color: var(--accent-muted); }
.nav-dot.active { border-color: var(--accent); background: var(--accent); transform: scale(1.2); }
.nav-dot.done { border-color: var(--accent); background: var(--accent); opacity: 0.5; }
/* Dot label on hover */
.nav-dot::after {
  content: attr(data-tip);
  position: fixed; /* use JS to set top/left from getBoundingClientRect */
  background: var(--text);
  color: #fff;
  padding: var(--sp-1) var(--sp-3);
  border-radius: var(--radius-sm);
  font-size: var(--text-xs);
  font-family: var(--font-body);
  white-space: nowrap;
  pointer-events: none;
  opacity: 0;
  transition: opacity var(--dur-fast);
}
```

**Progress JS:**
```javascript
function updateProgress() {
  const scrolled = window.scrollY;
  const total = document.documentElement.scrollHeight - window.innerHeight;
  document.getElementById('progress').style.width = (scrolled / total * 100) + '%';
}
window.addEventListener('scroll', () => requestAnimationFrame(updateProgress), { passive: true });
```

**Keyboard navigation:**
```javascript
const modules = [...document.querySelectorAll('.module')];
let current = 0;

function scrollTo(i) {
  i = Math.max(0, Math.min(i, modules.length - 1));
  current = i;
  modules[i].scrollIntoView({ behavior: 'smooth' });
}

document.addEventListener('keydown', (e) => {
  if (['INPUT','TEXTAREA','SELECT'].includes(e.target.tagName)) return;
  if (e.key === 'ArrowDown' || e.key === 'ArrowRight') { scrollTo(current + 1); e.preventDefault(); }
  if (e.key === 'ArrowUp'   || e.key === 'ArrowLeft')  { scrollTo(current - 1); e.preventDefault(); }
});
```

---

## Module HTML Template

```html
<section class="module" id="m{N}" style="background: var(--bg)">
  <!-- Alternate: style="background: var(--bg-alt)" -->
  <div class="module-content">
    <header class="module-header reveal">
      <div class="module-num">0{N}</div>
      <h1 class="module-title">Module Title</h1>
      <p class="module-subtitle">One sentence: what will the learner be able to do after this?</p>
    </header>

    <div class="module-body">

      <section class="screen reveal">
        <h2 class="screen-heading">Screen Title</h2>
        <!-- content -->
      </section>

      <!-- More screens... -->

    </div>
  </div>
</section>
```

```css
.module-header { margin-bottom: var(--sp-12); }
.module-num {
  font-family: var(--font-display);
  font-size: var(--text-6xl);
  font-weight: 800;
  color: var(--accent);
  opacity: 0.12;
  line-height: 1;
  margin-bottom: var(--sp-2);
  letter-spacing: -0.04em;
}
.module-title {
  font-family: var(--font-display);
  font-size: var(--text-4xl);
  font-weight: 700;
  color: var(--text);
  letter-spacing: -0.02em;
  line-height: var(--leading-tight);
  margin: 0 0 var(--sp-3);
}
.module-subtitle {
  font-size: var(--text-lg);
  color: var(--text-secondary);
  margin: 0;
  max-width: 60ch;
}
.screen {
  margin-bottom: var(--sp-16);
  padding-bottom: var(--sp-16);
  border-bottom: 1px solid var(--border-light);
}
.screen:last-child { border-bottom: none; }
.screen-heading {
  font-family: var(--font-display);
  font-size: var(--text-2xl);
  font-weight: 600;
  color: var(--text);
  margin: 0 0 var(--sp-6);
  letter-spacing: -0.01em;
}
```

---

## Callout Boxes

```html
<!-- Aha! moment -->
<div class="callout aha">
  <div class="callout-icon">💡</div>
  <div class="callout-body">
    <strong>Aha! moment:</strong> The key insight here is...
  </div>
</div>

<!-- Warning / gotcha -->
<div class="callout warning">
  <div class="callout-icon">⚠️</div>
  <div class="callout-body">
    <strong>Watch out:</strong> This is where bugs usually hide...
  </div>
</div>

<!-- AI steering tip — unique to Rikai -->
<div class="callout steering">
  <div class="callout-icon">🎯</div>
  <div class="callout-body">
    <strong>AI steering tip:</strong> When asking Claude to modify this, say...
  </div>
</div>

<!-- Pro tip -->
<div class="callout tip">
  <div class="callout-icon">🔍</div>
  <div class="callout-body">
    <strong>Pro tip:</strong> ...
  </div>
</div>
```

```css
.callout {
  display: flex; align-items: flex-start; gap: var(--sp-4);
  padding: var(--sp-5) var(--sp-6);
  border-radius: var(--radius-md);
  margin: var(--sp-8) 0;
  border-left: 4px solid;
}
.callout-icon { font-size: 1.25rem; line-height: 1.5; flex-shrink: 0; }
.callout-body { font-size: var(--text-base); line-height: var(--leading-normal); }
.callout.aha      { background: var(--accent-light);   border-color: var(--accent);   }
.callout.warning  { background: var(--warning-light);  border-color: var(--warning);  }
.callout.steering { background: var(--info-light);     border-color: var(--info);     }
.callout.tip      { background: var(--success-light);  border-color: var(--success);  }
```

> **New in Rikai — "AI Steering" callout:** When a concept has a direct implication for how to instruct AI better, add a `callout.steering` box. E.g., "AI steering tip: When adding a new API endpoint, tell Claude to add it to `routes/` and register it in `app.ts` — it won't know about the registration step unless you say so."

---

## Scroll Setup (HTML body)

```html
<html>
<body style="margin:0; overflow-x:hidden; scroll-snap-type: y proximity; scroll-behavior: smooth;">
```