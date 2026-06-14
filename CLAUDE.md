# SageMyHealth — Calculator Template Spec

## Tech constraints
- Pure static HTML/CSS/JS — **no frameworks, no imports, no build step**
- Each file is 100% self-contained (all CSS in `<style>`, all JS at bottom of `<body>`)
- Must work at `file://` protocol in Chrome (no relative fetch calls)

---

## Canonical HTML structure (copy exactly)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>[Name] — SageMyHealth</title>
  <meta name="description" content="..." />
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=Playfair+Display:wght@700&display=swap" rel="stylesheet" />
  <style>
    /* Brand tokens → two-column layout → form → results column → footer */
    /* See spec below */
  </style>
</head>
<body>

<!-- NAV -->
<nav class="site-nav">
  <div class="nav-inner">
    <a href="../index.html" class="nav-logo">
      <img src="assets/logo.png" alt="SageMyHealth" style="height: 54px; width: auto; object-fit: contain;"/>
    </a>
    <ul class="nav-links">
      <li><a href="../index.html#categories">Calculators</a></li>
      <li><a href="../about.html">About</a></li>
      <li><a href="../blog.html">Health Guides</a></li>
    </ul>
    <a href="../index.html#categories" class="nav-btn">Browse All</a>
  </div>
</nav>

<!-- PAGE -->
<main class="calc-page">
  <div class="calc-wrapper">

    <nav class="breadcrumb" aria-label="Breadcrumb">
      <a href="../index.html">Home</a>
      <span class="bc-sep">›</span>
      <a href="../categories/[category].html">[Category]</a>
      <span class="bc-sep">›</span>
      <span>[Calculator Name]</span>
    </nav>

    <header class="page-header">
      <div class="calc-tag">[emoji] [Category]</div>
      <h1>[Calculator Name]</h1>
      <p>[One-sentence description]</p>
    </header>

    <!-- TWO-COLUMN GRID -->
    <div class="calc-columns">

      <!-- LEFT: Form card -->
      <div class="form-card">
        <div class="form-card-header">
          <span class="form-card-title">Enter your information</span>
          <!-- optional unit toggle here -->
        </div>
        <div class="form-body">
          <!-- fields -->
          <button class="calc-btn" onclick="calculate()">Calculate</button>
          <p class="disclaimer">...</p>
        </div>
      </div>

      <!-- RIGHT: Results column -->
      <div class="results-col">

        <!-- Result card: HIDDEN until calculate() runs -->
        <div id="result-panel" style="display:none">
          <div class="result-card">
            <!-- result content -->
            <div class="note-block">...</div>
          </div>
        </div>

        <!-- About: ALWAYS visible -->
        <div class="info-card">
          <h3>About [topic]</h3>
          <p>...</p>
        </div>

        <!-- You might also like: ALWAYS visible -->
        <div class="related-section">
          <p class="related-title">You might also like</p>
          <div class="related-grid">
            <a href="[file].html" class="related-card">
              <div class="related-icon">[emoji]</div>
              <div class="related-info"><strong>[Name]</strong><span>[Subtitle]</span></div>
            </a>
            <!-- 4 cards total -->
          </div>
        </div>

      </div><!-- /.results-col -->

    </div><!-- /.calc-columns -->

  </div>
</main>

<!-- FOOTER -->
...
```

---

## Canonical CSS (copy verbatim into every new calculator)

### Brand tokens (:root)
```css
:root {
  --sage: #3B6A3A; --sage-dark: #0A3D2A; --sage-mid: #5C8A5C;
  --sage-light: #EBF2EC; --sage-pale: #F2F7F3;
  --cream: #E3FFB3; --white: #FFFFFF;
  --text-dark: #1A2E1E; --text-mid: #4A5568; --text-light: #718096; --placeholder: #AAB0B8;
  --border: #E2EBE4; --border-mid: #C8D9CC;
  --red-soft: #EDAE71; --red-text: #991B1B;
  --amber-soft: #EECA8F; --amber-text: #92400E;
  --green-soft: #D1FAE5; --green-text: #3D473D;
  --shadow-xs: 0 1px 2px rgba(59,106,58,0.06); --shadow-sm: 0 1px 6px rgba(59,106,58,0.08);
  --shadow-md: 0 4px 16px rgba(59,106,58,0.12); --shadow-lg: 0 8px 32px rgba(59,106,58,0.16);
  --r-sm: 8px; --r: 12px; --r-lg: 20px; --r-xl: 28px;
}
```

### Layout
```css
.calc-page { padding: 36px 0 72px; }
.calc-wrapper { max-width: 1200px; margin: 0 auto; padding: 0 24px; }

/* TWO-COLUMN — 480px left / 1fr right */
.calc-columns { display: grid; grid-template-columns: 480px 1fr; gap: 24px; align-items: start; }

/* RIGHT COLUMN */
.results-col { display: flex; flex-direction: column; gap: 20px; }
```

### Result panel reveal
```css
/* Use display:none / display:'' (NOT .visible class) on #result-panel wrapper */
/* Inside result-panel, use a .result-card with shadow-lg */
```

### Info card (About)
```css
.info-card {
  background: white; border-radius: var(--r-xl);
  box-shadow: var(--shadow-sm); padding: 22px 24px;
}
.info-card h3 { font-size: 14px; font-weight: 700; margin-bottom: 8px; color: var(--text-dark); }
.info-card p  { font-size: 13px; color: var(--text-mid); line-height: 1.75; }
```

### Related section ("You might also like")
```css
/* NO border-top, NO margin-top, NO padding-top — gap on results-col handles spacing */
.related-section { }
/* Title: normal case, NOT uppercase, NOT letter-spacing */
.related-title { font-size: 16px; font-weight: 700; color: var(--text-dark); margin-bottom: 12px;
  text-transform: none; letter-spacing: normal; }
.related-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
.related-card {
  background: white; border: 1.5px solid var(--border);
  border-radius: var(--r); padding: 14px 16px;
  display: flex; align-items: center; gap: 12px;
  transition: all .18s; text-decoration: none; color: inherit;
}
.related-card:hover { border-color: var(--sage); box-shadow: var(--shadow-sm); transform: translateY(-1px); }
.related-icon {
  width: 36px; height: 36px; background: var(--sage-light);
  border-radius: 9px; display: flex; align-items: center;
  justify-content: center; font-size: 16px; flex-shrink: 0;
}
.related-info strong { display: block; font-size: 13px; font-weight: 600; }
.related-info span  { font-size: 11px; color: var(--text-light); }
```

### Note block (disclaimer)
```css
.note-block {
  background: var(--cream); border-radius: var(--r);
  padding: 13px 16px; font-size: 12px; color: var(--text-light);
  line-height: 1.7; display: flex; gap: 8px; align-items: flex-start;
}
```

---

## Rules — never break these

1. **Two-column grid always**: `calc-columns` with `480px 1fr`. NEVER single-column or max-width centered layout.
2. **Right column is `results-col`**: flex column, gap 20px. Three stacked items: `#result-panel` (hidden) → `info-card` (always visible) → `related-section` (always visible).
3. **result-panel hidden by default**: Use `style="display:none"` on a wrapper `<div id="result-panel">`. JS shows it with `document.getElementById('result-panel').style.display = ''`. Never use `.visible` CSS class toggle on the outer wrapper.
4. **No dividers in results-col**: The gap handles spacing. Never add `border-top` to `.related-section` or `.info-card`.
5. **"You might also like" title**: Always sentence case. Never `text-transform: uppercase`. Defined with both `text-transform: none` AND `letter-spacing: normal` to override any cascade from old `.related-title` definitions.
6. **No `<hr>` dividers inside the result card** — use margin/padding only.
7. **Related links must point to existing calculators only** (bmi.html, tdee.html, heart-rate.html, blood-glucose.html, period-tracker.html, lung-capacity.html, calories-burned.html, brain-fatigue.html, hair-loss-risk.html, iron-deficiency.html, anxiety-score.html, sleep-need.html, biological-age.html).
8. **Disclaimer goes inside the result card** (`.note-block`), not in the results-col.
9. **Footer**: Same structure across all pages. Reference bmi.html or period-tracker.html.
10. **Responsive**: Always include `@media (max-width: 1000px) { .calc-columns { grid-template-columns: 1fr; } }`.

---

## Phase 1 calculator inventory

| File | Status | Category |
|------|--------|----------|
| bmi.html | ✅ done | Weight & Body |
| tdee.html | ✅ done | Weight & Body |
| heart-rate.html | ✅ done | Fitness |
| blood-glucose.html | ✅ done | Health |
| period-tracker.html | ✅ done | Women's Health |
| iron-deficiency.html | ✅ done | Health |
| anxiety-score.html | ✅ done | Mental Health |
| lung-capacity.html | ✅ done | Fitness |
| calories-burned.html | ✅ done | Fitness |
| brain-fatigue.html | ✅ done | Mental Health |
| hair-loss-risk.html | ✅ done | Health |
| sleep-need.html | ✅ done | Sleep |
| biological-age.html | ✅ done | Health |
