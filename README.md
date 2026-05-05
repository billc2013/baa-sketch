# BAA Website Sketch

A scratch-pad website for the **Biomimicry as an Authentic Anchor** project
(TERC + Tufts, NSF DRL-2300433). Built as a small Astro static site so the
content is just Markdown files in folders.

## For colleagues editing content

You don't need to install anything. Most edits can be made directly on GitHub:

1. Open the file you want to edit in the GitHub web UI
   (e.g. `src/pages/classroom-resources/pre-built-lessons.md`).
2. Click the pencil icon to edit.
3. Change text, links, or add/remove list items.
4. Scroll down, write a short note about what you changed, and commit.
5. Vercel rebuilds the site automatically — your edit is live in ~30–60 seconds.

### Markdown quick reference

| Want | Type |
|---|---|
| A heading | `## Heading` (more `#` = smaller heading) |
| **Bold** | `**bold**` |
| *Italic* | `*italic*` |
| A link | `[click here](https://example.com)` |
| A bullet list | start each line with `- ` |
| An image | `![description](/path/to/image.png)` |
| A table | see existing pages for a copy-paste-able example |

If you've used LaTeX, this will feel like a dramatically simpler shorthand.
Math notation in `$...$` works the same way it does in LaTeX.

### Where things live

```
src/
├── layouts/BaseLayout.astro        # the page shell (header + footer); don't edit casually
├── components/
│   ├── Header.astro                # top nav
│   └── Footer.astro                # NSF disclaimer + logos
├── pages/                          # every .md file here becomes a page
│   ├── index.astro                 # home (layout only — prose lives in src/content/home/)
│   ├── about.md
│   ├── classroom-resources/
│   │   ├── index.md
│   │   ├── pre-built-lessons.md
│   │   └── ingredients.md
│   ├── professional-development/
│   │   └── index.md
│   └── research-and-team/
│       └── index.md
├── content/                        # editable text fragments imported by .astro pages
│   └── home/
│       ├── intro.md                # home page hero text
│       └── why-biomimicry.md       # home page "Why Biomimicry?" section
└── styles/global.css               # site-wide styling
public/                             # drop images / PDFs here; reference as /filename.png
```

To add a brand-new page, create `src/pages/<some-name>.md` with this header:

```markdown
---
layout: ../layouts/BaseLayout.astro
title: Your Page Title
description: One-line summary for search engines.
---

# Your Page Title

Your content here…
```

## For Bill (the maintainer)

### Local development

```sh
npm install        # one-time
npm run dev        # live preview at http://localhost:4321
npm run build      # produces dist/ for static hosting
npm run preview    # preview the production build locally
```

### Deployment

Push to `main` on GitHub. Vercel is connected to the repo and rebuilds
automatically. Every branch and PR also gets its own preview URL.

### Source documents

The two `.xlsx` files in this directory are the project's planning docs and
are **not** part of the site:

- `BAA template_based on RiS.xlsx` — the TERC web team's sitemap template
- `Disseminate What v How.xlsx` — the content inventory / dissemination matrix
