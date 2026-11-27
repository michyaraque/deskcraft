# Tokens

Starting values. When the project already has a good system, keep its values and fix how they are used. Let the brief move them.

## Spacing

4, 8, 12, 16, 20, 24, 32, 40, 48, 64

- Inside a control: 4 to 12. Between rows of a list: 2 to 8. Between groups: 12 to 24. Between sections: 24 to 32.
- Desktop rows are denser than web rows: 28 to 32 compact, 32 to 36 default.
- Settings rows: 12 to 16 vertical padding, label block on the left, control right aligned.

## Window

- Default size around 1500 x 940 on a large screen, clamped to 94 % x 88 % of the screen it opens on.
- Minimum size that still shows every control at 1280 x 720 with 150 % scaling, about 1180 x 600 logical px.
- Title bar 40 to 44 tall. Caption buttons 46 wide on Windows. Status bar 24 to 28.
- Side panels 240 to 300, a control column 300 to 380, the main canvas fluid.
- Modal dialogs 420 to 560 wide; a settings window 860 to 960 wide with a tab list of 180 to 200.

## Type

- The platform UI family unless the brand has one: Segoe UI on Windows, the system font (SF Pro) on macOS, Cantarell, Inter or the desktop default on Linux. One mono for paths, values and keys: Cascadia Mono, SF Mono, JetBrains Mono.
- Sizes: 11, 12, 12.5, 13, 14, 16, 20. Body 12.5 to 13 on desktop.
- Weights 400 and 600; 700 only for the product name.
- Sentence case everywhere. No uppercase tracked labels, except at most one small group header style.

## Radius

- Controls 6 to 8, cards and panels 10 to 12, dialogs 12 to 14, pills only for chips and toggles.
- Inner radius = outer radius minus the padding between them, never below 4.

## Colour

Two themes from one neutral ramp, one accent, three text roles.

| Role | Dark | Light |
| --- | --- | --- |
| Background | #1c1a17 | #f3f0ea |
| Panel | #23211d | #faf8f4 |
| Raised (active row, input) | #2b2824 | #ebe6de |
| Hover | #34302b | #e2dcd2 |
| Border | #3d3832 | #d5cec2 |
| Soft border | #2e2b26 | #e4ded4 |
| Text | #ede6da | #2a2621 |
| Secondary text | #aaa194 | #635c52 |
| Muted text and icons | #958c7f | #857e73 |
| Accent (primary button, progress) | #eee2cb | #2e2a25 |
| Ink on accent | #1f1b15 | #faf8f4 |

These are a warm example. Tint the ramp toward the product's world and keep saturation very low, so no surface reads as a colour. Semantic colours (ok, warning, danger) are toned to sit inside the ramp.

- Modal backdrop: black at 55 to 72 % in dark, the text colour at 30 to 35 % in light.
- Selection: the accent or a warm highlight at about 30 % alpha.
- Focus ring: a dimmed accent, 1 px, never removed.

## Motion

- One easing (ease-out). 120 to 160 ms for hover and press, 180 to 240 ms for panels and dialogs entering.
- Spinners turn once a second. No looping motion on anything that is not working.
- A toast stays 6 to 8 s for an error, 3 to 4 s for information.

## Progress

- Update progress at most every 50 to 100 ms; redraw only when the value changed.
- Show a time left after a few seconds of stable rate, rounded to seconds under a minute and to minutes above.
