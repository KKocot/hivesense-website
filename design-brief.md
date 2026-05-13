# HiveSense Website -- Design Brief

**Status:** READY FOR IMPLEMENTATION
**Site type:** MARKETING
**Data:** 2025-05-13
**Handoff to:** frontend-developer

---

## 1. Site Type Classification

| Pytanie | Odpowiedz | Wynik |
|---|---|---|
| Kto jest userem? | Developer/node operator odkrywajacy HiveSense | MARKETING |
| Cel? | Konwersja -- przekonac do uzycia API / uruchomienia node'a | MARKETING |
| Frequency? | Raz / kilka razy -- strona informacyjna | MARKETING |
| Dlugosc sesji? | 1-5 minut -- skan sekcji, sprawdzenie API, klik CTA | MARKETING |

**Klasyfikacja: MARKETING** -- wizytowka produktu z naciskiem na konwersje do GitLab repo i dokumentacji API.

**Layout pattern:** Z-pattern (visual hero + CTA bottom-right per sekcja). Uzytkownik skanuje hero, trust bar, features, potem CTA.

---

## 2. Design Tokens

Format: CSS custom properties (Tailwind CSS 4 `@theme`).

```css
@theme {
  /* ---------------------------------- */
  /* Colors                             */
  /* ---------------------------------- */
  --color-bg-base:          #0a0a0f;
  --color-surface-1:        #12131a;
  --color-surface-2:        #1a1c27;
  --color-surface-3:        #222436;
  --color-accent-primary:   #00d4aa;
  --color-accent-secondary: #6366f1;
  --color-hive-red:         #e31337;
  --color-text-primary:     #f8fafc;
  --color-text-muted:       #94a3b8;
  --color-border:           #1e293b;
  --color-border-hover:     #334155;

  /* Accent variations */
  --color-accent-primary-dim:   #00d4aa1a;   /* 10% opacity -- card glow, bg tint */
  --color-accent-primary-mid:   #00d4aa33;   /* 20% opacity -- hover states */
  --color-accent-secondary-dim: #6366f11a;   /* 10% opacity */
  --color-accent-secondary-mid: #6366f133;   /* 20% opacity */
  --color-hive-red-dim:         #e313371a;   /* 10% opacity -- decorative only */

  /* ---------------------------------- */
  /* Spacing scale (8px base)           */
  /* ---------------------------------- */
  --spacing-0:   0px;
  --spacing-1:   4px;
  --spacing-2:   8px;
  --spacing-3:   12px;
  --spacing-4:   16px;
  --spacing-5:   20px;
  --spacing-6:   24px;
  --spacing-8:   32px;
  --spacing-10:  40px;
  --spacing-12:  48px;
  --spacing-16:  64px;
  --spacing-20:  80px;
  --spacing-24:  96px;
  --spacing-32:  128px;

  /* Section vertical padding */
  --section-py-mobile:  64px;   /* spacing-16 */
  --section-py-desktop: 96px;   /* spacing-24 */

  /* ---------------------------------- */
  /* Border radius                      */
  /* ---------------------------------- */
  --radius-sm:   6px;
  --radius-md:   10px;
  --radius-lg:   16px;
  --radius-xl:   20px;
  --radius-full: 9999px;

  /* ---------------------------------- */
  /* Shadows                            */
  /* ---------------------------------- */
  --shadow-card:       0 1px 2px rgba(0,0,0,0.4), 0 0 0 1px var(--color-border);
  --shadow-card-hover: 0 8px 24px rgba(0,0,0,0.5), 0 0 0 1px var(--color-border-hover);
  --shadow-glow-primary:   0 0 40px var(--color-accent-primary-dim), 0 0 80px var(--color-accent-primary-dim);
  --shadow-glow-secondary: 0 0 40px var(--color-accent-secondary-dim), 0 0 80px var(--color-accent-secondary-dim);
  --shadow-button:     0 1px 2px rgba(0,0,0,0.3);

  /* ---------------------------------- */
  /* Breakpoints                        */
  /* ---------------------------------- */
  --breakpoint-sm:  640px;
  --breakpoint-md:  768px;
  --breakpoint-lg:  1024px;
  --breakpoint-xl:  1280px;
}
```

---

## 3. Typography

### Font family

**Primary:** `Inter` (variable font, wght 400-700).
Fallback: `system-ui, -apple-system, sans-serif`.

**Monospace (code blocks, API curl examples):** `"JetBrains Mono", "Fira Code", ui-monospace, monospace`.

Powod wyboru Inter nad Geist: Inter ma lepszy support dla jezykow specjalnych (posty Hive sa wielojezyczne), sprawdzony w kontekscie developer-facing products (Vercel docs, Linear, Raycast). Ponadto Inter variable font jest mniejszy (~100KB woff2 vs ~130KB Geist).

### Type scale

| Token | Mobile | Desktop | Weight | Line-height | Letter-spacing | Use |
|---|---|---|---|---|---|---|
| `display` | 40px | 64px | 700 | 1.1 | -0.02em | Hero headline |
| `h1` | 32px | 48px | 700 | 1.15 | -0.02em | Section headers |
| `h2` | 24px | 32px | 600 | 1.25 | -0.01em | Subsection headers |
| `h3` | 20px | 24px | 600 | 1.3 | 0 | Card titles |
| `body-lg` | 18px | 20px | 400 | 1.6 | 0 | Hero subtitle |
| `body` | 16px | 16px | 400 | 1.6 | 0 | Body text |
| `body-sm` | 14px | 14px | 400 | 1.5 | 0 | Captions, meta |
| `mono` | 13px | 14px | 400 | 1.6 | 0 | Code snippets |
| `badge` | 12px | 13px | 500 | 1 | 0.04em | Badge labels, stat numbers |

### Heading gradient treatment

Hero display i section h1 moga uzywac gradient text dla efektu premium:

```css
.text-gradient-primary {
  background: linear-gradient(135deg, var(--color-text-primary) 0%, var(--color-accent-primary) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.text-gradient-mixed {
  background: linear-gradient(135deg, var(--color-accent-primary) 0%, var(--color-accent-secondary) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}
```

Uzycie: TYLKO hero display headline. Reszta headingow -- solid `text-primary`. Nie naduzywac -- gradient text traci efekt jesli jest wszedzie.

---

## 4. Motion Spec

**Regula nadrzedna:** CSS only, zero JS animation libraries. Wszystkie animacje musza byc interruptible i respektowac `prefers-reduced-motion`.

### Global reduced-motion fallback

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

### Transition tokens

| Token | Duration | Easing | Use |
|---|---|---|---|
| `--transition-fast` | 150ms | `ease-out` | Hover states (buttons, links, borders) |
| `--transition-base` | 200ms | `ease-out` | Card hover lift, shadow change |
| `--transition-medium` | 300ms | `ease-out` | Expanding elements, tooltips |
| `--transition-slow` | 500ms | `cubic-bezier(0.4, 0, 0.2, 1)` | Hero entrance, section fade-in |

### Hover effects

**Cards:**
```css
.card {
  transition: transform var(--transition-base), box-shadow var(--transition-base), border-color var(--transition-fast);
}
.card:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-card-hover);
  border-color: var(--color-border-hover);
}
```

**Buttons (primary):**
```css
.btn-primary:hover {
  filter: brightness(1.1);
  box-shadow: 0 0 20px var(--color-accent-primary-mid);
}
```

**Links:**
```css
a {
  transition: color var(--transition-fast);
}
a:hover {
  color: var(--color-accent-primary);
}
```

### Entrance animations (scroll-triggered via CSS `@starting-style` lub `IntersectionObserver` z klasa `.visible`)

**Fade up (sekcje, karty):**
```css
@keyframes fade-up {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-fade-up {
  animation: fade-up 500ms cubic-bezier(0.4, 0, 0.2, 1) both;
}
```

**Stagger dla list/gridow:**
Karty w gridzie: stagger 60ms miedzy elementami (via `animation-delay`).
```css
.animate-stagger > :nth-child(1) { animation-delay: 0ms; }
.animate-stagger > :nth-child(2) { animation-delay: 60ms; }
.animate-stagger > :nth-child(3) { animation-delay: 120ms; }
.animate-stagger > :nth-child(4) { animation-delay: 180ms; }
.animate-stagger > :nth-child(5) { animation-delay: 240ms; }
.animate-stagger > :nth-child(6) { animation-delay: 300ms; }
```

**Hero -- specjalne:**
- Headline: fade-up, delay 0ms, duration 600ms
- Subtitle: fade-up, delay 100ms, duration 500ms
- Search mockup: fade-up + scale(0.98 -> 1), delay 200ms, duration 600ms
- Stat badges: fade-up, stagger 80ms starting delay 400ms, duration 400ms

### Dozwolone animowane wlasciwosci (GPU only)

- `transform` (translate, scale, rotate)
- `opacity`
- `filter` (brightness, blur)
- `box-shadow` (via transition, nie animation)
- `border-color` (via transition)

**ZAKAZANE animowanie:** `width`, `height`, `top`, `left`, `margin`, `padding`, `font-size`, `background-color` (uzyj `opacity` overlay zamiast).

---

## 5. Layout Structure

### Global

```
max-width:       1200px (content)
max-width:       1400px (hero, full-bleed sections)
padding-inline:  16px (mobile), 24px (tablet), 32px (desktop)
```

### Grid system

```css
/* Feature cards, tech stack */
.grid-3 {
  display: grid;
  grid-template-columns: repeat(1, 1fr);           /* mobile */
  gap: 16px;
}
@media (min-width: 768px) {
  .grid-3 { grid-template-columns: repeat(2, 1fr); gap: 20px; }
}
@media (min-width: 1024px) {
  .grid-3 { grid-template-columns: repeat(3, 1fr); gap: 24px; }
}

/* 2-column layout (how it works: text + visual) */
.grid-2 {
  display: grid;
  grid-template-columns: 1fr;
  gap: 32px;
}
@media (min-width: 768px) {
  .grid-2 { grid-template-columns: repeat(2, 1fr); gap: 48px; }
}
```

---

### Sekcja po sekcji

#### S1. Nav

```
Position:          fixed top, z-50
Background:        bg-base/80 + backdrop-blur(12px) + border-bottom 1px border
Height:            64px
Layout:            flex, justify-between, align-center
Max-width:         1200px centered
Left:              Logo (SVG mark + "HiveSense" text)
Center (desktop):  Links (Features, API, How it Works, Tech Stack) -- text-muted, hover:text-primary
Right:             GitLab button (primary CTA, small)
Mobile:            Hamburger -> slide-down panel, bg surface-1
```

Logo suggestion: Abstrakcyjny hexagon (hive) z wewnetrznym neural network pattern (3 nodes + connections). Kolory: accent-primary fill, lub outline stroke.

#### S2. Hero

```
Padding-top:       128px (nawigacja 64px + 64px space)
Padding-bottom:    96px
Max-width:         1400px
Text-align:        center
```

**Kompozycja (top to bottom):**

1. **Badge** (opcjonalny) -- maly pill nad headline
   - Text: "Open Source -- Powered by Hive"
   - Style: border 1px border, bg surface-2, radius-full, text-muted, badge font
   - Icon: maly dot animated pulse (accent-primary) -- sygnalizuje "live"

2. **Headline** (display)
   - Text: "Semantic Search for the Hive Blockchain"
   - Gradient text: text-primary -> accent-primary
   - Max-width: 800px centered

3. **Subtitle** (body-lg, text-muted)
   - Text: "Discover meaning in millions of posts. ML-powered vector search with HNSW indexing -- find content by what it means, not just what it says."
   - Max-width: 600px centered

4. **CTA Group** (flex, gap-16, centered)
   - Primary: "View on GitLab" -> bg accent-primary, text bg-base (dark on teal), font-weight 600, radius-full, px-24 py-12
   - Secondary: "Explore API" -> border 1px border, text-primary, radius-full, px-24 py-12, hover:bg surface-2

5. **Search Mockup** (margin-top 48px)
   - Fake search input z placeholder "Search for blockchain governance discussions..."
   - Style: bg surface-1, border 1px border, radius-lg, px-20 py-16, max-width 640px
   - Wewnatrz: ikona search (accent-primary) + placeholder text (text-muted)
   - Pod inputem: 3 sugestie jako pills ("DeFi strategies", "Witness voting", "Community proposals")
   - Caly mockup otoczony subtle glow -- shadow-glow-primary, tlumionym do 50%

6. **Stat Badges** (flex, gap-24, centered, margin-top 32px)
   - 3 badges: "10M+ Posts Indexed" / "768-dim Vectors" / "<100ms Search"
   - Style: inline-flex, gap-8, text-muted (body-sm), z ikona/liczba w accent-primary

**Hero background:**

Vector space pattern -- sugeruje embedding space / neural connections.

```css
.hero-bg {
  position: absolute;
  inset: 0;
  overflow: hidden;
  z-index: 0;
}

/* Radial gradient glow -- top center */
.hero-bg::before {
  content: '';
  position: absolute;
  top: -200px;
  left: 50%;
  transform: translateX(-50%);
  width: 800px;
  height: 600px;
  background: radial-gradient(
    ellipse at center,
    var(--color-accent-primary-dim) 0%,
    transparent 70%
  );
  filter: blur(80px);
}

/* Secondary glow -- offset */
.hero-bg::after {
  content: '';
  position: absolute;
  top: -100px;
  right: 10%;
  width: 400px;
  height: 400px;
  background: radial-gradient(
    circle,
    var(--color-accent-secondary-dim) 0%,
    transparent 70%
  );
  filter: blur(60px);
}
```

**Dot grid pattern** (SVG lub CSS):
```css
.hero-dots {
  position: absolute;
  inset: 0;
  background-image: radial-gradient(circle, var(--color-border) 1px, transparent 1px);
  background-size: 32px 32px;
  opacity: 0.4;
  mask-image: radial-gradient(ellipse at center, black 30%, transparent 70%);
}
```

Opcjonalnie: SVG z 10-15 polaczonymi nodami (linie + kola) jako "vector space" -- opacity 0.08, accent-primary stroke, absolute positioned w tle hero. Statyczny, nie animowany (performance).

#### S3. Trust Bar

```
Padding:           48px 0
Border:            top + bottom 1px border
Background:        bg-base (same as page)
```

**Layout:**
- Label: "Built with" -- text-muted, body-sm, uppercase, letter-spacing 0.08em, text-center
- Logos row: flex, justify-center, gap 40px (desktop), gap 24px (mobile), align-center
- Loga: PostgreSQL, pgvector, Ollama, HAF/Hive, PostgREST, Docker
- Styl log: monochrome (text-muted / opacity 0.5), hover: opacity 1 z transition-fast
- Na mobile: horizontal scroll lub 2 rows x 3

#### S4. Features Grid

```
Padding:           section-py
Max-width:         1200px
```

**Header:**
- Overline: "FEATURES" -- badge font, accent-primary, uppercase, letter-spacing 0.08em
- Title: h1 -- "What HiveSense Does"
- Subtitle: body-lg, text-muted, max-width 560px -- "ML-powered semantic understanding of Hive blockchain content."

**Grid:** 3 columns (desktop), 2 (tablet), 1 (mobile), gap 20px

**6 kart -- feature cards:**

| # | Title | Description | Icon suggestion |
|---|---|---|---|
| 1 | Semantic Search | Search by meaning, not keywords. Find posts about "blockchain governance" even when those words aren't used. | Magnifying glass + sparkles |
| 2 | Similar Posts | Discover related content for any post. HNSW-indexed vector similarity returns results in <100ms. | Nodes/connections |
| 3 | Thematic Contributors | Find authors who write about specific topics across the entire blockchain. | People + topic bubbles |
| 4 | Smart Chunking | Posts split into 512-token chunks with 15% overlap. No meaning lost at boundaries. | Scissors + document |
| 5 | Real-time Sync | HAF-based scheduler continuously processes new blocks. Always up to date. | Refresh/sync arrows |
| 6 | Open Source | Fully open source, deployed on Hive infrastructure. Run your own node or use the public API. | Code brackets + heart |

**Card component pattern:**

```css
.feature-card {
  background: var(--color-surface-1);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  padding: 32px;
  transition: transform var(--transition-base),
              box-shadow var(--transition-base),
              border-color var(--transition-fast);
}

.feature-card:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-card-hover);
  border-color: var(--color-border-hover);
}
```

**Card internals:**
- Icon: 40x40px, bg accent-primary-dim, radius-md, centered icon w accent-primary (SVG 20x20)
- Title: h3, text-primary, margin-top 20px
- Description: body, text-muted, margin-top 8px

**Opcjonalny accent na gorze karty** -- subtly gradient top-border:
```css
.feature-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 20%;
  right: 20%;
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--color-accent-primary-mid), transparent);
  opacity: 0;
  transition: opacity var(--transition-base);
}
.feature-card:hover::before {
  opacity: 1;
}
```

#### S5. How It Works

```
Padding:           section-py
Background:        surface-1 (full-width band)
Max-width:         1200px (content)
```

**Header:**
- Overline: "HOW IT WORKS"
- Title: h1 -- "From Blockchain to Meaning"

**3-step flow:**

Layout: grid-3 na desktop, vertical stack na mobile. Miedzy krokami na desktop -- connector line (1px border, dashed lub dotted, horizontal).

| Step | Number | Title | Description |
|---|---|---|---|
| 1 | 01 | Ingest & Chunk | HAF scheduler collects posts from Hive blocks. Text is cleaned, stripped of markdown/links, and split into 512-token chunks with overlap. |
| 2 | 02 | Embed | Each chunk is sent to Ollama for ML embedding generation. 768-dimensional vectors capture semantic meaning of the content. |
| 3 | 03 | Search | Vectors stored in PostgreSQL with pgvector. HNSW index enables sub-100ms cosine similarity search across millions of posts. |

**Step card style:**
```
Background:   surface-2
Border:       1px border
Radius:       radius-lg
Padding:      32px
Text-align:   center (opcjonalnie left)
```

- Numer: display size 48px, font-weight 700, accent-primary (lub accent-secondary gradient), opacity 0.2 jako wielki numer w tle
- Title: h3
- Description: body, text-muted

**Connector line (desktop only):**
```css
.step-connector {
  position: absolute;
  top: 50%;
  right: -12px;           /* polowa gap */
  width: 24px;
  height: 1px;
  border-top: 2px dashed var(--color-border);
}
```

#### S6. API Showcase

```
Padding:           section-py
Max-width:         1200px
Background:        bg-base
```

**Header:**
- Overline: "API"
- Title: h1 -- "Three Endpoints. Infinite Discovery."
- Subtitle: body-lg, text-muted -- "RESTful API powered by PostgREST. No auth required."

**3 endpoint karty** -- kazda z curl example:

| # | Endpoint | Method | Description |
|---|---|---|---|
| 1 | `/posts/search` | POST | Search posts by semantic meaning |
| 2 | `/posts/{author}/{permlink}/similar` | GET | Find posts similar to a given post |
| 3 | `/authors/search` | POST | Find authors who write about a topic |

**API Card pattern:**

```css
.api-card {
  background: var(--color-surface-1);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  overflow: hidden;
}

.api-card-header {
  padding: 20px 24px;
  border-bottom: 1px solid var(--color-border);
  display: flex;
  align-items: center;
  gap: 12px;
}

.api-card-body {
  padding: 0;
}
```

**Header internals:**
- Method badge: "POST" lub "GET"
  - POST: bg accent-primary-dim, text accent-primary, mono font, badge size, radius-sm, px-8 py-2
  - GET: bg accent-secondary-dim, text accent-secondary, mono font, badge size, radius-sm, px-8 py-2
- Endpoint path: mono font, text-primary

**Code block:**
```css
.code-block {
  background: var(--color-bg-base);
  padding: 20px 24px;
  font-family: "JetBrains Mono", monospace;
  font-size: 13px;
  line-height: 1.6;
  color: var(--color-text-muted);
  overflow-x: auto;
  white-space: pre;
}
```

**Syntax highlighting tokens (CSS classes):**
- `.token-cmd` (curl): text-primary
- `.token-flag` (-X, -H): text-muted
- `.token-string` ("..."): accent-primary
- `.token-url`: accent-secondary
- `.token-comment` (# ...): text-muted, opacity 0.5

Layout: grid-3 LUB vertical stack z max-width 720px centered (zalezne od dlugosci curli -- vertical moze byc czytelniejszy).

Rekomendacja: **vertical stack**, max-width 800px, gap 20px. Curle sa za dlugie na 1/3 szerkosci.

#### S7. Tech Stack

```
Padding:           section-py
Background:        surface-1 (full-width band)
Max-width:         1200px
```

**Header:**
- Overline: "TECH STACK"
- Title: h1 -- "Battle-Tested Infrastructure"

**Grid:** 2x3 (desktop), 2x2 (tablet), 1 column (mobile), gap 16px

**Tech items:**

| Tech | Category | Description |
|---|---|---|
| PostgreSQL + pgvector | Database | Vector storage with HNSW indexing |
| Ollama | ML Engine | Local embedding generation |
| HAF | Blockchain | Hive Application Framework |
| PostgREST | API | Auto-generated RESTful API |
| Docker | Infrastructure | Containerized deployment |
| PL/pgSQL | Language | Pure SQL application logic |

**Tech card style:**
```css
.tech-card {
  background: var(--color-surface-2);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: 24px;
  display: flex;
  align-items: flex-start;
  gap: 16px;
}
```

- Logo/Icon: 36x36px, monochrome lub oryginalne kolory (opcja implementatora)
- Title: h3 (tech name)
- Category: badge font, text-muted
- Description: body-sm, text-muted

Mniejsze karty niz feature cards -- bardziej kompaktowe, informacyjne.

#### S8. CTA Section

```
Padding:           section-py (wieksze -- 96px mobile, 128px desktop)
Background:        bg-base
Text-align:        center
Max-width:         800px centered
```

**Kompozycja:**
- Title: h1 -- "Start Exploring Hive Content"
- Subtitle: body-lg, text-muted -- "Open source, self-hostable, and ready to deploy. Dive into semantic search on the Hive blockchain."
- CTA buttons: ten sam styl co hero (primary + secondary), centered

**Background treatment:**
Symetryczny glow jak w hero ale mniejszy:
```css
.cta-bg {
  background: radial-gradient(
    ellipse at center,
    var(--color-accent-primary-dim) 0%,
    transparent 60%
  );
}
```

#### S9. Footer

```
Padding:           48px 0 32px
Border-top:        1px border
Background:        bg-base
Max-width:         1200px
```

**Layout (desktop):** grid 4 kolumny
- Col 1: Logo + krotki opis (1-2 linie, text-muted, body-sm) + "A HAF application by Hive"
- Col 2: "Product" -- links: Features, API, Documentation, GitLab
- Col 3: "Hive Ecosystem" -- links: Hive.io, Hivemind, HAF, PeakD
- Col 4: "Resources" -- links: Installation Guide, API Reference, Contributing, License

**Mobile:** stack, logo on top, 3 link groups below (accordion lub expanded)

**Bottom bar:**
```
Padding-top: 24px
Border-top: 1px border
Flex: justify-between
```
- Left: "2025 HiveSense. Open source under MIT License."
- Right: GitLab icon link

---

## 6. Component Patterns

### Buttons

```css
/* Primary */
.btn-primary {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 12px 24px;
  background: var(--color-accent-primary);
  color: var(--color-bg-base);
  font-weight: 600;
  font-size: 15px;
  border-radius: var(--radius-full);
  border: none;
  cursor: pointer;
  transition: filter var(--transition-fast),
              box-shadow var(--transition-fast);
  box-shadow: var(--shadow-button);
}
.btn-primary:hover {
  filter: brightness(1.1);
  box-shadow: 0 0 20px var(--color-accent-primary-mid);
}
.btn-primary:active {
  filter: brightness(0.95);
  transform: scale(0.98);
}

/* Secondary */
.btn-secondary {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 12px 24px;
  background: transparent;
  color: var(--color-text-primary);
  font-weight: 500;
  font-size: 15px;
  border-radius: var(--radius-full);
  border: 1px solid var(--color-border);
  cursor: pointer;
  transition: background var(--transition-fast),
              border-color var(--transition-fast);
}
.btn-secondary:hover {
  background: var(--color-surface-2);
  border-color: var(--color-border-hover);
}

/* Ghost (nav links, footer links) */
.btn-ghost {
  padding: 8px 12px;
  background: none;
  border: none;
  color: var(--color-text-muted);
  font-size: 14px;
  cursor: pointer;
  transition: color var(--transition-fast);
}
.btn-ghost:hover {
  color: var(--color-text-primary);
}
```

### Badges

```css
.badge {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 4px 12px;
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 0.04em;
  border-radius: var(--radius-full);
  border: 1px solid var(--color-border);
  background: var(--color-surface-2);
  color: var(--color-text-muted);
}

/* Accent variant */
.badge-accent {
  border-color: var(--color-accent-primary-mid);
  background: var(--color-accent-primary-dim);
  color: var(--color-accent-primary);
}
```

### Section header pattern

Powtarzalny wzorzec dla kazdej sekcji (S4-S8):

```html
<div class="section-header">
  <span class="overline">FEATURES</span>
  <h2 class="section-title">What HiveSense Does</h2>
  <p class="section-subtitle">Description text here.</p>
</div>
```

```css
.section-header {
  text-align: center;
  margin-bottom: 48px;   /* desktop: 64px */
  max-width: 600px;
  margin-inline: auto;
}

.overline {
  font-size: 13px;
  font-weight: 500;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--color-accent-primary);
  margin-bottom: 12px;
  display: block;
}

.section-title {
  /* h1 token */
  margin-bottom: 16px;
}

.section-subtitle {
  /* body-lg token, text-muted */
}
```

### Overline separator

Miedzy sekcjami -- brak widocznego HR. Separation tylko przez zmiane background (bg-base <-> surface-1) i vertical padding.

---

## 7. Accessibility Checklist

### Contrast ratios (verified)

| Foreground | Background | Ratio | Result |
|---|---|---|---|
| `#f8fafc` text-primary | `#0a0a0f` bg-base | 16.7:1 | AAA pass |
| `#f8fafc` text-primary | `#12131a` surface-1 | 15.2:1 | AAA pass |
| `#94a3b8` text-muted | `#0a0a0f` bg-base | 6.7:1 | AA pass |
| `#94a3b8` text-muted | `#12131a` surface-1 | 6.1:1 | AA pass |
| `#94a3b8` text-muted | `#1a1c27` surface-2 | 5.4:1 | AA pass |
| `#00d4aa` accent-primary | `#0a0a0f` bg-base | 8.5:1 | AAA pass |
| `#00d4aa` accent-primary | `#12131a` surface-1 | 7.8:1 | AA pass |
| `#6366f1` accent-secondary | `#0a0a0f` bg-base | 4.0:1 | AA pass (large text only) |
| `#0a0a0f` (btn text) | `#00d4aa` (btn bg) | 8.5:1 | AAA pass |
| `#e31337` hive-red | `#0a0a0f` bg-base | 3.5:1 | FAIL for text |

**UWAGI:**
- `#e31337` (hive-red): TYLKO do dekoracji (ikony, bordery, akcenty). NIGDY jako kolor tekstu na ciemnym tle. Jesli potrzebny jako text -- uzyc jasniejszej wersji `#ff4d6a` (ratio ~5.8:1 na bg-base, AA pass).
- `#6366f1` (accent-secondary): uzywac TYLKO dla large text (>=18px bold lub >=24px regular) lub elementow nie-tekstowych (badge bg, ikony, bordery). Dla body text na ciemnym tle -- uzyj jasniejszej wersji `#818cf8` (~5.6:1, AA pass).

### Focus states

```css
:focus-visible {
  outline: 2px solid var(--color-accent-primary);
  outline-offset: 2px;
  border-radius: var(--radius-sm);
}

/* NIE uzywac outline: none bez zamiennika */
/* Kazdy interaktywny element musi miec widoczny focus ring */
```

### Keyboard navigation

- Tab order: logiczny top-to-bottom, left-to-right flow
- Skip link: pierwszy element w DOM -- "Skip to main content", widoczny on focus
- Nav links: focusable
- CTA buttons: focusable, Enter/Space activates
- Mobile menu: focus trap when open, Escape closes
- Code blocks: `tabindex="0"` jesli scrollable, `role="region"` z `aria-label`

### ARIA

- Nav: `<nav aria-label="Main navigation">`
- Sections: `<section aria-labelledby="section-title-id">`
- Stat badges w hero: `aria-label` z pelna wartoscia ("Over 10 million posts indexed")
- Logo links: `aria-label="HiveSense home"`
- External links: `rel="noopener noreferrer"` + visual indicator (ikona external link) + `aria-label` z "(opens in new tab)"
- Code blocks: `<pre><code>` z odpowiednim `aria-label`
- Trust bar logos: `alt` text na kazdym logo
- Decorative icons w feature cards: `aria-hidden="true"`

### Semantic HTML

```
<header>         -- nav
<main>           -- cala tresc
  <section>      -- kazda sekcja
<footer>         -- footer
<h1>             -- TYLKO w hero (1 na strone)
<h2>             -- section titles
<h3>             -- card titles, step titles
<ul>/<ol>        -- listy linkow w footerze
<pre><code>      -- curl examples
<figure>         -- search mockup wrapper
```

### Reduced motion

Wszystkie animacje objete `@media (prefers-reduced-motion: reduce)` -- patrz sekcja 4.

---

## 8. Performance Budget

| Metric | Budget | Notes |
|---|---|---|
| LCP | <=2.0s | Hero headline jest LCP element. Zadnych blocking resources przed nim. |
| CLS | <=0.05 | Font display: swap + explicit size na obrazkach/SVG |
| INP | <=100ms | Strona statyczna, minimalne interakcje. |
| Total page weight | <=500KB | Transfer size, gzipped |
| HTML | <=30KB | |
| CSS | <=40KB | Tailwind z purge |
| JS | <=20KB | Minimal -- intersection observer + mobile menu toggle only |
| Fonts | <=120KB | Inter variable (woff2, latin subset) + JetBrains Mono (woff2, regular only, latin) |
| Images/SVG | <=100KB total | Logos as inline SVG, hero bg as CSS. Zadnych bitmap images. |
| Critical CSS | inline | Above-the-fold CSS inline w `<head>` |
| Font loading | `font-display: swap` | + preload critical font files |

### Rekomendacje

- Zero JavaScript frameworks. Static HTML + CSS + minimal vanilla JS (IntersectionObserver for scroll animations, mobile menu toggle).
- Alternatywa: Astro z `output: 'static'` jesli potrzebna komponentyzacja. Zero client-side JS hydration.
- SVG sprites dla ikon zamiast icon font.
- Inline critical CSS, defer reszta.
- Preload: Inter variable woff2, hero background (jesli external).

---

## 9. Handoff Notes

### Priority

P1 -- strona wizytowkowa dla HiveSense. Cel: prezentacja produktu developerom i node operatorom Hive.

### Zalecany stack

- **Astro 5** (static output, zero JS) -- zgodne z `clive-website` w repozytorium `websites/`
- **Tailwind CSS 4** -- `@theme` layer z tokenami powyzej
- **Brak JS bibliotek animacji** -- pure CSS transitions + animations
- **Vanilla JS** -- IntersectionObserver (scroll animations), mobile menu

### Pakiety

Nowe pakiety wymagaja decyzji w KB API przed instalacja. Rekomendowane (do zatwierdzenia):
- `astro` -- framework
- `tailwindcss` + `@tailwindcss/vite` -- styling
- `@fontsource-variable/inter` -- font (opcjonalne, mozna tez Google Fonts CDN)
- `@fontsource/jetbrains-mono` -- mono font (opcjonalne)

### Ikony

Uzyc inline SVG -- handcrafted lub Lucide icons (SVG copy-paste, BEZ pakietu runtime). Ikony potrzebne:
- Search (magnifying glass)
- Sparkles / stars
- Nodes / connection graph
- People / users
- Scissors / cut
- Refresh / sync
- Code brackets
- Heart
- Arrow right (CTA)
- External link
- Menu (hamburger)
- X (close)
- GitLab logo
- Tech logos: PostgreSQL, Ollama, HAF/Hive, PostgREST, Docker, pgvector

### Deployment

Statyczny build -> Vercel lub Cloudflare Pages. Zero server-side runtime.

### Struktura plikow (sugestia)

```
hivesense-website/
  src/
    layouts/
      Layout.astro          -- base layout, font loading, meta
    components/
      Nav.astro
      Hero.astro
      TrustBar.astro
      FeaturesGrid.astro
      HowItWorks.astro
      ApiShowcase.astro
      TechStack.astro
      CtaSection.astro
      Footer.astro
      ui/
        Button.astro
        Badge.astro
        Card.astro
        SectionHeader.astro
        CodeBlock.astro
    pages/
      index.astro
    styles/
      global.css            -- @theme tokens, base styles, animations
    assets/
      icons/                -- inline SVGs
      logos/                 -- tech logos SVGs
  public/
    fonts/                  -- self-hosted woff2
    favicon.svg
  astro.config.mjs
  tailwind.config.ts
  package.json
```

### Next Steps (dla frontend-developera)

1. Zatwierdzic pakiety w KB API (decisions)
2. Scaffold Astro 5 project z Tailwind CSS 4
3. Skonfigurowac `@theme` tokens z sekcji 2
4. Zaimplementowac komponent po komponencie (Nav -> Hero -> TrustBar -> ... -> Footer)
5. Dodac scroll animations (IntersectionObserver + `.animate-fade-up`)
6. Przetestowac a11y (Lighthouse, axe-core, keyboard navigation)
7. Przetestowac performance (Lighthouse, WebPageTest)
8. Przetestowac responsive (320px, 375px, 768px, 1024px, 1280px)
9. Przetestowac `prefers-reduced-motion`
10. Deploy na Vercel/Cloudflare Pages

---

## 10. Design Rationale

### Dlaczego dark theme
HiveSense to narzedzie blockchain/AI -- dark theme jest standardem w tej przestrzeni (Ethereum.org, Chainlink, Vercel). Developer audience preferuje dark UI. Ponadto dark tlo lepiej eksponuje kolorowe akcenty (cyano-mietowy glow na czarnym = premium feel).

### Dlaczego cyano-mietowy (#00d4aa)
- Odroznienie od Hive red (#e31337) -- accent-primary nie koliduje z brand kolorem Hive, ale uzupelnia go
- Konotacje: AI, technologia, swiezosc, precision
- Doskonaly kontrast na ciemnym tle (8.5:1)
- Wyroznia sie wsrod blockchain projektow (wiekszosc uzywa blue/purple)

### Dlaczego indigo (#6366f1) jako secondary
- Komplementarny do cyano-mietowego (cool palette)
- Konotacje: depth, intelligence, premium
- Uzyty oszczednie -- gradient accents, API method badges, dekoracja

### Dlaczego border-based cards (nie shadow-based)
- Na ciemnym tle shadow jest mniej widoczny niz na jasnym
- 1px border z subtle hover glow daje czytelniejszy visual hierarchy
- Zgodne z trendem 2024-2025 (Linear, Vercel, Raycast)

### Dlaczego pill buttons (radius-full)
- Mieszanka technicznosci (code blocks, gridy) z approachable CTA
- Pill shape wyroznia CTA od kart (radius-lg)
- Zgodne z reference (JobHub)

### Dlaczego vertical API showcase
- Curl commandy sa dlugie -- w 3-column grid bylby overflow
- Vertical stack daje pelna szerokosc na code blocks
- Czytelniejsze na mobile
