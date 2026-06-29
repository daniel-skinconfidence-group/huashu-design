# SkinOS Context for huashu-design

> Read this file BEFORE starting any task while working in a SkinOS repo.
> Last updated: 2026-06-29 (ENG-1637: Skinmart taste now resolves from the unified `brand-dna` / `.impeccable.md` source, the same one the admin Impeccable skills read; ttc/gloskinbody unchanged). Prior: 2026-04-27 (Tier 2/3 additions: brand-spec starters, Lucide pinning, Stitch override tightened).

This is a **private fork** of [alchaincyf/huashu-design](https://github.com/alchaincyf/huashu-design) maintained at [daniel-skinconfidence-group/huashu-design](https://github.com/daniel-skinconfidence-group/huashu-design). All upstream design philosophy, anti-AI-slop rules, asset protocol, and starter components apply unchanged. This file adds SkinOS-specific context that overrides defaults.

---

## When to use this skill (vs deferring to others)

This skill is for **standalone HTML design artefacts** — prototypes, slide decks, motion design, infographics, design exploration. It is **not** for production code in the SkinOS repos.

**Use this skill when:**
- Producing a hi-fi mockup or interactive prototype as a standalone HTML file
- Pitching a design direction (3 variants, design-direction advisor mode)
- Exploring motion / animation as a video deliverable (MP4/GIF)
- Building a presentation deck that may export to PPTX
- Producing an infographic or data visualisation as a static asset

**Defer to existing skills instead when:**
| Task | Skill |
|---|---|
| Production React/Next.js component code in `app/` or `components/` | `frontend-design` (or its plugin variant `frontend-design:frontend-design`) |
| UI guideline review of existing code | `web-design-guidelines` |
| Animating an already-built feature | `animate` |
| Email/EDM design or copy | `edm` |
| Final visual polish / spacing pass | `polish` |
| UX copy, error messages | `clarify` |
| Layout / spacing rhythm on existing UI | `arrange` |
| Typography on existing UI | `typeset` |
| Tone down aggressive UI | `quieter` |
| Strip back excess | `distill` |

If the user asks for production component code in a SkinOS repo, **stop and recommend `frontend-design` instead**. This skill is sandbox/exploration only.

---

## Brand context (multi-brand)

SkinOS serves three brands. **Every design output must declare which brand it's for** as the first question if not obvious.

| Brand code | Brand name | Tone (high level) |
|---|---|---|
| `ttc` | The Treatment Centre | Clinical / professional / Australian premium |
| `skinmart` | Skinmart | Mass-market / value / accessible |
| `gloskinbody` | GLO Skin Body | Lifestyle / hero ingredient-led |

**Skinmart taste source — canonical (ENG-1637).** For **Skinmart**, brand voice, register, anti-patterns, and visual decisions resolve from the single `brand-dna`-derived taste source — the **same** source the admin Impeccable design skills read — not a standalone copy here, so there is one non-drifting Skinmart taste path:
- In `content-studio`: the generated `.impeccable.md` at the repo root (regenerated from `brand-dna` via `scripts/generate-impeccable.ts`; it is marked "GENERATED — do not hand-edit").
- Origin / single source of truth: `Skin-Confidence-Group/scg-strategy` → `brand-dna/skinmart/{PRODUCT,DESIGN,anti-patterns}.md` layered over `brand-dna/group/{shared-laws,anti-patterns}.md`.
- Do **not** re-author Skinmart taste in this file — read it from `brand-dna` / `.impeccable.md`.

**ttc / gloskinbody:** no `brand-dna` source exists yet (Skinmart-first; GLO + TTC fast-follow). Until it does, use the tone table above plus live `brand_kits` (Supabase) for those two brands, as before.

**Brand asset locations** (when they exist — check before searching the web):
- Brand kits configured at `/brands` route in the admin app — `brand_kits` table in Supabase
- Theme assets: `shopify-theme/assets/` (per-store, look for brand-specific naming)
- Logos and product imagery: ask the user first; many are in `~/Downloads`, `_archive/`, or Shopify CDN

**The §1.a Core Asset Protocol still applies**: even with brand kits, log/UI/product imagery extraction is required before the design pass.

---

## Stitch workflow integration

**This section overrides SKILL.md Step 2 ("Explore resources + extract assets") when a Stitch reference is present.**

If the user references a Stitch screen ID, screen name, or anything from `mcp__stitch__get_project`:

1. **Call `mcp__stitch__get_screen` first** — get `htmlCode.downloadUrl`
2. **Download and read the HTML** — it contains Tailwind classes with exact CSS values; these ARE the spec
3. **Extract from Tailwind classes**: colours (bg-*, text-*, border-*), spacing (p-*, m-*, gap-*), radius (rounded-*), shadows (shadow-*), typography (text-*, font-*, leading-*)
4. **Map extracted values to CSS variables** — do not hardcode inline; put them in `:root { --brand-*: ... }` 
5. Only then start the Junior Designer pass — assumptions/reasoning still go in the HTML head, but all visual values come from Stitch, not from brand-spec guesswork

**Why this overrides the design-direction advisor**: if a Stitch screen exists, requirements are not vague — the visual spec is already decided. Skip Phase 1–4 of the design-direction advisor flow entirely.

Stitch MCP tools: `mcp__stitch__get_project(name)` · `mcp__stitch__list_screens(projectId)` · `mcp__stitch__get_screen(name, projectId, screenId)`

---

## Working-directory conventions

**Where to write outputs:**
- For SkinOS platform-admin work: `docs/features/<feature-slug>/prototypes/` or `docs/superpowers/specs/<date>-<slug>/`
- For Shopify theme exploration: `shopify-theme/_design-prototypes/<slug>/` (gitignore this dir)
- **Never** drop output into `~/Downloads` or repo root

**Never open files into the user's editor** (`feedback_no_focus_stealing`). Compose silently or via background subagent, then deliver a clickable link.

**Standard delivery for HTML design artefacts** (every render, no exceptions):

1. Write the HTML file to the correct output path
2. Screenshot with Playwright at the correct viewport: `npx playwright screenshot "file:///abs/path.html" "output.png" --viewport-size=W,H --full-page`
3. Start a local HTTP server on a free port (check with `lsof -i :PORT` first, use 7474 as default):
   ```bash
   cd <output-dir> && python3 -m http.server 7474 &>/tmp/design-server.log &
   ```
   Skip this step if a server is already running on that port.
4. Open in the system browser: `open "http://localhost:7474/<filename>.html"`
5. Deliver in chat: clickable markdown link to the HTML file + the Playwright PNG screenshot

The browser tab stays open. All subsequent iterations just regenerate the file — the user hits **Cmd+R** to refresh. Do not restart the server on each iteration.

---

## Watermark — DISABLED by default

This fork **disables the upstream "Created by Huashu-Design" watermark for all output formats**. SkinOS work is internal/client-facing; a third-party watermark is off-brand.

Only add the watermark when the user explicitly asks ("add watermark", "watermark this"). See the `Skill 推广水印` section in `SKILL.md` for the template.

---

## Audio & video pipeline

The upstream skill ships 6 BGM tracks + 37 SFX assets and a full HTML→MP4/GIF pipeline. For SkinOS work this is **opt-in**:

- **Default**: produce silent MP4 only when the user asks for video
- **Opt-in**: BGM + SFX double-track if the user explicitly asks for "with music", "with sound", "publish-ready"
- Brand-appropriate music selection — check the brand kit / past campaigns before defaulting to upstream's tech/ad/educational/tutorial library

---

## Skill deferral cheat-sheet (production paths)

If the user asks to "make this real" or "ship this" after a huashu-design exploration:

1. Hand off the HTML/JSX to a `frontend-design` pass for production component conversion
2. For Shopify theme work, follow `feedback_shopify_theme_completion` (theme push checklist + verification)
3. For new admin UI features, run through `/feature` (guided feature dev) — not this skill
4. For email work, hand off to `edm`

---

## CDN pinning — Lucide icons

When using Lucide icons in any SkinOS HTML output, **pin to a specific version** — never `@latest`:

```html
<!-- Pinned Lucide (as of 2026-04-27) -->
<script src="https://unpkg.com/lucide@1.11.0/dist/umd/lucide.min.js"></script>
```

Then initialise after DOM ready: `lucide.createIcons();`

The upstream showcases (assets/showcases/website-ai-nav/*.html) incorrectly use `lucide@latest` — do not copy that pattern. Update the version pin when you know a newer stable release works correctly.

---

## SkinOS brand-spec starters

When the §1.a Core Asset Protocol requires a `brand-spec.md`, use the template below pre-filled with known SkinOS structural facts. **Fill in HEX values and font stacks by querying `brand_kits` in Supabase dev** (`mcp__supabase__execute_sql` — SELECT query is safe) before writing the file.

```sql
SELECT brand_code, name, config FROM brand_kits WHERE brand_code IN ('ttc','skinmart','gloskinbody');
```

The `config` JSONB column contains the colour palette, font stack, and logo URLs. Extract from there — do not guess.

### Template: `brand-spec.md`

```markdown
# <Brand Name> · Brand Spec
> Adopted: YYYY-MM-DD
> Brand code: ttc | skinmart | gloskinbody
> Asset completeness: complete | partial | inferred

## Brand overview
- **Tone**: [see table below]
- **Register**: [clinical/premium | mass-market/value | lifestyle/ingredient-led]

## 🎯 Core assets (extracted from brand_kits.config)

### Logo
- Main: [path or Shopify CDN URL from config]
- Reversed: [path or Shopify CDN URL]
- Usage: [header / footer / watermark — state which]

### Colours
- Primary:    #XXXXXX  (source: brand_kits.config)
- Secondary:  #XXXXXX
- Accent:     #XXXXXX
- Background: #XXXXXX
- Ink:        #XXXXXX

### Typography
- Display: [font name + weight — from config or Shopify theme]
- Body:    [font name + weight]

## 🗂 Asset locations (SkinOS-specific)
- Brand kit: Admin > /brands > [brand name]
- Shopify theme assets: shopify-theme/assets/ (look for brand code prefix)
- Logos / imagery: ask user (often ~/Downloads or _archive/)

## ❌ Prohibited
- [Anything from brand_kits that's explicitly excluded]
- Never use competitor brand colours or terminology
```

### Brand tone reference (pre-filled, no need to query)

| Code | Name | Tone | Register |
|---|---|---|---|
| `ttc` | The Treatment Centre | Clinical, professional, Australian premium | Trust-first, measured, credentialled |
| `skinmart` | Skinmart | Mass-market, value, accessible | Direct, clear, no-pretension |
| `gloskinbody` | GLO Skin Body | Lifestyle, hero ingredient-led | Aspirational, warmth, ingredient authority |

---

## What to ignore from upstream

- **Watermark default** — disabled (see above)
- **DJI/Stripe/Lovart/Linear examples** — replace with TTC/Skinmart/GLO Skin Body when teaching by example to the user
- **`assets/personal-asset-index.example.json`** — not used; SkinOS assets are in Supabase brand kits and Shopify theme
- **Chinese-only demos** (`demos/c1-ios-prototype.html` etc) — prefer the `-en` variants
- **Lucide `@latest`** in showcase HTMLs — always pin to a version (see CDN pinning section above)
