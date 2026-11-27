# Identity

## Name

- Search the name on GitHub, the app stores, the package registries of the stack and the web. Reject names shared with several known apps or generic words (Forge, Studio, Hub, Pro).
- Prefer a word or compound that says what the product does over an abstract word or a common noun.
- Rename once, everywhere, in one pass: crate or package names, binaries, window titles, installer, settings folders, log names, docs, the repository and its description. Then search the whole tree and the history for the old name and leave none, including in user-facing data files the app reads (sidecar files like `*.oldname.json`), with a migration that renames them.

## Icon

- Draw the mark for 16 px first; a detailed logo becomes a blur. A single strong silhouette in the product's world.
- Export every size the platform uses: Windows `.ico` with 16, 20, 24, 32, 40, 48, 64, 256; macOS `.icns` from 16 to 1024 at 1x and 2x; Linux PNGs in `hicolor/<size>/apps/` and an SVG when possible.
- Check each size at 100 % on light and dark backgrounds and look for stray pixels, halos from a removed background and off-colour edges. Fix them in the source and regenerate every size with a script that lives in the repository.
- The same mark in the title bar, the installer, the About screen, the taskbar and the README.

## File properties

- Windows version resource: CompanyName, FileDescription, FileVersion, ProductName, ProductVersion, LegalCopyright, OriginalFilename. Generated from the one version.
- macOS `Info.plist`: CFBundleName, CFBundleDisplayName, CFBundleIdentifier (reverse DNS, never changes), CFBundleShortVersionString, CFBundleVersion, NSHumanReadableCopyright.
- Linux `.desktop`: Name, Comment (one plain line), Exec, Icon, Categories, StartupWMClass matching the window class.

## Author

- The author's name and links (GitHub, X, a site) in the About screen and the README. In the plugin or package metadata as well.

## Visual identity outside the app

- The installer, the uninstaller, the release notes header image and the social preview use the app's palette and logo.
- Images for the README: the real app with real content, light and dark, at a common laptop size.
