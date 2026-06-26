<p align="center">
  <strong>n o u s</strong><br>
  <em>a typography-first Hugo theme</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Hugo-%3E%3D0.120-ff4088?style=flat-square&logo=hugo" alt="Hugo">
  <img src="https://img.shields.io/badge/License-MIT-blue?style=flat-square" alt="License">
  <img src="https://img.shields.io/badge/i18n-PT%20%7C%20EN-green?style=flat-square" alt="Multilingual">
</p>

---

Nous is a minimal Hugo theme for writers and researchers. No JavaScript frameworks, no build tools, no noise — just Playfair Display for headings, Lora for body text, and clean CSS.

Built for personal blogs, academic writing, essay collections, and publication portfolios.

## Preview

| Dark | Light |
|------|-------|
| ![dark](images/screenshot-dark.png) | ![light](images/screenshot-light.png) |

## Features

| | |
|-|-|
| **Post cards** | Cover image, tags, description, and date in styled cards |
| **Papers layout** | Academic publication cards with auto-generated citations (APA, ABNT, Chicago, MLA) |
| **Dark / Light mode** | Toggle with persistence via localStorage |
| **Multilingual** | i18n-ready, tested with Portuguese and English |
| **Cookie consent** | Bilingual bar, dismissible, respects localStorage |
| **Copy attribution** | Pasted text automatically includes author credit |
| **Selection colors** | Yellow on dark, orange on light |
| **Highlight & underline** | `==word==` in red, `<u>word</u>` with red underline |
| **Responsive** | Single-column, mobile-friendly |
| **RSS** | Feed out of the box |
| **Zero dependencies** | Vanilla JS, no build step |

## Quick start

### 1. Install

```bash
# Option A: submodule (recommended)
git submodule add https://github.com/derossimolina/nous-theme.git themes/nous

# Option B: clone
git clone https://github.com/derossimolina/nous-theme.git themes/nous
```

### 2. Configure

Add to your `hugo.toml`:

```toml
theme = "nous"

[params]
  shortTitle  = "Your Name"
  homeTitle   = "Your Name"
  homeContent = "A short description for the homepage"

[markup.goldmark.renderer]
  unsafe = true

[markup.goldmark.extensions.extras.mark]
  enable = true
```

### 3. Run

```bash
hugo server
```

## Post cards

Posts on the homepage and list pages are rendered as cards. Add optional fields to your post front matter to enrich the card:

```yaml
---
title: "My post title"
date: 2024-06-01
tags: ["topic-a", "topic-b"]
description: "A short summary shown below the title in the card."
cover:
    image: "/images/my-post/cover.jpg"
    alt: "Description of the cover image"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `title` | yes | Card heading (links to the full post) |
| `date` | yes | Shown in the bottom-right corner |
| `tags` | no | Rendered as pills above the title |
| `description` | no | Short summary below the title |
| `cover.image` | no | Path to the cover image (displayed edge-to-edge at the top) |
| `cover.alt` | no | Alt text for the cover image |

Cards degrade gracefully — if a field is missing, it is simply omitted.

## Papers layout

A dedicated layout for academic publication pages. Each paper is rendered as a card with an expandable abstract and citation selector. Citations in **APA**, **ABNT**, **Chicago**, and **MLA** are generated automatically from structured front matter.

Set `layout: "papers"` and list your publications:

```yaml
---
title: "Papers"
layout: "papers"
papers:
  - title: "Title of the article"
    authors: "Surname, A. B."
    journal: "Journal Name"
    volume: 10
    issue: 2
    pages: "30-45"
    year: 2024
    doi: "10.xxxx/example"
    abstract: "Abstract text. Supports basic HTML like <strong> and <br>."

  - title: "Another article (no DOI)"
    authors: "Surname, A."
    journal: "Another Journal"
    volume: 5
    pages: "1-20"
    year: 2022
    url: "https://example.com/article"
    abstract: "Falls back to URL when DOI is not provided."
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `title` | yes | Paper title (links to DOI or URL) |
| `authors` | yes | Author name as `Surname, Initials.` — ABNT uppercase is generated automatically |
| `journal` | yes | Full journal name |
| `volume` | yes | Volume number |
| `issue` | no | Issue number (omitted from citations when absent) |
| `pages` | yes | Page range with hyphen, e.g. `"1-29"` (converted to en-dash in APA/Chicago/MLA) |
| `year` | yes | Publication year |
| `doi` | no | DOI without the `https://doi.org/` prefix (preferred over `url`) |
| `url` | no | Direct link, used when `doi` is not provided |
| `abstract` | no | Abstract text; supports `<strong>`, `<br>` for structured abstracts |

Labels adapt to the site language — Portuguese (`Resumo`, `Citar`, `Copiar`) or English (`Abstract`, `Cite`, `Copy`).

## Full configuration

<details>
<summary>Expand full <code>hugo.toml</code> example</summary>

```toml
baseURL = "https://example.com/"
defaultContentLanguage = "pt"
title = "My Site"
theme = "nous"
paginate = 10

[params]
  description = "Notes, essays and reflections"
  shortTitle  = "J. Doe"
  homeTitle   = "J. Doe"
  homeContent = "Writing about things that matter"

[languages]
  [languages.pt]
    languageName = "Português"
    languageCode = "pt-BR"
    weight = 1
    [[languages.pt.menu.main]]
      name   = "postagens"
      url    = "/posts/"
      weight = 10
    [[languages.pt.menu.main]]
      name   = "artigos"
      url    = "/papers/"
      weight = 20
    [[languages.pt.menu.main]]
      name   = "ensaios"
      url    = "/essays/"
      weight = 30
    [[languages.pt.menu.main]]
      name   = "sobre"
      url    = "/about/"
      weight = 40

  [languages.en]
    languageName = "English"
    languageCode = "en-US"
    weight = 2
    [languages.en.params]
      description = "Notes, essays and reflections"
      homeContent = "Writing about things that matter"
    [[languages.en.menu.main]]
      name   = "posts"
      url    = "/posts/"
      weight = 10
    [[languages.en.menu.main]]
      name   = "papers"
      url    = "/papers/"
      weight = 20
    [[languages.en.menu.main]]
      name   = "essays"
      url    = "/essays/"
      weight = 30
    [[languages.en.menu.main]]
      name   = "about"
      url    = "/about/"
      weight = 40

[markup.goldmark.renderer]
  unsafe = true

[markup.goldmark.extensions.extras.mark]
  enable = true

[outputs]
  home = ["HTML", "RSS"]
```

</details>

## Content structure

```
content/
├── about.md
├── about.en.md
├── papers.md              # layout: "papers" + YAML data
├── papers.en.md
├── posts/
│   ├── my-post.pt.md      # with cover, tags, description
│   └── my-post.en.md
└── essays/
    ├── _index.pt.md
    ├── _index.en.md
    ├── my-essay.pt.md
    └── my-essay.en.md
```

## Writing extras

Nous extends standard Markdown with two visual accents for emphasis:

```markdown
This is ==highlighted text== in red.

This is <u>underlined text</u> with a red underline.
```

> Requires `[markup.goldmark.extensions.extras.mark] enable = true` and `[markup.goldmark.renderer] unsafe = true`.

## Customization

The entire theme is styled through CSS custom properties. Override them in your own stylesheet:

```css
:root {
  --bg:      #0b0b0b;   /* background        */
  --surface: #111111;   /* cards, code blocks */
  --border:  #1e1e1e;   /* dividers           */
  --text:    #c8c8c8;   /* body text          */
  --muted:   #555555;   /* secondary text     */
  --bright:  #ebebeb;   /* headings, hover    */
  --link:    #9a9a9a;   /* links              */
  --max:     660px;     /* content width      */
}
```

## Typography

| Element  | Font             | Weight |
|----------|------------------|--------|
| Headings | Playfair Display | 500    |
| Body     | Lora             | 400    |
| Code     | JetBrains Mono   | —      |

## License

[MIT](LICENSE) — use it, fork it, make it yours.

---

<p align="center">
  Made by <a href="https://derossimolina.github.io/">J. de Rossi Molina</a>
</p>
