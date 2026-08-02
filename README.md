# airsignal-docs

Source content for [airsignal.dev/docs](https://airsignal.dev/docs) and marketing legal pages
(`/terms`, `/privacy`, `/refunds`). This repo is public and read directly by the AirSignal web app
(`apps/web`) over the GitHub API — pushing to `main` updates the live site within ~2 minutes (ISR),
no deploy required.

## Structure

```text
content/
  docs/                   # product documentation → /docs/*
    meta.json
    index.mdx
    getting-started/
    guides/
    api/
    sdks/
      flutter.mdx         # full Flutter SDK guide (single page)
  legal/                  # marketing legal pages → /terms /privacy /refunds
    meta.json
    terms.mdx             # → /terms
    privacy.mdx           # → /privacy
    refunds.mdx           # → /refunds
assets/                   # images; use /assets/... paths in MDX (resolved via jsDelivr CDN)
```

## URL mapping

| Content path | Public URL |
|---|---|
| `content/docs/{path}` | `/docs/{path}` (`index.mdx` maps to the folder URL) |
| `content/legal/terms.mdx` | `/terms` |
| `content/legal/privacy.mdx` | `/privacy` |
| `content/legal/refunds.mdx` | `/refunds` |

## Adding a docs page

1. Create the `.mdx` file with frontmatter:

   ```mdx
   ---
   title: Page title
   description: One sentence shown in cards and search.
   ---

   Content here. Use Callout, Cards, Steps, Tabs components — no arbitrary imports.
   ```

2. Add the filename (without extension) to the parent folder's `meta.json` `pages` array. Pages
   not listed in `meta.json` still render but won't appear in the sidebar.
3. Commit and push to `main`.

## Editing legal pages

Edit `content/legal/{terms|privacy|refunds}.mdx`. Frontmatter fields:

```mdx
---
title: Terms of Service
description: Short SEO description.
lastUpdated: August 1, 2026
---
```

Do **not** put a top-level `# Title` in the body — the marketing layout renders `title` from
frontmatter. Keep headings at `##` and below. If you add a new legal slug, also add a matching
route in `apps/web` (`/terms`-style marketing page).

## Adding a new SDK

Prefer **one start-to-end guide** per SDK (`sdks/{name}.mdx`) with `##` sections for Installation,
Initialization, Identity, Push, etc. Add the filename to `sdks/meta.json`. Only split into a folder
if the SDK truly needs multiple independent pages.

### Coming soon (`badge`)

For SDKs that are not ready yet:

```mdx
---
title: iOS SDK
sidebarTitle: iOS
badge: Soon
description: Native iOS SDK — coming soon.
---
```

- Sidebar shows a **Soon** badge and the item is not clickable
- Hub `<Card badge="Soon">` is also non-clickable (omit `href` or keep it — Soon wins)
- Soon pages are omitted from the public sitemap
- When the SDK ships: remove `badge`, fill the guide, and add `href` on hub cards

`badge` can be any short label later (e.g. `Beta`) — the UI renders the string as-is.

## `meta.json` reference

```json
{
  "title": "Section title shown in sidebar",
  "pages": ["index", "---Divider Label---", "some-page", "sub-folder"]
}
```

- Order in `pages` controls sidebar order.
- `"---Label---"` entries render as non-clickable section dividers.
- Entries can be `.mdx` filenames (no extension) or sub-folder names.

## Available MDX components (docs)

- `<Callout type="info|warning|tip|danger">` — inline notices
- `<Cards><Card title href icon /></Cards>` — hub-page link grids
- `<Steps><Step title>...</Step></Steps>` — numbered walkthroughs
- `<Tabs items={["a","b"]}><Tab>...</Tab></Tabs>` — platform-specific snippets
- `<Endpoint method path />` — method + path bar with copy
- `<Required />` / `<Optional />` — optional styled badges (prefer a **Required** table column for compatibility)
- Fenced code blocks get syntax highlighting + a copy button automatically

Legal pages use the same MDX pipeline for prose (headings, links, lists, emphasis). Prefer plain
markdown there unless a Callout is genuinely useful.

## Content ownership

Canonical API/SDK contracts live in the main repo's `documentation.md`. When that file changes in
a way that affects a public guide here, update the matching page in the same change.
