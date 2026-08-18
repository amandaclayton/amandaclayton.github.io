# amandaclayton.org

A plain static website — HTML, one CSS file, one small JS file. No build step, no
framework, no account required to edit it. Any text editor works.

## Files

```
index.html         About / landing page (bio, EGEN, contact)
research.html      Publications, grouped by theme, with filters
book.html          The Climate Gender Gap book project
teaching.html      Courses and WIPP
data.html          QAROT dataset and replication materials
public-engagement.html  Press coverage, public writing, consulting, funding & affiliations
404.html           Shown for any URL that doesn't exist

css/style.css      All styling, including light/dark mode
js/site.js         Dark-mode toggle and the research-page theme filter
img/               Portrait and book-project figures
pdfs/              Article PDFs and the CV

CNAME              Tells GitHub Pages to serve the site at www.amandaclayton.org
.nojekyll          Tells GitHub Pages not to run Jekyll over these files
robots.txt         Search-engine directives
sitemap.xml        Page list for search engines

published-research.html, book-project.html, cv.html, contact.html, home.html
                   Redirect stubs so old Weebly links keep working
```

## Previewing changes locally

From this folder:

```bash
python3 -m http.server 8787
```

Then open <http://localhost:8787>. Press Ctrl-C to stop.

You can also just double-click `index.html` — most things work, though the
old-URL redirects won't.

## Adding a new publication

Open `research.html`, find the theme block it belongs in (each starts with
`<div class="theme-block" id="climate" ...>`), and copy an existing `<li class="pub">`
block. The structure is:

```html
<li class="pub">
  <div class="year">2027</div>
  <div>
    <p class="title">Title of the article</p>
    <p class="authors"><span class="me">Amanda Clayton</span> and Coauthor Name</p>
    <p class="venue"><em>Journal Name</em>. 12(3): 45–67.</p>
    <p class="links"><a href="pdfs/newpaper.pdf">PDF</a> <a href="https://doi.org/...">Journal</a></p>
    <p class="note">Optional note — press coverage, related work, etc.</p>
    <p class="note award">Optional award line, shown in gold.</p>
  </div>
</li>
```

`<span class="me">` is what bolds your own name. Papers listed under two themes are
simply pasted into both blocks. If you add a paper, also paste it into the
`<div id="chronological">` list further down so the "By year" view stays complete.

## Updating the CV

Drop the new PDF into `pdfs/` and update the filename everywhere it appears:

```bash
grep -rl "clayton_cv_may_2026.pdf" . --include=*.html
```

## Updating work in progress

The `Work in progress` view is a single block near the bottom of `research.html`,
`<div id="inprogress">`. Each entry is a `<li class="wip">` with a title, collaborators,
a one-line description, and an optional `<p class="status">` badge ("Under review",
"Manuscript drafted", and so on). Delete the `<p class="status">` line if an item has
no status worth showing.

Items that also sit under a theme appear twice by design — once in context, once in the
full list. The per-theme copies are the `<h4>In progress</h4>` lists inside each
`.theme-block`.

## Adding a new theme

1. Add a filter button in the `<div class="filters">` block on `research.html`:
   `<button type="button" data-filter="newkey" aria-pressed="false">Theme Name</button>`
2. Add a matching section: `<div class="theme-block" id="newkey" data-theme="newkey">`

The filtering JavaScript picks it up automatically — nothing else to configure.

## Deploying to GitHub Pages

1. Create a free account at <https://github.com> if you don't have one.
2. Create a new **public** repository named `amandaclayton.github.io`.
3. Upload the entire contents of this folder (not the folder itself — the files
   inside it) via **Add file → Upload files**. Drag them all in at once.
4. Go to **Settings → Pages**. Under "Build and deployment," source should be
   *Deploy from a branch*, branch `main`, folder `/ (root)`. Save.
5. In the same Pages settings, set **Custom domain** to `www.amandaclayton.org`
   and check **Enforce HTTPS** once it becomes available.
6. At your domain registrar (wherever amandaclayton.org is registered), set a
   `CNAME` record for `www` pointing to `amandaclayton.github.io`, and either
   forward the bare domain `amandaclayton.org` to `www`, or add these four A
   records for the bare domain:

   ```
   185.199.108.153
   185.199.109.153
   185.199.110.153
   185.199.111.153
   ```

7. DNS changes take anywhere from a few minutes to a few hours. Until then the
   site is live at <https://amandaclayton.github.io>.

Keep Weebly active until the new site is confirmed working at the custom domain,
then cancel it.

## Deploying to Netlify instead

Go to <https://app.netlify.com/drop> and drag this folder onto the page. It goes
live immediately at a random subdomain; add `www.amandaclayton.org` under
**Domain settings**. Netlify reads the `CNAME` file harmlessly. To update, drag
the folder again.

## Notes

- The site works with JavaScript disabled: all publications are visible, just
  unfiltered, and the theme follows the visitor's OS setting.
- Colors are defined once at the top of `css/style.css` as CSS variables, in two
  blocks — light and dark. Changing the accent color means editing `--accent` in
  both places.
