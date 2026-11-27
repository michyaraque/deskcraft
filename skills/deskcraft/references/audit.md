# Audit

Go through every group and note each sign you find with where it lives. Each line pairs the sign with its fix. The ones marked (auto) are found by the commands in [Mechanical checks](#mechanical-checks); the rest need you to run the app or read the code.

## Identity

- The name collides with many apps in a search > rename before the first public release
- Window title, installer, shortcut and OS app list use different names or spellings > one name everywhere
- The icon is blurry, has stray pixels or a background box at 16 and 32 px > redraw for small sizes, check every size of the .ico / .icns
- File properties empty or say the framework's defaults (company "Unknown", description "app") > product name, version, publisher, copyright
- No author or links anywhere in the app > an About section with the author and the repository

## Interface

- Colours written inline across the code instead of tokens > one token set, two themes
- Some panels stay dark in the light theme (or the reverse) > explicit background on every surface, both themes
- The window cuts controls off on a 1280 x 720 screen at 150 % > minimum size that fits, layout that compacts
- A saved window size larger than the current screen > clamp on load
- Custom title bar that does not drag, snap or maximise on double click > hand the drag to the OS, or use the native bar
- Actions reachable only with the mouse, no focus ring > tab order, focus ring, shortcuts
- A text field resets the cursor to the start when clicked past the end > clicks below the last line go to the end
- Canvas pan and zoom reset when the user selects something else > keep view state per surface
- Settings scattered over menus and dialogs > one Settings window, one setting per row

## Copy (auto for some strings)

- Slogans and taglines inside the app or installer, like `everything runs on your PC` > delete
- Helper lines that restate the label, like `To attach to a bug report` under `Logs` > delete
- Notes about how the screen works or what comes later, like `Your projects are saved elsewhere` in an installer > delete, or move to where the user acts
- Unicode arrows (U+2192) in paths and steps > `>` or plain words
- Exclamation marks, `Oops`, `Something went wrong` > what happened and what to do
- Hype words: seamless, supercharge, magic, unlock, effortless > what the thing does
- Title case in labels and buttons > sentence case
- Emoji in labels > nothing, or one line icon from the app's set
- More in [copy.md](copy.md)

## First run

- The app opens on an empty main screen with nothing installed > a first-run screen that checks and guides
- A default download chosen for the user > nothing preselected, each option with its size
- The user must download every variant to get in > one usable variant is enough
- A gated download fails with a status line > a dialog with the exact steps and a credential test
- New plugins or templates added on disk need a restart to appear > watch the folders, plus a refresh button
- Downloads that restart from zero after a network drop, or fill the disk > resume, check space first

## Long work and errors

- A spinner or an indeterminate bar for work that has a known size > measured progress
- A time left that climbs forever > show it only once the rate settles; if it keeps climbing, the work is thrashing, investigate
- No way to cancel a load or a long job > cancel at every step, free what it took
- The window freezes during work > move it off the UI thread
- The app freezes on one button and leaves nothing in the log > look for a lock taken twice (a deadlock), not a crash
- Errors shown as red text in a corner or only in the log > dialog with the fix, or a toast
- No log file, or logs without timestamps and panics > a log in the platform log folder with backtraces

## Performance

- Load time grows from seconds to minutes on a modest machine > measure there, with a cold cache
- Memory mode chosen from total RAM, not free RAM > decide from free memory at run time, leave room for the OS
- Large files read through many small reads or page faults > large sequential reads, read ahead
- Two copies of the same data in RAM while loading > read straight into the final buffer

## Data and privacy (auto for some)

- Files written next to the executable > platform data folders
- Developer paths or user names in code, UI or logs (auto) > environment shorthand or relative paths
- Tokens or keys in the repository (auto) > remove, rotate, rewrite history if it was ever pushed
- A token shown in clear text in Settings > masked, with a show button, no copy while hidden
- Settings written in place (a crash leaves half a file) > write a temp file and rename

## Installer and uninstaller

- Asks for admin rights for a per-user app > per-user location, no elevation
- Stock wizard of the installer tool, different look from the app > the app's own look, or at least its icon, name and colours
- Installer pages that pitch the product > standard installer wording
- Installing a new version leaves the old one in the app list > update in place, remove the previous installer's entry
- Uninstall leaves files, shortcuts or the app list entry behind > file list, remove shortcuts and the entry
- Uninstall deletes the user's own work or downloads without asking > keep them, ask with options off by default
- Opening the uninstaller directly does nothing useful > it uninstalls
- A button that does not close the window > every exit path tested

## Updates (auto)

- No updater at all (auto) > ship one before the next public release
- Update downloads without a checksum check > verify against the release's sums file
- The update asks the user to close the app and run a file by hand > restart into the installer, reopen the app
- Downloaded installers pile up in temp > delete on the next start

## Release and repository (auto for files and versions)

- No LICENSE, CHANGELOG, SECURITY or README (auto) > add them
- The version written in several files with different values (auto) > one source, the rest generated
- Release built by hand on the author's machine > workflow on version bump
- Release without notes, without a download guide or without checksums > all three
- CI slower than ten minutes on a warm cache > cache toolchains and SDKs, split jobs
- Commit messages like `update`, `fix stuff`, or with generated attribution lines > one conventional line each
- Requirements in the README guessed rather than measured > measure, say on what
- Stale branches or test artifacts in a repository about to go public > delete

## Signing

- Unsigned installer and executable > sign, see [signing.md](signing.md)
- macOS build not notarised > notarise and staple

## Mechanical checks

Run these from the repository root and write down the count of each, so the report can compare before and after. They find candidates, not verdicts: read every hit, and leave the ones that are right on purpose (a test fixture, a rule that names the pattern) with a note. Use `rg` when it is installed, `git grep -n -E` otherwise.

- **Project files.** `git ls-files | grep -i -E '^(README|LICENSE|COPYING|CHANGELOG|SECURITY)|^\.gitignore$|^\.github/workflows/'` and note what is missing.
- **Secrets.** `git grep -n -E 'ghp_[A-Za-z0-9]{30,}|github_pat_[A-Za-z0-9_]{30,}|sk-[A-Za-z0-9_-]{30,}|AKIA[0-9A-Z]{16}|xox[abp]-[A-Za-z0-9-]{20,}|BEGIN [A-Z ]*PRIVATE KEY'`, and the same pattern over the history with `git log -p --all | grep -n -E '…'`.
- **Developer paths.** `git grep -n -i -E '[A-Z]:[\\/]+Users[\\/]+[A-Za-z0-9._-]+|/Users/[A-Za-z0-9._-]+|/home/[a-z0-9._-]+'`; placeholders like `<name>`, `%USERNAME%`, `you` or `runner` are fine.
- **Sloppy strings.** In source files, string literals with `→`, `Oops`, `Something went wrong`, an exclamation mark at the end, hype words (seamless, supercharge, effortless, magical) or emoji: `git grep -n -i -E '"[^"]*(→|oops|something went wrong|seamless|supercharge|effortless|magical)[^"]*"' -- '*.rs' '*.ts' '*.tsx' '*.js' '*.vue' '*.svelte' '*.swift' '*.kt' '*.cs' '*.cpp' '*.py' '*.dart' '*.go'`. Then `→` in docs: `git grep -n '→' -- '*.md'`.
- **Locks taken twice.** `git grep -n -E '\.lock\(\)[^;]*\.lock\(\)'`, then read each hit: the same lock twice in one statement is a deadlock.
- **Version drift.** Read the version in every manifest (`package.json`, `Cargo.toml` under `[package]`, `tauri.conf.json`, `*.csproj`, `pyproject.toml`), the newest released section of `CHANGELOG.md` and the README's version badge. They must agree, except for internal packages that never ship.
- **No updater.** `git grep -n -i -E 'releases/latest|electron-updater|autoUpdater|plugin-updater|Sparkle|WinSparkle|Velopack|Squirrel|AppImageUpdate|checkForUpdates'` in the app's code. Nothing means no update path.
- **Generated attribution.** `git log --format='%h %B' | grep -i -E 'co-authored-by:.*(claude|anthropic|copilot|openai|chatgpt|gemini|cursor|codex)|generated with'`.
