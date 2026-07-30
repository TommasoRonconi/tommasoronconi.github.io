# Site maintenance notes — tommasoronconi.github.io

Personal reference for maintaining and extending the site.
Not rendered by Hugo (the root `README.md` is ignored by the build).

Live: <https://tommasoronconi.github.io/> · Repo: `TommasoRonconi/tommasoronconi.github.io`

---

## 0 · Stack & pinned versions

| Thing | Value | Where |
|---|---|---|
| Template | HugoBlox **Academic CV** | `hugoblox.yaml` |
| Hugo | **Extended 0.162.0** | `hugoblox.yaml` → `build.hugo_version` |
| Theme module | `HugoBlox/kit/modules/blox` @ `v0.0.0-20260527025321-61f41d3667f1` | `go.mod` |
| Deploy target | `github-pages` | `hugoblox.yaml` → `deploy.host` |
| Site URL | `https://tommasoronconi.github.io/` | `config/_default/hugo.yaml` → `baseURL` |

Push to `main` → `.github/workflows/deploy.yml` builds and publishes.
GitHub **Settings → Pages → Source** must stay on **GitHub Actions**.

### Local preview

```bash
npm install
npm run dev          # hugo server --disableFastRender → http://localhost:1313
```

Needs Hugo **Extended** 0.162.0 and Go (Hugo resolves the theme as a Go module).

---

## 1 · `layouts/` — local overrides ⚠️ READ BEFORE ANY THEME UPGRADE

Files in `layouts/` **silently shadow** the same path inside the theme module.
Shadowing is **exact-path**: one wrong directory and the file is inert, with no
error and no warning (this has bitten once already — see §4.7).

| File | Why it exists | On theme upgrade |
|---|---|---|
| `_partials/functions/build_links.html` | **BUG PATCH.** Upstream `{{ $seen.Set "set" (dict) }}` yields a **nil map** under Hugo 0.162.0, so the next `SetInMap` aborts the build on *every* page with author-provided `links:`. Patched to `(dict "__init__" true)`. | **Delete and retest.** Dead weight once upstream fixes it. |
| `_partials/views/citation.html` | **CUSTOMISATION.** Title-first publication lines, author truncation, always-visible bolded self, no empty `href=""`. | **Keep**, re-diff against new upstream. |
| `_partials/hbx/blocks/markdown/block.html` | **CUSTOMISATION.** Adds `design.width` (upstream hardcodes `max-w-prose`, ~65 chars, with no option). | **Keep**, re-diff — upstream may add a real width option. |
| `404.html` | **CUSTOMISATION.** Custom illustrated 404. | Keep. |
| `_partials/hooks/head-end/github-button.html` | **Shipped with the template.** Not a local addition. | Leave alone, or delete if unused. |

**Upgrade procedure**

```bash
hugo mod get -u github.com/HugoBlox/kit/modules/blox
# rename layouts/ → layouts.bak/, build, see what breaks.
# Re-introduce overrides one at a time; drop any upstream has fixed.
```

---

## 2 · Repo map (current)

```
config/_default/
  hugo.yaml       baseURL; disableAliases: false  ← required for redirects
  params.yaml     theme mode/pack, header toggles, citations options
  menus.yaml      8 flat items (see §5)
data/authors/
  me.yaml         MASTER PROFILE — feeds homepage bio AND /experience
content/
  _index.md       homepage: bio → My Research → Featured Pubs → Recent Pubs
                  (talks + news blocks are commented out at the bottom)
  research/       _index.md (3-card grid) + 3 axis bundles
  software/       _index.md (4-card grid) + galapy, scampy, trecs, cosmobolognalib
  publications/   29 imported entries (5 featured, each with a featured.* image)
  contacts.md     contact-info block + Links block
  experience.md   renders from me.yaml
  events/         stub only — reserved for the future News/calendar page
  under-construction/  placeholder page + redirect aliases
  authors/        _index.md with render: never (author pages disabled)
assets/media/     hero-bg.png · icon.svg · icon.png   ← Hugo asset pipeline
static/
  media/404-not-found.svg
  uploads/cv.pdf  → /uploads/cv.pdf  (stable, bookmarkable — don't rename)
publications.bib  29 entries — SOURCE OF TRUTH for the publication list
layouts/          see §1
```

**`assets/media/` vs `static/`**
- `assets/media/` → processed by Hugo (resized → WebP). Backgrounds and the
  favicon **must** live here or the build errors.
- `static/` → copied verbatim, stable URL. CV PDF, and images referenced by raw
  `<img src="/media/...">` (the 404 artwork).

---

## 3 · Routine tasks

### Add or update publications

`publications.bib` at the repo root is the source of truth.

1. Edit/append entries (ADS BibTeX export).
2. Push → `.github/workflows/import-publications.yml` opens a PR in ~2 min → review → merge.
3. Needs **Settings → Actions → General → Workflow permissions = Read and write**.

**Expand ADS journal macros first**, or names render garbled (`\aap` → `åp`):

```
{\aap} → {Astronomy & Astrophysics}      {\apj}  → {The Astrophysical Journal}
{\mnras} → {Monthly Notices of the RAS}  {\jcap} → {Journal of Cosmology and Astroparticle Physics}
```

⚠️ **Re-importing overwrites publication folders** — destroying `featured: true`
flags and `featured.*` images. Import only new entries, or re-apply the five
featured flags and images afterwards.

### Add a research axis

New bundle under `content/research/<slug>/index.md` with `title`, `summary`,
`weight` (controls card order), `tags` (first tag = card badge). The section's
`_index.md` cascades `show_breadcrumb: true`, so children automatically get a
"Research › …" breadcrumb back to the listing.

### Add a software package

New bundle under `content/software/<slug>/index.md`. Follow the existing pattern:
`**Role:**` / `**Stack:**` lines, then prose, then paper links. Set `weight` for
card order (lead-authored packages first). `featured: true` only matters if a
homepage software block with `featured_only` is added later — currently GalaPy
and SCAMPy carry it.

### Update the CV

Overwrite `static/uploads/cv.pdf`. Don't rename.

### Retire an "under construction" placeholder

`content/under-construction/index.md` holds `aliases:` (currently `/teaching/`).
Menu entries **Teaching** and **News** point straight at `/under-construction/`.

When a real section goes live: create the content, **remove its URL from
`aliases:`** (an alias and a real page at the same URL conflict), then point the
menu entry at the real URL.

---

## 4 · Hard-won rules (each cost a debugging session)

1. **`date:` must be a full `YYYY-MM-DD`.** `2026-07` fails as "not a parsable date".
2. **Every `links:` entry needs a valid `type:`** or the build dies with `index nil`:
   `pdf · preprint · doi · code · dataset · model · slides · video · poster ·
   project · site · source · bibtex · canonical · crosspost · discussion ·
   event · calendar · registration · demo`.
   No `docs` or `pypi` type — use `type: site` with a `label:`.
3. **`disableAliases: false`** in `hugo.yaml` — template ships it as `true`,
   which silently disables all redirects.
4. **Background images and favicons must be in `assets/media/`**, not `static/`.
5. **Demo content can ship in a schema its own pinned theme rejects.** Delete
   demo content rather than adapting it.
6. **Favicons and SVGs cache hard.** Hard-reload before concluding a change failed.
7. **Override paths must be exact.** A misplaced override fails *silently* — the
   build passes and nothing changes. After installing one, verify by grepping the
   built output for something it introduces, e.g.
   `grep -c 'max-width: 56rem' public/index.html`. Zero means it isn't loading.
8. **Renaming a content folder loses co-located assets** if you move only the
   `.md` files — `featured.png` was lost this way in the projects→software rename.
9. **`article-grid` always reserves a 16:9 image panel.** With no image it renders
   a gradient placeholder plus a generic glyph. Either supply images or switch to
   `view: citation` / `date-title-summary`, which have no image area.

---

## 5 · Current configuration

**Menu** (`menus.yaml`) — 8 flat items, no dropdown:

```
Bio(/) · Research · Papers(/publications/) · Software · Teaching* · Experience · News* · Contacts
                                                        * → /under-construction/
```

**Appearance** (`params.yaml`)
```yaml
theme:
  mode: dark          # a DEFAULT, not enforcement — localStorage overrides it
  pack: "dracula"
header:
  theme_toggle: false # hides the day/night button
  theme_picker: false # hides the theme-pack dropdown
```
Anyone with a stored preference keeps it — clear with
`localStorage.removeItem("wc-color-theme")`.

**Citations** (`params.yaml`) — consumed by the `citation.html` override
```yaml
citations:
  style: apa
  author_limit: 3            # 0 = show every author
  highlight_author:          # BOTH variants needed — the bib uses both
    - 'Tommaso Ronconi'      # 19 entries
    - 'T. Ronconi'           # 10 entries
```

**Markdown block width** — `design: { width: wide }` currently on the homepage
"My Research" block and both research prose blocks. Options: `prose` (default,
65ch) · `wide` (56rem) · `wider` (72rem) · `full` · any CSS length.

**Palette** (Dracula) — reuse for any new artwork:
```
bg #282a36 · line #44475a · muted #6272a4 · fg #f8f8f2
purple #bd93f9 · cyan #8be9fd · orange #ffb86c · yellow #f1fa8c
```

---

## 6 · Open items

**Images (highest visual impact)**
- [ ] Consider a figure on each research axis page (the SED + posterior figure
      from the research statement suits galaxy formation).

**Content**
- [ ] `me.yaml`: 3 remaining `TODO`s — Google Scholar, LinkedIn (matters for the
      industry-exit audience), English self-rating. ORCID and ADS are now filled.
- [ ] Research pages: `*Software:*` lines are plain text for SCAMPy, T-RECS and
      CosmoBolognaLib — those pages now exist, so make them links.
- [ ] 6 bib entries contain LaTeX non-breaking tildes rendering literally
      (`M.~M. Cueli`). Fix in the 6 generated `index.md` files rather than the
      `.bib` — avoids a destructive re-import.
- [ ] Scale numbers for SCAMPy / T-RECS / CosmoBolognaLib **Stack** lines
      (catalogue sizes, runtimes, cores). GalaPy has its verified ~1000 SEDs/s.
- [ ] Role wording unresolved: "ARC duty astronomer" (research statement) vs
      "Fixed-Term Researcher — HPC Expert" (`me.yaml`). Pick one for public use.

**Housekeeping**
- [ ] Consider `aliases: [/projects/]` on `content/software/_index.md` if any
      external links still point at the old URL.
- [ ] `internal-readme-news.yml` and `.github/FUNDING.yml` are HugoBlox-internal
      — safe to delete.
- [ ] Publication build warnings ("legacy flat `publication` string",
      "top-level `doi` deprecated") are harmless; `hugoblox migrate publications`
      would silence them.

---

## 7 · Next: Teaching and News

Agreed split, to avoid duplicating content:

- **Teaching** — the durable record: courses taught, levels, institutions,
  students supervised, plus links to GitHub repos with materials and recordings
  or streams of past lectures. Changes yearly. (5+ PhD-level courses and 3
  supervised students are in the CV.)
- **News** — the time-ordered stream: what I'm currently up to and where I'll be
  or have recently been. Talks, seminars, outreach. Changes monthly.
  (~30 talks, 7 invited + 1 colloquium since 2022, listed in the CV.)

Notes:
- `content/events/` exists as a stub and is the natural home for News. HugoBlox
  events support `date_end`, `location`, and `event`-type links.
- Prefer **one collection with a `category:` field** (outreach / teaching / talk /
  seminar) over separate sections — easier to maintain, allows filtering later.
- Wire up: point the menu entry at the new URL and remove the matching alias from
  `under-construction/index.md`.

---
