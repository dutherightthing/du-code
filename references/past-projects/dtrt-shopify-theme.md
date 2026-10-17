# dtrt-shopify-theme — Du The Right Thing store (Shopify theme)

- **Repo:** `~/Github Repositories/dutherightthing-theme` (github.com/dutherightthing/dutherightthing-theme, private). Running notes: its `NOTES.md`.
- **What:** custom Online Store 2.0 theme for dutherightthing.com that copies the feel of store.elevenlabs.io: a product circle (desktop ring, phone off-screen arc), a grid view, a product panel over the store, a dark cart drawer, About/FAQ/x Humanculture pages, and a restock "notify me" form. Checkout, Tapstitch (dropship) and apps stay on Shopify.
- **Stack:** Liquid + one `theme.css` + one vanilla `theme.js`, no build step. Shopify CLI (`shopify theme push --only <files>` to an unpublished theme, review on its preview link, then `shopify theme publish`). Live since 2026-10-17; old theme (Baseline) kept unpublished as the rollback.
- **Impact:** high. It's a live store that takes money, and it took 9 feedback rounds.

## How the work went
- Jerry reviewed each round on the preview link (`?preview_theme_id=`). The live site didn't change until he said "make this the default."
- Most rounds were about motion and feel, not features. "Glitchy" meant one of these, in order of how often:
  1. The stacking context broke `mix-blend-mode: multiply`, so white photo backgrounds showed.
  2. A full page reload or snapshot happened between store and product.
  3. A leftover animation loop kept moving tiles after the view changed.
  4. A FLIP scale stretched padded or rounded tiles.
- The breakthrough was measuring the reference site instead of guessing. Sample its inline transforms and styles every frame with JS while you click its controls. ElevenLabs' switch turned out to be:
  - about 1s with a symmetric ease-in-out;
  - real width and height changes, not a scale;
  - the tile chrome (padding, radius, background) fading on its own half of the move;
  - no stagger.
  Copying those numbers ended the "too fast / glitchy" feedback in one round.

## Reusable patterns
- **Tile background with multiply.** For product photos with white backgrounds on a tinted page, use `mix-blend-mode: multiply`. Then keep z-index, opacity < 1, transform and will-change off the photo's ancestors, or give the tile its own page-coloured background.
- **Product open without a reload.** Fetch the product page, parse it with DOMParser, inject a panel and `pushState`. Then glide the tile photo into place with the Web Animations API. Close is `history.back()`. Direct product URLs still render the full page.
- **Layout switch.** During the move, make each tile `position: fixed`, with `!important` only on position, margin and translate. Animate left, top, width and height with WAAPI (`fill: forwards`), and clean up on `animation.finished`, not on a timer. For rapid clicks, measure the current rects first, then cancel.
- **Phone carousel on an arc.** Put the ring centre off-screen. Derive the angle step from tile spacing, not 2π/n. Wrap each angle into one lap around the middle so the list loops off-screen.
- **Machine-readable store.** In the layout, add `{{ product | structured_data }}`, Organization JSON-LD and OG tags. Sort available products first by doing several Liquid passes, because `concat` silently fails on `collection.products`.
- **Restock demand without an app.** A `{% form 'customer' %}` with `contact[tags]` = `restock, restock-<handle>` creates tagged customers. It also subscribes them to marketing, so disclose that in the fine print. Shopify shows a hCaptcha on submit, which agents can't solve, so Jerry has to test it.

## Shopify traps (cost real time)
- The theme CSS minifier returns 502 for `::view-transition-*(name)` rules, and the site then renders unstyled. Inline those rules in the layout, and check the asset URL returns 200 after every CSS push.
- `?section_id=main` returns 404 for JSON templates, so fetch the full page instead.
- Served `theme.js` is minified with its comments stripped. Check a deploy by searching for code tokens, not comments, and allow for CDN lag.
- After publishing, every `theme push` to that id is live. Duplicate the theme for risky work.

## Verification traps
- A hidden or unfocused in-app browser pane freezes or throttles requestAnimationFrame and CSS animations. Layouts then look broken, and frame sampling returns 2 frames.
  - Call `getAnimations().forEach(a => a.finish())` before measuring.
  - To show a mid-animation frame, `pause()` the animation and set `currentTime`.
- `img.decode()` can hang, so race it against a timeout. Never let it block Close.
