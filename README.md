# Chef It Up — recipe site

A single-page recipe book. No build step, no dependencies. Works on GitHub Pages as-is.

## Files

- `index.html` — the whole site: styles, recipe data, and rendering code.
- `images/` — your own recipe photos. Each recipe's `image:` line is a photo address. They currently point to free stock photos on Unsplash. To use your own photo, save a landscape JPG or PNG here and change that recipe's line to `image: "images/<name>.jpg"`. If a photo can't load, the card shows a striped placeholder.

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
- `status` — `"locked"` (final), `"tweaking"` (still adjusting) or `"tryout"` (saved from a social post, not cooked yet). Change it to move a recipe between sections.
- `serves: "4"` or `makes: "1 loaf"`, `time`, optional `protein`, `carbs`, `fat`, `calories`, `estimated: true`.
- `image` — photo address: an Unsplash link, or a path like `images/jambalaya.jpg`.
- `ingredients` — array of `{ group: "Sauce", items: [...] }` (group optional).
- `steps`, `notes`, `tweaks` (tweaks only display while status is `"tweaking"`).
- `source: { label, url }` — original recipe link.
- `linkOnly: true` — shows just the source link (used before ingredients were copied in).
- `saved: { from, date, confidence, note }` — only on `"tryout"` cards: where the recipe was saved from (`"@creator on Instagram"`), the ISO date, how confident the automatic read was (`high`, `medium`, `low`) and a short note. Delete the block when you promote the recipe.

Update the `UPDATED` date string when you change things.

## Features already built in

- Search by name or ingredient; filter by kind.
- Tap ingredients and steps to check them off while cooking.
- Opening a recipe puts its id in the URL so a link opens straight to it.
- Light/dark follows the device setting. Prints cleanly with all recipes expanded.

## "Try it out": recipes saved from social media, automatically

Add a post link (Instagram reel, TikTok, YouTube short, photo carousel) to the Reminders list **Recipes** on the Mac or phone. Within about 15 minutes the post is downloaded, its caption, on-screen text and spoken audio are read, the recipe is extracted (ingredients with the creator's amounts, steps, serves or makes, and the macros when the creator states them) and a new card appears at the top of **Try it out**, with the post's thumbnail as its photo and a link back to the original. The reminder is checked off when the card is live. If no usable ingredient list can be read, the card is added link-only with a note saying so, never dropped silently.

Pipeline (Webs, 2026-11-05): `~/shared/scripts/sparks-reminders-process.py` (launchd, every 15 min) runs `sparks-link-fetch.sh --recipe`, which hands the transcript to `~/Webs/suits/recipes/recipe-publish.py`. Every change lands as one commit named `Try it out: <title>`; the ledger is `~/Webs/suits/recipes/ledger.jsonl`.

Once you have cooked one: change its `status` to `"tweaking"` or `"locked"`, fix anything the automatic read got wrong, and delete the `saved` block.
