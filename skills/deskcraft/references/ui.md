# Interface

A desktop app is used for hours. Quiet chrome, dense but even rhythm, and one place that carries colour: the user's own content.

## Shell

- Layout from left to right in the order work flows: a mode rail (icons with short labels, 64 to 72 wide), the controls of the current mode, the canvas or main content, then history or details. The content gets the space; panels keep fixed widths.
- Title bar: logo and name on the left, the current object (project, document, file) as a chip in the centre line, a few icon buttons (search, tools, downloads, settings) on the right, then the caption buttons. While a first-run screen is up, the title bar shows only the logo, the name and the caption buttons.
- Status bar: what the app is doing and live resources that matter for the product (memory, disk, connection, sync), each as a label, a small meter and a value. Nothing decorative.
- A minimal mode for small windows: rails collapse to icons, panels gain scroll, nothing overlaps.

## Components to reuse everywhere

- Button (neutral), primary button (accent), icon button (28 x 28), segmented control, switch with a label, chip, key cap for shortcuts, text input with a mono variant and a masked variant for secrets.
- Setting row: name, an optional one-line description that states a value or a limit, the control on the right.
- Dialog: title with an icon only when it means something (warning, error), one or two sentences, actions right-aligned with the primary last.
- Toast: top centre, above dialogs, one line, auto-dismissed; errors stay longer.
- Empty state: one line saying what is missing and the button that fixes it.

Build each once and reuse it. Two kinds of button or three switch styles in one app read as several apps glued together.

## Modals and focus

- One modal at a time, with a veil behind it. Esc closes the top one; a second Esc does what Esc does on the main screen (often Stop).
- Opening a dialog moves focus into it; closing returns focus to where it was.
- An alert raised while another window is open draws above every window, including Settings.
- Long work keeps the window usable: a progress strip inside the relevant panel, not a modal that blocks everything.

## Canvas and previews

- Pan, zoom and selection survive switching to another item. Double click resets the view.
- Live previews of work in progress show the real output as soon as it carries information. A placeholder animation may cover the first moment, then gives way to the real thing.
- Pixel content renders with nearest neighbour scaling at integer factors.

## Lists that change

- Lists of things on disk (plugins, templates, presets, results) refresh when the folder changes, debounced until the copy finishes, plus a refresh button in the list header.
- History keeps a result's settings next to it, so the user can restore them with one click.
- Deleting moves to the recycle bin or trash and can be undone for a few seconds.

## Theme

- Every colour is a function of the theme so the whole interface follows a switch on the next frame.
- Follow the OS setting (Windows: AppsUseLightTheme; macOS: the effective appearance; Linux: the portal's color-scheme) unless the user chose one.
- Check every screen in both themes, including dialogs, tooltips, the installer and the uninstaller.

## Patterns that read as generated

- Gradients from one hue to another, glows, glassy cards, colour blobs > solid surfaces and hairlines
- An icon in a tinted rounded square above every card > drop the tile
- Uppercase tracked eyebrows over every section > delete, the heading says it
- Pulsing dots not bound to a live state > delete
- Every heading followed by a subtitle > keep only subtitles that state a fact
- A different look in the installer, the uninstaller or the settings window > the same tokens and components
