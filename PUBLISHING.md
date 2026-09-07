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

```bash
hugo new content posts/2026-07-05-some-place/index.md
```

- Put images in that **same folder**; reference them as `![](image-1.jpg)`.
- Set `draft: false` in the front matter (drafts don't publish).
- Then run the `git add / commit / push` cycle above.

Front matter used on this site:

```yaml
---
title: "Some Place"
date: 2026-07-05
draft: false
slug: "some-place"
wp_published: 2026-07-05   # optional, original blog date
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
