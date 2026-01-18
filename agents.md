# KW Riding - Project Agent Guide

This document provides context and guidelines for AI assistants working on the KW Riding motorcycle travel blog project.

## Project Overview

**KW Riding** is a motorcycle travel blog and vlog showcasing motorcycle trips, tours, and route planning services by Kay & Will — Brazilians exploring the world on two wheels.

- **Site URL**: https://kwriding.netlify.app
- **Purpose**: Document motorcycle adventures, provide trip planning services, and share routes/tips
- **Target Audience**: Motorcycle enthusiasts looking for trip planning, routes, and adventure inspiration

## Technology Stack

### Core Technologies
- **Static Site Generator**: Hugo (Extended version required)
- **CMS**: Netlify CMS (git-gateway backend)
- **Hosting**: Netlify
- **Package Manager**: **Yarn** (NOT npm)
- **Build Tool**: Webpack
- **Theme**: `hugo-theme-gallery` (custom fork: `https://github.com/SchulzWill/hugo-theme-gallery.git`)

### Key Dependencies
- **hugo-bin**: `^0.139.0` (Extended version with SCSS support)
- **netlify-cms-app**: `^2.15.72`
- **React**: `^17.0.2` (for CMS preview templates)

## Project Structure

```
1clik-kwriding-cms/
├── bin/                    # Hugo executables (darwin, linux, exe)
├── site/                   # Hugo site directory
│   ├── config.toml        # Main Hugo configuration
│   ├── content/           # All content (markdown files)
│   │   ├── _index.md      # Homepage
│   │   ├── about.md       # About page
│   │   ├── gear/          # Gear showcase page
│   │   └── categories/    # Trip categories
│   │       ├── long-trips/    # Multi-day adventures
│   │       │   ├── _index.md  # Category page
│   │       │   ├── cover.jpg  # Category cover image
│   │       │   └── [trip-name]/  # Individual trips (page bundles)
│   │       │       ├── index.md
│   │       │       └── cover.jpg
│   │       └── day-trips/     # Single-day rides
│   │           ├── _index.md
│   │           ├── cover.jpg
│   │           └── [trip-name]/
│   ├── static/            # Static assets
│   │   ├── admin/         # Netlify CMS config
│   │   │   └── config.yml
│   │   └── img/           # Media files (CMS uploads)
│   └── layouts-old-kaldi/ # Backup of old theme layouts (DO NOT USE)
├── themes/                # Hugo themes
│   └── theme-gallery/     # Git submodule (custom fork)
├── src/                   # Source files (JS, CSS)
├── netlify.toml           # Netlify deployment config
├── package.json           # NPM/Yarn dependencies
└── yarn.lock             # Yarn lock file (use Yarn, not npm)
```

## Critical Configuration Rules

### 1. Package Manager: YARN (NOT npm)
- **Always use Yarn commands**: `yarn install`, `yarn build`, `yarn start`
- **Never use npm commands** in scripts or documentation
- `netlify.toml` is configured for Yarn: `NETLIFY_USE_YARN = "true"`
- Build command: `yarn build`
- **Do not create or commit `package-lock.json`** — use `yarn.lock` only

### 2. Hugo Extended Version Required
- Hugo Extended is **mandatory** for SCSS/SASS processing
- Configured in `package.json`:
  ```json
  "hugo-bin": {
    "buildTags": "extended"
  }
  ```
- Version: `^0.139.0` (minimum `0.123.0` required by theme)

### 3. Theme Configuration
- **Theme**: `theme-gallery` (located in `themes/theme-gallery/`)
- **Source**: Custom fork at `https://github.com/SchulzWill/hugo-theme-gallery.git`
- **Managed as**: Git submodule
- **Theme directory**: `themesDir = "../themes"` in `site/config.toml`
- **DO NOT** modify theme files directly unless absolutely necessary
- If theme modifications are needed, fork the theme and update `.gitmodules`

### 4. Content Organization Rules

#### Categories Structure
- All trips are organized under `site/content/categories/`
- Two main categories:
  - **`long-trips/`**: Multi-day motorcycle adventures
  - **`day-trips/`**: Single-day rides and local explorations

#### Page Bundles
- Each trip is a **page bundle** (folder with `index.md` and assets)
- Structure: `categories/[category-name]/[trip-name]/index.md`
- Cover images must be named `cover.jpg` and placed in the trip folder
- Example:
  ```
  categories/long-trips/picos-de-europa/
  ├── index.md
  └── cover.jpg
  ```

#### Required Front Matter for Trips
```yaml
---
date: 2024-04-01
title: Trip Name
description: Brief description shown in listings
categories: ["long-trips"]  # or ["day-trips"]
youtube_id: dQw4w9WgXcQ     # YouTube video ID (trip page hero)
map_embed_url: ""           # Optional iframe embed URL (Google Maps/MyMaps)
route_store_url: ""         # Optional external store link (Gumroad/Payhip/etc.)
route_store_label: ""       # Optional button label override
resources:
  - src: cover.jpg
    params:
      cover: true
---
```

#### Category Pages (`_index.md`)
- Each category must have an `_index.md` file
- Required front matter:
  ```yaml
  ---
  title: Category Name
  description: Category description
  menu:
    main:
      weight: 10  # Menu order (lower = earlier)
  params:
    featured: true  # Show on homepage
  resources:
    - src: cover.jpg
      params:
        cover: true
  ---
  ```

### 5. Gallery Theme Requirements

#### Albums Must Have Images
- **Critical**: Albums (trips) **will not appear** in listings without at least one image
- Always include a `cover.jpg` in each trip folder
- Reference it in front matter: `resources.params.cover: true`

#### Categories Need Content
- Categories **will not appear as buttons** on the homepage without albums
- If a category is empty, add a placeholder album (e.g., "Coming Soon")

#### Featured Content
- Use `params.featured: true` in front matter to feature content on homepage
- Can be applied to categories or individual trips
- Featured trips appear in homepage sections

### 6. Menu Configuration

#### Menu Items
- Defined in `site/config.toml` under `[menu]` section
- Also defined in content front matter: `menu.main.weight`
- Lower `weight` values appear first
- Current menu structure:
  - Home (weight: 1)
  - Long Trips (weight: 10)
  - Day Trips (weight: 20)
  - My Gear (weight: 80)
  - About (weight: 90)

#### Menu Best Practices
- Use consistent weight values (10, 20, 30, etc.) for spacing
- Don't duplicate menu entries (check both `config.toml` and content files)

### 7. Image Handling

#### Image Locations
- **CMS uploads**: `site/static/img/` (configured in Netlify CMS)
- **Content images**: Co-located with content in page bundles
- **Cover images**: Named `cover.jpg` in each trip/category folder

#### Image Processing
- Hugo image processing enabled in `site/config.toml`
- Quality: 75%
- Resample filter: CatmullRom
- EXIF data: Date preserved, GPS disabled (privacy)

#### Image Resources Pattern
```yaml
resources:
  - src: cover.jpg
    params:
      cover: true
      # hidden: true  # Use only if you want to hide from gallery but keep in front matter
```

**Important**: Setting `hidden: true` on cover images will hide the album from listings!

### 8. Netlify CMS Configuration

#### Backend
- **Type**: `git-gateway`
- **Branch**: `main`
- **Authentication**: Netlify Identity

#### Collections
- **long-trips**: Nested structure, depth: 2
- **day-trips**: Nested structure, depth: 2
- **pages**: Static pages (homepage, about, gear)
- **settings**: Site configuration

#### Field Conventions
- **Title**: Required string
- **Date**: Datetime widget
- **Description**: Text widget (shown in listings)
- **Categories**: Select widget (long-trips/day-trips)
- **Featured**: Boolean (show on homepage)
- **Private**: Boolean (hide from public listings)
- **Body**: Markdown widget (main content)

### 9. Content Guidelines

#### Homepage (`site/content/_index.md`)
- Must include cover image: `resources.params.cover: true`
- Content should be concise and action-oriented
- Focus on trip planning services and value proposition

#### Trip Pages
- Include detailed description in body
- Use markdown headings for structure (## Highlights, ## Route, etc.)
- **Video hero is theme-driven**: set `youtube_id` in front matter (preferred for consistent layout)
- Include route information, highlights, and personal experiences

### 9.1 Trip Page Layout Overrides (important)

Trip pages (`site/content/categories/**/<trip>/index.md`) use a **site-level layout override** so changes apply across all trips without editing the theme submodule.

- **Trip single layout**: `site/layouts/categories/single.html`
- **Trip partials**: `site/layouts/partials/`
  - `trip-video.html` (YouTube hero)
  - `trip-map.html` (optional map)
  - `trip-gallery.html` (gallery excluding cover image)
  - `trip-route-callout.html` (external route purchase link)

### 9.2 Post-theme CSS overrides (important)

The theme compiles CSS via Hugo Pipes (from `themes/theme-gallery/assets/css/main.scss`). To override theme styles without editing the theme, we load a small CSS file **after** the theme CSS:

- **Hook point**: `site/layouts/partials/head-custom.html`
- **CSS file**: `site/static/css/kwriding-overrides.css`

Use this for layout tweaks like “theater mode” video centering.

#### About Page
- Uses `layout: prose` for better typography
- Should tell the story of Kay & Will
- Include contact information or social links

#### Gear Page
- Showcase motorcycle, camera equipment, riding gear
- Can include images (add to page bundle)
- Use markdown lists for gear specifications

### 10. Build and Deployment

#### Local Development
```bash
yarn start          # Start dev server (Hugo + Webpack)
yarn preview         # Preview with drafts and future posts
yarn build           # Production build
yarn build:hugo      # Build Hugo only
yarn build:webpack   # Build assets only
```

#### Netlify Deployment
- **Build command**: `yarn build`
- **Publish directory**: `dist`
- **Deploy preview**: `yarn build:preview` (includes drafts)
- **Hugo version**: Managed via `hugo-bin` package

#### Build Process
1. Webpack processes JS/CSS assets
2. Hugo generates static site
3. Output to `dist/` directory
4. Netlify deploys `dist/` contents

### 11. Common Issues and Solutions

#### Albums Not Appearing
- **Cause**: Missing cover image
- **Solution**: Add `cover.jpg` to trip folder and reference in front matter

#### Categories Not Showing as Buttons
- **Cause**: Category has no albums
- **Solution**: Add at least one album to the category

#### Build Errors: SCSS/SASS
- **Cause**: Using non-extended Hugo version
- **Solution**: Ensure `hugo-bin` has `buildTags: "extended"` in `package.json`

#### Menu Items Missing
- **Cause**: Missing `menu.main.weight` in content front matter
- **Solution**: Add menu configuration to `_index.md` files

#### Theme Not Found
- **Cause**: Git submodule not initialized
- **Solution**: Run `git submodule update --init --recursive`

### 12. Language and Localization

#### Current Setup: Single Language (English)
- **Language code**: `en-us` in `site/config.toml`
- **Status**: Multilingual support was attempted but **reverted** due to theme incompatibility
- **Do NOT** enable multilingual features without extensive theme testing
- The `hugo-theme-gallery` theme may require modifications for proper multilingual support

### 13. Social Media Integration

#### Social Links
Configured in `site/config.toml`:
```toml
[params.socialIcons]
  instagram = "https://www.instagram.com/kwriding/"
  youtube = "https://www.youtube.com/@KWRiding"
```

#### YouTube Integration
- Use Hugo shortcode: `{{< youtube VIDEO_ID >}}`
- Place in markdown content body
- Example: `{{< youtube dQw4w9WgXcQ >}}`

### 14. Development Workflow

#### Making Changes
1. **Content changes**: Edit markdown files in `site/content/` or use Netlify CMS
2. **Configuration**: Edit `site/config.toml` or `netlify.toml`
3. **Theme modifications**: Edit files in `themes/theme-gallery/` (or fork theme)
4. **Assets**: Add to `src/` for processing or `site/static/` for direct inclusion

#### Testing Locally
```bash
yarn install          # Install dependencies
yarn start            # Start dev server
# Visit http://localhost:3000
```

#### Committing Changes
- Use descriptive commit messages
- Test builds locally before pushing
- Netlify will auto-deploy on push to `main` branch

### 15. Important Don'ts

❌ **DO NOT**:
- Use `npm` commands (use `yarn` instead)
- Modify theme files without understanding impact
- Remove cover images from trips (they won't appear in listings)
- Enable multilingual without testing theme compatibility
- Delete `yarn.lock` file
- Use non-extended Hugo version
- Create `package-lock.json` (use `yarn.lock` only)
- Modify files in `site/layouts-old-kaldi/` (backup only)

✅ **DO**:
- Use Yarn for all package management
- Test builds locally before deploying
- Include cover images for all trips
- Keep content organized in categories
- Use consistent front matter structure
- Update Netlify CMS config when adding new content types
- Document any theme customizations

### 16. Future Considerations

#### Potential Enhancements
- Multilingual support (requires theme modifications)
- Additional content types (reviews, tips, routes)
- Enhanced CMS fields (GPS coordinates, route files)
- Integration with mapping services
- Trip planning form/contact integration

#### Maintenance Tasks
- Keep Hugo and dependencies updated
- Monitor Netlify CMS for updates
- Review and optimize images regularly
- Backup content regularly (Git provides version control)

---

## Quick Reference

### Essential Commands
```bash
yarn install          # Install dependencies
yarn start            # Development server
yarn build            # Production build
yarn build:hugo       # Hugo build only
```

### Key File Locations
- **Hugo config**: `site/config.toml`
- **Netlify config**: `netlify.toml`
- **CMS config**: `site/static/admin/config.yml`
- **Homepage**: `site/content/_index.md`
- **Trips**: `site/content/categories/[category]/[trip]/index.md`

### Critical Front Matter Fields
- `title`: Page/trip title
- `description`: SEO and listing description
- `categories`: Array of category slugs
- `date`: Publication date
- `resources`: Image resources (cover images)
- `menu.main.weight`: Menu ordering
- `params.featured`: Feature on homepage

---

**Last Updated**: Based on project state as of latest conversation
**Maintained By**: Project team
**For Questions**: Refer to Hugo documentation, Netlify CMS docs, or theme README

