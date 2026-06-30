# purnamaanaking.github.io

Personal website of **Purnama Anaking** — Software Engineer & Lecturer at Telkom University Surabaya.

> "Technology with Purpose, Sharing with Heart."

## Tech Stack

- **Static Site Generator:** Hugo Extended
- **Theme:** [Hugoplate](https://github.com/zeon-studio/hugoplate) (zeon-studio)
- **CSS:** TailwindCSS 4 (via Hugo pipeline)
- **Package Manager:** Yarn
- **Deploy:** Netlify → `docs/`

## Development

```bash
yarn install   # install dependencies
yarn dev       # dev server at http://localhost:1313
yarn build     # production build → docs/
```

## Branch Strategy

| Branch | Purpose |
|---|---|
| `main` | Production — deployed to Netlify |
| `development` | Active development — PR to main when ready |
| `main-backup` | Backup of old blog post markdown files |

## Project Structure

```
content/english/        # all content (blog posts, pages)
assets/
  css/custom.css        # custom styles — add overrides here
  images/               # site images
  images/blog/          # blog post images (Hugo processing)
static/images/blog/     # blog post images (markdown rendering)
layouts/partials/       # local partial overrides
themes/hugoplate/
  layouts/              # Hugo templates
data/social.json        # social media links
config/_default/        # site configuration
  params.toml
  menus.en.toml
```

## License

Theme by [Zeon Studio](https://zeon.studio). Content © Purnama Anaking.
