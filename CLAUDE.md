# CLAUDE.md

Personal website of Purnama Anaking — Software Engineer & Lecturer at Telkom University Surabaya.

## Tech Stack

- **Static Site Generator:** Hugo Extended
- **Theme:** Hugoplate (zeon-studio), located at `themes/hugoplate/`
- **CSS:** TailwindCSS 4 (via Hugo pipeline)
- **Package Manager:** Yarn
- **Node:** 24.x
- **Deploy:** Netlify, build output to `docs/`

## Development

```bash
yarn install   # install dependencies
yarn dev       # dev server at http://localhost:1313
yarn build     # production build → docs/
```

## Branch Strategy

- `main` — production branch, deployed to Netlify
- `development` — active development branch, PR to main when ready
- `main-backup` — backup, contains old blog post markdown files

## Project Structure

```
content/english/        # all content (blog posts, pages)
layouts/partials/       # local partial overrides (takes priority over theme)
  search-index.html     # overrides Hugo module search partial
themes/hugoplate/
  layouts/              # Hugo templates
    index.html          # home page
    about/list.html     # about page
    blog/
      list.html         # blog list page
      single.html       # single post page
    _default/
      terms.html        # categories/tags list page
      taxonomy.html     # posts filtered by category/tag
    partials/
      essentials/
        header.html     # navbar
        footer.html     # footer
      widgets/
        categories.html # sidebar categories widget
        tags.html       # sidebar tags widget
assets/
  css/custom.css        # custom styles (add overrides here, not in theme)
  images/               # site images (main-pp.png = profile photo)
  images/blog/          # blog post images
static/images/blog/     # blog post images for markdown rendering
data/social.json        # social media links (key: main)
config/_default/
  menus.en.toml         # navigation menus (main + footer)
  params.toml           # site parameters
```

## Content

- Blog posts: `content/english/blog/*.md` — cards show featured image (from `image` front matter) on top, title/summary below
- About page: `content/english/about/_index.md`
- Home page content: `content/english/_index.md`
- Blog images: stored in both `assets/images/blog/` and `static/images/blog/`
  - `assets/images/blog/` — for Hugo image processing (shortcodes)
  - `static/images/blog/` — for markdown image syntax `![alt](/images/blog/...)`

### Blog Posts

7 posts published (all in `Life` category), 15 posts draft:

**Published:**
- `alhamdulillah-menikah-menjaga-diri-menjalankan-sunnah.md`
- `alhamdulillah-anak-ke-1-semoga-allah-menjaganya.md`
- `alhamdulillah-anak-ke-2-semoga-allah-menjaganya.md`
- `seorang-putri-semoga-allah-menjaganya.md`
- `alhamdulillah-anak-ke-4-semoga-allah-menjaganya.md`
- `di-saat-ingin-meninggalkan-musik.md`
- `kehidupan-yang-baik-dari-allah-untuk-orang-beriman-dan-beramal-shalih.md`

**Draft (hidden):** All Laravel/tech tutorial posts — set `draft: false` to publish.

## UI Conventions

- Layout changes go in `themes/hugoplate/layouts/` — never edit theme CSS directly, use `assets/css/custom.css`
- TailwindCSS utility classes used inline in Hugo templates
- Dark mode supported via `dark:` variant
- Primary color via `text-primary` / `bg-primary` (theme-aware)
- Rounded cards: `rounded-2xl`, borders: `border-border dark:border-darkmode-border`
- Page header pattern: label (uppercase tracking-widest text-primary) + h1 + subtitle
- Card pattern: `rounded-2xl border border-border bg-white dark:bg-darkmode-body p-6 hover:border-primary/40 hover:shadow-lg`
- Divider between hero and content: `<hr class="border-border dark:border-darkmode-border">`

## CSS Conventions (`assets/css/custom.css`)

Key classes defined:
- `.btn`, `.btn-primary`, `.btn-outline-primary` — button styles
- `.share-btn` — share button icons in single post
- `.social-icons li a` — social icon buttons (general)
- `footer .social-icons li a` — footer-specific social icons (white bg + border)
- `.arabic-text` — RTL Arabic text blocks, uses Amiri font from Google Fonts
- `.content blockquote` — styled quote with large decorative `"` mark
- `.notice`, `.notice.note/tip/info/warning/danger` — colored notice blocks
- `.content .highlight` — code block wrapper with shadow
- `.toc-box` — collapsible table of contents (not currently used in single post)
- `.copy-tooltip` — tooltip for copy link button
- Search modal styles prefixed with `.search-modal`, `.search-wrapper`, `.search-result-item`

## Features

| Feature | Status | Notes |
|---|---|---|
| Search | ✅ on | Modal search, blog section only |
| Dark mode | ✅ on | Via theme switcher in navbar |
| Share buttons | ✅ on | X, LinkedIn, WhatsApp, Facebook, Copy Link |
| Navigation button | ❌ off | Disabled in params.toml |
| Disqus comments | ❌ off | shortname cleared in hugo.toml |
| Preloader | ❌ off | |
| Announcement bar | ❌ off | |
| Cookie consent | ❌ off | |
| Google Analytics | ❌ off | Fill `google_tag_manager` in params.toml to enable |
| Google Adsense | ❌ off | Fill `google_adsense` in params.toml to enable |
| Subscription | ❌ off | |

## Hugo Modules (third-party, via cache)

These are loaded from Hugo module cache — templates can be overridden by placing files in `layouts/partials/`:
- `search-modal.html` — search UI modal
- `search-index.html` — overridden locally to fix search behavior

## Pages

| Page | Layout | URL |
|---|---|---|
| Home | `layouts/index.html` | `/` |
| About | `layouts/about/list.html` | `/about` |
| Blog list | `layouts/blog/list.html` | `/blog` |
| Blog post | `layouts/blog/single.html` | `/blog/:slug` |
| Categories | `layouts/_default/terms.html` | `/categories` |
| Tags | `layouts/_default/terms.html` | `/tags` |
| Category posts | `layouts/_default/taxonomy.html` | `/categories/:name` |
| Tag posts | `layouts/_default/taxonomy.html` | `/tags/:name` |

## Owner Info

- **Name:** Purnama Anaking
- **Role:** Software Engineer & Lecturer
- **University:** Telkom University Surabaya
- **Degree:** Bachelor's & Master's in Information Systems
- **Location:** Indonesia
- **Tagline:** "Technology with Purpose, Sharing with Heart."
- **Description:** "Software Engineer with 10+ years of experience, based in Indonesia. I build meaningful digital solutions and share thoughts on technology, faith, and life — because great work goes beyond the code."

## About Page — Tech Stack

Tech stack is hardcoded in `themes/hugoplate/layouts/about/list.html` (not from markdown). To update, edit the `$categories` slice directly in that file.

Current stack:
- **Languages:** PHP, JavaScript, TypeScript, Java, Dart, Python, Go
- **Frameworks & Libraries:** Laravel, React, Next.js, Flutter, Express.js, React Native, Gin, CodeIgniter, Node.js
- **Tools & Infra:** Git, Docker, Linux, AWS, Firebase, Google Cloud, Azure
- **Databases:** MySQL, PostgreSQL, MongoDB

## Notes

- `docs/` folder is build output — never edit manually, always regenerate via `yarn build`
- Blog post cards (list + related posts) and single post header show the `image` front matter as a featured image; posts without `image` set render without one (no error)
- Social icons defined in `data/social.json` under key `main`
- Footer and navbar menus sourced from `config/_default/menus.en.toml`
- Arabic font (Amiri) loaded via `themes/hugoplate/layouts/partials/essentials/style.html`
- Hugo syntax highlighting uses inline styles (monokai theme) — override wrapper only in CSS
- Search `show_description = false` to avoid duplicate title in results
- Blog post images: converted from `{{< image >}}` shortcode to markdown `![alt](/images/blog/...)` syntax
- Related posts section width matches article width (`lg:col-10 md:col-12`) in single post
