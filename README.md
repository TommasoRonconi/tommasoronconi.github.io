# Site build notes — tommasoronconi.github.io

Personal reminder for modifying the **HugoBlox Academic CV** template.
Not rendered by Hugo (root `README.md` is ignored by the build).

Template: HugoBlox `academic-cv` · Hugo Extended **0.162.0** (pinned in `hugoblox.yaml`).
Live URL target: <https://tommasoronconi.github.io/>

---

## 0 · One-time setup (do these first)

1. **Set the site URL.** `config/_default/hugo.yaml` → change
   `baseURL: 'https://example.com/'` to `baseURL: 'https://tommasoronconi.github.io/'`.
2. **Set the site identity.** `config/_default/params.yaml` → under `hugoblox.identity`:
   set `name: "Tommaso Ronconi"`, a `tagline`, a real `description`, and clear/replace
   `social.twitter` (leave blank if none).
3. **GitHub → Settings → Pages → Source = GitHub Actions.** (Not "Deploy from a branch".)
4. **GitHub → Settings → Actions → General → Workflow permissions = Read and write.**
   Required so the publications-import Action can open its PR.
5. **Drop in the author profile:** replace `data/authors/me.yaml` with the filled
   `me.yaml` I prepared, then complete its `TODO` placeholders (see §5).

After step 1–2 the deploy Action (already present in `.github/workflows/deploy.yml`,
using `actions/deploy-pages@v4`) will publish on every push to `main`.

---

## 1 · Keep / replace / delete map

Everything under `content/` is demo content unless you replace it.

| Path | What it is | Action |
|------|------------|--------|
| `data/authors/me.yaml` | **Master profile** — feeds homepage bio + `/experience` page | **Replace** with filled `me.yaml` |
| `content/_index.md` | Homepage: ordered **blocks** (bio, research, publications, talks, news, CTA) | **Edit** (see §2) |
| `content/experience.md` | `/experience` page — auto-renders from `me.yaml` | Keep (no edit needed) |
| `content/publications/_index.md` | Publications listing page (filters/search) | Keep |
| `content/publications/{conference-paper,journal-article,preprint}/` | Demo papers | **Delete** (before or after import) |
| `content/projects/{pandas,pytorch,scikit}/` | Demo projects | **Replace** with your packages (see `homepage_snippets.md` §3) |
| `content/events/example/` | Demo talk | **Replace** with real talks, or delete for now |
| `content/blog/*` (6 posts) | Demo posts — feed the "News" block | **Delete** (or replace with real news) |
| `content/courses/*` | HugoBlox tutorial course | **Delete** (see §6 re: Teaching) |
| `content/slides/example/` | Demo reveal.js slides | Delete unless you'll use slides |
| `content/authors/_index.md` | Author-pages switch (`render: never`) | Keep as-is |
| `cta-card` block inside `content/_index.md` | HugoBlox "build your own" promo (`demo: true`) | **Delete** the block |
| `.github/FUNDING.yml` | Sponsors HugoBlox author | Delete (optional) |
| `.github/workflows/internal-readme-news.yml` | HugoBlox-internal | Delete (optional) |
| `.github/workflows/{deploy,build,import-publications,upgrade}.yml` | The machinery | **Keep all** |
| `netlify.toml` | Netlify config | Harmless; delete if you like (you're on Pages) |
| `README.md` (template's marketing one) | — | Overwrite with this file |

---

## 2 · Homepage block order (`content/_index.md`)

Current order of `sections:` and the target:

```
1. resume-biography-3   → bio/hero (username: me). CV button → uploads/cv.pdf
2. markdown             → "My Research"  ......... replace text (snippets §1)
3. collection #papers   → Featured Publications ... keep (needs featured pubs)
4. collection           → Recent Publications ..... keep
   >>> INSERT HERE: collection #software (snippets §2)  ← sub-dominant software
5. collection #talks    → Talks (from events/) .... keep / fill events
6. collection #news     → News (from blog/) ....... keep / fill or remove
7. cta-card             → DELETE (HugoBlox promo)
```

The single change that implements your "engineering visible but sub-dominant"
decision is inserting the **software collection block after publications** —
paste-ready in `homepage_snippets.md` §2.

---

## 3 · Publications (BibTeX → pages, automated)

The repo ships `.github/workflows/import-publications.yml`. It triggers on a push
of a file named **`publications.bib` at the repository root**, runs
`academic import publications.bib content/publications/`, and **opens a PR** with
the generated page bundles (`index.md` + `cite.bib` per paper). Importer:
`academic` ≥ 0.10.0 (a.k.a. academic-file-converter / GetRD).

Steps:

1. Rename `TR.bib` → `publications.bib`, place it at the repo **root**, commit, push.
2. Wait ~1–2 min → review the auto-opened PR (diff of 29 new entries) → merge.
3. **Delete the 3 demo publication folders** listed in §1.
4. Open a few generated `content/publications/<id>/index.md` and check the
   `publication_types:` value the importer wrote — that string drives the type
   filters on the listing page. (Schema is version-specific; don't hand-write it,
   trust the importer's output, then tune the filter on `publications/_index.md`
   if needed.)
5. **Feature the key papers**: add `featured: true` to the front matter of the
   ones you want on the homepage. Suggested set (carries science + engineering
   + current SKAO work without over-weighting software):
   - `2024A&A...685A.161R` — GalaPy I (A&A)
   - `2026A&C....5501079R` — GalaPy implementation (A&C)
   - `2020MNRAS.498.2095R` — SCAMPy (MNRAS)
   - `2019MNRAS.488.5075R` — Cosmic voids uncovered (MNRAS)
   - `2026arXiv260325650R` — Painting a full radio sky (SKAO)

Categorisation sanity check: the `.bib` includes 1 `@phdthesis`, 1 `@software`
(ScamPy ASCL), 1 `@dataset` (VizieR) → these should **not** count as refereed
articles. The importer types them separately; verify the listing filters keep
that distinction so the refereed count stays honest.

---

## 4 · CV PDF

- Copy your CV to `static/uploads/cv.pdf`.
- In `content/_index.md`, set the biography button `url: uploads/cv.pdf`.
- Stable public URL will be `https://tommasoronconi.github.io/uploads/cv.pdf`.

(Optional, later: build the PDF from LaTeX in CI. Not worth it unless you update
the CV often.)

---

## 5 · Placeholders I could NOT fill (need your input)

These are in `me.yaml` marked `TODO`, because they are not in the CV/`.bib` and
I won't invent them:

- **ORCID iD** — add to `links` (uncomment the block).
- **Public ADS library URL** — add to `links`.
- **Google Scholar ID** — add if you maintain one.
- **LinkedIn URL** — add (matters for the industry-exit audience).
- **Institution URL** for INAF-IRA affiliation (verify before adding).
- **Education start dates** — end dates are filled; add starts to show ranges.
- **Skill `level` values (1–5)** — these are self-ratings; I set placeholders,
  set your own or delete the `level` keys.
- **English language level** — confirm/adjust.

---

## 6 · Where CV items that don't fit the profile go

`me.yaml` covers education, experience, skills, languages, awards. Not covered:

- **Teaching & mentoring** (your 5+ courses, 3 students): either repurpose the
  `content/courses/` section, or add a `markdown`/`collection` block. Do **not**
  leave the HugoBlox tutorial course content in place.
- **Service** (reviewer for A&A, MNRAS, A&C; SKAO/Euclid membership; WP lead):
  fold into the bio, the Experience summaries, or a short "Service" markdown block.
- **Talks** (~30, 7 invited): each goes in `content/events/<slug>/index.md`
  to populate the Talks block. Replace `content/events/example/`.

---

## 7 · Local preview (optional)

Requires Hugo **Extended** 0.162.0 and Node + pnpm.

```bash
npm install          # or: pnpm install
npm run dev          # = hugo server --disableFastRender  → http://localhost:1313
```

If you skip local preview, just push to `main` and let the deploy Action build.
Check progress under the repo's **Actions** tab.
