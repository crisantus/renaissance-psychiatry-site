# Responsive Renaissance Psychiatry Site Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a maintainable, standalone Renaissance Psychiatry landing page that preserves the approved visual direction, works across phone through desktop widths, and uses stable matching Unsplash photography.

**Architecture:** A single semantic HTML document owns the content, component styling, responsive breakpoints, and small progressive-enhancement script. A dependency-free browser harness loads the real page in an iframe, exercises its rendered DOM and interactions, and reports pass/fail results before responsive visual checks.

**Tech Stack:** HTML5, CSS3, vanilla JavaScript, browser-native test harness, Unsplash CDN images

## Global Constraints

- Create `renaissance-psychiatry-responsive.html` in the project root; do not alter the original Downloads export.
- Keep the page front-end-only and dependency-free.
- Preserve the navy, blue, aqua, white, and pale-blue palette and Plus Jakarta Sans typographic character.
- Use specific `images.unsplash.com` URLs, descriptive alt text, lazy loading below the fold, and coherent image-loading backgrounds.
- Below approximately 900px, use an accessible collapsible mobile menu; at phone widths, stack content and avoid horizontal overflow.
- Preserve the supplied placeholder business details where production data is not verified.
- Respect keyboard access, visible focus, semantic landmarks, and `prefers-reduced-motion`.

---

### Task 1: Add the real-browser acceptance harness

**Files:**
- Create: `tests/site-browser-test.html`
- Test: `tests/site-browser-test.html`

**Interfaces:**
- Consumes: the rendered document at `../renaissance-psychiatry-responsive.html`
- Produces: visible pass/fail rows in `#results` and a summary in `#summary` after exercising the real page DOM

- [ ] **Step 1: Write the browser harness**

```html
<iframe id="site" src="../renaissance-psychiatry-responsive.html" title="Site under test"></iframe>
<ol id="results"></ol>
<p id="summary">Waiting for site…</p>
<script>
  const tests = [];
  const check = (name, fn) => tests.push({ name, fn });
  const assert = (condition, message) => { if (!condition) throw new Error(message); };

  check('renders required landmarks and sections', doc => {
    assert(doc.querySelector('header'), 'missing header');
    assert(doc.querySelector('main'), 'missing main');
    assert(doc.querySelector('footer'), 'missing footer');
    ['top','about','services','conditions','providers','patients','book','contact']
      .forEach(id => assert(doc.getElementById(id), `missing #${id}`));
  });

  check('uses accessible stable photographs', doc => {
    const images = [...doc.images];
    assert(images.length >= 5, 'expected at least five photographs');
    images.forEach(image => {
      assert(image.src.startsWith('https://images.unsplash.com/'), 'image is not a stable Unsplash URL');
      assert(image.alt.trim(), 'image has no alt text');
    });
  });
</script>
```

- [ ] **Step 2: Serve and open the harness to verify RED**

Run: `python3 -m http.server 4173`

Open: `http://127.0.0.1:4173/tests/site-browser-test.html`

Expected: FAIL because `renaissance-psychiatry-responsive.html` returns 404 and required landmarks are missing.

- [ ] **Step 3: Add real interaction checks**

```js
check('opens and closes the mobile menu', doc => {
  const button = doc.querySelector('#menu-toggle');
  const menu = doc.querySelector('#mobile-menu');
  assert(button && menu, 'mobile menu controls missing');
  button.click();
  assert(button.getAttribute('aria-expanded') === 'true' && !menu.hidden, 'menu did not open');
  doc.dispatchEvent(new doc.defaultView.KeyboardEvent('keydown', { key:'Escape', bubbles:true }));
  assert(button.getAttribute('aria-expanded') === 'false' && menu.hidden, 'Escape did not close menu');
});

check('switches care tabs and toggles an FAQ', doc => {
  const tabs = [...doc.querySelectorAll('[role="tab"]')];
  assert(tabs.length === 2, 'care tabs missing');
  tabs[1].click();
  assert(tabs[1].getAttribute('aria-selected') === 'true', 'telehealth tab did not activate');
  const faq = doc.querySelector('.faq-question');
  faq.click();
  assert(faq.getAttribute('aria-expanded') === 'true', 'FAQ did not open');
});
```

- [ ] **Step 4: Complete the harness runner and retain the expected failure**

On iframe load, run every check against `iframe.contentDocument`, append `.pass` or `.fail` results, and set `#summary` to `N passed, M failed`. Reload the harness and confirm at least one visible failure before implementation.

- [ ] **Step 5: Commit the acceptance test**

```bash
git add tests/site-browser-test.html
git commit -m "test: define responsive site acceptance checks"
```

### Task 2: Build the semantic desktop page and photography

**Files:**
- Create: `renaissance-psychiatry-responsive.html`
- Test: `tests/site-browser-test.html`

**Interfaces:**
- Consumes: section copy and visual tokens from the approved specification and supplied export
- Produces: stable section IDs for in-page links, five or more `images.unsplash.com` photographs, and the DOM hooks used by Task 3

- [ ] **Step 1: Create the document shell and visual tokens**

Use the following base contract and expand it into the full document:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Renaissance Psychiatry LLC — Psychiatric care for every stage of life</title>
  <meta name="description" content="Outpatient psychiatric evaluations, medication management and psychotherapy for every stage of life.">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <style>
    :root { --navy:#141b41; --blue:#306bac; --sky:#6f9ceb; --aqua:#4ce0d2; --ink:#141b41; --muted:#4a5578; --pale:#f1f4fb; --line:#e2e8f5; --white:#fff; }
    * { box-sizing:border-box; }
    html { scroll-behavior:smooth; }
    body { margin:0; overflow-x:hidden; color:var(--ink); background:var(--white); font-family:"Plus Jakarta Sans",system-ui,sans-serif; }
    :focus-visible { outline:3px solid var(--aqua); outline-offset:3px; }
  </style>
</head>
<body>
  <a class="skip-link" href="#main">Skip to content</a>
  <header class="site-header"></header>
  <main id="main"></main>
  <footer id="contact"></footer>
</body>
</html>
```

- [ ] **Step 2: Implement every approved content section**

Add the notice, sticky navigation, hero, care tabs, services, conditions, age groups, providers, patient steps/payment cards, FAQs, booking CTA, and footer. Use the exact IDs `top`, `about`, `services`, `conditions`, `providers`, `patients`, `book`, and `contact`; retain the supplied crisis text and informational disclaimer.

- [ ] **Step 3: Add stable Unsplash images**

Add one eager hero image and four lazy age-group images using explicit `https://images.unsplash.com/photo-...` URLs with `auto=format&fit=crop` parameters. Each image must use a descriptive `alt`, `decoding="async"`, `object-fit:cover`, and a surrounding neutral background; do not use `source.unsplash.com` randomized endpoints.

- [ ] **Step 4: Run structural tests**

Reload: `http://127.0.0.1:4173/tests/site-browser-test.html`

Expected: semantic structure and image checks PASS; interaction checks may still fail until Task 3.

- [ ] **Step 5: Commit the desktop page**

```bash
git add renaissance-psychiatry-responsive.html
git commit -m "feat: build standalone psychiatry landing page"
```

### Task 3: Implement accessible interactions

**Files:**
- Modify: `renaissance-psychiatry-responsive.html`
- Test: `tests/site-browser-test.html`

**Interfaces:**
- Consumes: `#menu-toggle`, `#mobile-menu`, `[role="tab"]`, `[role="tabpanel"]`, and `.faq-question` elements from Task 2
- Produces: `setMenu(open: boolean): void`, `activateTab(tab: HTMLButtonElement): void`, and accordion click behavior that synchronizes visibility with ARIA state

- [ ] **Step 1: Add mobile-menu state synchronization**

```js
const menuToggle = document.querySelector('#menu-toggle');
const mobileMenu = document.querySelector('#mobile-menu');
function setMenu(open) {
  menuToggle.setAttribute('aria-expanded', String(open));
  mobileMenu.hidden = !open;
  document.body.classList.toggle('menu-open', open);
}
menuToggle.addEventListener('click', () => setMenu(menuToggle.getAttribute('aria-expanded') !== 'true'));
mobileMenu.querySelectorAll('a').forEach(link => link.addEventListener('click', () => setMenu(false)));
document.addEventListener('keydown', event => { if (event.key === 'Escape') setMenu(false); });
```

- [ ] **Step 2: Add keyboard-operable care tabs**

```js
const tabs = [...document.querySelectorAll('[role="tab"]')];
function activateTab(tab) {
  tabs.forEach(item => {
    const selected = item === tab;
    item.setAttribute('aria-selected', String(selected));
    item.tabIndex = selected ? 0 : -1;
    document.getElementById(item.getAttribute('aria-controls')).hidden = !selected;
  });
}
tabs.forEach((tab, index) => {
  tab.addEventListener('click', () => activateTab(tab));
  tab.addEventListener('keydown', event => {
    if (!['ArrowLeft', 'ArrowRight'].includes(event.key)) return;
    event.preventDefault();
    const offset = event.key === 'ArrowRight' ? 1 : -1;
    const next = tabs[(index + offset + tabs.length) % tabs.length];
    activateTab(next);
    next.focus();
  });
});
```

- [ ] **Step 3: Add FAQ accordion behavior**

```js
document.querySelectorAll('.faq-question').forEach(button => {
  button.addEventListener('click', () => {
    const answer = document.getElementById(button.getAttribute('aria-controls'));
    const open = button.getAttribute('aria-expanded') === 'true';
    button.setAttribute('aria-expanded', String(!open));
    button.querySelector('.faq-sign').textContent = open ? '+' : '−';
    answer.hidden = open;
  });
});
```

- [ ] **Step 4: Run interaction acceptance tests**

Reload: `http://127.0.0.1:4173/tests/site-browser-test.html`

Expected: interaction checks PASS; only missing responsive declarations, if any, remain failing.

- [ ] **Step 5: Commit interactions**

```bash
git add renaissance-psychiatry-responsive.html
git commit -m "feat: add accessible site interactions"
```

### Task 4: Add responsive layouts and complete verification

**Files:**
- Modify: `renaissance-psychiatry-responsive.html`
- Modify: `tests/site-browser-test.html`

**Interfaces:**
- Consumes: all page components and interaction hooks from Tasks 2 and 3
- Produces: final tablet and phone layout rules with no unintended horizontal overflow

- [ ] **Step 1: Add tablet and mobile CSS**

Add explicit breakpoints with these behaviors:

```css
@media (max-width: 1100px) {
  .desktop-nav, .header-actions { display:none; }
  .menu-toggle { display:inline-flex; }
  .hero-grid, .care-grid, .provider-grid, .booking-grid { grid-template-columns:1fr; }
  .services-grid, .ages-grid { grid-template-columns:repeat(2,minmax(0,1fr)); }
}
@media (max-width: 900px) {
  .section { padding-inline:24px; }
  .steps-grid, .payment-grid, .footer-grid { grid-template-columns:repeat(2,minmax(0,1fr)); }
  .hero-title { font-size:clamp(2.7rem,9vw,4.5rem); }
}
@media (max-width: 640px) {
  .section { padding-inline:18px; }
  .services-grid, .ages-grid, .steps-grid, .payment-grid, .footer-grid { grid-template-columns:1fr; }
  .hero-actions > *, .booking-actions > * { width:100%; text-align:center; }
  .stat-grid { grid-template-columns:1fr; }
  .booking-row { align-items:flex-start; flex-direction:column; }
}
```

- [ ] **Step 2: Add responsive reset details**

Ensure `img, svg { max-width:100%; }`, grid children use `min-width:0`, long email addresses use `overflow-wrap:anywhere`, sticky-header scroll targets use `scroll-margin-top`, and tap targets have a minimum height of `44px`.

- [ ] **Step 3: Run all automated checks**

Reload: `http://127.0.0.1:4173/tests/site-browser-test.html`

Expected: every visible harness result is PASS and the summary reports zero failures.

Run: `git diff --check`

Expected: no whitespace errors.

- [ ] **Step 4: Perform browser visual verification**

Open the page at widths 390px, 768px, 1024px, and 1440px. At each width confirm no horizontal scrollbar, readable text, intact image crops, visible calls to action, and sensible card stacking. At 390px and 768px exercise menu open/close and Escape; at every width exercise both care tabs and all FAQ toggles using mouse and keyboard.

- [ ] **Step 5: Commit the responsive and verified result**

```bash
git add renaissance-psychiatry-responsive.html tests/site-browser-test.html
git commit -m "feat: complete responsive psychiatry website"
```
