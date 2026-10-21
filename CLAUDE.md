# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Personal website of Purnama Anaking — Software Engineer & Lecturer at Telkom University Surabaya. Tagline: "Technology with Purpose, Sharing with Heart."

## Tech Stack

- **Static Site Generator:** Hugo Extended (v0.145.0), with Go modules (`go.mod`) for Hugoplate's component modules
- **Theme:** Hugoplate (zeon-studio), vendored at `themes/hugoplate/`
- **CSS:** TailwindCSS 4 via the Hugo pipeline (`css.TailwindCSS` in `themes/hugoplate/layouts/partials/essentials/style.html`), with a custom Tailwind plugin at `tailwind-plugin/tw-theme.js` that generates color/font utilities from `data/theme.json`
- **Package Manager:** npm/Yarn interchangeably (package.json present, `yarn.lock` ignored); Node 24.x
- **Deploy:** three independent targets built from the same repo, each with its own `HUGO_BASEURL` — see Deployment below

## Development

```bash
npm install          # or: yarn install
npm run dev           # dev server at http://localhost:1313
npm run build          # production build → docs/ (minified, fingerprinted)
npm run preview        # production-mode dev server (minified, with file watch)
npm run format          # prettier -w . (uses prettier-plugin-go-template for templates)
```

There is no test suite or linter beyond Prettier formatting.

## Deployment — three domains, three baseURLs

The same `docs/` output is deployed to three places, and each build must bake in its own domain as an absolute `baseURL` (asset URLs, the service worker registration, and SRI integrity hashes are origin-specific — a baseURL mismatch breaks the site with CORS/SRI console errors, not a visibly broken page):

| Target | Domain | How baseURL is set |
|---|---|---|
| GitHub Pages | `purnamaanaking.github.io` | `hugo.toml`'s `baseURL` (the default, used when no `HUGO_BASEURL` env var is set) |
| Netlify | `purnamaanaking.netlify.app` | `HUGO_BASEURL` env var in `netlify.toml` |
| Vercel | `purnamaanaking.vercel.app` | `HUGO_BASEURL` env var set in the Vercel project settings (not in a committed file); build runs via `vercel-build.sh`, which installs Go + Hugo on Vercel's Linux image before running `npm run build` |

`docs/` is committed to the repo (it's GitHub Pages' publish source directly from `main`), so every `npm run build` before a push regenerates and commits `docs/` using `hugo.toml`'s default baseURL (github.io). Netlify and Vercel each rebuild from source independently using their own env var, so they never see the committed `docs/`.

**Known failure mode:** running `npm run build` while a `npm run dev` server is still active on the same machine can leak the dev server's unminified output (with a `livereload.js` script tag) into `docs/` instead of a clean build. Always `pkill -f "hugo server"` (or confirm nothing is listening on :1313) before a production build that will be committed.

## Architecture

### Local overrides take priority over the theme

Hugo resolves templates from `layouts/` before falling back to `themes/hugoplate/layouts/`. This repo makes heavy use of that: most page-level templates and several component partials are overridden locally rather than edited in the theme, so the theme can in principle be updated without losing customizations. Current local overrides live under `layouts/`: page templates (home, about, blog list/single, taxonomy/terms, 404), the header/footer, and several `partials/components/*` and `partials/widgets/*` partials. When changing a page's layout or a shared component's markup, check `layouts/` first — editing the copy under `themes/hugoplate/` has no effect if a local override exists.

Theme CSS (e.g. `themes/hugoplate/assets/css/base.css`, `navigation.css`) is edited directly when a change is theme-wide and structural (e.g. heading font-weight rules, nav-link active state); one-off style additions go in `assets/css/custom.css` instead.

### Design tokens flow through `data/theme.json`, not Tailwind config

Colors and fonts are not hardcoded Tailwind classes. `data/theme.json` defines a `default` (light) and `darkmode` palette plus font families; `tailwind-plugin/tw-theme.js` reads that file at build time and generates CSS custom properties (`--color-<key>`) plus matching Tailwind utilities (`bg-<key>`, `text-<key>`, `border-<key>`, and `darkmode-<key>` variants for dark mode, applied via Tailwind's `dark:` variant). To change the site's palette or type, edit `data/theme.json` — do not add one-off hex colors in templates.

The current palette and type scale come from a custom-built design system ("Purnama"): an ink/neutral scale with a single warm accent color (`primary` token), Schibsted Grotesk for display/body text (`font-primary`), and JetBrains Mono for code/labels/eyebrows (`font-secondary`). Two separate color roles exist that look similar but aren't interchangeable: `primary` is the accent used for text/links/icons, while `action` is the solid-fill color for primary buttons — they resolve to different hex values in both light and dark mode, so don't assume `bg-primary` and `.btn-primary`'s background are the same token.

A recurring UI motif across section headers is a `</>` glyph prefix in mono font before an uppercase eyebrow label (e.g. "Blog", "Focus", "Browse"); `layouts/partials/header-motif.html` renders a decorative "code lines" pattern (stacked pill bars + a chevron pair) used behind the hero and other large empty areas, echoing the brand's bracket motif.

### Theme (dark/light) switching avoids flash-of-wrong-theme by running before paint

`layouts/partials/custom-script.html` is invoked from `themes/hugoplate/layouts/partials/essentials/head.html` and applies the `dark` class to `<html>` synchronously in `<head>`, before the body renders, based on `localStorage` / system preference / `site.Params.theme_default`. The actual toggle UI lives in `layouts/partials/components/theme-switcher.html` (a checkbox-driven icon swap). Keep these two concerns separate: the head script only decides the *initial* class on page load; the switcher partial only wires up the click-to-toggle behavior and syncs the checkbox's visual state. Do not reintroduce a "hide element, then un-hide via `DOMContentLoaded`" pattern for the switcher — that was tried before and causes a visible layout shift/flash of the surrounding navbar when the hidden control's box suddenly takes up space.

### Navbar layout is one flex row, not independently positioned pieces

`layouts/partials/essentials/header.html` lays out the logo, desktop nav links, search button, language/theme switcher, and mobile hamburger as siblings in a single flex row (logo pinned left via `margin-right: auto`, everything else packed to the right) — there is a separate, hidden-until-toggled `<ul>` for the mobile dropdown menu, reusing the same checkbox-based toggle trick as the theme switcher (`input:checked ~ selector` CSS, no JS). Don't reintroduce `position: absolute` centering for the nav links — it was tried and conflicts with this flex layout, and also isn't how the source design system's header works.

### Content and images

- Blog posts: `content/english/blog/*.md`, front matter controls `draft`, `categories`, `tags`, and an optional `image` field used for the card/hero image. Posts without `image` render without one — no error.
- About page body copy comes from `content/english/about/_index.md`, but the **tech stack list is hardcoded** as a `$categories` slice inside `layouts/about/list.html` (not data-driven) — edit that file directly to add/remove technologies.
- Blog images are duplicated in two places on purpose: `assets/images/blog/` (Hugo image processing pipeline, used by shortcodes/templates) and `static/images/blog/` (served as-is, used by plain markdown `![alt](/images/blog/...)` syntax in post bodies).
- Social links: `data/social.json` under the `main` key, consumed by both the footer and the about page's social icon row.
- Navigation menus: `config/_default/menus.en.toml` (main nav + footer nav).

## Branch Strategy

- `main` — production, auto-deployed to all three targets above
- `development` — active work, PR to `main` when ready
- `main-backup` — old blog post markdown files, not part of the active site
