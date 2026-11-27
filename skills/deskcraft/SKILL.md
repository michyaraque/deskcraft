---
name: deskcraft
description: Builds or tunes a commercial desktop app from A to Z, the parts a demo skips. A coherent interface, a first run that ends in the first result, long work with real progress and cancel, errors people can act on, an installer that looks like the product and installs without admin rights, updates from inside the app, a release pipeline, code signing and a repo ready to go public. Use when the user wants to ship, polish, package, install, update, release, sign or launch a desktop app, or asks for an installer, an updater, onboarding, a setup wizard, release builds or CI for one. Works for any stack, like Tauri, Electron, Qt, Flutter, .NET, SwiftUI, GTK or a Rust GPU UI, on Windows, macOS and Linux.
license: MIT
metadata:
  author: Michael Araque
  github: https://github.com/michyaraque
  x: https://x.com/michyaraque
---

# deskcraft

deskcraft was written by Michael Araque ([@michyaraque](https://x.com/michyaraque)) from taking a desktop app from prototype to public releases. Every rule here comes from something that went wrong on a real user's machine.

A desktop app is judged long before its main feature runs. The download, the warning Windows shows, the installer, the first window, the first wait, the first error and the first update all happen first, and a demo skips every one of them. deskcraft builds those parts, or tunes the ones that exist, so the app feels like one product from the download page to the tenth update.

## What good looks like

- **One product everywhere.** The same name, icon, palette and voice in the window, the installer, the uninstaller, the release page and Windows' list of apps. Nothing looks like it came from a different tool.
- **A first run that ends in a result.** The app checks what it needs, says what is missing, lets the user choose, downloads with progress and lands on the main screen ready to work. No dead end, no screen that sends people back to the start.
- **Long work that talks.** Every wait longer than a second shows real progress, a time left when it can be measured, and a way to cancel. Nothing freezes the window.
- **Errors with a way out.** A failure opens a dialog that says what happened and what to do, with the button that does it. Never a line of red text in a corner.
- **Quiet, exact words.** Labels and values, no slogans, no explanations of what the user can already see. The installer reads like any installer.
- **Updates that arrive.** The app finds new versions, installs them and opens again. The updater ships in the first public version, because a version without it can only be replaced by hand.
- **Proof, not hope.** Requirements, load times and sizes in the README are measured on real machines, including a slow one.

## Run it

1. **Learn the product and the stack.** Read the README, the build files and the entry point. Find out what the app does, who runs it, on what hardware, and how it is built, packaged and released today. Run it if you can, and take screenshots of the screens you will judge.
2. **Audit.** Go through [references/audit.md](references/audit.md) and list every sign you find, one line each with where it lives. For an existing repository, run its mechanical checks first (missing files, secrets, developer paths, version drift, no updater, sloppy strings, locks taken twice) and keep the counts to compare at the end.
3. **Decide the size of the job.**
   - **From zero.** No app yet, or a prototype. Build the skeleton in the order of [From zero](#from-zero), then the passes.
   - **Tune.** A working app with gaps. Run the passes, but only where the audit found something.
   - **Nothing to fix.** Say so, name at most three refinements, and stop.
4. **Write the brief and stop.** Five short lines, described below. Show the audit and the brief and wait for the user's answer, because the brief decides the installer, the update channel and the release matrix, and changing it later means redoing them. Skip the wait only when the user already said to just do it.
5. **Work in passes, in order.** Finish a pass before the next. The order matters: identity comes before the shell because the installer and the window share it, the first run comes before packaging because the installer has to hand over to it, and updates come before the first public release.
6. **Verify like a user.** Follow [references/testing.md](references/testing.md). Install, run, update and uninstall in a throwaway profile, never on the user's real install. Test the slow path: a cold disk, a small screen, the cheapest supported hardware.
7. **Report.** The brief, a few lines per pass, what you measured, and what is left for the user, like buying a certificate or uploading a social preview.

## Brief

Five lines, from what the product is, before touching anything.

- **Product.** What it does and for whom, in one plain sentence.
- **Machines.** The weakest machine it must run on, with numbers: OS version, RAM, disk, GPU and VRAM if it uses one. This line decides loading strategy, build flavours and the requirements table.
- **Platforms.** Windows, macOS, Linux, and which architectures. One build per real difference (an old GPU generation, Apple silicon), never more.
- **Distribution.** Where people get it (GitHub releases, a website, a store), whether it is open source, and the update channel that follows from that.
- **Identity.** The name, the icon and two or three concrete words for its character, like `warm, exact, crafted` or `calm, technical`. Words that describe every app, like modern, sleek or clean, do not count.

## From zero

When there is no app yet, build the unglamorous parts first, while they are cheap:

1. The repository: license, README, CHANGELOG, `.gitignore`, a version in one place ([references/repo.md](references/repo.md)).
2. CI that builds, lints and tests on every push ([references/release.md](references/release.md)).
3. A window with the app's own shell and the design tokens ([references/ui.md](references/ui.md)).
4. The installer and the uninstaller ([references/installer.md](references/installer.md)).
5. The updater and the release workflow, so version 0.1 can already update itself ([references/updates.md](references/updates.md)).
6. Then the features, inside a product that already ships.

## 1. Identity

- One name, checked against the store, GitHub and a web search before anything carries it. A name shared with many apps (Forge, Studio, Hub) is lost in search; rename while it is cheap.
- One icon, drawn for small sizes. Check it at 16, 24, 32, 48 and 256 px on a light and a dark taskbar, pixel by pixel. A single stray pixel shows at 16 px.
- The same name and icon in the window title, the installer, the uninstaller, the shortcuts, the file properties (company, product, version, copyright) and the OS list of installed apps.
- Credit the author where people look: an About section with the name and links, and the repository's README.
- Details, sizes and the social preview are in [references/brand.md](references/brand.md).

## 2. Shell and interface system

- One set of design tokens: a neutral ramp with three text roles, one accent, a spacing scale, a radius scale, two weights. Every colour in the app is a function of the theme, so light and dark switch on the next frame. Values in [references/tokens.md](references/tokens.md).
- Follow the system theme by default, with a manual override. Every surface has an explicit background in both themes.
- A custom title bar is fine when it keeps the platform's behaviour: drag, double click to maximise, snap layouts, the caption buttons where the platform puts them. If it cannot, use the native one.
- The window fits the smallest common screen: 1280 x 720 at 150 % scaling on a laptop. Set a minimum size that still shows every control, remember size and position, and clamp a saved size that no longer fits.
- Every action reachable from the keyboard, with a visible focus ring; a command palette (Ctrl or Cmd + K) once there are more than a dozen actions; shortcuts listed in Settings.
- One instance at a time unless the product needs more; a second launch focuses the first window.
- Settings: one setting per row, label on the left, control on the right, grouped in a few tabs. Changes apply at once and persist without a Save button.
- Text rules and the patterns that read as generated are in [references/copy.md](references/copy.md) and [references/ui.md](references/ui.md).

## 3. First run

- The first window checks what the app needs (hardware, permissions, downloads) and shows the result as rows with a state each: ready, missing, unsupported. Unsupported says why and what would work.
- Nothing large is chosen for the user. Each option shows its size and what it gives, with its own download button.
- Downloads resume, check free disk space first, show progress and size, and can be cancelled.
- A download that needs an account, a token or a license acceptance walks the user through it in steps, with a button that tests the credential before downloading, and links to the exact page to accept. Prefer a source that needs none.
- Readiness is per thing the user can run. If a product has variants, having one of them is enough to enter the app; never demand all of them.
- New files the user adds (plugins, templates, presets) appear without a restart. A manual refresh button stays as well.
- Patterns and anti-patterns in [references/onboarding.md](references/onboarding.md).

## 4. Long work and errors

- Progress is measured, not estimated: bytes read of bytes expected, steps done of steps planned. Show a time left only once the rate is stable.
- Every long operation can be cancelled, and cancel works at once: the worker checks a flag at every natural step, frees what it took, and the UI says it stopped, not that it failed.
- Nothing heavy runs on the UI thread. A blocked UI thread looks like a crash to the user and to the OS, and leaves nothing in the log.
- A lock is never taken twice in one expression or call chain. A deadlock freezes the window without a panic or a log line, so it is the hardest bug to report.
- Big files load at the disk's speed: large sequential reads, read ahead in the order they are needed, no full copy in RAM when the machine cannot spare it. Measure with a cold cache on the slowest supported disk.
- Choose memory strategies from free memory at run time, not from a guess. A mode that fits a 64 GB workstation can swap a 16 GB laptop into a 15 minute load.
- Errors that need the user open a dialog above every window with a title, one or two plain sentences and the button that fixes it. Errors that do not need them become a toast. Every error is written to a log with a timestamp, and panics with a backtrace.
- Details in [references/performance.md](references/performance.md).

## 5. Data, settings and privacy

- Use the platform's folders, never the install folder: settings in the roaming or config dir, caches and large downloads in the local or cache dir, logs in their own folder. Paths per OS are in [references/installer.md](references/installer.md).
- Settings are written atomically (write a temp file, then rename) and a missing or broken file means defaults, never a crash.
- Secrets like API tokens are masked in the UI with a button to show them, cannot be copied while hidden, and stay on the machine.
- Never show developer paths, account names or build machine paths in the UI or the logs. Show user paths with environment shorthand like `%LOCALAPPDATA%\App` or `~/Library/Application Support/App`.
- No network call that the user did not cause or cannot turn off, except the update check, which has its own setting.

## 6. Installer and uninstaller

- Install per user, without admin rights, into the platform's per-user location. Ask for elevation only to remove an old machine-wide install.
- The installer looks like the product (its palette, type and icon) and reads like any installer: destination folder, space required, a desktop shortcut option, the license link, install, done, run. No pitch, no notes about features.
- It updates in place when the app is already installed and replaces installs made by an earlier installer technology without leaving two copies.
- It registers in the OS list of apps with name, version, publisher, icon and size, and its uninstaller is reachable from there and by opening it directly.
- The uninstaller removes exactly what was installed (keep a file list), keeps the user's own work, and asks before deleting downloaded data or settings, both off by default.
- A silent mode for scripts on both. Every window closes from its buttons, Esc and the title bar.
- Per-platform recipes in [references/installer.md](references/installer.md).

## 7. Updates

- Ship the updater in the first public version. Every version without it has to be replaced by hand.
- The app checks a release feed shortly after start and every few hours, with a setting to turn it off, and picks the package for its own build flavour.
- A newer version shows as one quiet button in the title bar and in Settings. Download, verify the checksum, then restart into the installer, which waits for the app to exit, installs over it and opens it again.
- A portable copy cannot update itself in place; it opens the release page instead.
- Downloaded installers are deleted on the next start.
- Details and the hand-over protocol in [references/updates.md](references/updates.md).

## 8. Release pipeline

- The version lives in one place and everything else is generated from it: file properties, installer name, archive names, the about screen.
- Semantic versioning: a fix is a patch, any new feature is a minor, only a break is a major.
- A changelog in Keep a Changelog format with an Unreleased section; the release notes come from the version's section.
- Pushing a commit that bumps the version builds every package, publishes a release named `App vX.Y.Z` with a `vX.Y.Z` tag, the notes, what to download for which machine, and `SHA256SUMS.txt`. A push that keeps the version publishes nothing.
- CI is fast enough that people wait for it: cache the toolchain, the dependencies and the heavy SDKs, run the cheap checks first and the slow ones in parallel.
- Commits are small, one line, conventional (`feat(scope): …`, `fix(scope): …`), and describe the change from the user's side.
- Workflow templates and the full list in [references/release.md](references/release.md).

## 9. Signing and launch

- Unsigned apps get a warning on Windows (SmartScreen) and are blocked on macOS (Gatekeeper). Sign every executable and installer, and notarise on macOS. Free and cheap options are in [references/signing.md](references/signing.md).
- Before a repository goes public, audit its whole history: secrets, personal paths, emails, generated attribution lines, test data. Delete stale branches. Turn on private vulnerability reporting.
- Requirements in the README are measured, with the machine they were measured on.
- A social preview image (1280 x 640, under 1 MB) made from the product's own world.
- The checklist in [references/repo.md](references/repo.md).

## Do not

- Do not add text that explains the obvious, slogans, or notes about how the screen was designed. Cut first; write only what the user acts on.
- Do not ship a build that cannot update itself to the public.
- Do not hide an error in a status line or a log only.
- Do not run anything heavy on the UI thread, and do not hold a lock while waiting for another.
- Do not test installs, updates or uninstalls on the user's real installation or profile.
- Do not guess requirements or timings; measure them.
- Do not put the author's machine paths, tokens or personal data in the code, the UI, the logs or the history.
- Do not add an AI tool as a commit co-author or put generated attribution lines in commits unless the user asks.

## Checklist

- [ ] The brief is written and every pass follows it
- [ ] One name and icon everywhere: window, installer, uninstaller, shortcuts, file properties, OS app list
- [ ] Design tokens with light and dark themes; no hard-coded colours outside them
- [ ] The window fits 1280 x 720 at 150 % and remembers its size
- [ ] Every action works from the keyboard, with a visible focus ring
- [ ] First run reaches the main screen with only one option downloaded
- [ ] Every wait over a second shows measured progress and can be cancelled
- [ ] Errors open a dialog with the action that fixes them, and every error is in the log
- [ ] No lock taken twice, no heavy work on the UI thread
- [ ] Settings, caches and logs in the platform folders; nothing written next to the executable
- [ ] Secrets masked, never copied while hidden, never logged
- [ ] Per-user install without admin rights, listed in the OS app list, clean uninstall
- [ ] Installer and uninstaller read like any installer and close from every button
- [ ] The updater ships, verifies checksums and reopens the app after updating
- [ ] Version in one place, changelog with the version's section, release on version bump
- [ ] Every package signed, or the user knows exactly what signing costs and how to get it
- [ ] History audited before going public; requirements measured
- [ ] The mechanical checks in [references/audit.md](references/audit.md) leave no hit without a reason
