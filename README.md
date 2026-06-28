# t-microcement-content

Source for [www.t-microcement.com](https://www.t-microcement.com/).

This repo holds **everything Hugo renders** — pages, photos, CSS, HTML
templates, and the site config. It's a self-contained Hugo site; you
can preview locally with `hugo server` and there's no second repo to
clone.

The build pipeline (Dockerfile, CI workflow) lives in the sibling
[`t-microcement-website-builder`](https://github.com/TobiasHofmaenner/t-microcement-website-builder)
repo, which only the operator touches.

## Quick map

```
content/        # Markdown — the pages everyone reads
static/         # Photos, PDFs, favicons — anything served as-is
assets/css/     # The site stylesheet (one file, edit freely)
layouts/        # HTML templates (Hugo) — change with care
hugo.toml       # Site config — brand name, contact, etc.
```

## Editing a page

Open the corresponding file in `content/`, edit the text below the
`---` block, commit. The site picks up the change on its next scheduled
rebuild (within ~1 hour). For an immediate rebuild, click
**Run workflow** on the builder repo's
[Actions page](https://github.com/TobiasHofmaenner/t-microcement-website-builder/actions).

```
content/_index.md          # Homepage (driven by structured front-matter)
content/product.md         # Product page
content/contact.md         # Contact page
content/downloads.md       # Downloads page
content/gallery/_index.md  # Gallery (groups + photo lists)
content/guide/_index.md    # Guide (Q&A, application notes, tutorials)
```

The `---` block at the top of each file (the **front matter**) holds
structured fields the page uses. On most pages the only field that
matters is `title`; the homepage and gallery have richer structures —
look at the existing values for guidance.

## Adding a photo

Drop the image into `static/photos/` (e.g.
`static/photos/kitchen.jpg`) and reference it in markdown:

```markdown
![Kitchen finish](/photos/kitchen.jpg)
```

Photos served from `static/` end up at the site root (so
`static/photos/kitchen.jpg` becomes `/photos/kitchen.jpg`).

To add a photo to the **gallery**, edit
`content/gallery/_index.md` and append an entry under the right
heading's `photos:` list.

## Adding a new page

1. Create `content/<name>.md` with:
   ```markdown
   ---
   title: "Your page title"
   ---

   Page content here.
   ```
2. Add a link in `layouts/partials/header.html` if you want it in the
   top nav.

## Adjusting the look

- **Colours, spacing, fonts** — edit `assets/css/main.css`. CSS
  variables (`--bg`, `--fg`, `--accent`) are at the top.
- **Page layout / HTML structure** — edit files under `layouts/`.
  The base template is `layouts/_default/baseof.html`; per-section
  templates live in `layouts/_default/list.html`,
  `layouts/_default/single.html`, etc.
- **Site title, description, contact** — edit `hugo.toml`. Inline
  comments mark what's safe to change and what isn't.

If you're not sure whether an edit is safe, preview locally first
(see below) — Hugo will refuse to build if you've broken a template.

## Previewing locally

You need [Hugo extended](https://gohugo.io/installation/) installed
locally (any recent version is fine; the production build uses 0.140).
On Windows: `winget install Hugo.Hugo.Extended`.

```bash
git clone https://github.com/TobiasHofmaenner/t-microcement-content.git
cd t-microcement-content
hugo server -D    # http://localhost:1313
```

Edits to any file under `content/`, `layouts/`, `assets/` or `static/`
reload the browser instantly. Stop with `Ctrl-C`.

## Do not break these

- `hugo.toml`'s `baseURL` — must stay `https://www.t-microcement.com/`
  or every internal link breaks.
- `assets/css/main.css` — referenced by name from
  `layouts/_default/baseof.html`. Edit content, don't rename or delete.
- `static/img/logo.svg`, `static/favicon.svg` — referenced by header
  and `<head>`. Replace the contents if you want, keep the filenames.

Anything else: experiment freely. If you push something that breaks
the build, the deployed site stays on the last good version; nothing
goes down.
