# KW Riding

KW Riding is a motorcycle travel blog and vlog by Kay & Will. The site showcases trip galleries (long trips and day trips), embeds the trip videos, and promotes route-planning services and route downloads.

- **Production site**: `https://kwriding.netlify.app`

## Stack

- **Static site generator**: Hugo (Extended)
- **Theme**: `themes/theme-gallery/` (git submodule, custom fork)
- **CMS**: Netlify CMS (git-gateway) at `/admin`
- **Hosting**: Netlify
- **Assets tooling**: Webpack
- **Package manager**: **Yarn (required)** — do not use npm

## Requirements

- **Node.js** (use a modern LTS)
- **Yarn v1**
- **Git submodules** (for the theme)

## Local development

Install dependencies:

```bash
cd /Users/wsm/Git/private/1clik-kwriding-cms
yarn install
```

Initialize the theme submodule (only needed on first clone):

```bash
git submodule update --init --recursive
```

Run the dev server:

```bash
yarn start
```

Build for production:

```bash
yarn build
```

Build deploy-preview (includes drafts + future content):

```bash
yarn build:preview
```

## Where to edit content

All Hugo content is inside `site/content/`.

- **Homepage**: `site/content/_index.md`
- **Trips (albums)**: `site/content/categories/`
  - **Long trips**: `site/content/categories/long-trips/<trip-slug>/index.md`
  - **Day trips**: `site/content/categories/day-trips/<trip-slug>/index.md`

Each trip is a **page bundle** (folder with `index.md` and assets).

## Required cover rules (important)

The gallery theme requires at least one image resource per trip for it to appear in listings.

- Each trip folder must include a **`cover.jpg`**
- The trip front matter must include:

```yaml
resources:
  - src: cover.jpg
    params:
      cover: true
```

The cover is used on the home/gallery and list pages. The trip page itself hides the cover image.

## Trip pages: video hero + optional map/store blocks

Trip single pages (under `site/content/categories/**`) are rendered with a site override layout so all trips share the same structure.

In each trip’s front matter you can set:

- `youtube_id`: YouTube video ID (shows as the hero video)
- `map_embed_url`: iframe-ready embed URL (optional)
- `route_store_url`: external store URL (optional)
- `route_store_label`: button label override (optional)

Example:

```yaml
youtube_id: dQw4w9WgXcQ
map_embed_url: ""
route_store_url: ""
route_store_label: ""
```

Your markdown body (`.Content`) remains fully rendered for SEO (intro + headings like `## Highlights`, `## The Riding`, etc.).

## Where the overrides live

We avoid editing the theme submodule directly. Site-level overrides live in:

- **Trip layout override**: `site/layouts/categories/single.html`
- **Trip partials**: `site/layouts/partials/` (`trip-video.html`, `trip-map.html`, `trip-gallery.html`, `trip-route-callout.html`)
- **Post-theme CSS overrides** (loaded after the theme CSS):
  - `site/layouts/partials/head-custom.html`
  - `site/static/css/kwriding-overrides.css`

## Netlify CMS

Netlify CMS config:
- `site/static/admin/config.yml`

Once deployed, the CMS is available at `/admin` on your Netlify site.
