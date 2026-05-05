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
├── pages/                          # every .md / .astro here becomes a page
│   ├── index.astro                 # home (layout only — prose lives in pages/home/)
│   ├── about.md
│   ├── classroom-resources/
│   │   ├── index.md
│   │   ├── pre-built-lessons.md
│   │   └── ingredients.md
│   ├── professional-development/
│   │   └── index.md
│   ├── research-and-team/
│   │   └── index.md
│   └── home/                       # text fragments imported by index.astro
│       ├── intro.md                # home page hero text
│       └── why-biomimicry.md       # home page "Why Biomimicry?" section
└── styles/global.css               # site-wide styling
public/                             # uploaded files (PDFs, images, slides) — see below
├── lessons/                        # full lesson PDFs / PPTX
├── worksheets/                     # student worksheets
├── slides/                         # PD decks
└── images/                         # photos, diagrams
```

### Uploading files (PDFs, images, slides) and linking to them

Anything you put in the `public/` folder is hosted at the root of the site.
For example, `public/lessons/biorobots-1-hour-activity.pdf` is reachable at
`/lessons/biorobots-1-hour-activity.pdf`.

**To upload a file via GitHub (no CLI needed):**

1. In the GitHub repo, click into the `public/` folder, then into the
   appropriate subfolder (`lessons/`, `worksheets/`, `slides/`, `images/`).
   If the subfolder doesn't exist yet, you can create it on the fly when
   you upload — see step 3.
2. Click **Add file → Upload files**.
3. Drag your file in. (To create a new subfolder at the same time, type
   `subfolder-name/` into the filename area before dropping the file.)
4. Scroll down, write a short commit message ("Add Biorobots 1-hour PDF"),
   and commit.

**To link to it from a `.md` page:**

```markdown
[Biorobots 1-hour activity (PDF)](/lessons/biorobots-1-hour-activity.pdf)
```

**To embed an image:**

```markdown
![Diagram of the design cycle](/images/design-cycle.png)
```

**Filename rules of thumb** — saves headaches later:

- Lowercase only.
- Use hyphens, not spaces (`design-cycle.png`, not `Design Cycle.png`).
- Drop parentheses and version markers like `(1)` — those become ugly URL
  encoding (`%20`, `%28`).
- Use a clear, descriptive name; the URL is the file's permanent address.

### Adding a brand-new page

Create `src/pages/<some-name>.md` with this header:

```markdown
---
layout: ../layouts/BaseLayout.astro
title: Your Page Title
description: One-line summary for search engines.
---

# Your Page Title

Your content here…
```

If the page should appear in the top navigation, ask Bill to add it to
`src/components/Header.astro` — that's a code-side edit.

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
