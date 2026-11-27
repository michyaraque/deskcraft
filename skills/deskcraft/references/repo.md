# Repository and launch

## Files

- `README.md`: what the app does in one paragraph, a screenshot, a download section that says which file is for which machine, measured requirements, how to build, the author with links. A second language as `README.<lang>.md` when the audience needs it.
- `LICENSE`, `CHANGELOG.md`, `CONTRIBUTING.md`, `SECURITY.md` (how to report privately), `docs/ARCHITECTURE.md` (one table: file or folder, what it does), `docs/RELEASING.md` (the release steps).
- `.gitignore` for build output, packaged releases, local scratch and editor folders. `.gitattributes` with line endings fixed per type.
- `.github/`: workflows, issue templates with a link to private vulnerability reporting, a pull request template, dependabot grouped monthly.
- No scripts in the repository that do not take part in building, testing or releasing. One-off helpers stay outside it.

## Requirements table

Measure each row on a real machine and say which. For apps with heavy assets, show the modes and what they cost:

| Setup | GPU memory | RAM | First result |
| --- | --- | --- | --- |
| Mode A, measured on machine X | … | … | … |
| Mode B, measured on machine Y | … | … | … |

## Before going public

1. **History.** Search every commit, not only the tip: tokens (`ghp_`, `github_pat_`, `sk-`, `AKIA`, private key headers), personal paths (`C:\Users\<name>`, `/Users/<name>`, `/home/<name>`), emails, test data, and generated attribution lines (`Co-Authored-By:` an AI tool). The mechanical checks in [audit.md](audit.md) cover the working tree and the history.
2. **Rewrite if needed** before anyone clones: a fresh history on a new branch or `git filter-repo`, then force-push and delete every other branch. Pull request refs of closed PRs cannot be deleted by you and keep old commits visible; if they contain something sensitive, ask GitHub Support or publish from a fresh repository.
3. **Names in code.** Third-party product or game names used as descriptions in comments can be rewritten neutrally; strings that are part of a data format or a protocol stay, because changing them breaks compatibility.
4. **Settings.** Description and topics, private vulnerability reporting on, the default branch protected, a social preview uploaded.

## Social preview

- 1280 x 640, PNG, under 1 MB, uploaded by hand in Settings > General > Social preview (there is no API).
- Made from the product's world, not a screenshot: the product's own output or a scene it would make, the logo, the name and one plain line of what it is. Dark text areas for the title, the visual on the other side.
- No claims that will age (`for Windows` when other platforms are coming).

## Launch

- A release with installers for every platform and flavour, checksums and notes.
- A short post with the social preview, one sentence of what it does, the download link. A video of the first result helps more than a feature list.
- A way to hear back: issues, a discussion board or a contact link in the About screen.
