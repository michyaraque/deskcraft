# Updates

## When

- In the first public version. Users of a version without the updater must reinstall by hand once; every version after that updates itself.
- If the stack has a maintained updater (Tauri's updater plugin, electron-updater, Sparkle, WinSparkle, Velopack, Flatpak or a store), use it and still apply the rules below. Otherwise the recipe here is small and works with any release host.

## Checking

- A few seconds after start (off the start-up path) and then every 12 hours, on a worker thread.
- A setting `Check for updates automatically`, on by default, and a `Check for updates` button that works even when it is off.
- Ask the host's API for the latest release (`https://api.github.com/repos/<owner>/<repo>/releases/latest` with a `User-Agent`), read the tag, strip the `v`, compare as numbers (major, minor, patch), not as text: `1.10.0` is newer than `1.9.3`.
- Pick the asset for this build: the flavour is known at compile time (`App-<v>-setup.exe` or `App-<v>-legacy-setup.exe`, per platform and architecture).
- An environment variable that pretends another current version lets you test the whole flow from a development build.

## Showing

- When a newer version exists, one quiet button in the title bar, `Update to 1.3.1`, that turns into `Downloading 42 %` and then `Restart to update`.
- The same state in Settings > About: a row `Updates` with the state as its description, a `What's new` button that opens the release page, and the action (`Download and install`, `Restart and update`, `Check for updates`).
- Failures in a check are logged and shown only in Settings; a failed check never interrupts work.

## Downloading

- Download to a temp folder (`<temp>/app-update/`) as `.part`, hashing as it goes, then rename.
- Verify the SHA-256 against the release's `SHA256SUMS.txt` (the `sha256sum` format: hash, two spaces, file name). A mismatch deletes the file and shows the error. No sums file, no update.
- Delete that folder on the next start; the installer that is running at that moment is not needed any more.

## Installing

The app and the installer hand over with a tiny protocol:

1. The app starts the downloaded installer with `--update --wait <its process id>` and closes its window.
2. The installer opens its window straight on the progress screen with a `Close App` step, waits for that process to exit (with a time limit), then retries the exe for a few seconds until Windows releases its file.
3. It installs over the existing folder (same destination, same shortcut choice), updates the app list entry, and starts the app again.
4. It closes itself when done. If anything fails, the error stays on screen with `Try again`.

On macOS, the same flow with the new .app replacing the old one in place (Sparkle does this). On Linux, AppImageUpdate replaces the AppImage; Flatpak and distribution packages update through their own tools, so the app only shows that an update exists.

## Portable copies

- A portable copy (a zip, an AppImage outside the user's control) has no installer record to update. Detect it (no `uninstall.lst` next to the exe, or no app list entry) and make the button open the release page instead.

## Releases that feed it

- Every release carries the installers for every flavour and `SHA256SUMS.txt`.
- The latest release must always be a full release, never a draft or a pre-release, or the API's `latest` skips it.
- A release with a broken installer is fixed by publishing a new patch, never by replacing assets of the same version (clients may already have its checksum).
