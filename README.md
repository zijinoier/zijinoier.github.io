# wenlc.cn

Personal academic website for Licheng Wen, built with Jekyll and the al-folio theme.

## Local Development

This repository targets Ruby 3.2.0. If you use `rbenv`, the checked-in `.ruby-version` file selects the right runtime automatically.

```bash
bundle install
bundle exec jekyll build
bundle exec jekyll serve
```

Open `http://127.0.0.1:4000` after the server starts.

## Content

- Homepage content lives in `_pages/about.md`.
- Homepage structure lives in `_layouts/about.html`.
- Homepage-specific styles live in `_sass/_home.scss`.
- Publications are managed through `_bibliography/papers.bib`.
- News items are managed through `_news/`.

Projects are intentionally not displayed on the homepage. The old al-folio example project pages are disabled so placeholder content is not published by direct URL.

## Deployment

The production site is configured for `https://wenlc.cn`. Build locally before pushing:

```bash
bundle exec jekyll build
```
