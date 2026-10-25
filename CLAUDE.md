# defangchen.github.io

Personal academic site built on the al-folio Jekyll theme. Pushing to `main` deploys it to GitHub Pages (`.github/workflows/deploy.yml`).

## Rule: every PDF must stay readable by AI agents (under 10 MB)

Papers and materials on this site should be easy for AI agents (Claude and others) to read, not only people. Agent fetch tools often reject downloads over 10 MB. Claude's fetch failed on arXiv's 42 MB PDF of 2305.19947 for that reason, not because of rate limiting, so a full-size copy here would fail the same way.

Before committing any PDF under `assets/`:

1. Run `bin/compress-pdf assets/pdf/<file>.pdf` (needs `brew install ghostscript poppler`). It tries 300, 220, 200 and 150 dpi and keeps the first result under 10 MB. It refuses the result if pages or text went missing, and moves the original to `_pdf_originals/`, which is gitignored and never published.
2. Look at the most figure-heavy page of the result, for example with `pdftoppm -f N -l N -r 200 -png file.pdf out`. Plot labels blur at 150 dpi.
3. Keep the filename, so the `pdf={...}` entry in `_bibliography/papers.bib` doesn't change.

Never commit a PDF over 10 MB. To check: `find assets -name '*.pdf' -size +10000000c`.

## Notes

- Paper buttons (Abs, arXiv, PDF, Code…) come from fields in `_bibliography/papers.bib`, rendered by `_layouts/bib.liquid`. `pdf={file.pdf}` links to `/assets/pdf/file.pdf`.
- Abstracts in `papers.bib` are written into the page HTML, which agents read reliably, so keep them filled in.
- Local preview: `docker compose up -d`, then open http://localhost:8080.
