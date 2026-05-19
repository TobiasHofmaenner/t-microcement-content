# t-microcement-content

Marketing content for [www.t-microcement.com](https://www.t-microcement.com/).

This repo holds **only the things you edit**: page text (markdown),
photos, and the site stylesheet. The website's HTML structure, build
pipeline, and deployment all live in
[`t-microcement-website-builder`](https://github.com/TobiasHofmaenner/t-microcement-website-builder)
and you don't need to touch them.

## Editing a page

Open the corresponding file in `content/`, edit the text below the
`---` block, commit. The site picks up the change on its next scheduled
rebuild (within ~1 hour), or run "Run workflow" on the builder repo's
Actions page for an immediate rebuild.

```
content/_index.md      # Homepage
content/services.md    # Services page
content/contact.md     # Contact page
```

The `---` block at the top of each file (the **front matter**) sets the
page title. Leave the format alone, change the value.

## Adding a photo

Drop the image into `static/` (e.g. `static/photos/kitchen.jpg`) and
reference it in markdown:

```markdown
![Kitchen finish](/photos/kitchen.jpg)
```

Photos served from `static/` end up at the site root (so
`static/photos/kitchen.jpg` becomes `/photos/kitchen.jpg`).

## Adding a new page

1. Create `content/<name>.md` with:
   ```markdown
   ---
   title: "Your page title"
   ---

   Page content here.
   ```
2. Tell the builder repo's maintainer to add a link in the site nav.

## Adjusting the look (CSS)

The site stylesheet lives at `assets/css/main.css`. Edit it freely —
colors, fonts, spacing, layout. Don't *rename* or *delete* `main.css`
though; the builder repo's HTML templates reference that exact path and
the build will fail if it's missing.

Examples of safe edits:
- Change the brand color: search for `--brand` and replace the hex value
- Bigger headings: bump `h1 { font-size: ... }`
- More vertical breathing room: increase the `padding` on `section`

If you need a totally new visual structure (different header layout,
new component types), that's a builder-repo change — ping the
maintainer.

## Trying changes locally (optional)

If you have Hugo installed, you can preview locally:

```bash
git clone https://github.com/TobiasHofmaenner/t-microcement-website-builder.git ../builder
cp -r content static assets ../builder/
cd ../builder
hugo server -D    # http://localhost:1313
```
