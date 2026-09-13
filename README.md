# RQ Global — website

Marketing website for RQ Global, a structural engineering consultancy practising in
Ontario, Canada. Domain: **rqglobal.ca**

---

## ⚠️ Status: not built yet

**There is no website in this repository yet.** It currently holds project setup
only — this README, the build brief in `CLAUDE.md`, editor config, and ignore
rules. There is no `package.json`, no `src/`, and nothing to run or deploy.

Cloning this today gets you the setup, not a site. The commands below are
written for the scaffold that is being built, so they are ready when it lands,
but **they will not work until it does.**

## The rule that outranks every other rule here

**Nothing in this site may be invented.** No project names, client names,
testimonials, award claims, licence numbers, engineer counts, or years in
business get written unless someone has supplied them as fact.

This site goes into procurement processes. A fabricated credential is a
disqualification and potentially a professional-conduct matter. Every value not
yet confirmed is wrapped in a visible `[PLACEHOLDER: ...]` marker and listed in
`CONTENT-TODO.md`, which must be empty before launch.

---

## Prerequisites

- **Node 22.12 or newer** (`node -v` to check). Astro requires it.
- npm 9.6.5 or newer.
- git.

## Local development

```bash
git clone https://github.com/Cricazzy/rqglobal-website.git
cd rqglobal-website
npm install
npm run dev          # local dev server, usually http://localhost:4321
```

To produce the production files:

```bash
npm run build        # writes static HTML/CSS/JS to dist/
npm run preview      # serve dist/ locally to check it before deploying
```

**`dist/` is what gets hosted.** It is plain static files — no Node, no
database, no PHP. Any host that serves static files will serve this site.

---

## Deploying

### Option A — GitHub Pages (free preview link)

Best for sharing work in progress before the real domain is live. Gives a URL
like `https://cricazzy.github.io/rqglobal-website`.

1. In `astro.config.mjs`, set the site and base path:

   ```js
   import { defineConfig } from 'astro/config'

   export default defineConfig({
     site: 'https://cricazzy.github.io',
     base: '/rqglobal-website',
   })
   ```

   Internal links must then include the base: `<a href="/rqglobal-website/about">`.

2. Add `.github/workflows/deploy.yml`:

   ```yaml
   name: Deploy to GitHub Pages
   on:
     push:
       branches: [ main ]
     workflow_dispatch:

   permissions:
     contents: read
     pages: write
     id-token: write

   jobs:
     build:
       runs-on: ubuntu-latest
       steps:
         - name: Checkout your repository using git
           uses: actions/checkout@v7
         - name: Install, build, and upload your site
           uses: withastro/action@v6

     deploy:
       needs: build
       runs-on: ubuntu-latest
       environment:
         name: github-pages
         url: ${{ steps.deployment.outputs.page_url }}
       steps:
         - name: Deploy to GitHub Pages
           id: deployment
           uses: actions/deploy-pages@v5
   ```

3. Repository **Settings → Pages → Source → GitHub Actions**.

Push to `main` and it publishes itself.

> When moving to the real domain, `base` must be removed and `site` changed to
> `https://rqglobal.ca`, or every internal link breaks.

### Option B — Hostinger (the production host)

**Read this first, because it is the thing people get wrong.** Hostinger's Git
integration **pulls files, it does not build them.** The files committed to the
repo are the files served. Astro's source does not run in a browser — it has to
be built into `dist/` first. So pointing Hostinger's Git deploy at this repo's
`main` branch would publish the source code and serve a blank page.

Pick one of these three. **B1 is the simplest and the recommended starting point.**

#### B1. Build locally, upload `dist/` (simplest, no automation)

1. `npm run build` on your machine.
2. hPanel → **Files** → **File Manager** → open `public_html`.
3. Delete whatever is in `public_html` (the default parking page).
4. Upload **the contents of `dist/`** — not the `dist` folder itself. `index.html`
   must sit directly in `public_html`, not in `public_html/dist/`.

Repeat on every update. Takes about a minute.

#### B2. Hostinger Git, pointed at a built branch

Keeps deploys one click, without uploading by hand.

1. Build locally and commit `dist/` to a separate branch (e.g. `deploy`). This
   branch deliberately contains built output; `main` never does.
2. hPanel → **Advanced** → **Git** → **Connect with GitHub**, which installs
   Hostinger's GitHub App on your account (OAuth — no SSH keys needed).
3. Grant access to this repository and select it.
4. Set the branch to `deploy` and the directory to `public_html`.
   **The target directory must be empty for the first deployment.**
5. Deploy. Each later deploy replaces the files in that directory.

#### B3. GitHub Actions builds, then ships to Hostinger automatically

The proper setup once the site is live: a workflow runs `npm run build` on push
and transfers `dist/` to `public_html` over FTP or SSH, using Hostinger
credentials stored as GitHub repository secrets — never committed. Set this up
when the site is actually shipping; B1 is enough before then.

#### Domain and SSL

Point `rqglobal.ca` at the Hostinger account (hPanel → Domains), then issue the
free SSL certificate. Confirm `https://rqglobal.ca` loads and that plain `http://`
redirects to it.

---

## Adding a project to the site

Once the scaffold exists, projects are Astro content collections — a project is
a markdown file, not a code change:

1. Create a markdown file in the projects content directory.
2. Fill every field in the frontmatter schema: name, location, client, architect,
   year, building type, gross floor area, storeys above/below grade, gravity
   system, lateral system, foundation type, code framework and edition, seismic
   category, RQ Global's scope, engineer of record.
3. Anything not yet confirmed goes in as `[PLACEHOLDER: ...]` and gets an entry
   in `CONTENT-TODO.md`.
4. `npm run build` and deploy.

The schema is validated at build time, so a missing required field fails the
build rather than shipping a half-filled project page.

---

## Repository layout

| Path | What it is |
| --- | --- |
| `CLAUDE.md` | The build brief — design direction, performance budget, content rules |
| `.mcp.json` | Browser tooling config for local development |
| `.claude/skills/` | Third-party design tooling, **gitignored** — not ours to republish |
| `design-references/` | Reference material, **gitignored** for the same reason |

`.claude/skills/` and `design-references/` exist on the original development
machine but are deliberately absent from this public repository. Nothing in the
build depends on them.

---

## Licence

Not yet determined. Until one is added, all rights reserved — this is a
commercial site for a named firm, not a template.
