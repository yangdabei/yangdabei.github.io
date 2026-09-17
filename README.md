# yangdabei.github.io

Yangda Bei's personal website, built with [Hugo](https://gohugo.io/) and the
[Shibui theme](https://github.com/ntk148v/shibui).

## Edit the site

- Home page: `content/_index.md`
- Mathematics: `content/mathematics.md`
- Research: `content/research.md`
- Fun: `content/fun.md`
- Writings: `content/writings/`
- Publications shown on the home page: `data/publications.yml`
- Theme overrides: `assets/css/custom.css` and `layouts/`
- Site settings and navigation: `hugo.toml`

Files in the old Jekyll directories (`_pages`, `_posts`, `_data`, and similar)
are retained as migration source material, but Hugo does not publish them.

## Run locally

Install Hugo 0.93.0 or newer, initialise the theme submodule, and start the
development server:

```bash
git submodule update --init --recursive
hugo server
```

Open <http://localhost:1313/>. Build the production site with:

```bash
hugo --gc --minify
```

The GitHub Actions deployment workflow publishes the generated site to the
existing `gh-pages` branch whenever changes are pushed to `main` or `master`.
