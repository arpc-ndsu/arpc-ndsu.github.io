# arpc-ndsu.github.io

The open-research home of the [Agricultural Risk Policy Center](https://www.arpc-ndsu.com/)
at North Dakota State University: publications, software and data.

Live at **https://arpc-ndsu.github.io/**

## Why this repository is named what it is

GitHub serves a repository called `<owner>.github.io` from the **domain root**.
That is the only reason `https://arpc-ndsu.github.io/publications/` works. In a
repository with any other name the same files would serve one level down, at
`/arpc-ndsu/publications/`, and every absolute link in the site would be wrong.
Do not rename it.

## How it is built

A vendored [academicpages](https://github.com/academicpages/academicpages.github.io)
Jekyll site, built by **GitHub Pages' own Jekyll**. There is no build step to
run and no CI to maintain: push to `main` and GitHub publishes it.

The theme's machinery lives in `_layouts`, `_includes` and `_sass` and is not
usually edited. Everything the site actually says comes from:

| File | Holds |
|---|---|
| `_config.yml` | site title, description, the sidebar identity |
| `_data/navigation.yml` | the top navigation |
| `_data/publications.yml` | every publication |
| `_data/software.yml` | the Software and Tools page |
| `_data/datasets.yml` | the Data page |
| `index.md` | the home page |

### Previewing locally

```
bundle install
bundle exec jekyll serve
```
Then open <http://localhost:4000>. The `github-pages` gem pins Jekyll and every
allowed plugin to the versions GitHub runs, so what you see locally is what will
be published.

## Adding a publication

Add an entry to `_data/publications.yml`:

```yaml
- title: "Crop Insurance Penetration in the United States, 2025"
  authors: "Tsiboe, F., R. Chakravorty, H. Zhao, D. Turner, and S. Steinbach"
  year: 2026
  type: "ARPC Report"          # or ARPC White Paper / ARPC Working Paper
  site: "https://www.arpc-ndsu.com/publication/..."
```

That is enough to list it. It gains a **Read online** link the moment you give
it an `interactive:` folder, and a **PDF** link the moment you give it a `pdf:`.

## Adding an interactive edition

The interactive edition is a single self-contained HTML file written by
`arpcEditor::render_arpc_interactive()`. It is a complete document with its own
`<html>` element, so it is **not** a Jekyll page — it is copied through
untouched.

1. Put it at `publications/<slug>/index.html`, with the report's PDF beside it:

   ```
   publications/
     crop-insurance-penetration-2025/
       index.html
       ARPC Report 2026-02.pdf
   ```

2. Point the entry at it:

   ```yaml
   interactive: "crop-insurance-penetration-2025"
   pdf: "/publications/crop-insurance-penetration-2025/ARPC Report 2026-02.pdf"
   ```

3. Render it with the address it will live at, so the canonical link, the link
   preview and Google Scholar's PDF URL are right:

   ```r
   arpcEditor::render_arpc_interactive(
     report_dir,
     output = ".../publications/crop-insurance-penetration-2025/index.html",
     url    = "https://arpc-ndsu.github.io/publications/crop-insurance-penetration-2025/",
     pdf    = "ARPC Report 2026-02.pdf",
     doi    = "10.22004/ag.econ.XXXXXX")
   ```

   `pdf` is a bare file name on purpose: it is joined onto `url` to build the
   absolute `citation_pdf_url` that Scholar requires, which is why the PDF wants
   to sit in the same folder as the page.
