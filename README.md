# Wat-RS.github.io

Website for the **Watershed Remote Sensing Lab (Wat-RS)** at the University of
Northern British Columbia, built with [Quarto](https://quarto.org).

<https://watershedlab.ca>

## Structure

| Path | Purpose |
| --- | --- |
| `_quarto.yml` | Site config: navbar, theme, footer, output dir |
| `index.qmd` | Landing page (lab overview, news feed, projects, partners) |
| `people.qmd` | Students, mentees and collaborators |
| `opportunities.qmd` | Openings for students and postdocs |
| `pubs.qmd` | Publications |
| `outreach.qmd` | Media, science communication and public talks |
| `cv.qmd` | Curriculum vitae |
| `courses.qmd` | Courses, workshops and events |
| `resources.qmd` | Data sources and tools |
| `custom.scss` | Brand palette and all custom styling, layered over Bootstrap `cosmo` |
| `images/logo/` | Logo variants (see below) |
| `academicicons/` | Vendored [Academicons](https://jpswalsh.github.io/academicons/) for the Scholar/ResearchGate navbar icons |
| `CNAME` | Custom domain for GitHub Pages |
| `docs/` | **Rendered site — committed, and what GitHub Pages serves** |

## Building

From the terminal:

```sh
quarto render      # build into docs/
quarto preview     # live-reload preview while editing
```

From R: `quarto::quarto_render()` (or `source("render.R")`). In RStudio, the
**Render** / **Build Website** button does the same.

`docs/` is regenerated on every render, so commit it along with your source
changes — GitHub Pages publishes from `main` → `/docs`.

## Deployment

GitHub Pages serves `docs/` from the default branch (`main`). Two files make that work,
and both are listed under `project: resources:` in `_quarto.yml` so they are
copied into `docs/` on every render — without that, each rebuild would wipe
them and break the site:

- `.nojekyll` — stops GitHub running Jekyll over the output
- `CNAME` — the custom domain, `watershedlab.ca`

DNS is hosted at Cloudflare. The apex is canonical; `www` redirects to it.
To make `www` canonical instead, change the single line in `CNAME` to
`www.watershedlab.ca`, re-render, and update the domain in
**Settings → Pages**.

## Logo

`images/logo/` holds variants generated from `watrs-logo-source.png`:

| File | Use |
| --- | --- |
| `watrs-horizontal.png` | Navbar brand — emblem and wordmark side by side |
| `watrs-favicon.png` | Favicon |
| `watrs-mark.png` | Emblem on a white disc, transparent outside — social cards, any background |
| `watrs-stacked.png` | Full stacked lockup for slides and print |
| `watrs-logo-source.png` | Original master artwork |

## Notes

- `pubs.qmd` and `cv.qmd` set `engine: knitr` so the inline
  `` `r lubridate::today()` `` "last updated" stamp evaluates. Quarto only
  auto-selects knitr when a file has R *chunks*, not inline code alone.
- The homepage news feed is a `.column-margin` div wrapping a plain
  `.news-scroll` div. It must contain **no markdown heading** — Quarto moves a
  margin div's classes onto the generated `<section>` when a heading is
  present, which silently breaks any CSS hung on the container. Hence the
  `.news-head` div rather than `### News`.
- Quarto stamps `data-bs-theme="dark"` on the navbar element regardless of
  `$navbar-bg`. With the white navbar that would flip Bootstrap's dark colour
  variables for everything nested inside it, including the dropdown menus, so
  `custom.scss` re-asserts the light values.

## Editing in VS Code

Install the [Quarto extension](https://marketplace.visualstudio.com/items?itemName=quarto.quarto),
then use **Ctrl+Shift+K** to render or **Ctrl+Shift+P → Quarto: Preview** for a
live preview in a side panel.

Quarto 1.10 is installed at
`%LOCALAPPDATA%\Programs\Quarto\bin`. If `quarto` is not found in a terminal,
that directory is on the user PATH but the shell was started before it was
added — restart VS Code, or for the current session only:

```powershell
$env:Path = [Environment]::GetEnvironmentVariable('Path','Machine') + ';' +
            [Environment]::GetEnvironmentVariable('Path','User')
```
