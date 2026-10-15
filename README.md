# The Long Way Around

Source for [sleekeagle.github.io](https://sleekeagle.github.io), built with Jekyll and the
[al-folio](https://github.com/alshedivat/al-folio) theme. Pushing to `master` runs `.github/workflows/deploy.yml`,
which builds the site and publishes it to the `gh-pages` branch (GitHub Pages serves from `gh-pages`).

## Where things live

| What | Where |
| --- | --- |
| Home page text | `_pages/about.md` |
| Long-form writing | `_posts/YYYY-MM-DD-slug.md` |
| Thoughts (short, unpolished) | `_thoughts/YYYY-MM-DD-slug.md` |
| Projects | `_projects/*.md` (`category: research` or `side projects`, ordered by `importance`) |
| Publications | `_bibliography/papers.bib` (`selected = {true}` shows it on the home page) |
| CV page | `_data/cv.yml`; PDF in `assets/pdf/Lahiru_Wijayasingha_CV.pdf` |
| Social links | `_data/socials.yml` |
| Images, audio, PDFs | `assets/img/`, `assets/audio/`, `assets/pdf/` |

## Posting a thought

Create `_thoughts/2026-10-20-some-slug.md`:

```markdown
---
date: 2026-10-20
headline: Optional title # leave out for an untitled thought
tags: [ideas]
---

Whatever is on your mind.
```

Add `more: true` to show only the first paragraph on the Thoughts page with a "keep reading" link.

## Writing a post

```markdown
---
layout: post
title: My post
date: 2026-10-20
description: One line shown in the post list.
tags: [machine-learning]
categories: [articles]
---
```

For an article published elsewhere (Medium, LinkedIn), add `redirect: https://...` and use `categories: [external]`.
