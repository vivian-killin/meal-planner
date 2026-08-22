# 🌿 Meal Planner

A single-file weekly meal planner. Save recipes, tell it what's already in your
fridge, and it picks the week's dinners and builds a categorised shopping list.

No build step, no server, no accounts — `index.html` is the whole app.

## Running it

Open `index.html` in a browser, or publish it with GitHub Pages:

1. Push `index.html` to a repo
2. **Settings → Pages → Deploy from branch → main**
3. Share `https://<user>.github.io/<repo>/`

Everyone who opens that link gets their own private copy: all data lives in
their browser's `localStorage`, nothing is uploaded anywhere.

## How it works

- **This Week** — list what you have on hand, then generate a plan. Recipes are
  ranked by how much of them you already own, and the planner aims for two
  thirds veggie-rich meals (two or more distinct vegetables, not counting
  onion and garlic).
- **Shopping** — ingredients from the week's meals, merged and grouped by aisle,
  with anything already in your pantry moved to a separate "already have"
  section. Tick items off as you shop; ticks survive reloads.
- **Recipes** — search, filter and sort your library. Meat / fish / vegetarian
  is detected from the ingredients.
- **Add Recipe** — paste a recipe URL and hit Fetch to pull the name,
  ingredients, time and servings from the page's schema.org data. Seasonings
  (salt, spices, oil, vinegar, water…) are stripped automatically, since you
  always have those.

## Known limits

- **Fetching is best-effort.** The browser can't read another site directly, so
  Fetch goes through a public CORS proxy. Larger recipe sites block proxies —
  when that happens you get a clear error and can paste the ingredients in by
  hand. The URL you fetch is the only thing sent to the proxy.
- **Data is per-browser.** Clearing site data, or switching device or browser,
  loses everything. Use **Settings → Export backup** and keep the JSON file.

## Editing

Everything is in `index.html`: CSS in `<head>`, markup, then the script.

A few conventions worth keeping:

- Anything interpolated into `innerHTML` goes through `esc()`, and any URL that
  ends up in an `href` goes through `safeUrl()` — recipe data is untrusted, it
  can come from any website via the fetcher.
- Ingredient matching uses `hasWord()`, not `String.includes()`. Substring
  matching produced false hits like "mince" inside "minced garlic" and "egg"
  inside "eggplant".
- Click handling is delegated: add `data-act="…"` to an element and a handler
  to the `ACTIONS` table, rather than inline `onclick` attributes.
