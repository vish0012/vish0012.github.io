# Updating this site

A stable, no-build personal website. Plain HTML and CSS — no Node, no React, no
CMS. To make a change: edit a file, commit, push. GitHub Pages rebuilds in 1–2
minutes.

## File map

```
/
├── index.html         Homepage (hero, about, selected pubs, recent, teaching, contact)
├── publications.html  Full publication list, grouped by year
├── cv.html            Curriculum vitae — full
├── news.html          News archive, grouped by year
├── sitemap.xml        For search engines (update when adding pages)
├── photo.jpg          Hero portrait
└── assets/
    ├── style.css      All styling
    └── images/        Publication thumbnails, figures
```

---

## Adding a news entry

Most common update. Two places to edit:

1. **`news.html`** — the permanent archive. Find the right year section
   (`<!-- ============ 2026 ============ -->`), add a new `<li>` at the **top**
   of that year's `<ul class="news-list">`:

   ```html
   <li>
     <span class="news-date">May 2026</span>
     <span>Your news text. <strong>Bold for emphasis.</strong> <a href="https://example.com">Link</a>.</span>
   </li>
   ```

2. **`index.html`** — optional. If the news is recent and noteworthy, also paste
   the same `<li>` at the top of the Recent section on the homepage so visitors
   see it without clicking through. Find `<section id="recent">`, then the
   `<ul class="news-list">` inside.

**Annual housekeeping:** roughly once a year, the homepage Recent section gets
too long. Cut the oldest 2–3 entries from `index.html` and leave them in
`news.html` only.

---

## Adding a publication

Two layouts. Pick based on whether the paper has a strong figure to show.

### Text-only (default — for most papers)

Open `publications.html`. Find the right year. Add this `<article>`:

```html
<article class="pub pub-text" id="paper-slug-2026">
  <div class="pub-body">
    <h3>Full Paper Title Here</h3>
    <p class="pub-authors"><strong>Vishal Chauhan</strong>, Co-author Two, Co-author Three</p>
    <p class="pub-venue"><em>Venue Name · City, Country</em></p>
    <p class="pub-abstract">One or two sentences summarizing what the paper does and the main finding.</p>
    <p class="pub-links">
      <a href="https://doi.org/...">DOI</a>
      <span class="sep">·</span>
      <a href="https://...">PDF</a>
    </p>
  </div>
</article>
```

- The `id="paper-slug-2026"` lets you deep-link to that paper from anywhere
  (e.g. `publications.html#paper-slug-2026`). Use lowercase, hyphens, no spaces.
- Add an honourable-mention badge inside the `<h3>` if the paper earned one:
  ```html
  <h3>Paper Title <span class="hm-badge">🏆 Honourable Mention</span></h3>
  ```

### With thumbnail (for top 2–3 papers per year)

Same structure, but **remove `pub-text`** from the class and add a `<figure>`:

```html
<article class="pub" id="paper-slug-2026">
  <figure class="pub-thumb">
    <img src="assets/images/thumb-mypaper.png" alt="Description of the figure" />
  </figure>
  <div class="pub-body>
    <!-- same content as above -->
  </div>
</article>
```

Save thumbnails in `assets/images/`. Keep file size under ~500 KB. PNG or JPG
both work.

### Homepage Selected publications

For your 2–3 strongest papers, also add to `index.html` in the
`<section id="publications">`. Use the same `<article class="pub">` structure
with a thumbnail. The homepage only shows a curated short list — the full list
lives in `publications.html`.

---

## Adding or replacing an image

Drop the file into `assets/images/`:

```bash
cp ~/Downloads/myimage.jpg ./assets/images/
```

Then reference it from HTML:

```html
<img src="assets/images/myimage.jpg" alt="Description for screen readers and SEO" />
```

**Tips:**
- Keep individual images under 500 KB. Resize/compress with Preview on macOS
  (Tools → Adjust Size, then File → Export, Quality: ~80%) or
  [squoosh.app](https://squoosh.app).
- Use descriptive filenames: `thumb-chi-2026.png` reads better than
  `image_4_final_v2.png`.
- Always set `alt` text — required for accessibility and helps SEO.

---

## Replacing the portrait

The hero photo is at `/photo.jpg` (root, not in assets). Replace it directly:

```bash
cp ~/Downloads/new-photo.jpg ./photo.jpg
```

Same filename, no HTML changes. Aim for ~1500 px wide, around 500 KB.

---

## Editing the About blurb

Open `index.html` and look for `<section id="about">`. Edit the two paragraphs
inside. Keep it short — two or three sentences each works best.

---

## Editing your CV

Everything in `cv.html` is hand-written HTML. Each entry follows this pattern:

```html
<div class="cv-entry">
  <div class="cv-when">Oct 2023 – present</div>
  <div class="cv-what">
    <strong>Position Title</strong> · Organization
    <ul>
      <li>Bullet point one.</li>
      <li>Bullet point two.</li>
    </ul>
  </div>
</div>
```

For new sections (e.g. a "Talks" section), copy the structure of an existing
`<div class="cv-section">` block.

---

## Shipping changes

After saving your edits, run from the repo root:

```bash
git add .
git status                                    # always check what you're about to commit
git commit -m "Short description of change"
git push
```

Wait 1–2 minutes for GitHub Pages to rebuild. Open
`https://vish0012.github.io/` in an incognito window (Cmd+Shift+N in Chrome,
Cmd+Shift+P in Safari) — that bypasses browser cache so you see the live
version cleanly.

**If `git push` fails with `HTTP 400` on large pushes:**

```bash
git config http.postBuffer 524288000
git push origin main
```

(One-time setup. The buffer increase is permanent for this repo.)

---

## Design rules to preserve the look

- **Colors live in CSS variables** at the top of `assets/style.css`. Don't
  hard-code colors elsewhere. The brand colors are `--accent` (muted green) and
  `--ink` (text).
- **Three fonts**: Source Serif 4 (italic, for display), Source Sans 3 (body),
  JetBrains Mono (dates, metadata, eyebrow labels). Don't introduce new fonts.
- **Section eyebrows** (the small uppercase mono labels) use
  `<p class="section-eyebrow">Label</p>`.
- **No emoji** in headings or body text. Only the 🏆 award badge is allowed,
  inside `<span class="hm-badge">`.
- **Keep image alt text descriptive.** Screen readers, SEO, and search bots all
  read it.

---

## What NOT to add

- No Google Analytics, Facebook pixel, or third-party trackers — keeps the site
  fast and private.
- No JavaScript frameworks. The whole site works with HTML and CSS alone, which
  is why it'll keep working in 2035.
- No build tooling. If you find yourself adding `npm install`, stop and ask
  whether it's really needed.

---

## Backup

The whole site is in this Git repo. To pull it down on a new machine:

```bash
git clone https://github.com/vish0012/vish0012.github.io.git
cd vish0012.github.io
open index.html
```

Done. The repo is the backup.
