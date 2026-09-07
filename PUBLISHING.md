# Publishing cheat sheet

Site: <https://travels.loganfamily.co.uk/> · Repo: `gmlogan/loganvantravels`

Deploys are **automatic**: every push to `main` triggers the GitHub Actions
workflow (`.github/workflows/hugo.yml`), which builds with Hugo and publishes to
GitHub Pages in ~30–40s. You never configure Pages by hand.

Run everything below from the project root: `/Users/graham/websites/loganvantravels`

---

## Everyday cycle

Preview locally while editing (live-reloads on save):

```bash
hugo server -D
```

Publish when happy:

```bash
git add -A
git commit -m "Add Foo Bar trip"
git push
```

Live a minute later.

---

## New post

**Easiest — the helper script** (names the folder, fills the front matter,
prefills the date line, opens it in the editor):

```bash
bin/new-post "The Bell Inn, Somewhere"
```

Add `2026-07-04` as a second argument to date it other than today. Then write
the body, drop images in that **same folder** (`![](image-1.jpg)`), and run the
`git add / commit / push` cycle.

**In Front Matter CMS** — the "Create content" button uses the `post` content
type from `frontmatter.json`: it makes the `YYYY-MM-DD-slug/index.md` bundle
with the front matter filled in.

**By hand / `hugo new`** — `hugo new content posts/some-place/index.md` uses
`archetypes/posts.md`; rename the folder to add the `YYYY-MM-DD-` prefix and fix
the `slug`.

Front matter used on this site:

```yaml
---
title: "Some Place"
date: 2026-07-05
draft: false
slug: "some-place"
wp_published: 2026-07-05   # original blog date; = date for new posts
---
```

---

## Check the build before pushing (optional)

```bash
hugo --gc --minify
```

Non-zero exit or `ERROR:` lines → fix before pushing. A broken build fails the
Action and the previous live site stays up.

---

## Watch / troubleshoot the deploy

```bash
gh run watch --repo gmlogan/loganvantravels --exit-status
```

```bash
gh run view --repo gmlogan/loganvantravels --log-failed
```

Force a redeploy with no code change:

```bash
gh workflow run "Deploy Hugo site to Pages" --repo gmlogan/loganvantravels
```

Actions dashboard: <https://github.com/gmlogan/loganvantravels/actions>

---

## Notes

- **Theme** is a git submodule (`themes/ananke`). After `git clone` on a new
  machine: `git submodule update --init --recursive`. To update it later:
  `git submodule update --remote themes/ananke` then commit.
- `public/` and `resources/` are build output — git-ignored, never committed.
- Custom-domain config lives in `static/CNAME` + `baseURL` in `hugo.toml`.
  Don't remove `static/CNAME` or Pages drops the domain.
- If the site looks stale right after a deploy, it's browser/CDN cache —
  hard-refresh (Cmd-Shift-R).
