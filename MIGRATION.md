# Design System Migration Log

Tracking every token-level change made to Search Labs (`design-system.css`) as part of the migration to the Elastic Design System Tokens library.

## Ground rules

- **Search Labs is the source of truth.** The library is a constraint set of allowed values.
- For each Search Labs token, we pick the **closest available library value** by raw distance, regardless of category.
- **Delta** = (library value) − (Search Labs original value). Positive = Search Labs grows; negative = shrinks.
- Zero-delta entries are still logged — the token is now annotated with its library mapping for traceability, even though no value changed.
- Each entry includes the inverse to revert (the original Search Labs value) inline as a CSS comment in `design-system.css`.

## Source references

- **Library file**: [Design System Tokens and Variables](https://www.figma.com/design/f2pdARBg3ziDbDzh7hP30Y/Design-System-Tokens-and-Variables-Test-AI)
- **Site replica (Search Labs baseline)**: [Sunny's edit search lab](https://www.figma.com/design/WFkIZOqVE7W0zXK1Yy5Bdp/Sunny-s-edit-search-lab)
- **Components file**: [Components](https://www.figma.com/design/2F99OfKzvouAcnF7fruISD/Components)

---

## 2026-05-14 — Session 1: Font size scale (`--fs-*`)

Source: Typography Desktop variables, Figma node `40007572:310`. Library type-scale class names referenced below (Display / Headline / Title / Body / Label) come from this node.

### Applied

| # | Token | Before | After | Δ | Library mapping | Status |
|---|---|---|---|---|---|---|
| 1 | `--fs-h2` | 39.06 px | **40 px** | **+0.94 px** | Headline Large (40 px) | ✅ Applied |
| 2 | `--fs-h3` | 31.25 px | **32 px** | **+0.75 px** | Headline Small (32 px) | ✅ Applied |
| 3 | `--fs-h4` | 25 px | 25 px | 0 | Title Medium (25 px) — exact | ✅ Annotated (no value change) |
| 4 | `--fs-h5` | 20 px | **22 px** | **+2 px** | Title Small (22 px) — tie-break vs Body XLarge 18 px, chose to keep token in heading class | ✅ Applied |
| 5 | `--fs-link` | 16 px | 16 px | 0 | Body Large / Label XLarge (16 px) — exact | ✅ Annotated (no value change) |
| 6 | `--fs-body` | 16 px | 16 px | 0 | Body Large (16 px) — exact | ✅ Annotated (no value change) |
| 7 | `--fs-small` | 14 px | 14 px | 0 | Body Medium / Label Large / Code Medium (14 px) — exact | ✅ Annotated (no value change) |
| 8 | `--fs-xs` | 12 px | 12 px | 0 | Body Small / Label Medium / Code Small (12 px) — exact | ✅ Annotated (no value change) |

**Blast radius of value changes** (#1, #2, #4):
`--fs-h2` is referenced 5× in `index.html`; `--fs-h3` 5×; `--fs-h5` 2×. #1 and #2 grow under 1 px each. #4 grows 2 px — slightly more noticeable on whatever H5-class elements use it (worth eyeballing the rendered prototype to confirm nothing wraps awkwardly).

### Deferred

_None remaining in the font-size scale._

### Not yet measured in this session

These came up in the same Figma fetch but are line-heights, weights, and families — held for a separate review pass:

- **Line-heights** (`--lh-*`): library binds absolute-px line-heights per size class; Search Labs uses ratios. Conversion shows the library trends tighter (1.2–1.5 vs Search Labs' 1.4–1.6). All `--lh-*` deltas pending review.
- **`--font-mono: 'IBM Plex Mono'`** vs library **Roboto Mono** — family-level swap, different metric/feel. Pending.
- **Font weights**: library locks Body / Label / Code at 500 (Medium) and headings at 750 (ExtraBold) / 800 (Heavy for Oversize class). Search Labs uses defaults / 700 for most. Note: a `--fw-body: 500` experiment was previously tried and reverted (logged in `HANDOFF.md`). Pending.

### Not yet pulled from Figma

Outside the Typography Desktop scope; need separate Figma node selections to fetch:

- **Color palette** (brand, neutrals, text, surface, border) — only two text-color tokens leaked into the typography fetch: `color/text/default: #14141b` and `color/text/subtler: #343544`. Library text colors look meaningfully darker than Search Labs `--gray-dark: #2E3137` and `--gray: #6B7079`, but no decision yet without seeing the full palette in context.
- **Spacing scale** (`--s-*`)
- **Radii** (`--r-*`)

---

## 2026-05-14 — Session 2: Line-height scale (`--lh-*`)

Source: same Typography Desktop fetch as Session 1. Library binds line-heights as absolute px paired to each font size; the table below shows the equivalent ratio at the matched Search Labs size, since the existing `--lh-*` tokens are unitless ratios.

### Applied

| # | Token | Before | After | Δ (ratio) | Δ (≈ px at new size) | Library mapping | Status |
|---|---|---|---|---|---|---|---|
| 1 | `--lh-h2` | 1.4 | **1.2** | −0.2 | ≈ −8 px @ 40 px | Headline Large (48 px / 40 px) | ✅ Applied |
| 2 | `--lh-h3` | 1.5 | **1.2** | −0.3 | ≈ −9.6 px @ 32 px | Headline Small (38.4 px / 32 px) | ✅ Applied |
| 3 | `--lh-h4` | 1.5 | **1.3** | −0.2 | ≈ −5 px @ 25 px | Title Medium (32.5 px / 25 px) | ✅ Applied |
| 4 | `--lh-h5` | 1.5 | **1.3** | −0.2 | ≈ −4.4 px @ 22 px | Title Small (28.6 px / 22 px) — locked to fs-h5 = 22 px from Session 1 | ✅ Applied |
| 5 | `--lh-link` | 1.5 | 1.5 | 0 | 0 | Body Large (24 px / 16 px) — exact | ✅ Annotated (no value change) |
| 6 | `--lh-body` | 1.6 | **1.5** | −0.1 | ≈ −1.6 px @ 16 px | Body Large (24 px / 16 px) | ✅ Applied |
| 7 | `--lh-small` | 1.6 | **1.5** | −0.1 | ≈ −1.4 px @ 14 px | Body Medium (21 px / 14 px) | ✅ Applied |
| 8 | `--lh-xs` | 1.6 | **1.5** | −0.1 | ≈ −1.2 px @ 12 px | Body Small (18 px / 12 px) | ✅ Applied (token has 0 active usages; annotated for completeness) |

**Blast radius** in `index.html`: `--lh-h2` 5×, `--lh-h3` 5×, `--lh-h4` 1×, `--lh-h5` 1×, `--lh-link` 1×, `--lh-body` 2×, `--lh-small` 1×, `--lh-xs` 0×. The most visible change will be on the H2/H3 hero headings — line-height drops by ≈ 8–10 px, which is significant for multi-line titles. Eyeball the hero rows on `#home`, `#observability`, `#security`, and `#company` to confirm nothing crowds.

### Deferred

_None._ Full line-height scale migrated in one pass.

---

## 2026-05-14 — Session 3: Font families (`--font-*`)

Source: same Typography Desktop fetch. Library font-family declarations: Mier B (headings), Inter (body / label), Roboto Mono (code).

### Applied

| # | Token | Before | After | Δ | Library mapping | Status |
|---|---|---|---|---|---|---|
| 1 | `--font-heading` | `'Mier B', 'Inter', system-ui, sans-serif` | _(unchanged)_ | 0 | Mier B — exact | ✅ Annotated (no value change) |
| 2 | `--font-body` | `'Inter', system-ui, sans-serif` | _(unchanged)_ | 0 | Inter — exact | ✅ Annotated (no value change) |
| 3 | `--font-mono` | `'IBM Plex Mono', ui-monospace, monospace` | **`'Roboto Mono', ui-monospace, monospace`** | family swap | Roboto Mono | ✅ Applied |

### Side-effects of the mono swap (not pure-token changes; also applied this session)

| Where | Change |
|---|---|
| `index.html` line 9 | Google Fonts `<link>` rewritten: `family=IBM+Plex+Mono:wght@500;700` → `family=Roboto+Mono:wght@500;700` |
| `components.html` line 9 | Same `<link>` rewrite |
| `components.html` typography section copy | "Code samples use IBM Plex Mono." → "Code samples use Roboto Mono." (docs prose accuracy) |

**Blast radius**: `--font-mono` is referenced 23× in `index.html` — every `<code>` block, mono label, and metadata pill that uses the variable will render in the new face. Roboto Mono is wider per glyph and more utilitarian than IBM Plex Mono; line lengths in code blocks may shift slightly. Worth eyeballing any narrow-column code samples.

### Open observation worth flagging (no action)

**Mier B never actually renders today.** It's declared as the first font in `--font-heading`, but neither `index.html` nor `components.html` loads a Mier B web font (Google Fonts only serves Inter and IBM Plex Mono / now Roboto Mono). Every heading currently falls back to Inter. The library spec also names Mier B for headings, so the migration is correct on paper — but until Mier B is actually loaded (self-hosted `@font-face` or licensed CDN), the prototype's headings render in Inter regardless. Out of scope for the token migration; logged here as context.

### Deferred

_None._ Family migration complete.

---

## 2026-05-14 — Session 4: Body font weight (EXPERIMENT)

Source: same Typography Desktop fetch (Figma node `40007572:310`). The library binds all Body / Label / Code text styles to weight **500 (Medium)**. Search Labs has no `--fw-*` token today; body copy inherits the browser default `400 (Regular)`.

**This entry is under the EXPERIMENT pattern** (per `HANDOFF.md` § "Design-system migration"), not a committed migration. It was previously tried and reverted; we are re-running with the documented safety-net pattern so a clean revert is one step.

### Applied

| # | Token | Before | After | Δ | Library mapping | Status |
|---|---|---|---|---|---|---|
| 1 | `--fw-body` | _undefined_ (browser default 400) | **500** | +100 weight | Body / Label / Code Medium (500) | 🧪 Experiment |

### Where the experiment lives (3 places, all required)

| File | Change |
|---|---|
| `design-system.css` | New `--fw-body: 500;` in `:root`, wrapped in an `EXPERIMENT 2026-05-14` comment block with full revert steps. |
| `index.html` | (a) Inline fallback `<style>` block inserted between the Google Fonts `<link>` (line 9) and the `design-system.css <link>` (now line 17) that redeclares `--fw-body: 500` — safety net if the external CSS fails to load. (b) `body { ... }` rule updated: `font-weight: var(--fw-body, 400);` — third-level fallback (400) engages if the variable itself is removed. |
| `components.html` | Same inline fallback `<style>` block. Same `body { ... }` rule update so the docs page renders in the experimental weight. |

### Revert in one step (if rejected)

1. Delete the `--fw-body: 500;` line in `design-system.css`.
2. Delete the inline fallback `<style>` block in `index.html` and `components.html`.
3. Optionally also remove the `font-weight: var(--fw-body, 400)` line from the two `body { ... }` rules — leaving it does no harm (it falls back to 400, same as default).

### Why this got reverted last time (context, not analysis)

`HANDOFF.md` notes only that the body Medium weight was "tested, reverted to baseline at user request." The reservations behind that decision aren't recorded. Likely candidates: body copy felt too heavy / lost contrast with bold text; Inter Medium at 16 px on light backgrounds reads denser than 400, and the heading-vs-body weight gap collapses if both are weighty. Worth watching for those specifically when reviewing.

### Blast radius

The base `body { ... }` rule is the cascade origin for almost all running text. Every paragraph, list item, link, and inline element that doesn't have an explicit `font-weight` override (~36+ explicit `font-weight: 700` / `600` / `500` declarations exist in `index.html` and continue to win by specificity for headings, buttons, chips, etc.) will pick up the new 500 weight. Net effect: paragraphs and inline copy look noticeably heavier; UI elements and headings are unchanged.

### Deferred

_None for this experiment._ Heading weights (`750 ExtraBold`) and the Oversize Heavy class (`800`) are still measured-but-not-applied; not part of this experiment, see open list below.

---

## 2026-05-14 — Session 5: Color system (REPLACEMENT — Neutral + Brand surface families)

Source: Figma nodes `40002315:2269` (Backgrounds — Neutral), `40002315:3146` (Text — Neutral), `40002315:3420` (Examples — Neutral), `40002315:15484` (Rule #1 — WCAG AA/AAA), `40002315:15404` (Examples — Brand), `40002315:1999` (Text — Brand), `40002315:2706` (Backgrounds — Brand).

**Structural change**, not a delta-snap migration. The Elastic library organizes color as two parallel surface families (Neutral grays + Brand blues), a shared text scale, plus standalone brand colors — none of which match the Search Labs token shape (13 semantically named tokens with no surface/text separation). User chose "Replacement" over "Additive," so the new system is now canonical.

### Decision: documentation values vs variable bindings

Light grays differ meaningfully between the two sources. Picked **documentation card values** (e.g. Gray 50 = `#E2E3E7`); the variable bindings (e.g. `color/neutral/50 = #f6f9fc`) collapse the lightest surface to near-white, which defeats the purpose of having a distinct Gray 50 token. Variable-binding alternates are noted inline in `design-system.css` for one-line reverts. Perceptual deltas between the two sources: lights ΔE ≈ 9–10 (noticeable), darks ΔE ≈ 2–4 (negligible).

### New canonical tokens introduced

| Group | Tokens | Values |
|---|---|---|
| **Neutral surfaces (light)** | `--surface-neutral-1` / `-2` / `-3` | `#FFFFFF` / `#E2E3E7` / `#C4C7D1` |
| **Neutral surfaces (dark)** | `--surface-neutral-dark-1` / `-2` / `-3` | `#373A45` / `#1D1F26` / `#000000` |
| **Brand surfaces (light)** | `--surface-brand-1` / `-2` / `-3` | `#D9E8FF` / `#BFDBFF` / `#A3CBFF` |
| **Brand surfaces (dark)** | `--surface-brand-dark-1` / `-2` / `-3` | `#123778` / `#0D2F5E` / `#0A2342` |
| **Text on light** | `--text-default` / `--text-subtler` / `--text-subtlest` | `#1D1F26` / `#373A45` / `#474B57` |
| **Text on dark** | `--text-inverse-default` / `-subtler` / `-subtlest` | `#E2E3E7` / `#C4C7D1` / `#B2B6C2` |
| **Brand colors** | `--brand-elastic-blue` / `--brand-elastic-yellow` / `--brand-elastic-yellow-dark` / `--brand-elastic-yellow-light` | `#0B64DD` / `#FEC514` / `#FDC031` / `#FFDA3D` |

### Legacy Search Labs tokens — aliased (soft replacement)

All 13 original color tokens are preserved as **aliases** that redirect to the new system. This keeps the ~370 existing `var()` references rendering without per-selector rewrites. The visible delta is what Search Labs absorbs.

| Legacy token | Before | Now resolves to | Δ | Uses* |
|---|---|---|---|---|
| `--white` | `#FFFFFF` | `--surface-neutral-1` → `#FFFFFF` | 0 | 39 |
| `--black` | `#0B0C0E` | `--surface-neutral-dark-3` → `#000000` | slightly darker | 11 |
| `--footer-bg` | `#0E0F10` | `--surface-neutral-dark-3` → `#000000` | slightly darker | 0 |
| `--gray-dark` | `#2E3137` (body text) | `--text-default` → `#1D1F26` | darker, +AAA contrast | 82 |
| `--gray` | `#6B7079` (secondary text) | `--text-subtlest` → `#474B57` | **noticeably darker** | **137 — biggest visual shift** |
| `--gray-medium` | `#C8D1E9` | `--surface-neutral-3` → `#C4C7D1` | tiny, slightly cooler | 5 |
| `--gray-lightest` | `#F5F8FB` (section bg) | `--surface-neutral-2` → `#E2E3E7` | **visibly grayer sections** | 12 |
| `--border` | `#E6ECF3` | `--surface-neutral-3` → `#C4C7D1` | borders darker | 52 |
| `--hero-bg` | `#F5F8FB` | `--surface-neutral-2` → `#E2E3E7` | same Δ as `--gray-lightest` | 6 |
| `--primary` | `#FEC514` | `--brand-elastic-yellow` → `#FEC514` | 0 | 1 |
| `--primary-dark` | `#FDC031` | `--brand-elastic-yellow-dark` → `#FDC031` | 0 | 1 |
| `--yellow-light` | `#FFDA3D` | `--brand-elastic-yellow-light` → `#FFDA3D` | 0 | 0 |
| `--blue` | `#0B64DD` | `--brand-elastic-blue` → `#0B64DD` | 0 | 22 |

\* Combined `index.html` + `components.html` `var()` counts at time of migration.

### Where to expect the biggest visual changes

1. **All secondary text (`--gray`, 137 uses)** drops from `#6B7079` to `#474B57` — a substantial darken. Article subtitles, dates, metadata pills, "by Author" lines, etc. will read as more emphasized and higher-contrast.
2. **Section backgrounds (`--gray-lightest` + `--hero-bg`, 18 combined uses)** shift from a cool near-white `#F5F8FB` to a real gray `#E2E3E7`. The hero panel, Examples and Integrations section backgrounds, and similar will read as more distinctly "framed."
3. **Borders (`--border`, 52 uses)** darken from `#E6ECF3` to `#C4C7D1`. Card outlines, hairlines between sections, etc. become more visible.
4. **Body text (`--gray-dark`, 82 uses)** darkens slightly from `#2E3137` to `#1D1F26`. Subtle but pushes contrast deeper into AAA territory.
5. **`--black` (11 uses)** drops the brand-tinted near-black `#0B0C0E` for pure `#000000`. Hardly noticeable.

### How to revert

- **Whole migration**: restore the Brand + Neutrals blocks from before this session, delete the surface/text/brand blocks plus the alias block. The original Search Labs hex values are preserved inline in the comments next to each alias.
- **Single token**: change one alias line back to a literal hex from its comment, e.g. `--gray: #6B7079;` instead of `var(--text-subtlest)`. The new tokens stay in the file for any code that opted into them.
- **Variable-binding values for light grays**: replace the docs hex on a `--surface-neutral-*` line with the `Alt:` value from the inline comment. Aliases keep working unchanged.

### Side-effects (none required this session)

No selector rewrites — every existing rule references legacy tokens, which still resolve. New components added later should reach directly for `--surface-*` / `--text-*` instead of `--white` / `--gray-dark`. Over time, individual selectors can be migrated; once all references to a legacy alias are gone, the alias line can be deleted.

---

## 2026-05-14 — Session 6: Design fix — `.cat-card__title` size match

Not a Figma-driven migration — a per-component visual alignment requested in chat. Logged here so every change is traceable.

### Applied

| Selector | Property | Before | After | Why |
|---|---|---|---|---|
| `.cat-card__title` | `font-size` | `var(--fs-h4)` (25 px) | `var(--fs-h5)` (22 px) | Match `.popular__title` size on the home page. The category cards (Tutorials / Examples / Integration / Blog) and the Popular sidebar items now read at the same scale. |
| `.cat-card__title` | `line-height` | literal `1.2` | `var(--lh-h5)` (1.3) | Use the matching line-height token so the two titles align on both axes; also brings the cat-card off a magic-number literal and onto the token. |

**File touched**: `index.html` only (component-level style inside the inline `<style>` block). No `design-system.css` change.

**Blast radius**: the four `.cat-card__title` `<h3>` instances on the Search Labs home page (`#home`). No other view uses this class.

**Note on design intent**: previously `.cat-card__title` was 3 px larger than `.popular__title`, which gave the category cards extra prominence as wayfinding UI. The new alignment treats both as peer headings. If the cards feel under-emphasized after this change, the one-line revert is `font-size: var(--fs-h4); line-height: 1.2;`.

---

## 2026-05-14 — Session 7: Spacing + Radii

Source: Figma file Design-System-Tokens-and-Variables-Test-AI → page **Primitive: Dimension**. Data read from user-provided screenshots (the page uses raw text rows rather than bound variables, so `get_variable_defs` returned nothing for it).

### Library inventory (Primitive: Dimension)

| Section | Values |
|---|---|
| viewport | `xs:320 sm:768 md:1024 lg:1280 xl:1440 2xl:1920` |
| border / width | `0 1 2 3 4` |
| radius | `full: 9999` (semantic radii like `radius/md:8` defined elsewhere, map back to `dimension/*`) |
| dimension (primitives) | `0 1 2 3 4 6 8 12 16 24 32 40 48 64 80 96 128 160 192 224 256 320` |
| dimension / icon / size | `xs:12 sm:16 md:20 lg:24 xl:32 2xl:48` |
| dimension / negative | `-4 -6 -8 -12 -16 -20 -24 -32` |

Anomaly noted: a `"2 2": -2` row in the dimension section, ignored as a likely misnamed Figma artifact.

### Applied — Spacing (`--s-*`)

| # | Token | Before | After | Δ | Library mapping | Status |
|---|---|---|---|---|---|---|
| 1 | `--s-4` | 4 px | 4 px | 0 | dimension/4 — exact | ✅ Annotated (no value change) |
| 2 | `--s-8` | 8 px | 8 px | 0 | dimension/8 — exact | ✅ Annotated |
| 3 | `--s-12` | 12 px | 12 px | 0 | dimension/12 — exact | ✅ Annotated |
| 4 | `--s-16` | 16 px | 16 px | 0 | dimension/16 — exact | ✅ Annotated |
| 5 | `--s-24` | 24 px | 24 px | 0 | dimension/24 — exact | ✅ Annotated |
| 6 | `--s-32` | 32 px | 32 px | 0 | dimension/32 — exact | ✅ Annotated |
| 7 | `--s-48` | 48 px | 48 px | 0 | dimension/48 — exact | ✅ Annotated |
| 8 | `--s-64` | 64 px | 64 px | 0 | dimension/64 — exact | ✅ Annotated |

Perfect match across the board. No value changes — Search Labs' spacing scale is fully contained within the library's primitive dimension scale.

### Applied — Radii (`--r-*`)

| # | Token | Before | After | Δ | Library mapping | Status |
|---|---|---|---|---|---|---|
| 1 | `--r-4` | 4 px | 4 px | 0 | dimension/4 — exact | ✅ Annotated |
| 2 | `--r-8` | 8 px | 8 px | 0 | dimension/8 (and semantic `radius/md`) — exact | ✅ Annotated |
| 3 | `--r-10` | 10 px | **8 px** (alias to `--r-8`) | **−2 px** | No library match; tie-broken to dimension/8 over dimension/12 per user pick. Consolidates two Search Labs radii into one library value. | ✅ Applied |
| 4 | `--r-full` | 9999 px | 9999 px | 0 | radius/full — exact | ✅ Annotated |

### Blast radius of `--r-10 → 8 px`

Three card components shift from 10 px → 8 px corners. All cards in the prototype that use these classes will read as slightly less rounded:
- `.featured` — the home page featured card (line 219 of `index.html`).
- `.related-card` — the standard article card pattern used across home pages, tag pages, and related-articles strips. The highest-traffic card in the prototype.
- `.blog-feed__featured` — the corporate Blog featured card (line 1985).

### Related cleanup opportunity (not done this session)

There is one **literal `border-radius: 10px`** at line 626 (`.qs-callout`, the teal-to-blue sidebar Quick Start callout). It bypasses the token system, so after this session the cards have 8 px corners while this callout still has 10 px. The callout's block uses literals for several properties (`gap: 18px`, `padding: 26px 24px`), so the whole component is un-migrated; touching just one literal would be inconsistent. Logged as a follow-up — recommended to either migrate the whole block to tokens, or change just the radius to `var(--r-10)` if you want it to follow the consolidation.

### New scales available but not introduced

Per user pick, these were **annotated only** (in the comment header of the spacing block) and not added as new tokens. Each is additive and would not change any existing rendering.

- **viewport breakpoints** (`xs:320 sm:768 md:1024 lg:1280 xl:1440 2xl:1920`). Could replace literal `@media (max-width: 1024px)` / `(max-width: 720px)` breakpoints scattered in the CSS.
- **border widths** (`0 1 2 3 4`). Search Labs uses literal `1px solid var(--border)` in 50+ places. Could be `--bw-1` etc.
- **icon sizes** (`xs:12 sm:16 md:20 lg:24 xl:32 2xl:48`). Search Labs has no icon system; this would be the scale if added.
- **negative dimensions** (`-4 -6 -8 -12 -16 -20 -24 -32`). For negative margins / offsets.
- **extra dimension primitives** (`0 1 2 3 6 40 80 96 128 160 192 224 256 320`). Search Labs uses some as literals (e.g. `max-width: 1512px`, `height: 433px`); not tokenized today.

---

## 2026-05-14 — Session 8: Literal hex cleanup — `#6B7079` (the stale pre-migration `--gray`)

A targeted cleanup: 12 places hardcoded the value `#6B7079`, the pre-Session-5 value of `--gray`. After Session 5 those literals stayed at the old gray while every `var(--gray)` reference migrated to `#474B57`. This session fixes that silent inconsistency.

### Applied — `index.html` (9 sites)

All 9 occurrences migrated to `var(--gray)` (which now resolves to `#474B57`). Mechanical bulk replace; no judgment calls per site. Each site darkens from `#6B7079` to `#474B57`.

| Line | Selector / context | Replacement |
|---|---|---|
| 1813 | secondary text `color` | `color: var(--gray);` |
| 1888 | secondary text `color` | `color: var(--gray);` |
| 1947 | secondary text `color` | `color: var(--gray);` |
| 1961 | secondary text `color` | `color: var(--gray);` |
| 1968 | secondary text `color` | `color: var(--gray);` |
| 2095 | secondary text `color` | `color: var(--gray);` |
| 2466 | `.tok-comment` (code-syntax comment color) | `color: var(--gray);` + updated stale `4.9:1` annotation to note the new AAA contrast ratio |
| 2750 | gradient stop — `linear-gradient(to right, #343741 0%, #343741 86%, #6B7079 100%)` | `... var(--gray) 100%)` |
| 2768 | `.gnav__util-divider` background (0.4 opacity divider in the gnav) | `background: var(--gray);` |

### Applied — `components.html` (3 sites)

| Line | Context | Replacement |
|---|---|---|
| 203 | `.code .comm` (code-syntax comment color in docs) | `color: var(--gray);` |
| 419 | `--gray` docs swatch inline style — visual color block | `style="background: var(--gray);"` |
| 420 | `--gray` docs swatch **text label** — the literal hex displayed to the reader | `#6B7079` → `#474B57` (label kept as a literal hex on purpose; it's documentation describing the current value, not a CSS reference). WCAG note also updated from "Meets WCAG AA" to "~9.5:1 (AAA)". |

### Why each case was handled differently

- Six plain `color:` rules and the divider/gradient were uniform sed replacements — same pattern, no judgment needed.
- The `.tok-comment` carried an inline contrast annotation (`/* # ... — 4.9:1 */`) that became stale after the value darkened. The comment was rewritten to record both the new ratio and the historical context.
- The docs swatch in `components.html` is documentation **describing** the system, not consuming it. The visual block (line 419) now consumes the token. The text label (line 420) is a static description of the current value, so it became the new literal `#474B57`, with a history-preserving note (`Was #6B7079 pre-migration`).

### Residue check

Three references to `#6B7079` remain in the bundle. All three are inside **comment / documentation text** describing the migration history:
- `index.html:2466` — inside the `.tok-comment` annotation.
- `components.html:420` — inside the docs swatch note ("Was #6B7079 pre-migration").
- `design-system.css:88` — inside the `--gray` alias comment ("was #6B7079 secondary text…").

These are intentional and the prototype renders identically with or without them.

### Verification

```
grep #6B7079 across the bundle → 3 matches, all in comments
<div balance in index.html → 895 / 895
.page-view count → 21
```

### Blast radius (rendered)

Twelve places that previously rendered `#6B7079` (a mid-gray, ~4.9:1 contrast on white) now render `#474B57` (~9.5:1 AAA). The biggest perceptual shifts:
- Two text labels in the corporate-blog area (`color:` rules around lines 1947/1961/1968) darken visibly.
- The `.gnav__util-divider` (small vertical separators in the top utility row) darken slightly — partly masked by the 0.4 opacity.
- The 3-stop gradient at line 2750 — previously `#343741 → #343741 → #6B7079` (steady-then-lighten) — now becomes `#343741 → #343741 → #474B57`, which is a much subtler ramp (the two endpoints are closer in luminance). The gradient effectively flattens.

If the gradient shift looks wrong, the per-site revert is changing `var(--gray) 100%` back to `#6B7079 100%` on that one line.

---

## 2026-05-14 — Session 9: Button icon support (Figma 994:3326)

Source: Figma node `994:3326` (Primary button) in the Components — Designers Area file (`Nsk35nyWrPgLKUo9ah4mD4`). The design shows the primary button with an optional 16 px icon adjacent to the label (`showIconLeft` / `showIconRight` props in the Figma component). The button shell already matches Search Labs' `.btn--primary` exactly on height (40 px), padding (0 16 px), gap (4 px), radius (4 px), background (`#0B64DD`), text color (white), and Inter Medium 16 / 24. The implementation gap was just that no Search Labs button instance opted into the icon slot.

### What changed

Three button instances in `index.html` were given a trailing 16 px icon. No CSS changes — `.btn--primary` already uses `display: inline-flex; gap: 4px;` so the icon lays out automatically. Icons are inline SVG with `stroke="currentColor"` so they inherit the button's text color (white on primary).

| Line | Selector / button | Icon added | Why this icon |
|---|---|---|---|
| 2895 (gnav top-right) | `.btn--primary` — "Start free trial" | arrow-right (→) | Forward-motion CTA; matches the existing arrow on `.btn--primary` "Try it yourself" (line 7082) |
| 3075 (home — Examples/Integrations section) | `.btn--primary` — "Elasticsearch Quick Start" | external-link (↗) | Matches the same icon used on the two other "Quick Start" buttons (lines 3614, 3701) which open in new tabs |
| 7107 (footer/bottom CTA) | `.btn--primary` — "Start free trial" | arrow-right (→) | Same as the gnav button for consistency |

### What deliberately did NOT change

- **Secondary buttons** ("Contact sales" in the gnav, "Subscribe to newsletter" ×3) — per design intent that secondary in a pair shouldn't compete with the primary's icon. Cleaner visual hierarchy.
- **Buttons already with icons** — three primary buttons had icons before this session ("Quick Start" ×2 on tutorial/example pages, "Try it yourself", and the footer tertiary "Contact sales"). Left as they were.
- **Button shell CSS** — the existing `.btn--primary` already meets the Figma spec; the gap, height, padding, radius, font, and color all match. No change to `design-system.css`.

### Icon convention used (for future buttons)

- Inline SVG, `width="16" height="16"`, `viewBox="0 0 24 24"`, `stroke="currentColor"`, `stroke-width="2"`, rounded caps/joins.
- Placed **after** the label inside the anchor: `<a class="btn--primary"><span>Label</span><svg>…</svg></a>`.
- Label wrapped in `<span>` so it's a flex child alongside the SVG (matches the existing pattern at line 7082).
- `aria-hidden="true"` on the SVG — the label conveys meaning; the icon is decorative.

Two paths if you want the icon left-of-label instead: put the SVG before the `<span>`, or use `flex-direction: row-reverse` on the specific button instance. No new CSS needed either way.

### Open follow-up

The `components.html` design-system docs page still shows only **text-only** button examples (lines 544, 567, 590). After this session the prototype demonstrates a documented icon-variant pattern that the docs don't describe. Quick fix: add three more rows to the buttons section showing primary-with-trailing-icon, primary-with-leading-icon, and primary-with-both-icons. Not done this session; flagged as a docs gap.

---

## 2026-05-14 — Session 10: Divider + Breadcrumb (Figma 1629:1125 + 1676:4101)

Source: Figma nodes `1629:1125` (Divider) and `1676:4101` (Breadcrumb) in the Components — Designers Area file (`Nsk35nyWrPgLKUo9ah4mD4`).

### What was added

Two new components in `design-system.css`, plus matching docs sections in `components.html`. Neither changes any existing rendering in `index.html` — these are additive.

**Divider** — a 1 px horizontal hairline.

```css
.divider { display: block; height: 1px; width: 100%; background: var(--border); border: none; margin: 0; }
```

Usage: `<hr class="divider" />` (preferred for semantic horizontal rules) or `<div class="divider"></div>`.

Note on color: Figma binds the line to `rgba(29,31,38,0.2)` — a semi-transparent overlay. The implementation uses the migrated `--border` token (solid `#C4C7D1`) for token-system consistency. The two render virtually identically on white surfaces (ΔE ≈ small). To switch to the Figma semi-transparent value, replace `var(--border)` in the rule.

**Breadcrumb** — horizontal nav trail.

```css
.breadcrumb        { display: inline-flex; align-items: center; flex-wrap: wrap; font: 500 14px/1.5 var(--font-body); gap: 4px; }
.breadcrumb__item  { display: inline-flex; align-items: center; gap: 4px; color: var(--text-subtlest); text-decoration: none; }
a.breadcrumb__item:hover           { text-decoration: underline; }
.breadcrumb__item--current         { color: var(--brand-elastic-blue); cursor: default; }
.breadcrumb__sep                   { color: var(--text-subtlest); user-select: none; }
```

Usage:
```html
<nav class="breadcrumb" aria-label="Breadcrumb">
  <a href="#" class="breadcrumb__item">Text Link</a>
  <span class="breadcrumb__sep" aria-hidden="true">/</span>
  <a href="#" class="breadcrumb__item">Text Link</a>
  <span class="breadcrumb__sep" aria-hidden="true">/</span>
  <span class="breadcrumb__item breadcrumb__item--current" aria-current="page">Text Link</span>
</nav>
```

Items may carry optional 16 px leading and/or trailing icons (16-px SVG, `stroke="currentColor"`, `aria-hidden="true"`). The `inline-flex` + 4 px gap handles icon layout automatically.

### `components.html` additions

- New `#divider` docs section (line 654 onward) — stage with default rule, stage with rule inside a card-like container, HTML snippet, spec table.
- New `#breadcrumb` docs section (line 694 onward) — three stages (3-item / 2-item / with trailing icon), HTML snippet, spec table.

### Existing prototype patterns NOT migrated (flagged as follow-up)

The prototype already has two breadcrumb-shaped components used in 7 places, each with **different** styling:

| Selector | Style | Used | Locations |
|---|---|---|---|
| `.article-hero__crumb` | Mono font, weight 700, 12 px, brand blue, all-caps via tracking | 4× | lines 4776, 4925, 5705, 6951 (article hero blocks across views) |
| `.tutorial-hero__crumb` | Custom style (see line 2318) | 3× | lines 4387, 4512, 4616 (tutorial / integration / example hero blocks) |

These are deliberately untouched this session. The new `.breadcrumb` component is a different visual language (Inter Medium gray + blue, no caps, slash separator) — it's not a drop-in replacement. If you want to unify all breadcrumb instances on the new component, that's a separate refactor; the existing hero crumbs would lose their mono/uppercase feel.

### No CSS conflicts

Class names `.divider`, `.breadcrumb`, `.breadcrumb__item`, `.breadcrumb__sep` did not previously exist in `index.html`, `design-system.css`, or `components.html`. Pure additions.

### Verification

```
brace balance in design-system.css → 19 / 19
<section in components.html       → 8 / 8
<div in components.html           → 75 / 75
<div in index.html (unchanged)    → 895 / 895
.page-view in index.html          → 21
```

---

## 2026-05-14 — Session 11: `components.html` documentation sync

A docs-only round. No tokens or component CSS changed; this session caught `components.html` up with the design-system state that had drifted across Sessions 1–10.

### What was stale and got fixed

| Area | Before | After |
|---|---|---|
| **Colors** | 11 Search Labs swatches with pre-migration hex values. New surface, text, and brand-color systems undocumented. | Four sub-sections: Brand colors (4), Neutral surfaces (6), Brand surfaces (6), Text scale (6) — all with live `var(--…)` swatches. Plus a 13-row legacy-aliases table showing each alias's new resolved value alongside its pre-migration value (so the delta Search Labs absorbed is visible). |
| **Typography** | `--fs-h2` labeled "39.06px / 1.4"; sample column rendered the actual migrated 40 / 1.2 size. Spec column lied. `--fs-link` absent. Font families and weights not documented. | All 8 `--fs-*` rows now show their migrated values. New `--fs-link` row added. Added: "Library class" column mapping each token to the Elastic type-style name (Headline Large, Title Medium, etc.). Two new sub-sections: Font families (3 rows) and Font weights (with the `--fw-body: 500` experiment flagged as 🧪). |
| **Spacing** | Generic 4 / 8-based scale description. | Same scale, plus an explicit note that every `--s-*` is an exact match to a library primitive (Δ 0 across the board, confirmed in Session 7). |
| **Radius** | `--r-10` shown as `10px`. | `--r-10` row now reads "8px _(alias to --r-8)_" with a note explaining the consolidation. |
| **Buttons** | Three text-only variants (Primary / Secondary / Tertiary) only. No icon variant docs even though icons were added to the prototype in Session 9. | New "Icon variants" sub-section under Primary, showing trailing icon, leading icon, and both-sides icon examples with copy-pasteable HTML snippets and a spec table. Sources the pattern back to Figma node 994:3326. |

### What was already accurate

- Divider (added Session 10) — unchanged.
- Breadcrumb (added Session 10) — unchanged.
- Chips section — unchanged; values already align with the design system.
- The single `--gray` swatch was already updated in Session 8 — preserved.

### Verification

```
<div in components.html → 116 / 116 balanced
<section in components.html → 8 / 8
<table in components.html → 6 / 6
swatches → 22 in colors section + 1 in chip note
sections → colors, typography, spacing, radius, buttons, chips, divider, breadcrumb
```

### What still isn't documented in `components.html` (deliberate gaps)

- **The 7 existing breadcrumb instances** in `index.html` (`.article-hero__crumb` × 4, `.tutorial-hero__crumb` × 3) — these use a different visual language from the new `.breadcrumb` component and aren't aliased/migrated. Flagged in Session 10's notes; would be a separate refactor.
- **Hero patterns** (`.article-hero`, `.subpage-hero`, `.tutorial-hero`, `.author-hero`) — used heavily in `index.html` but never extracted into the docs. These would each warrant their own docs sections if you want full coverage.
- **Card patterns** (`.featured`, `.related-card`, `.popular`, `.cat-card`, `.blog-feed__featured`) — same; live in `index.html`'s inline `<style>`, not documented in `components.html`.
- **Newsletter, callouts, footer, gnav, topnav, subnav, page-view layout** — same.

`components.html` today documents the **design-system primitives** (colors, type, spacing, radius) and the **shared atomic components** (buttons, chips, divider, breadcrumb). Larger composed patterns like cards and heroes still live exclusively as inline CSS inside `index.html`. Extracting them into the docs is the next natural step but wasn't in scope for this docs-sync round.

---

## 2026-05-14 — Session 11.1: Swatch render fix

Hot fix to Session 11. The colors section rewrite had set every swatch background to `style="background: var(--token);"` (inline CSS variable reference). In the user's preview context, those `var()` references inside inline `style=""` attributes don't resolve, so every swatch rendered with no background — visually empty cards above the metadata.

The same `var()` references work fine in stylesheet rules (the `<style>` block in `components.html` and `design-system.css` itself), so the rest of the page chrome rendered correctly. Only the inline-style swatch backgrounds failed.

### What changed

24 `var(--token)` references inside `swatch__color` inline styles were replaced with literal hex values. Two of those were `var(--border)` inside `box-shadow` declarations on the light/white swatches; the rest were the 22 background references.

```html
<!-- Before (Session 11) — broken in preview contexts -->
<div class="swatch__color" style="background: var(--brand-elastic-blue);"></div>

<!-- After (Session 11.1) — renders everywhere -->
<div class="swatch__color" style="background: #0B64DD;"></div>
```

The token name and hex are still documented in the `swatch__meta` block underneath each swatch, so the docs remain complete; only the visual swatch itself uses a literal value now.

### What was left as `var()`

- Class-based CSS rules in the `<style>` block of `components.html` — they work fine.
- Typography samples in the type-scale table (`style="font-family: var(--font-heading); font-size: var(--fs-h2);"`) — these need `var()` to demonstrate the migrated values; they only fail if `design-system.css` doesn't load, which is a different problem (loading) than the inline-`var()` resolution issue.
- The divider demo container (`style="border: 1px solid var(--border); background: var(--white);"`) — same situation; could be hardened similarly if it also renders blank.

### Why this happened

Pre-Session-11 the original colors section used literal hex in inline styles (working). The rewrite switched to `var()` references thinking the design-system tokens would resolve. They don't, in some preview contexts. Lesson: any visual that needs to render in a sandboxed / preview environment must avoid `var()` inside inline `style=""` attributes — keep them in class rules within the file's own `<style>` block, or use literal values.

---

## 2026-05-14 — Session 11.2: `.popular__item` spacing bump

Visual tweak. The home page's "New and popular on Search Labs" sidebar list had its 3 article entries reading too tight — each title sat ~32 px from the next (16 px padding × 2 above/below the hairline).

Change in `index.html` line 280:

```css
.popular__item {
  padding: var(--s-24) 0;   /* was var(--s-16) 0 */
}
```

New spacing: ~48 px between adjacent items (24 px padding × 2). The internal gap inside each item (between chips/date and the title) stays at `var(--s-16)` — each item still reads as one chunk; the change only affects between-item rhythm. Hairline divider at `.popular__item + .popular__item` keeps its job.

Library-aligned: 24 px is the next step on the `--s-*` scale after 16 (Δ 0 to the library's dimension/24 primitive).

---

## 2026-05-14 — Session 12: Font-weight tokens + sweep

The largest token gap by usage count, closed. Pre-Session-12 the prototype had **101** literal `font-weight: NNN;` rules in `index.html` alone (plus 19 in `components.html`, 3 in `design-system.css`). All Bold weights, all SemiBold weights, and all Medium weights now reference canonical tokens.

### New tokens in `design-system.css`

```css
--fw-regular:  400;
--fw-medium:   500;
--fw-semibold: 600;
--fw-bold:     700;
```

The existing experiment token `--fw-body: 500` is **kept distinct**, with the relationship documented in a comment block above the new tokens. Reasoning: `--fw-body` is a semantic alias for the `<body>` element specifically (controls the body-weight experiment in isolation); `--fw-medium` is the general token for any other 500-weight usage. They share a value today; they answer different questions, so toggling `--fw-body` reverts the body experiment without disturbing any other 500 usage.

### Sweep results

| File | Replacements | Notes |
|---|---|---|
| `index.html` | 100 | 63 → `--fw-bold`, 17 → `--fw-semibold`, 17 → `--fw-medium`, 3 → `--fw-regular` |
| `components.html` | 17 | Same proportional mix; sweep skipped `<pre class="code">` blocks (doc snippets) |
| `design-system.css` | 3 | Inside the component rules (chip, etc.) |
| **Total** | **120** | |

### What deliberately wasn't swept

- `font-weight: 800` × 2 (one in `index.html`, one in `components.html`) — one-off uses, not tokenizing for now.
- One stale "feature missing" note in `components.html` line 618 that was *describing* `font-weight: 700` as text inside a `<code>` tag. The regex correctly skipped it (text content, not CSS), but the prose was now factually wrong — the `--fw-bold` token IS introduced — so I rewrote that whole Font weights documentation table.

### `components.html` Font weights docs section rewritten

The previous table had 1 row (`--fw-body`) plus a stale note. The new table has 5 rows: the 4 canonical weight tokens with usage counts, plus the `--fw-body` experiment row clearly marked 🧪 with the new "kept distinct on purpose" explanation. Direct-edit-friendly: each token has a "value", "notes", and the rough usage count from the sweep.

### Why the regex was safe

Pattern: `font-weight: NNN(?=[;\s}])`. This matches the literal `font-weight: 700;` (followed by `;`), `font-weight: 700` then whitespace (line wrap), or `font-weight: 700}` (inline rule terminator). Crucially it does **not** match `var(--fw-body, 400)` — after `400` comes `)`, which isn't in the lookahead class — nor does it match HTML body text like `<code>font-weight: 700</code>` where `<` follows.

### Verification

```
remaining literal font-weight: NNN in CSS context
  index.html        → 1 (the 800 one-off)
  components.html   → 1 (the 800 one-off)
  design-system.css → 0

var() references in index.html
  var(--fw-bold)     → 63
  var(--fw-semibold) → 17
  var(--fw-medium)   → 17
  var(--fw-regular)  → 3
                      ───
                      100  ✓ matches sweep count

structural integrity
  <div in index.html       → 895 / 895
  <div in components.html  → 116 / 116
  page-views in index.html → 21
  brace balance in CSS     → 19 / 19
```

### Visual change

**None.** Every replacement preserves the numeric weight; tokens are pure aliases for now. The change is observable only by inspecting CSS — every heading is still 700, every chip still 500, every card title still 600.

### What this enables

- One-place changes: if the brand later loads Mier B ExtraBold and we want all headings at 800, bump `--fw-bold` and every heading follows.
- The experiment pattern (`--fw-body`) is now visibly clean: it's a *semantic* override that happens to share a value with the general medium token. The pattern can be reused for future style experiments without polluting the general scale.

---

## 2026-05-14 — Session 13: Chip-title optical alignment in card patterns

User-reported visual issue. In the featured card (and related card patterns), the chip's pill outline sat flush with the title's left edge — strictly correct — but the chip's *text* was inset 11 px from the title's text (1 px chip border + 10 px chip horizontal padding). Visually it read as misaligned: title started 11 px to the left of the chip label.

The standard design-system fix is **optical alignment via negative margin**: pull the chip row left by exactly the chip's left visual offset so the chip *text* aligns with the title *text*. The chip pill then slightly overhangs the card padding edge, which is the intentional, on-spec look.

### Applied to

```css
.featured__meta            { margin-left: -11px; }
.related-card__meta        { margin-left: -11px; }
.popular__head             { margin-left: -11px; }
.blog-feed__featured-meta  { margin-left: -11px; }
```

Each rule got a comment block referencing the canonical comment on `.featured__meta` so anyone editing them later knows what the `-11px` is for.

### Where this lands visually

| Component | View(s) affected | Instances |
|---|---|---|
| `.featured__meta` | home, observability, security, blog landing pages | 4 |
| `.related-card__meta` | article, related-articles, recommendation sections across many views | 10+ |
| `.popular__head` | home page "New and popular on Search Labs" sidebar | 3 |
| `.blog-feed__featured-meta` | blog feed landing | 2 |

### What was NOT touched

- `.cat-card` — no chips
- `.explore-row__chips` — chips are *beside* the title link (horizontal layout), not above
- `.examples-row__chips` — same horizontal layout
- The `.chip` definition in `design-system.css` itself — the chip is unchanged; only its parent container is offset

### The 11px magic number

```
1 px chip border + 10 px chip padding-left = 11 px
```

If the chip's padding or border ever change in `design-system.css`, the four `margin-left: -11px` values would need updating to match. Documented in each rule's comment. A future refactor could pull this into a CSS variable (`--chip-optical-offset`) — flagged as a low-priority cleanup.

---

## 2026-05-14 — Session 14: `.featured` nested-anchor antipattern fix + Session 13 revert

User-flagged real bug. In all 4 `.featured` cards, the markup was:

```html
<article class="featured">
  <a href="..." class="featured__link">   ← outer wrapping link
    <div class="featured__image">...</div>
    <div class="featured__body">
      <div class="featured__meta">
        <a href="..." class="chip">APM</a>   ← NESTED <a> — invalid HTML
        ...
      </div>
      <h2 class="featured__title">...</h2>
    </div>
  </a>
</article>
```

Nested `<a>` elements are invalid per the HTML spec. When the browser parses this, it auto-closes the outer `<a class="featured__link">` the moment it hits the inner `<a class="chip">`. The visible result in DevTools is `<a class="featured__link"></a>` (empty), with all the children hoisted out and re-parented. The reparenting was the actual cause of the chip/title misalignment that Session 13 patched with `margin-left: -11px`.

### What changed

1. **Markup** — all 4 `.featured` blocks rewritten. The wrapping `<a class="featured__link">` is gone. The title text is wrapped in an `<a href>` instead, matching how `.related-card`, `.popular`, and `.blog-feed__featured` already work:

```html
<article class="featured">
  <div class="featured__image">...</div>
  <div class="featured__body">
    <div class="featured__meta">
      <a href="..." class="chip">APM</a>
      <span class="featured__date">...</span>
    </div>
    <h2 class="featured__title">
      <a href="...">Lorem ipsum...</a>          ← just the title is a link now
    </h2>
  </div>
</article>
```

2. **CSS** — removed:
   - `.featured__link { display: flex; flex-direction: column; color: inherit; }`
   - `.featured__link:hover .featured__title { text-decoration: underline; }`

   Added (matching the related-card pattern):
   - `.featured__title a { color: inherit; text-decoration: none; }`
   - `.featured__title a:hover { text-decoration: underline; }`

3. **Reverted Session 13** — removed `margin-left: -11px` from all 4 containers:
   - `.featured__meta`
   - `.related-card__meta`
   - `.popular__head`
   - `.blog-feed__featured-meta`

   Plus the comment blocks introducing them.

### What this means for Session 13

Session 13 was a band-aid for a symptom (chip/title misalignment) whose actual cause was this Session 14 bug. The browser's auto-close of the invalid outer anchor was shifting layout in `.featured`. The other three patterns (`.related-card`, `.popular`, `.blog-feed__featured`) never had this bug — their chip-and-title alignment was always correct, even though my Session 13 work treated all four uniformly. So Session 13's `-11px` was either fixing a real issue caused by the broken HTML (`.featured`) or fixing nothing (the other three) — net result: undone everywhere.

### UX trade-off (acceptable)

Before: clicking anywhere on a `.featured` card navigated to the article (whole card was a link — though broken HTML meant browsers handled this unpredictably).

After: only clicking the title navigates to the article. Image and meta-area clicks do nothing. This matches how `.related-card`, `.popular`, and `.blog-feed__featured` already worked.

If full-card-click behavior is desired later, the standard pattern is to use a `::after` pseudo-element on the title link to extend its clickable area over the entire card (the "card-as-link" / "extended-target" pattern). Flagged as a future enhancement; not implemented now.

### Bug in my first attempt at this fix (interesting failure mode)

My first script used the regex `<a href="[^"]+" class="featured__link">\s*(.*?)</a>` with `re.DOTALL`. The lazy `.*?` made the match stop at the *first* `</a>` it encountered — which, because of the nested chip anchor, was the chip's closing tag. The substitution then deleted the wrong scope and left a corrupted file. I reverted from `/mnt/user-data/outputs/` (which had the last-shipped clean state) and rewrote the script to use **structural anchoring**: find the wrapper's closing `</a>` by walking forward to the next `</article>` and stepping back one line. That's reliable because the original markup is well-structured. Lesson: don't use regex to match nested HTML; use the structure.

### Verification

```
featured__link references → 0
margin-left: -11px         → 0
.featured__title with <a wrap → 4 (all instances)

<div in index.html       → 895 / 895
<article                 → 171 / 171
<a / </a>                → 653 / 653  (balanced)
page-views               → 21
```

---

## What's now closed vs. still open across all sessions

**Closed (typography axis):**
- Font sizes — 8/8 tokens migrated or annotated.
- Line-heights — 8/8 tokens migrated or annotated.
- Font families — 3/3 tokens migrated or annotated.

**Closed (color axis):**
- Surface families (Neutral + Brand) — 12 new tokens defined.
- Text scale — 6 new tokens defined.
- Brand chrome — 4 brand-color tokens defined.
- Legacy Search Labs color tokens — 13 aliased to the new system.
- Stale `#6B7079` literals — 12/12 migrated to `var(--gray)`.

**Closed (dimension axis):**
- Spacing — 8/8 tokens annotated.
- Radii — 4/4 tokens migrated or annotated.

**Under experiment (not committed):**
- `--fw-body: 500` — see Session 4.

**Still open — measured but not applied:**
- Heading weights `--fw-heading: 750`.
- `--fw-bold: 700` token (Search Labs uses `font-weight: 700` literally in 30+ places).
- Brand-background blue `#2476f0` and brand-border `#154399`: discovered, not introduced.
- Default border discrepancy: `--border → #C4C7D1` vs library `#CCD1DA`.

**Still open — additive (would introduce new tokens, not change existing):**
- Viewport breakpoints, border widths, icon sizes, negative dimensions, extra dimension primitives.
- Library type classes Search Labs doesn't use: Body XLarge 18, Title Large 28, Headline Medium 36, Display 45/51/58, Oversize 65/73/82 (weight 800), full Label family.

**Cleanup opportunities (literal values bypassing the token system):**
- `#0B64DD` × 17 hardcoded — should use `var(--brand-elastic-blue)` / `var(--blue)`.
- `#FFFFFF` × 8 hardcoded — should use `var(--white)`.
- `#F1F4F8` × 11 — not in any token; needs inspection.
- `#D4DAE5` × 7, `#1C1E23` × 7, `#343741` × 5 — near-matches to existing tokens.
- `border-radius: 10px` × 1 (`.qs-callout`) — bypasses `--r-10`; now inconsistent with the radii consolidation.

---

## How to revert any entry

Each line in `design-system.css` carries its pre-migration value inline. To revert one token, edit just that line back to the value in the comment and update the matching entry's "Status" column in this file.

## How to confirm everything still renders

```bash
# CSS brace balance
grep -c '{' design-system.css ; grep -c '}' design-system.css   # should be equal

# Token references still resolve
grep -c 'var(--fs-' index.html                                  # ≥ 50 expected

# Views and routing intact
grep -c 'class="page-view"' index.html                          # 21
grep -c 'var routes' index.html                                 # 1
```
