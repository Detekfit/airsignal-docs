# airsignal-docs

Source content for [airsignal.dev/docs](https://airsignal.dev/docs). This repo is public and read
directly by the AirSignal web app (`apps/web`) over the GitHub API — pushing to `main` updates the
live site within ~2 minutes (ISR), no deploy required.

## Structure

```text
content/docs/
  meta.json           # root nav order + section dividers
  index.mdx           # /docs
  getting-started/
    meta.json
    index.mdx         # /docs/getting-started
    quickstart.mdx
    concepts.mdx
  guides/
    meta.json
    index.mdx
    create-an-app.mdx
    api-keys.mdx
    service-account.mdx   # how to get a Google service account JSON
  api/
    meta.json
    index.mdx
    authentication.mdx
    subscribers.mdx
    notifications.mdx
  sdks/
    meta.json
    index.mdx
    flutter/               # nested section — one folder per SDK
      meta.json
      index.mdx
      installation.mdx
      initialization.mdx
      identity.mdx
      push.mdx
```

## URL mapping

`content/docs/{path}` → `https://airsignal.dev/docs/{path}`. An `index.mdx` inside a folder maps
to the folder's own URL (e.g. `getting-started/index.mdx` → `/docs/getting-started`).

## Adding a page

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

## Adding a new SDK section

Copy the `sdks/flutter/` folder pattern (own `meta.json` + `index.mdx` + topic pages), then add the
folder name to `sdks/meta.json`.

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

## Available MDX components

- `<Callout type="info|warning|tip|danger">` — inline notices
- `<Cards><Card title href icon /></Cards>` — hub-page link grids
- `<Steps><Step title>...</Step></Steps>` — numbered walkthroughs
- `<Tabs items={["a","b"]}><Tab>...</Tab></Tabs>` — platform-specific snippets
- Fenced code blocks get syntax highlighting + a copy button automatically

## Content ownership

Canonical API/SDK contracts live in the main repo's `documentation.md`. When that file changes in
a way that affects a public guide here, update the matching page in the same change.
