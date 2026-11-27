# Signing

Prices and programs change; check each provider's page before recommending one, and tell the user what you checked.

## Windows

Unsigned downloads show SmartScreen's "Windows protected your PC". A signature names the publisher; SmartScreen still warns new publishers until the signature gathers reputation from downloads. An EV certificate is no longer a guaranteed way around that, so do not sell one as the fix.

- **SignPath Foundation.** Free code signing for open source projects that meet its rules (OSI license, public repository, builds in CI). Signs in their service from your CI artifacts. The usual best choice for an open source app.
- **Azure Trusted Signing.** A monthly subscription with identity validation; eligibility for individuals depends on the country. Cheap and integrated with signtool and CI.
- **Certum Open Source Code Signing.** A low-cost certificate for open source developers, on a smart card or cloud HSM.
- **Microsoft Store (MSIX).** The store signs the package; users install without warnings.
- **Commercial OV certificates.** From the usual CAs, now issued on hardware tokens or cloud HSMs, which complicates CI.

Sign every executable you ship (the app, helper exes, the installer and the uninstaller) with a timestamp, so signatures stay valid after the certificate expires: `signtool sign /fd sha256 /tr <timestamp url> /td sha256 …`.

## macOS

- An Apple Developer Program membership (yearly fee) gives a Developer ID Application certificate.
- Sign with the hardened runtime and the entitlements the app needs, notarise with `notarytool submit --wait`, then `stapler staple` the app or DMG.
- Without notarisation Gatekeeper blocks the app; users can still open it from System Settings, but most will not.

## Linux

- AppImages can embed a GPG signature; Flatpak repositories and distribution packages are signed by their repository. Publish the checksums and, when you have one, sign `SHA256SUMS.txt` with a published key.

## Until it is signed

- Say it in the README, with the exact steps for the warning (`More info > Run anyway` on Windows) and the checksums to verify the download.
- Publish only from CI, so a signature added later covers the same reproducible artifacts.
