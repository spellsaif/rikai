# Rikai — Interactive Elements

Complete implementation patterns for every interactive element. Pick what serves each module's teaching goal. Read this file before building any interactive elements.

---

## Table of Contents
1. [Code ↔ English Translation Block](#1-code--english-translation-block)
2. [Animated Data Flow](#2-animated-data-flow)
3. [Group Chat Animation](#3-group-chat-animation)
4. [Multiple-Choice Quiz](#4-multiple-choice-quiz)
5. [Scenario / Debugging Quiz](#5-scenario--debugging-quiz)
6. [Drag-and-Drop Matching](#6-drag-and-drop-matching)
7. [Architecture Diagram (clickable)](#7-architecture-diagram-clickable)
8. [Annotated File Tree](#8-annotated-file-tree)
9. [Step Cards (numbered sequence)](#9-step-cards-numbered-sequence)
10. [Feature / Pattern Cards](#10-feature--pattern-cards)
11. [Side-by-Side Comparison](#11-side-by-side-comparison)
12. [Glossary Tooltips](#12-glossary-tooltips)
13. [Spot-the-Bug Challenge](#13-spot-the-bug-challenge)
14. [Layer Toggle](#14-layer-toggle)
15. [AI Steering Tip (Callout variant)](#15-ai-steering-tip)

---

## 1. Code ↔ English Translation Block

**The most important teaching element in Rikai.** Real code on the left, plain-English explanation on the right, line by line. Always use verbatim code from the actual codebase.

```html
<div class="xlate reveal">
  <div class="xlate-code">
    <span class="xlate-badge">CODE</span>
    <pre><code>
<span class="cl"><span class="kw">const</span> token = <span class="fn">sign</span>(payload, secret, {</span>
<span class="cl">  <span class="prop">expiresIn</span>: <span class="str">'7d'</span></span>
<span class="cl">});</span>
    </code></pre>
  </div>
  <div class="xlate-english">
    <span class="xlate-badge">PLAIN ENGLISH</span>
    <div class="xlate-lines">
      <p>Create a signed token that proves who this user is...</p>
      <p>...and make it expire after 7 days (so stolen tokens don't last forever)</p>
      <p>The result is a compact string we send back to the browser</p>
    </div>
  </div>
</div>
```

**Syntax highlighting classes** (apply inside `<span>` tags):
- `.kw` — keyword (const, let, return, async, await)
- `.fn` — function name
- `.str` — string value
- `.prop` — object property
- `.num` — number
- `.cmt` — comment (lighter color)
- `.type` — type annotation (TypeScript)

```css
.xlate {
  display: grid;
  grid-template-columns: 1fr 1fr;
  border-radius: var(--radius-md);
  overflow: hidden;
  box-shadow: var(--shadow-md);
  margin: var(--sp-8) 0;
}
.xlate-code {
  background: var(--bg-code);
  color: var(--text-code);
  padding: var(--sp-6);
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  line-height: 1.75;
  position: relative;
  overflow: hidden; /* NO horizontal scroll */
}
.xlate-code pre, .xlate-code code {
  white-space: pre-wrap;
  word-break: break-word;
  overflow-x: hidden;
  margin: 0; padding: 0;
  background: transparent;
}
.xlate-english {
  background: var(--bg-surface-warm);
  padding: var(--sp-6);
  font-size: var(--text-sm);
  line-height: 1.75;
  border-left: 3px solid var(--accent);
}
.xlate-badge {
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  opacity: 0.4;
  display: block;
  margin-bottom: var(--sp-4);
}
.xlate-lines p { margin: 0 0 var(--sp-3); color: var(--text-secondary); }
.xlate-lines p:last-child { margin-bottom: 0; }
/* Syntax colors */
.kw   { color: #CBA6F7; }
.fn   { color: #89DCEB; }
.str  { color: #A6E3A1; }
.prop { color: #89B4FA; }
.num  { color: #FAB387; }
.cmt  { color: #6C7086; font-style: italic; }
.type { color: #F9E2AF; }
/* Responsive */
@media (max-width: 680px) {
  .xlate { grid-template-columns: 1fr; }
  .xlate-english { border-left: none; border-top: 3px solid var(--accent); }
}
```

---

## 2. Animated Data Flow

Shows how data moves through the system step by step. Each step highlights the active actor and the message traveling between them.

```html
<div class="flow-diagram reveal" id="flow-auth">
  <div class="flow-actors">
    <div class="flow-actor" data-id="browser" style="--actor-color: var(--actor-1)">
      <div class="flow-actor-icon">🌐</div>
      <div class="flow-actor-name">Browser</div>
    </div>
    <div class="flow-actor" data-id="api" style="--actor-color: var(--actor-2)">
      <div class="flow-actor-icon">⚙️</div>
      <div class="flow-actor-name">API Server</div>
    </div>
    <div class="flow-actor" data-id="db" style="--actor-color: var(--actor-3)">
      <div class="flow-actor-icon">🗄️</div>
      <div class="flow-actor-name">Database</div>
    </div>
  </div>

  <div class="flow-steps">
    <div class="flow-step" data-from="browser" data-to="api">
      <div class="flow-step-num">1</div>
      <div class="flow-step-label">POST /login with email + password</div>
    </div>
    <div class="flow-step" data-from="api" data-to="db">
      <div class="flow-step-num">2</div>
      <div class="flow-step-label">SELECT * FROM users WHERE email = ?</div>
    </div>
    <div class="flow-step" data-from="db" data-to="api">
      <div class="flow-step-num">3</div>
      <div class="flow-step-label">Return user record + hashed password</div>
    </div>
    <div class="flow-step" data-from="api" data-to="browser">
      <div class="flow-step-num">4</div>
      <div class="flow-step-label">Return signed JWT token</div>
    </div>
  </div>

  <button class="flow-btn" onclick="runFlow('flow-auth')">▶ Trace the Request</button>
</div>
```

```css
.flow-diagram { background: var(--bg-surface); border-radius: var(--radius-lg); padding: var(--sp-8); box-shadow: var(--shadow-md); }
.flow-actors { display: flex; justify-content: space-around; margin-bottom: var(--sp-8); }
.flow-actor { text-align: center; opacity: 0.4; transition: opacity var(--dur-mid), transform var(--dur-mid); }
.flow-actor.active { opacity: 1; transform: scale(1.1); }
.flow-actor-icon { font-size: 2rem; margin-bottom: var(--sp-2); }
.flow-actor-name { font-size: var(--text-xs); font-family: var(--font-mono); text-transform: uppercase; letter-spacing: 0.08em; color: var(--text-muted); }

.flow-steps { display: flex; flex-direction: column; gap: var(--sp-3); }
.flow-step { display: flex; align-items: center; gap: var(--sp-4); padding: var(--sp-3) var(--sp-5); border-radius: var(--radius-sm); background: var(--bg); border: 1.5px solid var(--border); opacity: 0.35; transition: all var(--dur-mid); }
.flow-step.active { opacity: 1; border-color: var(--accent); background: var(--accent-light); transform: translateX(6px); }
.flow-step.done { opacity: 0.6; border-color: var(--success); }
.flow-step-num { width: 28px; height: 28px; border-radius: 50%; background: var(--accent); color: white; font-size: var(--text-xs); font-family: var(--font-mono); font-weight: 600; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
.flow-step-label { font-size: var(--text-sm); color: var(--text); font-family: var(--font-mono); }

.flow-btn { margin-top: var(--sp-6); padding: var(--sp-3) var(--sp-6); background: var(--accent); color: white; border: none; border-radius: var(--radius-full); font-size: var(--text-sm); font-family: var(--font-body); font-weight: 600; cursor: pointer; transition: background var(--dur-fast), transform var(--dur-fast); }
.flow-btn:hover { background: var(--accent-hover); transform: translateY(-1px); }
```

```javascript
function runFlow(id) {
  const diagram = document.getElementById(id);
  const steps = [...diagram.querySelectorAll('.flow-step')];
  const actors = {};
  diagram.querySelectorAll('.flow-actor').forEach(a => { actors[a.dataset.id] = a; });
  const btn = diagram.querySelector('.flow-btn');

  // Reset
  steps.forEach(s => s.classList.remove('active', 'done'));
  Object.values(actors).forEach(a => a.classList.remove('active'));
  btn.disabled = true;

  let i = 0;
  const interval = setInterval(() => {
    if (i > 0) { steps[i - 1].classList.replace('active', 'done'); }
    if (i >= steps.length) {
      clearInterval(interval);
      Object.values(actors).forEach(a => a.classList.remove('active'));
      btn.disabled = false;
      btn.textContent = '↺ Run Again';
      return;
    }
    const step = steps[i];
    step.classList.add('active');
    Object.values(actors).forEach(a => a.classList.remove('active'));
    if (actors[step.dataset.from]) actors[step.dataset.from].classList.add('active');
    if (actors[step.dataset.to]) actors[step.dataset.to].classList.add('active');
    i++;
  }, 900);
}
```

---

## 3. Group Chat Animation

Makes component communication feel human. Components send each other messages in an iMessage-style chat. Plays automatically when scrolled into view.

```html
<div class="chat reveal" id="chat-request">
  <!-- messages are injected by JS -->
</div>
```

```javascript
const chatScripts = {
  'chat-request': [
    { from: 'Frontend', msg: 'Hey API, user wants their dashboard data', align: 'left',  color: 'var(--actor-1)' },
    { from: 'API',      msg: 'On it — let me check with the database',   align: 'right', color: 'var(--actor-2)' },
    { from: 'Database', msg: 'Here are 47 records for user #8821',        align: 'left',  color: 'var(--actor-3)' },
    { from: 'API',      msg: 'Got it. Filtering to the last 30 days...',  align: 'right', color: 'var(--actor-2)' },
    { from: 'API',      msg: 'Done. Here\'s the formatted response 📦',   align: 'right', color: 'var(--actor-2)' },
    { from: 'Frontend', msg: 'Perfect, rendering the charts now ✨',      align: 'left',  color: 'var(--actor-1)' },
  ]
};

function playChat(id) {
  const container = document.getElementById(id);
  const script = chatScripts[id];
  if (!script || container.dataset.played === 'true') return;
  container.dataset.played = 'true';
  container.innerHTML = '';

  script.forEach((msg, i) => {
    setTimeout(() => {
      const bubble = document.createElement('div');
      bubble.className = `chat-msg ${msg.align}`;
      bubble.innerHTML = `
        <div class="chat-name" style="color:${msg.color}">${msg.from}</div>
        <div class="chat-bubble" style="--bubble-color:${msg.color}">${msg.msg}</div>
      `;
      bubble.style.opacity = '0';
      bubble.style.transform = msg.align === 'left' ? 'translateX(-12px)' : 'translateX(12px)';
      container.appendChild(bubble);
      requestAnimationFrame(() => {
        bubble.style.transition = 'opacity 300ms ease, transform 300ms ease';
        bubble.style.opacity = '1';
        bubble.style.transform = 'translateX(0)';
      });
      container.scrollTop = container.scrollHeight;
    }, i * 700);
  });
}

// Trigger on scroll into view
const chatObserver = new IntersectionObserver((entries) => {
  entries.forEach(e => { if (e.isIntersecting) playChat(e.target.id); });
}, { threshold: 0.3 });
document.querySelectorAll('.chat').forEach(c => chatObserver.observe(c));
```

```css
.chat {
  background: var(--bg-surface);
  border-radius: var(--radius-lg);
  padding: var(--sp-6);
  box-shadow: var(--shadow-md);
  min-height: 260px;
  max-height: 400px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: var(--sp-4);
  scroll-behavior: smooth;
}
.chat-msg { display: flex; flex-direction: column; max-width: 72%; }
.chat-msg.right { align-self: flex-end; align-items: flex-end; }
.chat-msg.left  { align-self: flex-start; align-items: flex-start; }
.chat-name { font-size: var(--text-xs); font-family: var(--font-mono); font-weight: 600; margin-bottom: 3px; text-transform: uppercase; letter-spacing: 0.05em; }
.chat-bubble {
  padding: var(--sp-3) var(--sp-4);
  border-radius: 18px;
  font-size: var(--text-sm);
  line-height: 1.5;
  color: var(--text);
  background: var(--bg);
  border: 1.5px solid var(--border);
}
.chat-msg.right .chat-bubble {
  background: color-mix(in srgb, var(--bubble-color) 12%, var(--bg-surface));
  border-color: color-mix(in srgb, var(--bubble-color) 30%, var(--border));
}
```

---

## 4. Multiple-Choice Quiz

```html
<div class="quiz reveal" id="quiz-m2">
  <div class="quiz-q" data-correct="b">
    <p class="quiz-question">You want to add a "export to CSV" feature. Based on what you just learned, which layer should own this logic?</p>
    <div class="quiz-options">
      <button class="quiz-opt" data-val="a" onclick="pickOpt(this)">The frontend — it's a UI feature</button>
      <button class="quiz-opt" data-val="b" onclick="pickOpt(this)">The service layer — it's business logic</button>
      <button class="quiz-opt" data-val="c" onclick="pickOpt(this)">The database — that's where the data is</button>
    </div>
    <div class="quiz-feedback" data-correct="Service layer is right. The export logic (formatting rows, applying filters, building the CSV) is business logic that belongs in the service layer — not in the UI and not in the database. When you ask AI to add it, say: 'add a generateCsvExport() function to the orders service.'" data-wrong="Think about where the *logic* lives. The UI shows things. The database stores things. The service layer *transforms and processes* things — that's where CSV generation belongs."></div>
  </div>

  <button class="quiz-submit" onclick="checkQuiz('quiz-m2')">Check Answer</button>
</div>
```

```css
.quiz { background: var(--bg-surface); border-radius: var(--radius-lg); padding: var(--sp-8); box-shadow: var(--shadow-md); }
.quiz-question { font-size: var(--text-base); font-weight: 600; color: var(--text); margin: 0 0 var(--sp-6); line-height: var(--leading-snug); }
.quiz-options { display: flex; flex-direction: column; gap: var(--sp-3); margin-bottom: var(--sp-6); }
.quiz-opt {
  display: flex; align-items: center; gap: var(--sp-3);
  padding: var(--sp-4) var(--sp-5);
  border: 2px solid var(--border);
  border-radius: var(--radius-md);
  background: var(--bg);
  cursor: pointer;
  text-align: left;
  font-size: var(--text-base);
  font-family: var(--font-body);
  color: var(--text);
  transition: border-color var(--dur-fast), background var(--dur-fast), transform var(--dur-fast);
}
.quiz-opt:hover:not(:disabled) { border-color: var(--accent-muted); background: var(--accent-light); transform: translateX(3px); }
.quiz-opt.chosen  { border-color: var(--accent); background: var(--accent-light); }
.quiz-opt.correct { border-color: var(--success); background: var(--success-light); }
.quiz-opt.wrong   { border-color: var(--error); background: var(--error-light); }

.quiz-feedback {
  padding: 0; max-height: 0; overflow: hidden; opacity: 0;
  transition: max-height var(--dur-mid) var(--ease-out), opacity var(--dur-mid), padding var(--dur-mid);
  border-radius: var(--radius-sm);
  font-size: var(--text-sm);
  line-height: var(--leading-normal);
}
.quiz-feedback.show {
  padding: var(--sp-4) var(--sp-5);
  max-height: 200px;
  opacity: 1;
}
.quiz-feedback.correct-fb { background: var(--success-light); color: var(--success); }
.quiz-feedback.wrong-fb   { background: var(--error-light);   color: var(--error);   }

.quiz-submit {
  padding: var(--sp-3) var(--sp-7);
  background: var(--accent);
  color: white; border: none;
  border-radius: var(--radius-full);
  font-size: var(--text-sm); font-family: var(--font-body); font-weight: 600;
  cursor: pointer;
  transition: background var(--dur-fast), transform var(--dur-fast);
}
.quiz-submit:hover { background: var(--accent-hover); transform: translateY(-1px); }
.quiz-submit:disabled { opacity: 0.4; cursor: default; transform: none; }
```

```javascript
function pickOpt(btn) {
  const block = btn.closest('.quiz-q');
  block.querySelectorAll('.quiz-opt').forEach(o => o.classList.remove('chosen'));
  btn.classList.add('chosen');
}

function checkQuiz(id) {
  const quiz = document.getElementById(id);
  const blocks = [...quiz.querySelectorAll('.quiz-q')];
  let allAnswered = true;

  blocks.forEach(block => {
    const chosen = block.querySelector('.quiz-opt.chosen');
    const feedback = block.querySelector('.quiz-feedback');
    if (!chosen) { allAnswered = false; return; }

    const isCorrect = chosen.dataset.val === block.dataset.correct;
    block.querySelectorAll('.quiz-opt').forEach(o => { o.disabled = true; o.style.cursor = 'default'; });

    if (isCorrect) {
      chosen.classList.add('correct');
      feedback.textContent = '✓ ' + feedback.dataset.correct;
      feedback.className = 'quiz-feedback show correct-fb';
    } else {
      chosen.classList.add('wrong');
      block.querySelector(`[data-val="${block.dataset.correct}"]`).classList.add('correct');
      feedback.textContent = feedback.dataset.wrong;
      feedback.className = 'quiz-feedback show wrong-fb';
    }
  });

  if (!allAnswered) return;
  quiz.querySelector('.quiz-submit').disabled = true;
}
```

---

## 5. Scenario / Debugging Quiz

A richer format: presents a realistic situation and asks the learner to reason about it.

```html
<div class="scenario reveal">
  <div class="scenario-badge">🔍 Debugging Scenario</div>
  <p class="scenario-situation">
    A user reports that their saved preferences disappear every time they close the browser. You check the code and see preferences are saved with <code>sessionStorage.setItem('prefs', data)</code>. What's the problem, and what's the fix?
  </p>
  <div class="scenario-options">
    <button class="scenario-opt" onclick="checkScenario(this, false)" data-explain="The data is getting saved correctly — sessionStorage.setItem works. The problem is *where* it's saved.">The data isn't being saved at all</button>
    <button class="scenario-opt" onclick="checkScenario(this, true)"  data-explain="Exactly. sessionStorage only lasts for the current browser tab session. The fix is switching to localStorage, which persists across sessions. When steering AI: 'change sessionStorage to localStorage for user preferences.'">sessionStorage clears when the tab closes</button>
    <button class="scenario-opt" onclick="checkScenario(this, false)" data-explain="The server isn't involved here — sessionStorage is browser-only. This is a client-side storage issue.">The server isn't sending the preferences back</button>
  </div>
  <div class="scenario-feedback" id="sf-1"></div>
</div>
```

```css
.scenario { background: var(--bg-surface); border-radius: var(--radius-lg); padding: var(--sp-8); box-shadow: var(--shadow-md); }
.scenario-badge { display: inline-block; background: var(--info-light); color: var(--info); font-size: var(--text-xs); font-family: var(--font-mono); font-weight: 600; text-transform: uppercase; letter-spacing: 0.08em; padding: var(--sp-1) var(--sp-3); border-radius: var(--radius-full); margin-bottom: var(--sp-4); }
.scenario-situation { font-size: var(--text-base); line-height: var(--leading-loose); color: var(--text); border-left: 3px solid var(--info); padding-left: var(--sp-5); margin: 0 0 var(--sp-6); }
.scenario-options { display: flex; flex-direction: column; gap: var(--sp-3); }
.scenario-opt { padding: var(--sp-4) var(--sp-5); border: 2px solid var(--border); border-radius: var(--radius-md); background: var(--bg); text-align: left; font-size: var(--text-base); font-family: var(--font-body); cursor: pointer; transition: all var(--dur-fast); }
.scenario-opt:hover:not(:disabled) { border-color: var(--accent-muted); transform: translateX(4px); }
.scenario-opt.correct { border-color: var(--success); background: var(--success-light); }
.scenario-opt.wrong { border-color: var(--error); background: var(--error-light); }
.scenario-feedback { margin-top: var(--sp-5); padding: var(--sp-4) var(--sp-5); border-radius: var(--radius-md); font-size: var(--text-sm); line-height: var(--leading-normal); display: none; }
.scenario-feedback.show { display: block; }
.scenario-feedback.correct-fb { background: var(--success-light); color: var(--success); }
.scenario-feedback.wrong-fb { background: var(--error-light); color: var(--error); }
```

```javascript
function checkScenario(btn, isCorrect) {
  const container = btn.closest('.scenario');
  container.querySelectorAll('.scenario-opt').forEach(o => { o.disabled = true; o.style.cursor = 'default'; });
  btn.classList.add(isCorrect ? 'correct' : 'wrong');
  if (!isCorrect) container.querySelector('.scenario-opt[onclick*="true"]').classList.add('correct');
  const fb = container.querySelector('.scenario-feedback');
  fb.textContent = (isCorrect ? '✓ ' : '') + btn.dataset.explain;
  fb.className = `scenario-feedback show ${isCorrect ? 'correct-fb' : 'wrong-fb'}`;
}
```

---

## 6. Drag-and-Drop Matching

Match concepts to descriptions. Touch-friendly.

```html
<div class="dnd reveal" id="dnd-layers">
  <div class="dnd-chips" id="dnd-chips-layers">
    <div class="dnd-chip" draggable="true" data-id="routes">Routes</div>
    <div class="dnd-chip" draggable="true" data-id="services">Services</div>
    <div class="dnd-chip" draggable="true" data-id="models">Models</div>
  </div>
  <div class="dnd-targets">
    <div class="dnd-target" data-correct="routes">
      <div class="dnd-target-label">Receives HTTP requests and calls the right function</div>
      <div class="dnd-drop-zone" data-target-id="routes-zone">Drop here</div>
    </div>
    <div class="dnd-target" data-correct="services">
      <div class="dnd-target-label">Contains the business logic and rules</div>
      <div class="dnd-drop-zone" data-target-id="services-zone">Drop here</div>
    </div>
    <div class="dnd-target" data-correct="models">
      <div class="dnd-target-label">Defines the shape of data in the database</div>
      <div class="dnd-drop-zone" data-target-id="models-zone">Drop here</div>
    </div>
  </div>
  <button onclick="checkDnd('dnd-layers')" class="quiz-submit" style="margin-top: var(--sp-6)">Check Matches</button>
</div>
```

```css
.dnd-chips { display: flex; gap: var(--sp-3); flex-wrap: wrap; margin-bottom: var(--sp-6); }
.dnd-chip {
  padding: var(--sp-2) var(--sp-5);
  background: var(--accent); color: white;
  border-radius: var(--radius-full);
  font-family: var(--font-mono); font-size: var(--text-sm); font-weight: 600;
  cursor: grab; user-select: none;
  transition: transform var(--dur-fast), box-shadow var(--dur-fast);
}
.dnd-chip:active { cursor: grabbing; transform: scale(1.05); }
.dnd-chip.dragging { opacity: 0.5; }
.dnd-targets { display: flex; flex-direction: column; gap: var(--sp-4); }
.dnd-target { display: flex; align-items: center; gap: var(--sp-4); padding: var(--sp-4); background: var(--bg); border-radius: var(--radius-md); border: 1.5px solid var(--border); }
.dnd-target-label { flex: 1; font-size: var(--text-sm); color: var(--text-secondary); }
.dnd-drop-zone {
  min-width: 100px; min-height: 38px;
  border: 2px dashed var(--border);
  border-radius: var(--radius-sm);
  display: flex; align-items: center; justify-content: center;
  font-size: var(--text-xs); color: var(--text-muted); font-family: var(--font-mono);
  transition: border-color var(--dur-fast), background var(--dur-fast);
}
.dnd-drop-zone.drag-over { border-color: var(--accent); background: var(--accent-light); }
.dnd-drop-zone.correct { border-color: var(--success); background: var(--success-light); color: var(--success); }
.dnd-drop-zone.wrong { border-color: var(--error); background: var(--error-light); color: var(--error); }
```

```javascript
(function() {
  let dragging = null;

  document.addEventListener('dragstart', e => {
    if (!e.target.classList.contains('dnd-chip')) return;
    dragging = e.target;
    e.target.classList.add('dragging');
    e.dataTransfer.setData('text/plain', e.target.dataset.id);
  });
  document.addEventListener('dragend', e => {
    if (dragging) dragging.classList.remove('dragging');
    dragging = null;
    document.querySelectorAll('.dnd-drop-zone').forEach(z => z.classList.remove('drag-over'));
  });
  document.addEventListener('dragover', e => {
    if (e.target.classList.contains('dnd-drop-zone')) { e.preventDefault(); e.target.classList.add('drag-over'); }
  });
  document.addEventListener('dragleave', e => {
    if (e.target.classList.contains('dnd-drop-zone')) e.target.classList.remove('drag-over');
  });
  document.addEventListener('drop', e => {
    if (!e.target.classList.contains('dnd-drop-zone')) return;
    e.preventDefault();
    const zone = e.target;
    zone.classList.remove('drag-over');
    zone.dataset.placed = e.dataTransfer.getData('text/plain');
    const chip = document.querySelector(`.dnd-chip[data-id="${zone.dataset.placed}"]`);
    if (chip) { zone.textContent = chip.textContent; chip.style.visibility = 'hidden'; }
  });

  window.checkDnd = function(id) {
    const container = document.getElementById(id);
    container.querySelectorAll('.dnd-target').forEach(target => {
      const zone = target.querySelector('.dnd-drop-zone');
      zone.classList.remove('correct', 'wrong');
      if (zone.dataset.placed === target.dataset.correct) zone.classList.add('correct');
      else if (zone.dataset.placed) zone.classList.add('wrong');
    });
  };
})();
```

---

## 7. Architecture Diagram (clickable)

Click a component to learn what it does and what it connects to.

```html
<div class="arch-diagram reveal" id="arch-main">
  <svg viewBox="0 0 700 380" xmlns="http://www.w3.org/2000/svg" style="width:100%;height:auto">
    <!-- Boxes drawn with inline SVG, IDs match data-id below -->
    <rect x="20" y="160" width="140" height="60" rx="10" fill="var(--actor-1)" opacity="0.15" stroke="var(--actor-1)" stroke-width="2"/>
    <text x="90" y="195" text-anchor="middle" font-family="var(--font-display)" font-weight="600" font-size="14" fill="var(--actor-1)" style="cursor:pointer" onclick="showArch('frontend')">Frontend</text>
    <!-- More components + arrows... -->
  </svg>
  <div class="arch-info" id="arch-info">
    <p class="arch-info-placeholder">Click any component to learn what it does →</p>
  </div>
</div>
```

```javascript
const archData = {
  'frontend': {
    title: 'Frontend (React)',
    owns: 'Everything the user sees and interacts with',
    talksto: 'API Server (via fetch/axios)',
    tip: 'When adding a new page or UI feature, this is always your starting point. Tell AI: "add this to the frontend components folder."'
  },
  'api': {
    title: 'API Server (Express)',
    owns: 'The rules for what users are allowed to do',
    talksto: 'Database, external APIs, Frontend (sends responses)',
    tip: 'When adding a new feature that needs data, you\'ll add a route here. Tell AI: "add a GET /api/... endpoint to the routes folder."'
  }
};

window.showArch = function(id) {
  const d = archData[id];
  if (!d) return;
  document.getElementById('arch-info').innerHTML = `
    <strong class="arch-info-title">${d.title}</strong>
    <p><strong>Owns:</strong> ${d.owns}</p>
    <p><strong>Talks to:</strong> ${d.talksto}</p>
    <div class="callout steering" style="margin:var(--sp-4) 0 0"><div class="callout-icon">🎯</div><div class="callout-body"><strong>AI steering tip:</strong> ${d.tip}</div></div>
  `;
};
```

---

## 8. Annotated File Tree

Visual representation of the project structure with explanations.

```html
<div class="file-tree reveal">
  <div class="ft-item ft-folder open">
    <span class="ft-icon">📁</span>
    <span class="ft-name">src/</span>
  </div>
  <div class="ft-children">
    <div class="ft-item ft-folder" onclick="ftToggle(this)">
      <span class="ft-icon">📁</span>
      <span class="ft-name">components/</span>
      <span class="ft-desc">React UI components — what users see</span>
    </div>
    <div class="ft-item ft-file important" onclick="ftHighlight(this)">
      <span class="ft-icon">📄</span>
      <span class="ft-name">App.tsx</span>
      <span class="ft-desc">Entry point — the "main stage"</span>
    </div>
    <div class="ft-item ft-file" onclick="ftHighlight(this)">
      <span class="ft-icon">📄</span>
      <span class="ft-name">api.ts</span>
      <span class="ft-desc">All HTTP calls to the backend live here</span>
    </div>
  </div>
</div>
```

```css
.file-tree { background: var(--bg-code); border-radius: var(--radius-md); padding: var(--sp-5); font-family: var(--font-mono); font-size: var(--text-sm); }
.ft-item { display: flex; align-items: center; gap: var(--sp-3); padding: var(--sp-2) var(--sp-3); border-radius: var(--radius-sm); cursor: pointer; transition: background var(--dur-fast); }
.ft-item:hover { background: rgba(255,255,255,0.06); }
.ft-item.active { background: rgba(255,255,255,0.1); }
.ft-item.important .ft-name { color: var(--accent); font-weight: 600; }
.ft-icon { font-size: 1rem; }
.ft-name { color: var(--text-code); }
.ft-desc { color: #6C7086; font-size: var(--text-xs); margin-left: auto; }
.ft-folder .ft-name { color: #CBA6F7; }
.ft-children { padding-left: var(--sp-6); border-left: 1px solid rgba(255,255,255,0.08); margin-left: var(--sp-4); }
```

---

## 9. Step Cards (numbered sequence)

For explaining a process as ordered steps.

```html
<div class="steps stagger-children reveal">
  <div class="step reveal">
    <div class="step-num">1</div>
    <div class="step-body">
      <h4 class="step-title">Request arrives</h4>
      <p class="step-text">The browser sends a POST request to /api/checkout with the cart contents</p>
    </div>
  </div>
  <div class="step reveal">
    <div class="step-num">2</div>
    <div class="step-body">
      <h4 class="step-title">Validation</h4>
      <p class="step-text">The middleware checks: is the user logged in? Is the cart non-empty?</p>
    </div>
  </div>
  <!-- etc -->
</div>
```

```css
.steps { display: flex; flex-direction: column; gap: var(--sp-4); position: relative; }
.steps::before { content: ''; position: absolute; left: 20px; top: 40px; bottom: 40px; width: 2px; background: var(--border); }
.step { display: flex; gap: var(--sp-5); align-items: flex-start; }
.step-num {
  width: 40px; height: 40px; border-radius: 50%;
  background: var(--accent); color: white;
  font-family: var(--font-display); font-weight: 700; font-size: var(--text-base);
  display: flex; align-items: center; justify-content: center;
  flex-shrink: 0; position: relative; z-index: 1;
  box-shadow: 0 0 0 4px var(--bg);
}
.step-title { font-family: var(--font-display); font-weight: 600; font-size: var(--text-base); margin: 0 0 var(--sp-2); }
.step-text { font-size: var(--text-sm); color: var(--text-secondary); margin: 0; line-height: var(--leading-normal); }
```

---

## 10. Feature / Pattern Cards

For showing a set of concepts side by side.

```html
<div class="cards stagger-children">
  <div class="card reveal">
    <div class="card-icon">🔒</div>
    <h4 class="card-title">Authentication</h4>
    <p class="card-text">Proves who you are. Uses JWT tokens stored in memory.</p>
  </div>
  <div class="card reveal">
    <div class="card-icon">🛡️</div>
    <h4 class="card-title">Authorization</h4>
    <p class="card-text">Checks what you're allowed to do. Role-based: admin vs user.</p>
  </div>
</div>
```

```css
.cards { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: var(--sp-5); margin: var(--sp-6) 0; }
.card { background: var(--bg-surface); border-radius: var(--radius-md); padding: var(--sp-6); border: 1.5px solid var(--border); transition: transform var(--dur-mid), box-shadow var(--dur-mid); }
.card:hover { transform: translateY(-3px); box-shadow: var(--shadow-md); }
.card-icon { font-size: 1.75rem; margin-bottom: var(--sp-4); }
.card-title { font-family: var(--font-display); font-weight: 600; font-size: var(--text-base); margin: 0 0 var(--sp-2); }
.card-text { font-size: var(--text-sm); color: var(--text-secondary); margin: 0; line-height: var(--leading-normal); }
```

---

## 11. Side-by-Side Comparison

```html
<div class="compare reveal">
  <div class="compare-col">
    <div class="compare-head bad">❌ Without caching</div>
    <ul class="compare-list">
      <li>Every request hits the database</li>
      <li>200ms+ response times</li>
      <li>Database becomes the bottleneck</li>
    </ul>
  </div>
  <div class="compare-col">
    <div class="compare-head good">✓ With caching</div>
    <ul class="compare-list">
      <li>Repeat requests served from memory</li>
      <li>&lt;5ms response times</li>
      <li>Database only queried for fresh data</li>
    </ul>
  </div>
</div>
```

```css
.compare { display: grid; grid-template-columns: 1fr 1fr; gap: var(--sp-5); }
.compare-col { background: var(--bg-surface); border-radius: var(--radius-md); padding: var(--sp-6); border: 1.5px solid var(--border); }
.compare-head { font-family: var(--font-display); font-weight: 700; font-size: var(--text-base); margin-bottom: var(--sp-4); padding-bottom: var(--sp-3); border-bottom: 1.5px solid var(--border); }
.compare-head.bad { color: var(--error); }
.compare-head.good { color: var(--success); }
.compare-list { margin: 0; padding-left: var(--sp-5); display: flex; flex-direction: column; gap: var(--sp-2); }
.compare-list li { font-size: var(--text-sm); color: var(--text-secondary); line-height: var(--leading-normal); }
@media (max-width: 600px) { .compare { grid-template-columns: 1fr; } }
```

---

## 12. Glossary Tooltips

**CRITICAL IMPLEMENTATION:** Must use `position: fixed` and append to `document.body`. Any other approach clips inside overflow containers.

```html
<!-- Mark terms inline like this: -->
<p>The server uses a <span class="term" data-def="A small signed string the server creates to prove who you are — like a wristband at a concert. You show it on every request so the server doesn't have to look you up in the database each time. When talking to AI: 'store the JWT in memory, not localStorage.'">JWT token</span> to authenticate requests.</p>
```

```css
.term {
  border-bottom: 1.5px dashed var(--accent-muted);
  cursor: pointer;
  color: inherit;
  transition: color var(--dur-fast);
}
.term:hover { color: var(--accent); }

/* The floating tooltip — injected by JS */
.tooltip-popup {
  position: fixed;
  z-index: 9999;
  background: var(--text);
  color: #F5F2ED;
  padding: var(--sp-3) var(--sp-5);
  border-radius: var(--radius-md);
  font-size: var(--text-sm);
  font-family: var(--font-body);
  line-height: var(--leading-normal);
  max-width: 320px;
  box-shadow: var(--shadow-lg);
  pointer-events: none;
  opacity: 0;
  transform: translateY(4px);
  transition: opacity var(--dur-fast), transform var(--dur-fast);
}
.tooltip-popup.visible {
  opacity: 1;
  transform: translateY(0);
}
```

```javascript
(function() {
  let popup = null;

  function getOrCreatePopup() {
    if (!popup) {
      popup = document.createElement('div');
      popup.className = 'tooltip-popup';
      document.body.appendChild(popup);
    }
    return popup;
  }

  document.addEventListener('mouseover', (e) => {
    const term = e.target.closest('.term');
    if (!term) return;

    const tip = getOrCreatePopup();
    tip.textContent = term.dataset.def;

    const rect = term.getBoundingClientRect();
    const tipWidth = 320;
    let left = rect.left;
    let top = rect.bottom + 8;

    // Keep within viewport
    if (left + tipWidth > window.innerWidth - 16) left = window.innerWidth - tipWidth - 16;
    if (left < 8) left = 8;
    if (top + 100 > window.innerHeight) top = rect.top - 8 - tip.offsetHeight;

    tip.style.left = left + 'px';
    tip.style.top = top + 'px';

    requestAnimationFrame(() => tip.classList.add('visible'));
  });

  document.addEventListener('mouseout', (e) => {
    const term = e.target.closest('.term');
    if (!term) return;
    if (popup) popup.classList.remove('visible');
  });

  // Mobile: tap to toggle
  document.addEventListener('click', (e) => {
    const term = e.target.closest('.term');
    if (!term) return;
    e.stopPropagation();
    const tip = getOrCreatePopup();
    if (tip.classList.contains('visible')) {
      tip.classList.remove('visible');
    } else {
      tip.textContent = term.dataset.def;
      const rect = term.getBoundingClientRect();
      let left = rect.left;
      let top = rect.bottom + 8;
      if (left + 320 > window.innerWidth - 16) left = window.innerWidth - 336;
      if (left < 8) left = 8;
      tip.style.left = left + 'px';
      tip.style.top = top + 'px';
      requestAnimationFrame(() => tip.classList.add('visible'));
    }
  });

  document.addEventListener('click', (e) => {
    if (!e.target.closest('.term') && popup) popup.classList.remove('visible');
  });
})();
```

---

## 13. Spot-the-Bug Challenge

Show real code with a subtle bug. Let the learner identify it.

```html
<div class="bug-challenge reveal">
  <div class="bug-header">🐛 Spot the Bug</div>
  <p class="bug-prompt">This function is supposed to return all orders for a user, but it sometimes returns everyone's orders. What's wrong?</p>
  <div class="xlate">
    <div class="xlate-code">
      <span class="xlate-badge">CODE</span>
      <pre><code>
<span class="cl"><span class="kw">async function</span> <span class="fn">getOrders</span>(userId) {</span>
<span class="cl">  <span class="kw">const</span> orders = <span class="kw">await</span> db.<span class="fn">query</span>(</span>
<span class="cl">    <span class="str">'SELECT * FROM orders'</span></span>
<span class="cl">  );</span>
<span class="cl">  <span class="kw">return</span> orders;</span>
<span class="cl">}</span>
      </code></pre>
    </div>
    <div class="xlate-english">
      <span class="xlate-badge">WHAT IT SHOULD DO</span>
      <div class="xlate-lines">
        <p>Accept a user ID as input...</p>
        <p>Query the database for orders...</p>
        <p>...but ONLY for this specific user</p>
        <p>Return just those orders</p>
      </div>
    </div>
  </div>
  <button class="quiz-submit" onclick="revealBug(this)">Reveal the Bug</button>
  <div class="bug-reveal" style="display:none">
    <div class="callout warning">
      <div class="callout-icon">⚠️</div>
      <div class="callout-body">The SQL query is missing a <code>WHERE user_id = ?</code> clause. It fetches ALL orders, then ignores the <code>userId</code> parameter entirely. The fix: <code>'SELECT * FROM orders WHERE user_id = ?', [userId]</code>. This is a data leak — every user could see everyone's orders. <strong>AI steering tip:</strong> When asking AI to add filtered queries, always explicitly say "filter by [field]" — it won't add the WHERE clause unless you ask.</div>
    </div>
  </div>
</div>
```

```javascript
window.revealBug = function(btn) {
  btn.style.display = 'none';
  btn.nextElementSibling.style.display = 'block';
};
```

---

## 14. Layer Toggle

Let learners toggle between abstraction layers (e.g., HTTP → Express → Business logic).

```html
<div class="layer-toggle reveal">
  <div class="lt-tabs">
    <button class="lt-tab active" onclick="showLayer(this, 'user')" data-layer="user">👤 User's View</button>
    <button class="lt-tab" onclick="showLayer(this, 'network')" data-layer="network">🌐 Network</button>
    <button class="lt-tab" onclick="showLayer(this, 'code')" data-layer="code">⚙️ Code</button>
  </div>
  <div class="lt-panels">
    <div class="lt-panel active" data-layer="user">
      <p>You click "Save" and see a green "Saved!" message appear.</p>
    </div>
    <div class="lt-panel" data-layer="network">
      <p>A POST request goes to /api/settings with your preferences as JSON. The server returns <code>{"ok": true}</code>.</p>
    </div>
    <div class="lt-panel" data-layer="code">
      <p>The React component calls <code>api.saveSettings(data)</code>. That runs a fetch. The Express route calls <code>SettingsService.update(userId, data)</code>. Prisma writes to the DB.</p>
    </div>
  </div>
</div>
```

```css
.lt-tabs { display: flex; gap: var(--sp-2); margin-bottom: var(--sp-5); border-bottom: 1.5px solid var(--border); padding-bottom: var(--sp-3); }
.lt-tab { padding: var(--sp-2) var(--sp-5); border-radius: var(--radius-full); background: transparent; border: 1.5px solid var(--border); font-size: var(--text-sm); font-family: var(--font-body); cursor: pointer; transition: all var(--dur-fast); }
.lt-tab.active { background: var(--accent); color: white; border-color: var(--accent); }
.lt-panel { display: none; font-size: var(--text-base); line-height: var(--leading-loose); color: var(--text); }
.lt-panel.active { display: block; }
```

```javascript
window.showLayer = function(tab, layer) {
  const container = tab.closest('.layer-toggle');
  container.querySelectorAll('.lt-tab').forEach(t => t.classList.remove('active'));
  container.querySelectorAll('.lt-panel').forEach(p => p.classList.remove('active'));
  tab.classList.add('active');
  container.querySelector(`.lt-panel[data-layer="${layer}"]`).classList.add('active');
};
```

---

## 15. AI Steering Tip

A dedicated callout type unique to Rikai. Use whenever understanding a concept changes how the learner would instruct AI.

```html
<div class="callout steering">
  <div class="callout-icon">🎯</div>
  <div class="callout-body">
    <strong>AI steering tip:</strong>
    Now that you know the service layer owns business logic, when adding features tell Claude:
    <em>"Add a <code>calculateDiscount()</code> method to the OrderService class in <code>services/order.service.ts</code>."</em>
    This is much more effective than "add a discount feature" — Claude won't have to guess where it goes.
  </div>
</div>
```

These callouts are the beating heart of Rikai's practical value. Every architectural concept should translate into one concrete "when you talk to AI, say it like this" instruction.