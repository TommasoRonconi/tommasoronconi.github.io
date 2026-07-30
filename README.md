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

Deployment is automatic: push to `main` → `.github/workflows/deploy.yml` builds and
publishes. GitHub **Settings → Pages → Source** must stay on **GitHub Actions**.

### Local preview (optional)

```bash
npm install
npm run dev          # hugo server --disableFastRender → http://localhost:1313
```

Needs Hugo **Extended** 0.162.0 and Go installed (Hugo resolves the theme as a Go
module). If you skip local preview, push and watch the **Actions** tab.

---

## 1 · `layouts/` — local overrides ⚠️ READ BEFORE ANY THEME UPGRADE

Any file in `layouts/` **silently shadows** the same path inside the theme module.
This is powerful and invisible — when something renders oddly after an upgrade,
look here first.

| File | Why it exists | On theme upgrade |
|---|---|---|
| `_partials/functions/build_links.html` | **BUG PATCH.** Upstream does `{{ $seen.Set "set" (dict) }}`; under Hugo 0.162.0 zero-arg `dict` yields a **nil map**, so the next `SetInMap` aborts the build on *every* page with author-provided `links:`. Patched to `(dict "__init__" true)`. | **Delete and retest.** Once upstream fixes it, this file is dead weight and will freeze an old version of the partial. |
| `_partials/views/citation.html` | **CUSTOMISATION.** Title-first publication lines, author truncation, always-visible bolded self, no empty `href=""` links. | **Keep**, but re-diff against the new upstream version so you don't lose upstream improvements. |
| `404.html` | **CUSTOMISATION.** Custom illustrated 404. | Keep. |
| `_partials/hooks/head-end/github-button.html` | **Shipped with the template** (loads `buttons.github.io`). Not a local addition. | Leave alone, or delete if you never use GitHub star buttons. |

**Upgrade procedure**

```bash
hugo mod get -u github.com/HugoBlox/kit/modules/blox
# then: temporarily rename layouts/ → layouts.bak/, build, and see what breaks.
# Re-introduce overrides one at a time; drop any that upstream has fixed.
```

---

## 2 · Repo map (current state)

```
config/_default/
  hugo.yaml       baseURL; disableAliases: false  ← required for redirects
  params.yaml     theme mode/pack, header toggles, citations options
  menus.yaml      nav: Bio · Projects · Experience · Courses · News
data/authors/
  me.yaml         MASTER PROFILE — feeds homepage bio AND /experience
content/
  _index.md       homepage blocks (bio → My Research → Featured Pubs → Recent Pubs)
  experience.md   /experience — renders from me.yaml; blocks listed here
  publications/   29 imported entries (5 featured, each with featured.* image)
  software/       galapy/ only so far
  events/         stub only — reserved for the future calendar page
  under-construction/  placeholder page + redirect aliases
  authors/        _index.md with render: never (author pages disabled)
assets/media/     hero-bg.png · icon.svg · icon.png   ← Hugo asset pipeline
static/
  media/404-not-found.svg
  uploads/cv.pdf  → https://tommasoronconi.github.io/uploads/cv.pdf
publications.bib  29 entries — SOURCE OF TRUTH for the publication list
layouts/          see §1
```

**`assets/media/` vs `static/`** — the distinction matters:
- `assets/media/` → processed by Hugo (resized, converted to WebP). Backgrounds and
  the favicon **must** live here or the build errors out.
- `static/` → copied verbatim, stable public URL. Use for the CV PDF and for images
  referenced by raw `<img src="/media/...">` (e.g. the 404 artwork).

---

## 3 · Routine tasks

### Add or update publications

`publications.bib` at the repo root is the source of truth.

1. Edit/append entries (ADS BibTeX export).
2. Commit + push → `.github/workflows/import-publications.yml` fires → opens a PR
   within ~2 min → review the diff → merge.
3. Requires **Settings → Actions → General → Workflow permissions = Read and write**
   (already set; if PRs stop appearing, check this first).

**Before importing, expand ADS journal macros** or journal names render garbled
(`\aap` → `åp`). Already done for the current file; redo for new entries:

```
{\aap}   → {Astronomy & Astrophysics}
{\mnras} → {Monthly Notices of the Royal Astronomical Society}
{\apj}   → {The Astrophysical Journal}
{\jcap}  → {Journal of Cosmology and Astroparticle Physics}
```

⚠️ **Re-importing overwrites existing publication folders.** That destroys
`featured: true` flags and any `featured.*` images. Either import only new entries,
or re-apply the five featured flags + images afterwards.

**Featured publications** (5, homepage `article-grid`): add `featured: true` to the
front matter, and drop any file named `featured.*` into that publication's folder.
The image also becomes the header of that publication's own page.

### Add a project / software package

Create `content/software/<name>/index.md`. Model it on `galapy/`.
Still to add: **SCAMPy**, **T-RECS**, **CosmoBolognaLib**.

### Update the CV

Overwrite `static/uploads/cv.pdf`. The URL is stable and bookmarkable — don't rename.

### Retire the "under construction" placeholder

`content/under-construction/index.md` holds `aliases:` (currently `/teaching/`).
Menu entries **Courses** and **News** point straight at `/under-construction/`.

When a real section goes live:
1. Create the real content section.
2. Remove its URL from `aliases:` (an alias and a real page at the same URL conflict).
3. Point the menu entry at the real URL in `menus.yaml`.

---

## 4 · Hard-won rules (each one cost a broken build)

1. **`date:` must be a full `YYYY-MM-DD`.** `date: 2026-07` fails with
   *"not a parsable date"*.
2. **Every `links:` entry needs a valid `type:`.** Missing or invalid → the build
   crashes with an `index nil` error. Valid vocabulary:
   `pdf · preprint · doi · code · dataset · model · slides · video · poster ·
   project · site · source · bibtex · canonical · crosspost · discussion ·
   event · calendar · registration · demo`.
   There is **no `docs` or `pypi` type** — use `type: site` with a `label:`.
3. **`disableAliases: false`** in `hugo.yaml` — the template ships it as `true`,
   which silently disables all redirects.
4. **Background images and favicons must be in `assets/media/`**, not `static/`.
5. **Demo content can ship in a schema its own pinned theme rejects** — the demo
   event used the old typeless `links:` format and crashed the build. When in
   doubt, delete demo content rather than adapting it.
6. **Favicons and SVGs cache aggressively.** After changing them, hard-reload or use
   a private window before concluding the change didn't work.

---

## 5 · Current configuration decisions

**Appearance** (`params.yaml`)
```yaml
theme:
  mode: dark          # a DEFAULT, not enforcement — localStorage overrides it
  pack: "dracula"
header:
  theme_toggle: false # hides the day/night button
  theme_picker: false # hides the theme-pack dropdown
```
Both toggles are off so visitors can't leave dark mode. Note: anyone whose browser
already stored a preference keeps it — clear with
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

**Palette in use** (Dracula) — reuse these for any new artwork:
```
bg #282a36 · line #44475a · muted #6272a4 · fg #f8f8f2
purple #bd93f9 · cyan #8be9fd · orange #ffb86c · yellow #f1fa8c
```

---

## 6 · Open items

**Content**
- [ ] Projects: add SCAMPy, T-RECS, CosmoBolognaLib (only GalaPy exists).
      Frame T-RECS and CosmoBolognaLib as **contributor** with the specific module
      you own — both are led by others (Bonaldi; Marulli), and an unqualified
      "maintainer" is weaker in front of a reviewer than a precise claim.
- [ ] `me.yaml`: Google Scholar and LinkedIn links still commented out
      (LinkedIn matters for the industry-exit audience); English level is a
      placeholder self-rating; MSc/BSc start dates still commented.
- [ ] 6 bib entries contain LaTeX non-breaking tildes rendering literally
      (`M.~M. Cueli`). Fix in `publications.bib` (`~` → space) *or* directly in the
      6 generated `index.md` files — the latter avoids a destructive re-import.

**Structure / navigation**
- [ ] **No nav entry points to `/publications/`.** The full filterable list of 29 is
      reachable only by scrolling the homepage. For an audience of academic
      reviewers this is the most costly omission on the site. One-line fix:
      ```yaml
      - name: Publications
        url: publications/
        weight: 15
      ```
- [ ] Software is **not** on the homepage — it lives only at `/software/`. The
      "visible but sub-dominant" plan was a `collection` block placed *after*
      publications. Optional; add if you want engineering visible without navigating
      to another page.
- [ ] `internal-readme-news.yml` and `.github/FUNDING.yml` are HugoBlox-internal —
      safe to delete.

---

## 7 · Planned: the calendar page

Single page covering **outreach · lecturing · talks · seminars**, replacing the
removed "Recent & Upcoming Talks" and "Recent News" homepage blocks (currently
commented out at the bottom of `content/_index.md` — delete them once the calendar
exists).

Notes for when you build it:
- `content/events/` already exists as a stub and is the natural home. HugoBlox
  events support `date_end`, `location`, and `event`-type links — much of the
  calendar behaviour is already there.
- Prefer **one collection with a `category:` field** (outreach / teaching / talk /
  seminar) over separate sections — far easier to maintain, and it allows filtering
  or colour-coding by category later.
- ~30 talks (7 invited + 1 colloquium since 2022) and 5+ PhD-level courses are
  listed in the CV, ready to be transcribed.
- Wire it up: point the **News** (and/or **Courses**) menu entry at the new URL and
  remove the corresponding alias from `under-construction/index.md`.

---
