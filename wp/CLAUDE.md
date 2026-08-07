# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A **static prototype** built on top of a saved-from-browser copy of an auspreiser.de
(German price-comparison site) page — the E-Bikes category listing at
`https://www.auspreiser.de/kategorien/e-bikes-18635.html`.

There is **no build system, no package manager, no tests, and no server**. The deliverable
is the HTML file itself, edited by hand and previewed in a browser. The `.idea/` folder is
just JetBrains IDE metadata (the `.iml` labels it a JAVA_MODULE, which is irrelevant — there
is no Java here).

The larger goal (see the user's auto-memory `auspreiser-prototype.md`) is to hand-edit the
prototype HTML so it matches a target design/screenshot, adding/removing page sections and
rewiring images to the locally-saved assets.

## Layout

- `E-Bikes günstig kaufen bei auspreiser.de.html` — the single page to edit (~2778 lines).
- `E-Bikes günstig kaufen bei auspreiser.de_files/` — **all** local assets: CSS, JS, product
  `.jpg`s, brand/emoji `.svg`s, ad/tracking scripts. The HTML references these with the
  relative prefix `./E-Bikes günstig kaufen bei auspreiser.de_files/...`.

The `.html` and its `_files/` directory are a matched pair produced by "Save Page As". If you
rename or move one, the relative asset links break — keep the `_files` folder name in sync
with the `.html` base name, or fix every reference.

## Editing conventions

- **CSS lives in the vendored bundle**, not inline. Styling comes from
  `_files/generic-*.css` (the big `generic-a8c0a2b5....css` is the main stylesheet;
  `generic.comp-*.css` is a theme override for `auspreiser-like`). To restyle, either add a
  small `<style>` block in the HTML `<head>` or edit the vendored CSS — prefer a scoped
  `<style>` block so vendored files stay diff-clean.
- **Match existing class names** when adding markup. The page uses a BEM-ish scheme:
  page structure is `nav.category-menu`, `aside.sidebar`, `section.content`, `footer.footer`;
  product tiles use `offer-item` / `tile-content` / `tile-img-container` / `tile-text` /
  `price` / `shop-name` / `shipment`; filters use `sidebar__panel` / `sidebar__filter-item` /
  `filter-item-text`. Reuse these rather than inventing new ones so the vendored CSS applies.
- **Product images** are the long-hashed filenames in `_files/` (e.g.
  `cube-supreme-rt-...-200t180....jpg`). When swapping a product card, point its `<img src>`
  at one of the existing local `.jpg`s — do not rely on remote `https://www.auspreiser.de/...`
  URLs, since the prototype is meant to render offline.
- The vendored `_files/*.js` (`fbevents.js`, `ads.js`, `loader.js`, `index.module.js`) are
  tracking/ad/framework scripts from the original capture. They are not part of the prototype
  logic — leave them alone; do not try to "fix" or reformat them.

## Previewing

Open the `.html` directly in a browser (`open "E-Bikes günstig kaufen bei auspreiser.de.html"`).
Because assets are relative, no local server is required. Remote calls (ads, tracking, fonts
from auspreiser.de) will fail silently when offline — that is expected and does not affect the
prototype layout.
