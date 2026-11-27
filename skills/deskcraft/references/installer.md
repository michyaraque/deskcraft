# Installer

## Where things go

| | Windows | macOS | Linux |
| --- | --- | --- | --- |
| App (per user) | `%LOCALAPPDATA%\Programs\App` | `~/Applications/App.app` or `/Applications` | `~/.local/share/App` or an AppImage anywhere |
| Settings | `%APPDATA%\App` | `~/Library/Application Support/App` | `$XDG_CONFIG_HOME/app` (`~/.config/app`) |
| Large data, caches | `%LOCALAPPDATA%\App` | `~/Library/Caches/App` or Application Support | `$XDG_DATA_HOME/app`, `$XDG_CACHE_HOME/app` |
| Logs | `%LOCALAPPDATA%\App\logs` | `~/Library/Logs/App` | `$XDG_STATE_HOME/app` |
| Launcher | Start menu `.lnk` in `FOLDERID_Programs` | the .app itself | `~/.local/share/applications/app.desktop` + icon in `hicolor` |
| App list | `HKCU\Software\Microsoft\Windows\CurrentVersion\Uninstall\App` | none (drag to Trash) | the package manager, or the .desktop file |

Never write into the install folder after installing; it is replaced on every update.

## Choosing the tool

- **Own installer.** A small program in the app's own UI toolkit, with the app's files compressed and appended to it (a footer at the end of the file says where they start). Full control of the look and of updates; the most work. Worth it when the look matters or no tool fits.
- **Velopack.** Installer and updater for any stack on Windows, macOS and Linux, with delta updates. A good default when the stack has no updater of its own.
- **The framework's bundler.** Tauri (NSIS or MSI, DMG, AppImage, deb, rpm) and Electron (electron-builder) produce installers and pair with their own updater plugins.
- **Inno Setup, NSIS, WiX.** Mature Windows installers; Inno and NSIS are scriptable and per-user capable, WiX makes MSI for managed environments. They look like their tool unless heavily themed.
- **MSIX / Microsoft Store.** Signed by the store, sandboxed, automatic updates; restrictions on file access and drivers.
- **macOS.** A signed, notarised .app inside a DMG (or a zip). Sparkle for updates outside the App Store.
- **Linux.** AppImage for "download and run" (with AppImageUpdate for updates), Flatpak on Flathub for store-style installs and updates, deb and rpm for distribution repositories.

## Windows, per user

- Install to `%LOCALAPPDATA%\Programs\App` without elevation. Add a manifest with `requestedExecutionLevel level="asInvoker"`, because Windows' installer detection may elevate a 32-bit program named `*setup*` or `*install*`.
- Register under `HKCU\…\Uninstall\App` with `DisplayName`, `DisplayVersion`, `Publisher`, `URLInfoAbout`, `DisplayIcon` (the exe), `InstallLocation`, `UninstallString` (`"…\uninstall.exe" --uninstall`), `QuietUninstallString` (plus `--silent`), `EstimatedSize` in KB, `NoModify` and `NoRepair` set to 1.
- Shortcuts with `IShellLink`: Start menu always, desktop when the user chose it; target the exe, working directory its folder, icon from the exe.
- Write a file list (`uninstall.lst`) with every file installed. On update, remove files the old list had and the new one does not; on uninstall, remove exactly the listed files and the folders they leave empty. A file the user put in the folder stays.
- Copy the setup program without its payload as `uninstall.exe`. When run from inside the install folder, it copies itself to the temp folder and relaunches from there (a running exe cannot delete itself), and the copy deletes itself after exit.
- Opening `uninstall.exe` directly (double click) uninstalls, the same as `--uninstall`.
- Before writing, check whether the app is running (opening its exe for writing fails while it runs); wait a few seconds for one that is exiting, then ask the user to close it.
- If an older installer (another tool, another AppId) installed the app, run its uninstaller silently first and wait until its registry entry is gone. A machine-wide one asks for admin once; say so in that step.
- Silent mode: `setup.exe --silent [--dir <folder>] [--desktop]` and `uninstall.exe --uninstall --silent`. Exit code 0 on success, 1 with the error on stderr.

## The installer's own window

- The app's palette, type, logo and the same window chrome. A side panel with the logo and the version is enough identity.
- Screens: choose (destination with `Change…`, desktop shortcut switch, space required and license link) > progress (steps with a check, a spinner and pending dots, a bar with the current file and bytes) > done (success, `Run App` switch, `Finish`) or failed (the error, `Close`, `Try again`).
- Update mode: no choose screen, a `Close App` step that waits for the app's process, install, reopen, exit on its own. An error stays on screen.
- Unique element ids for every clickable thing, including the caption buttons; two controls sharing an id can swallow each other's clicks.
- Close the window from Close, Finish, Esc and the title bar's X by closing the window itself, and test each.

## Payload

- zstd at a high level with long-distance matching packs runtimes and binaries close to LZMA's size and unpacks about ten times faster: a 700 MB app installs in a second or two.
- Stream entries (path, size, bytes) and refuse paths that are absolute or contain `..`.
- A footer with a magic number, the payload offset and length, and the unpacked size; check that they add up to the file size before trusting them.

## Uninstall options

- Downloaded data and settings are kept by default; offer `Also remove the downloaded files (4.2 GB)` and `Also remove the settings` as switches, both off. Show the size.
- The user's own output (images, documents, projects) is never removed.
