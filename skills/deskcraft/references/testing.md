# Verifying like a user

Agents tend to test the code and skip the product. These checks run the product, without touching the user's own installation.

## Isolate

- Point the app at a throwaway profile through environment variables: `APPDATA` and `LOCALAPPDATA` on Windows, `HOME` on macOS, `XDG_CONFIG_HOME`, `XDG_DATA_HOME`, `XDG_CACHE_HOME` and `XDG_STATE_HOME` on Linux. Write a settings file into it to set up a state (a fake token, a chosen language, a last project).
- Add a flag for multiple instances so a test copy can run next to the user's.
- Never install, update or uninstall into the user's real profile. Before and after a test, check that the user's real app list entry, shortcuts and install folder are untouched.

## Drive the window

- Prefer a headless entry point (a CLI that runs the core features) for features and measurements; drive the GUI only to check the GUI.
- Launch flags that open a given screen (`--open-settings about`, `--open-project`) make screenshots repeatable.
- On Windows, run the app on a separate desktop (`CreateDesktop` + `CreateProcess` with `lpDesktop`) and capture it with `PrintWindow`; post key and mouse messages to its window. Nothing touches the user's screen, cursor or clipboard. On macOS and Linux, use a virtual display (Xvfb on Linux) or the accessibility API.
- To click, compute coordinates from a screenshot, send a mouse move before the button down, and verify the effect with a second screenshot or a process check, not by assuming.
- Check that a button closes the window by testing whether the process exited.

## The flows to run

1. **Install** into a temp profile, silent and with the window: files match the payload byte for byte, the app list entry has every field, shortcuts point at the exe.
2. **Update in place** over that install, then over an install made by the previous installer technology.
3. **Update from the app**: pretend an older current version, let it find the latest release, download, verify the checksum, restart into the installer and reopen.
4. **Uninstall** with the window, silent, by double clicking `uninstall.exe` and from the app list: nothing left but what the user chose to keep, no temp copy left behind.
5. **First run** with an empty profile: the check screen, one download, the main screen.
6. **Gated downloads** without a token, with a wrong one and with a good one.
7. **Long work**: progress moves, time left settles, cancel frees memory within a second or two.

## Slow path

- A cold cache copy of large files (unbuffered copy on Windows), read from the slowest disk you support.
- A memory limit lower than the development machine's (an environment variable that caps the GPU or RAM budget).
- The smallest screen: 1280 x 720 at 150 %.
- The oldest supported hardware generation, with the build flavour made for it.

## After every fix

- Lint with warnings as errors, run unit tests, and the ignored integration tests that matter for the change (network, files on disk).
- Rebuild the packages the user will run locally, so the folder they test from matches the code.
- Report what was measured and on what, and what could not be tested (dark theme when the machine is in light mode, a machine-wide install, signing).
