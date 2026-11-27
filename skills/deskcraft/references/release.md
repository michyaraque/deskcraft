# Release pipeline

## Versions

- One source of truth: the app's manifest (`Cargo.toml`, `package.json`, `tauri.conf.json`, the `.csproj`, `Info.plist` generated from a variable). Build scripts derive the file properties, the installer's version, the archive names and the About screen from it.
- Semantic versioning. Patch: fixes only. Minor: anything new a user can see, even small. Major: something users must change or relearn. A one-line change that adds a feature is still a minor.
- A version bump is its own commit: `chore: release 1.3.0`, touching only the manifest, the lock file, the README badge and the changelog.

## Changelog

Keep a Changelog format:

```markdown
## [Unreleased]

### Added
- …

## [1.3.0] - 2026-11-27

One sentence on what this version is about.

### Added
### Changed
### Fixed
```

- Write entries from the user's side, with the number that matters (`opens large projects in 2 s instead of 9 s`).
- On release, rename `Unreleased` to the version and date. The release notes are that section, cut out by the release workflow.

## Commits

- One line, conventional, ASCII, no body: `feat(installer): add an update mode that waits for the app to close`, `fix(app): …`, `perf(engine): …`, `docs: …`, `ci: …`, `build: …`, `test: …`, `chore: release X.Y.Z`.
- Small and many: one change a reviewer can read in a minute. Split a large session into commits that each build.
- Describe the effect, not the mechanics: `fix(app): open projects saved by older versions instead of showing an empty canvas`.
- No generated attribution lines and no AI co-authors unless the owner asks for them.
- The default branch keeps the owner's name for it (`master` or `main`); ask, do not rename.

## CI

- Jobs: format (cheapest, on Linux), then lint and tests in parallel on the target platforms. Treat warnings as errors in lint.
- Cache everything that does not change per commit: the toolchain, the dependency registry and build outputs keyed per job, a compiler cache (sccache, ccache), and heavy SDKs downloaded from their redistributable archives instead of full installers.
- A warm run should finish in under ten minutes. Public repositories get faster and free runners on GitHub; private ones count minutes, so cache harder there.
- Use the exact toolchain version in a file (`rust-toolchain.toml`, `.nvmrc`, `global.json`) so CI and machines agree.

## Release workflow

Triggered on push to the default branch; a job compares the manifest's version with existing tags and stops when the version already has a release. Then:

1. **Build matrix.** One job per platform and flavour, each producing the installer, the portable archive and, for macOS, the notarised DMG. Upload them as artifacts.
2. **Publish.** Download every artifact, write `SHA256SUMS.txt` with `sha256sum`, extract the version's changelog section, append a download guide (which file for which machine), and create the release with the tag `vX.Y.Z` and the title `App vX.Y.Z`, adding GitHub's generated list of changes.

A template is in [../templates/release.yml](../templates/release.yml).

- Do not push to the default branch while a release run is publishing; a tag created on a commit that is no longer the tip, with workflow files changed after it, can fail with `Resource not accessible by integration`.
- The workflow needs `contents: write` permission only in the publish job.
- Builds that need a GPU toolkit or an SDK pin its version in the matrix, so a legacy flavour keeps building with the old toolkit.

## Local packaging

- One script builds the release folder, the portable archive and the installer, with a flag to skip the build and a suffix for flavours. CI calls the same script, so local and released packages match.
- It collects the runtime libraries from the toolkit the build used and the third-party licenses into a `licenses/` folder.
- Smoke-test the portable folder with the toolkit off the `PATH`: the app must find everything it ships.
