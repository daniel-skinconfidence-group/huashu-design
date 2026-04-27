# SkinOS Context for huashu-design

> Read this file BEFORE starting any task while working in a SkinOS repo.
> Last updated: 2026-04-27

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

**Brand asset locations** (when they exist — check before searching the web):
- Brand kits configured at `/brands` route in the admin app — `brand_kits` table in Supabase
- Theme assets: `shopify-theme/assets/` (per-store, look for brand-specific naming)
- Logos and product imagery: ask the user first; many are in `~/Downloads`, `_archive/`, or Shopify CDN

**The §1.a Core Asset Protocol still applies**: even with brand kits, log/UI/product imagery extraction is required before the design pass.

---

## Stitch workflow integration

If the user references a **Stitch screen ID** or a screen from `mcp__stitch__get_project`, follow the SkinOS rule from `.claude/rules/visual-implementation.md` (in the platform-admin repo):

1. **Fetch the Stitch HTML first** via `mcp__stitch__get_screen` → download `htmlCode.downloadUrl`
2. **The HTML is the spec** — extract Tailwind classes for exact CSS values (colours, spacing, borders, radius, shadows, typography, layout)
3. Only then start the Junior Designer pass — assumptions/reasoning still go in the HTML head, but the visual values come from Stitch

This overrides the upstream "design-direction advisor for vague requirements" — if a Stitch reference exists, you have the design context.

---

## Working-directory conventions

**Where to write outputs:**
- For SkinOS platform-admin work: `docs/features/<feature-slug>/prototypes/` or `docs/superpowers/specs/<date>-<slug>/`
- For Shopify theme exploration: `shopify-theme/_design-prototypes/<slug>/` (gitignore this dir)
- **Never** drop output into `~/Downloads` or repo root

**Never open files into the user's editor** (`feedback_no_focus_stealing`). Compose silently or via background subagent, then deliver a clickable link.

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

## What to ignore from upstream

- **Watermark default** — disabled (see above)
- **DJI/Stripe/Lovart/Linear examples** — replace with TTC/Skinmart/GLO Skin Body when teaching by example to the user
- **`assets/personal-asset-index.example.json`** — not used; SkinOS assets are in Supabase brand kits and Shopify theme
- **Chinese-only demos** (`demos/c1-ios-prototype.html` etc) — prefer the `-en` variants
