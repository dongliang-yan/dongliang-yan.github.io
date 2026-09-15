# dongliang-yan.github.io

> **Working copy lives in `~/Public/dongliang-yan.github.io`.** macOS blocks tool access to
> `~/Documents` and `~/Desktop`; the copy still sitting in `~/Documents/dongliang-yan.github.io`
> is the older placeholder version and should be deleted or overwritten with this one.

Personal academic page. Plain HTML + CSS, no build step, no dependencies to install.

```
index.html        the whole site (layout + styles are inline, near the top)
assets/           photo, project figures, and your CV go here
.nojekyll         tells GitHub Pages to serve the files as-is
```

## Preview locally

```bash
cd ~/Public/dongliang-yan.github.io && python3 -m http.server 8765
```

Then open <http://localhost:8765>. Edit a file, refresh the browser — that's the whole loop.

## Publish to GitHub Pages

**The repo name must exactly match your GitHub username**, i.e. `<username>.github.io`.
This folder assumes your username is `DY`. If it isn't, rename the folder and the repo
to match your real username, or the page won't build. (GitHub usernames can't contain
dots — if you typed `dongliang-yan.github.io` meaning the site address, your username is `DY`.)

1. Create a **public** repo on GitHub named `dongliang-yan.github.io`. Don't add a README —
   this folder already has one.
2. From this folder:

```bash
git init -b main && git add -A && git commit -m "Personal site" && git remote add origin https://github.com/dongliang-yan/dongliang-yan.github.io.git && git push -u origin main
```

3. Wait ~1 minute. The site goes live at <https://dongliang-yan.github.io> — for
   `username.github.io` repos, Pages turns itself on automatically from the default
   branch, so there's nothing to configure. Check **Settings → Pages** if it doesn't
   appear.

Every later `git push` redeploys within a minute or so.

## What to edit

Everything you need to replace is either a `Your Name`-style placeholder or marked
with an `EDIT:` comment in the HTML.

- [ ] `<title>`, `<meta name="description">`, and the `og:` tags in `<head>`
- [x] Photo — `assets/profile.jpg`, cropped square from IMG_3389.HEIC, EXIF stripped
- [ ] Name, the optional second-script name line (delete it if unused), city/country
- [ ] The four sidebar links: GitHub, LinkedIn, Google Scholar, email
- [ ] Your CV → drop it at `assets/cv.pdf`
- [ ] About Me — four paragraphs
- [ ] Projects — copy a whole `<article class="project">` block per project.
      Figures go in `assets/`; ~800px wide is plenty. For videos, see *Project videos* below.
- [ ] Fun — side projects, one `<div class="entry">` each
- [ ] Publications — one `<li>` each. `<span class="me">` bolds your own name.

## Project videos

FASTER-AIR opens with a row of short, silent, looping videos (`<div class="media">`);
copy that row into another project to do the same. Videos in a row share one height —
each `<figure>` sets `--ar` to its video's width ÷ height, which this prints:

```bash
ffprobe -v error -select_streams v -show_entries stream=width,height -of csv=p=0 in.mp4
```

Re-encode a clip before adding it: H.264, no audio, and the index moved to the front so
it starts playing before it has finished downloading. Then take a poster frame, which
shows until the video plays:

```bash
ffmpeg -i in.mp4 -an -c:v libx264 -preset slow -crf 22 -pix_fmt yuv420p -movflags +faststart assets/name.mp4
ffmpeg -i in.mp4 -frames:v 1 -q:v 3 assets/name.jpg
```

Aim for a couple of MB per clip. Ultrasound speckle is expensive to encode and CAD
renders are cheap — the 6 s volume clip went from 6.4 MB to about 1 MB this way.
Visitors whose OS asks for reduced motion get the videos paused, with play controls.

## Google search

`robots.txt` lets every crawler in and points it at `sitemap.xml`, which lists the home
page. `index.html` carries a canonical URL and a JSON-LD `Person` block in `<head>` that
tells search engines who the page is about; when the LinkedIn icon gets a real URL, add
it to the block's `sameAs` list too, next to GitHub and Scholar. If you add another page,
list it in `sitemap.xml`.

## Colours and dark mode

All colours are CSS variables in the `:root` block at the top of each file. Change one
value and the whole page follows. There's a second block under
`@media (prefers-color-scheme:dark)` that supplies the dark palette — the page follows
the visitor's OS setting automatically. Delete that block if you want light-only.
