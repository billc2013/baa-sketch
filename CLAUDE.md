# CLAUDE.md

Notes for future Claude Code sessions working on this project. Keep concise.

## Project

**Biomimicry as an Authentic Anchor (BAA)** — a dissemination hub for an
NSF-funded TERC + Tufts middle-school biomimicry/engineering project.
Grant: **NSF DRL-2300433**.

The site is structurally modeled on TERC's Robots in Science site
(<https://www.terc.edu/robots-in-science/>) and follows the four-pillar IA:
Home → Classroom Resources → Professional Development → Research & Team.

Two source documents live in this directory but are **not** part of the site:

- `BAA template_based on RiS.xlsx` — TERC web team's sitemap template
- `Disseminate What v How.xlsx` — content inventory / dissemination matrix

Both are gitignored (`*.xlsx`).

## Audience for edits

Colleagues at TERC and Tufts. Code-aware (some are LaTeX users) but **not**
regular front-end developers. Editing model is intentionally Markdown-only:
they edit `.md` files via GitHub's web UI, push triggers Vercel rebuild,
edits go live in 30–60s. Expected edit rate: <5/week.

This audience drives most architecture decisions — see "Tradeoffs" below.

## Stack

- **Astro** (static) — chosen over Next.js/React because the site is
  content-heavy and we want zero-JS-by-default + first-class Markdown.
- **Plain CSS** in `src/styles/global.css` — no Tailwind. Academic look:
  serif headings (Georgia), deep blue accent (`#1f5f8b`), card grid on home.
- **GitHub** as the source of truth, **Vercel** for hosting (auto-deploys
  on push to `main`, preview URLs per branch/PR).
- No DB, no API, no Functions. If we need dynamic content later see
  "Tradeoffs" → "Drive/SharePoint sync."

## Layout / file organization

```
src/
├── layouts/BaseLayout.astro     # page shell — header, footer, global CSS import
├── components/{Header,Footer}.astro
├── pages/                        # Astro routing: every .md/.astro here = a URL
│   ├── index.astro               # home (layout in code, prose in imported .md)
│   ├── about.md
│   ├── classroom-resources/{index,pre-built-lessons,ingredients}.md
│   ├── professional-development/index.md
│   ├── research-and-team/index.md
│   └── home/{intro,why-biomimicry}.md   # imported by index.astro (see note)
└── styles/global.css
public/                          # static assets — images, PDFs (no images yet)
```

**Markdown frontmatter pattern** (every page-level `.md` needs this):

```markdown
---
layout: ../../layouts/BaseLayout.astro
title: Page Title
description: One-line summary.
---
```

## Pattern: structured page with editable prose

For pages that need code-driven layout (cards, side-by-side blocks) but
should still let colleagues edit the text:

1. Keep the page as `.astro` in `src/pages/`.
2. Put the editable prose in `.md` files.
3. Import with `import { Content as Foo } from '...md';` and render `<Foo />`.

The home page (`src/pages/index.astro`) is the canonical example.

### Note on where to put imported `.md` fragments

There are two valid spots:

- **`src/content/`** — clean, no URL side-effects. Recommended default.
- **`src/pages/<section>/...md`** — also works, but Astro will route every
  file there as its own page. Currently the home fragments live at
  `src/pages/home/intro.md` and `src/pages/home/why-biomimicry.md`,
  which means `/home/intro/` and `/home/why-biomimicry/` are reachable
  URLs. Harmless if nothing links to them, but worth knowing.

If we add more imported fragments later, prefer `src/content/`.

## Tradeoffs we've already debated

These have been considered and rejected — don't re-litigate without good reason:

- **Plain HTML/CSS/JS over Astro?** Considered. Rejected because shared
  header/footer would need copy-pasting across pages. Astro gives us shared
  layouts with no extra editing burden on colleagues.
- **Next.js / React?** Rejected. JSX requires a build/dev-server loop for
  every text edit; way too heavy for low-code editors.
- **Markdown CMS (Decap, Tina)?** Not yet. GitHub web UI is good enough at
  this scale and avoids a third service.
- **Drive / SharePoint sync at runtime via Modal/Vercel Functions?**
  Considered. Rejected for now (added complexity, weak auth story, .md
  authoring in Drive/SharePoint isn't actually nicer than GitHub). If we
  revisit, prefer **build-time sync** (cron/Action pulls Drive → commits)
  over runtime fetching.

## House rules

- **Don't add JS frameworks or CSS frameworks.** Plain CSS is intentional.
- **Don't add interactivity** (React islands, client-side state) unless
  there's a concrete user-facing reason. Goal is a static reference site.
- **Don't restructure the IA** without checking against the dissemination
  matrix (`Disseminate What v How.xlsx`) — the four-pillar nav mirrors how
  the project team thinks about the content.
- **Keep the grant number consistent.** It appears in:
  - `src/components/Footer.astro`
  - `src/pages/about.md`
  - `README.md`
  - this file

## Common tasks

### Add a new content page

Create `src/pages/<slug>.md` with the standard frontmatter. Add a nav entry
in `src/components/Header.astro` if it should be in the top nav.

### Add a sub-page under an existing pillar

Create `src/pages/<pillar>/<slug>.md`. The pillar's `index.md` should add a
link to it in its "On this page" list.

### Add an image / PDF

Drop the file in `public/` and reference as `/filename.ext` from any `.md`
or `.astro` file.

### Build / preview locally

```sh
npm install        # one-time
npm run dev        # http://localhost:4321
npm run build      # output to dist/
```
