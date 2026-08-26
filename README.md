# Janton-Configurator

Custom seating Janton configurator — a single, responsive page (`index.html`)
that works on both desktop and mobile, no separate mobile link required
(`mobile.html` just redirects to `index.html` for old links).

`index.html` is fully self-contained: every layer image, all pattern and
colour swatches, the vendored jsPDF library, the Eqwal Ability logo/circle
backdrop and the title font are embedded directly in the file as data URIs.
You can open it straight from disk (double-click it, or drag it into a
browser tab) with nothing else alongside it, or serve it from any static
file host / GitHub Pages — both work identically.

## How it's built

- Plain HTML/CSS/JS, no build step, no dependencies at runtime other than
  Google Fonts (Noto Sans body text) loaded over the network — everything
  else works offline.
- The Janton render is a single `<canvas>`. Fourteen pixel-aligned layer
  images (complete seating, shell, back, back lateral, seating, abduction
  wedge, sides, outline, headrest shell/upholstery/complete, footrest
  shell/upholstery/complete) are drawn back to front. "Complete seating" is
  always drawn first, in the back; when the headrest or footrest add-on is
  switched off, its shape is punched back out of that base render before
  anything else is drawn, so it disappears cleanly.
- Colours and patterns are applied with a canvas `multiply` blend against
  each layer's own artwork, so the original shading/highlights always
  show through.
- Shell patterns (43, incl. ABS Black), Vinyl (15), Platilon (4) and
  Airmesh (7) swatches all come from the Eqwal Ability reference sheets
  included in the source hand-off.
- Tabs: **Shell** (pattern only) → **Headrest** (optional, toggle switch;
  when on, choose Pattern/Vinyl/Platilon for the shell and
  Platilon/Airmesh for the inside upholstery) → **Footrest** (identical
  structure to Headrest) → **Upholstery** (six collapsible sections — Back,
  Back lateral, Seating, Abduction wedge, Sides, Outline — each offering
  Platilon and Airmesh).
- **View order** unlocks once a shell finish and all six upholstery
  sections are chosen (and the headrest/footrest finish too, if that
  add-on is switched on). It opens an order-review screen with the full
  breakdown and a **Save as PDF** button that renders the current preview
  image plus the full selection list to a PDF, entirely client-side.

## Updating an asset

If you need to swap a layer image, replace the source PNG, then
regenerate the matching data URI in `index.html` (base64-encode the file
and paste it into the `LAYER_SRC` object). Pattern/colour swatches live in
the `PATTERNS` / `VINYL` / `PLATILON` / `AIRMESH` arrays near the top of
the inline script.

## Fonts

- Body text uses **Noto Sans**, loaded from Google Fonts (falls back to
  the system sans-serif if offline).
- The title ("Customize your Janton.") uses the licensed **Bogue
  Semibold Italic**, embedded in the `@font-face` rule in `index.html`
  (same asset used by the AFO and Cheneau configurators).
