# First run

The first run ends when the user has a first result, not when the window opens.

## The check screen

Show the product name and version, then rows, each with a state:

- **The machine.** What the app found (GPU and memory, OS version, permissions) and whether it is enough. When it is not, say what the minimum is and what would work, like `Needs macOS 13 or newer` or `Use the legacy package for this graphics card`.
- **What to download.** Every option side by side with its publisher, size and one line from its own description, with a `Details` link and its own `Download` button. Options that cannot run here are dimmed with the reason.
- **Already have it.** A way to point at files the user already has (`Choose folder…`), so nobody downloads the same gigabytes twice.
- **Optional extras.** Plugins, presets, themes: a button that opens their folder.
- **Language.** A segmented control at the bottom: the languages, plus `System`.

The primary button at the end stays disabled with a plain reason (`Choose a template to start`) until one option is ready. Nothing is preselected.

## Downloads

- Check free space before starting. Resume after a drop. Download to a `.part` file and rename at the end.
- Progress per file and in total, with sizes, speed and a cancel button. Several downloads queue in one downloads panel reachable from the title bar.
- When it ends, the row turns ready without a restart, and if it was the only option, the app enters the main screen with it selected.
- A failure opens a dialog with the reason and the action, not a red line.

## Credentials and licenses

When a download needs an account, a token or a license acceptance:

1. A page in Settings with numbered steps, each a row with a link button: create an account, accept the license (one button per gated repository), create a token with the right scope (link straight to the creation page with the type preselected), paste and test.
2. The token field is masked, with a show button. Testing calls the provider's identity endpoint and then checks access to every gated file the catalog lists, showing a row per file with what is still missing and a button to fix it.
3. A download that hits a gate opens a dialog that names the repository and offers `Accept the license` and `Add token`, then asks the user to press Download again.
4. Prefer mirrors that need no gate for the default files. Keep gated sources only for optional extras, and test that every default file downloads without a token.

## Variants

- When a product comes in variants (standard and lite, two languages, two qualities), each variant is ready on its own: its own main file plus the shared ones. A user with only one variant enters the app and uses it.
- Download buttons fetch exactly the files of their variant. Shared files come from a source that does not require accepting the other variant's terms.
- Readiness and downloads use the same rule in code, so they cannot disagree.

## Returning users

- Restore the last mode, document, window size and position.
- If the last selection is no longer on disk, fall back to what is there instead of showing the first-run screen again.
- Never show the first-run screen to someone who has a usable setup; offer it again only from a menu or a flag.

## Empty states

- One line saying what is missing and the button that adds it: `No plugins yet. Copy plugin files into your plugins folder.` + `Open the plugins folder`.
