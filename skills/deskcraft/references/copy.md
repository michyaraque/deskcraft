# Copy

An interface is used, not read. Keep the words a person needs to act and cut the rest. When in doubt, delete.

## Cut

- Slogans and taglines inside the app, the installer or the About screen: `everything runs on your PC`, `Your ideas, beautifully organized`. The product name and version say enough.
- Helper text under a label that says what the label already says: `Logs: To attach to a bug report`. Keep a description only when it states a value or a limit the user cannot see.
- Notes about where things are or what happens later: `Templates are not included: when the app opens you pick which ones to download`. The screen where it happens explains it, by being there.
- Explanations of the screen: `Click the button below to get started`, `This list updates in real time`.
- Decorative status chains: `12 items · Updated 2 m ago · Synced · v2.1`. Keep the one fact the user acts on.
- Emoji, sparkles and badges like `New`, `Beta`, `AI` that no data drives.

## Write

- Sentence case for every title, label, button and menu item.
- Buttons are verbs that say what happens: `Download`, `Install`, `Run App`, `Remove`. Not `OK` when a verb fits.
- Short labels: `Search`, not `Search anything…`. A placeholder shows a format (`XXXX-XXXX-XXXX`, `owner/repo/file`), not a sentence.
- Numbers with units and one format: `720 MB`, `1.2 GB`, `3.4 s`, `12 %` (with the locale's spacing rules).
- Paths with environment shorthand: `%LOCALAPPDATA%\App\cache`, `~/Library/Application Support/App`. Never the author's or the user's real home path in docs or screenshots.
- `>` between steps of a path through the UI: `Settings > About`, never a Unicode arrow (U+2192).
- No exclamation marks. No `Oops`. No `please` in labels.
- No trailing period on labels, buttons, titles and one-line toasts. Full sentences in dialogs keep it.

## Errors

One or two sentences: what happened, then what to do. Then the button that does it.

| Instead of | Write |
| --- | --- |
| `Error 403` | `This download needs a signed-in account. Add your access key and press Download again.` + `Add key`, `Open the account page` |
| `Something went wrong` | `The download did not finish: the disk is full. Free 6 GB and press Download again.` |
| `Failed to initialise renderer` | `The graphics libraries the app ships with are missing. Install it with its installer or unzip the whole portable folder.` |
| `Cancelled by user` shown as an error | `Load cancelled.` as an info toast |

- Name the thing (the repo, the file, the device) and the number (the size missing, the version needed).
- Never blame the user, never apologise twice, never show a stack trace in the dialog (it goes to the log).

## Installer wording

Like any installer. Nothing about the product's features.

| Screen | Text |
| --- | --- |
| Title | `Install App` / `Update App` / `Reinstall App` |
| Subtitle | `Version 1.2.0` |
| Notes | `App 1.1.0 will be updated to version 1.2.0.` / `App 1.2.0 is already installed on this computer.` |
| Fields | `Destination folder`, `Create a desktop shortcut` |
| Footer | `Space required: 720 MB. By installing you accept the license.` |
| Steps | `Copy files`, `Create shortcuts`, `Register the application` |
| Working | `Installing App…` / `Please wait while the process completes.` |
| Done | `Installation complete` / `App was installed successfully.` / `Run App` / `Finish` |
| Uninstall | `App will be removed from this computer.` / `Also remove the downloaded files (4.2 GB)` / `Also remove the settings` |

## Two languages

- Keep every language of a string together, in one file per language with the same keys or next to each other in the code, and fail the build when a key is missing in one of them.
- Follow the OS language by default, with a manual choice in Settings and on the first-run screen.
- Measure the longest translation in every fixed-width control.
