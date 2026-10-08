# Chef It Up — recipe site

A single-page recipe book. No build step, no dependencies. Works on GitHub Pages as-is.

## Files

- `index.html` — the whole site: styles, recipe data, and rendering code.
- `images/` — recipe photos. Each recipe's `image:` line names the file it expects (e.g. `images/jambalaya.jpg`). Landscape JPG or PNG. Until a file exists, the card shows a striped placeholder with the expected filename.

## Publish on GitHub Pages

1. Create a repository (e.g. `recipes`). Private is fine — GitHub Pages works on private repos on paid plans; on a free plan the repo must be public for Pages to serve it.
2. Upload `index.html` and the `images/` folder to the repo root.
3. Settings → Pages → Source: "Deploy from a branch", branch `main`, folder `/ (root)`. Save.
4. The site appears at `https://<username>.github.io/<repo>/` within a minute or two.
5. On phones: open the URL in Safari/Chrome → Share → "Add to Home Screen" for an app-like icon.

## Adding or editing recipes

All recipes live in the `RECIPES` array near the top of the `<script>` in `index.html`. Copy an existing block, change the values, save. Fields:

- `id` — unique, lowercase, hyphens. Used for links (`#jambalaya`) and the default photo name.
- `title`, `blurb`, `kind` (Breakfast, Main, Bread, Dessert, Drink, Sauce — any word; the filter chips build themselves from whatever kinds exist).
- `status` — `"locked"` (final) or `"tweaking"` (still adjusting). Change it to move a recipe between sections.
- `serves: "4"` or `makes: "1 loaf"`, `time`, optional `protein`, `calories`, `estimated: true`.
- `image` — path to the photo.
- `ingredients` — array of `{ group: "Sauce", items: [...] }` (group optional).
- `steps`, `notes`, `tweaks` (tweaks only display while status is `"tweaking"`).
- `source: { label, url }` — original recipe link.
- `linkOnly: true` — shows just the source link (used before ingredients were copied in).

Update the `UPDATED` date string when you change things.

## Features already built in

- Search by name or ingredient; filter by kind.
- Tap ingredients and steps to check them off while cooking.
- Opening a recipe puts its id in the URL so a link opens straight to it.
- Light/dark follows the device setting. Prints cleanly with all recipes expanded.
