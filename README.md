# Homelab Site

Personal site built with Jekyll + [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/),
hosted on GitHub Pages. Started as a homelab build log, intended to grow into
an IT/DevOps portfolio.

## Structure

```
_config.yml       Site-wide settings (title, theme, nav, collections)
_data/navigation.yml   Top nav links
_posts/           Blog-style posts (homelab log entries)
_projects/         Project writeups (its own collection, /projects/slug/)
_pages/           Standalone pages (About, Posts archive, Projects archive)
index.md          Homepage — lists recent posts
assets/           Images and other static files
```

## Local development

Requires Ruby (3.x recommended) and Bundler.

```bash
bundle install
bundle exec jekyll serve
```

Site will be at `http://localhost:4000`. Live-reloads on file changes.

## Publishing

Push to the `main` branch of your `username.github.io` repo. In the repo's
**Settings → Pages**, set the source to `Deploy from a branch` → `main` →
`/ (root)`. GitHub builds and deploys automatically — no CI config needed
since `github-pages` gem keeps you on GitHub's supported plugin/version set.

## Adding content

**New blog post:** add a file to `_posts/` named `YYYY-MM-DD-title.md` with
front matter:

```yaml
---
title: "Post Title"
date: 2026-07-15
categories: [category]
tags: [tag1, tag2]
excerpt: "One-line summary shown in listings."
---
```

**New project:** add a file to `_projects/`, no date-prefix required in the
filename:

```yaml
---
title: "Project Name"
date: 2026-07-15
excerpt: "One-line summary."
tech_stack: [Docker, Ansible]
---
```

## Notes

- Theme is loaded via `remote_theme` so GitHub Pages builds it server-side —
  no need to vendor the theme gem.
- `minimal_mistakes_skin` in `_config.yml` controls the color scheme; swap
  it anytime (`default`, `dark`, `neon`, `mint`, etc.).
- `Gemfile.lock` is gitignored here. If you want reproducible local builds
  across machines, remove it from `.gitignore` and commit it.
