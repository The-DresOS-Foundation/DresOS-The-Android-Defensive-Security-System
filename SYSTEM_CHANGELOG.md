
# DresOS Android Defensive Security System - Changelog

---

## July 17, 2026 Update

Debloated and refined the entire guide to make building the system much easier to understand, got rid of URL Checker, Fake Traveller along with some other apps that our own brand new app DresSecureComms now covers, reviewed the security throughout and made it less convoluted along with changing some steps to make it all a seamless and flawless build guide.

---

## June 19, 2026 Update

---

### WebView - DresOS WebView Replaces AOSmium as the Module Engine

The `dresoswv` WebView module no longer ships the AOSmium engine. It now installs **DresOS WebView**, DresOS's own Chromium build produced from Cromite (Chromium 145.0.7632.120), signed with the DresOS release key. The module keeps the same id, so flashing this version over an existing AOSmium install updates it in place: the AOSmium APK and its allowlist entry are replaced with DresOS WebView, the stale `webview_provider` selection is cleared on the next boot, and DresOS WebView is promoted automatically. No uninstall and no manual step.

Step 5 (the module list), Step 7 (the deep dive), the What You Will Need table, the Replacement Overview, and the Part 3 App Suite entry (item 16) have all been updated to name DresOS WebView and `org.dresos.webview` in place of AOSmium. The download now points at `DresOS-WebView-v2_2_0.zip`.

What changed in the module:

- Engine swapped from AOSmium (`org.axpos.aosmium_wv`, AXP.OS signed) to DresOS WebView (`org.dresos.webview`, DresOS signed). Signing cert SHA-256 `B7815F746C3183C66CAD631079CB669C0494A8BE542A2A96E5975D8FABCD92DE` (DresOS RSA 4096).
- The static RRO now whitelists `org.dresos.webview` with the DresOS certificate in `config_webview_packages`.
- Host requirements updated to Magisk 29.0 or newer, Android 10 through 16, arm64 only. The old two-APK ABI selection is gone; a single arm64 APK ships.
- Updating over the AOSmium build clears any stale provider selection that still points at `org.axpos.aosmium_wv`, so the swap is seamless.
- Activation, the post-fs-data bootloop sentinel, the inert mode fallback, and the recovery safe stock WebView restore are unchanged.

The engine is also published on its own now as a standalone signed APK in a dedicated repository, `https://github.com/DresOperatingSystems/DresOS-WebView`, with IzzyOnDroid and Obtainium support. That standalone app is the only brand new release in this cycle; the WebView module is an in place source update and is not cut as a new release.

---

### microG - DresOS microG Module Bumped to v3.0.1

The DresOS microG module has been bumped to v3.0.1. A weekly GitHub Actions pipeline now pulls the latest officially signed microG core (GmsCore, Companion, GsfProxy) from the official microG F-Droid repo and auto-bumps the module when upstream changes. The bundled microG APKs and the install logic are unchanged. The module description was also corrected: microG coexists with the DresOS WebView module, the AOSmium WebView module it previously named having been replaced.

---

### Magisk Modules Roadmap

- dresoswv: DresOS WebView - Released v2.2.0, engine now DresOS WebView (Cromite based)
- dresosmicrog: DresOS microG - Released v3.0.1
- dresosperms: Dangerous permission revocation from system apps - In active development
- dresosdebloat: Core Google app and system bloat removal - Planned
- dresosafwall: AFWall+ pre-configured iptables rules - Planned
- dresosoverlay: System-level telemetry and advertising ID disabling - Planned
- dresosfossify: Fossify suite system app installer - Planned
- dresosheliboard: HeliBoard default keyboard installer - Planned

---

## June 5, 2026 Update

---

### microG - DresOS microG Module Rebuilt and Released as v3.0.0

The DresOS microG Magisk module has been rebuilt from the ground up and released as v3.0.0, replacing the v2.0.0 design entirely. It is now a pure file overlay with no Zygisk payload, no Xposed or LSPosed dependency, and no boot-time PackageManager work, which means it physically cannot bootloop the device and it coexists cleanly with the AOSmium WebView module.

Step 5 (Install Magisk Modules), Step 6 (Set Up microG), the What You Will Need table, and the SECURITY_ARCHITECTURE.md Layer 8 entry have all been updated to match.

What the module does in one flash:

- Ships the officially signed microG GmsCore 0.3.15, Companion (FakeStore at com.android.vending), GsfProxy, DroidGuard Helper, Aurora Store, and Aurora Services as privileged system apps under product, with a privapp-permissions allowlist generated from the bundled manifests.
- Signature spoofing is provided by the ROM rather than by the module. Because the bundled microG APKs carry the official microG key, any ROM with microG signature spoofing support spoofs them automatically once they are placed in priv-app. There is no bundled hook and no Xposed framework on any ABI.
- Aurora Services now lands as a priv-app with its own permissions allowlist, so silent prompt-free installs through Aurora Store work without the standard Android installer prompt for every app. This resolves the earlier Aurora Privileged Extension issue.
- Detects an already-present, upstream-signed microG (CalyxOS, LineageOS for microG, iodeOS, /e/OS) by reading the X.509 cert SHA-256 via `cmd package dump` and leaves the ROM's copy in place, staging only the Aurora components on those ROMs.
- Hard refuses to install on GrapheneOS, which deliberately blocks signature spoofing and ships its own Sandboxed Google Play.

---

### App Suite - Gallery, Authenticator, Calendar, and Keyboard Design

- The gallery app has moved from Fossify Gallery to Aves Libre, an open-source photo and video manager that keeps everything on the device with no cloud and no trackers.
- Stratum has been added as the offline two-factor authenticator: open-source, single permission, encrypted exportable backups, and no network access, so it can be blocked in AFWall+.
- Tuta Calendar has been added as the end-to-end encrypted calendar that syncs across devices through your Tuta account, alongside the local-only Fossify Calendar.
- A ready-made DresOS HeliBoard design has been added, a privacy-tuned configuration restored as a single backup file from HeliBoard Backup and restore, hosted in the guide repository under `assets`.

---

### Documentation - Link Audit and Video Demo

- Every version-pinned download link in the guide has been converted to a stable form, either an F-Droid package page or a GitHub releases-latest page, so the links do not rot as apps update.
- Part 3 of the guide was completed end to end. The app suite now documents nineteen apps, with DuckDuckGo Privacy Browser and AOSmium WebView in their correct positions and the Aves Libre, Stratum, and Tuta Calendar entries renumbered after them. The Compatible Devices list, the license note, and the closing were finalized, and the table of contents was corrected to match.
- A full Motorola Moto g32 video demo of the running system has been linked in the guide and embedded on the website.

---

### Magisk Modules Roadmap

- dresoswv: AOSmium WebView - Released v2.2.0
- dresosmicrog: DresOS microG - Released v3.0.0
- dresosperms: Dangerous permission revocation from system apps - In active development
- dresosdebloat: Core Google app and system bloat removal - Planned
- dresosafwall: AFWall+ pre-configured iptables rules - Planned
- dresosoverlay: System-level telemetry and advertising ID disabling - Planned
- dresosfossify: Fossify suite system app installer - Planned
- dresosheliboard: HeliBoard default keyboard installer - Planned

---

## May 25, 2026 Update

---

### microG - DresOS microG Module Released as v2.0.0

The DresOS microG Magisk module has been released as v2.0.0 and replaces the older manual microG plus signature-spoofing-framework setup in the build guide. It is a single Magisk flash that ships the full microG suite as systemless privileged apps with a bundled Zygisk signature spoof scoped to the microG process only.

Step 5 (Install Magisk Modules) and Step 6 (Set Up microG) in the build guide have been rewritten end to end. The Step 5 module list, the Step 6 setup procedure, the SECURITY_ARCHITECTURE.md Layer 8 entry, the What You Will Need table, and the Magisk Modules Roadmap have all been updated to reflect the new module.

What the module does in one flash:

- Stages microG GmsCore 0.3.7.250932, microG Companion (FakeStore at com.android.vending), microG GsfProxy, microG DroidGuard Helper, Aurora Store, and Aurora Services as systemless privileged apps. APKs and matching privapp permissions XML land in the same partition (`system/product/priv-app` on API 28 plus, `system/priv-app` on API 26 and 27), satisfying Android 11 plus same partition enforcement.
- Bundles a Zygisk based signature spoof, scoped to the microG process only, with a Google certificate read from the module directory. On arm64 and x86_64 LSPosed is NOT required. On armeabi-v7a, armeabi, x86, or riscv64 the bundled hook does not ship a prebuilt and LSPosed plus FakeGApps remains the fallback.
- Detects ROMs that already ship a working upstream signed microG (CalyxOS, LineageOS for microG, iodeOS, /e/OS) by reading the X.509 cert SHA-256 via `cmd package dump`. On those ROMs the bundled microG is skipped and only the Aurora components are staged.
- Hard refuses to install on GrapheneOS, which deliberately blocks signature spoofing and ships its own Sandboxed Google Play.
- Runs all PackageManager state mutations from `service.sh` after `sys.boot_completed` plus a settling delay. `customize.sh` does file work only. This avoids the racy install time PMS work that caused the older manual microG setup to be fragile on Pixel and several LineageOS builds.
- Runtime Google debloat via `pm disable-user --user 0`, persisted in `/data/system/users/0/package-restrictions.xml`, reversible by `pm enable` or by uninstalling the module. No directory level `.replace` markers on priv-app dirs, which would have hidden the ART OAT cache on Android 14 plus and missed boot complete.
- Per component bootloop sentinel. If any single piece (the Zygisk hook, the priv-app overlay, or the debloat pass) does not survive three consecutive boots, only that piece is disabled on the next boot and the rest of the module continues to operate.

Known issue in v2.0.0: the Aurora Privileged Extension (`com.aurora.services`) did not always land as a system priv-app on every device, so some users saw the standard Android installer prompt for each Aurora Store install instead of the silent install path through Aurora Services. This was resolved in the v3.0.0 rebuild, which stages Aurora Services as a priv-app with its own permissions allowlist.

Diagnostics live at `/data/adb/modules/dresosmicrog/logs/` and runtime state under `/data/adb/dresosmicrog/`. The Action button on the module in the Magisk app reprints the full status dashboard, regrants runtime permissions, and restarts the microG components.

---

## May 15, 2026 Update

---

### WebView - DresOS AOSmium WebView Module Rewritten to v2.1.0

The AOSmium WebView Magisk module has been rewritten end to end and released as v2.1.0, with a redesigned activation pipeline and two layers of bootloop safety.

The Step 5 module list, the Step 7 deep dive, the App Suite section 16 entry, and the Magisk Modules Roadmap have all been updated to reflect the new mechanism. The old language describing pm install, Magisk .replace files, and manual selection in Developer Options has been removed from the guide entirely.

What the module does now in one flash:

- Validates the host environment: Magisk 24.0 or newer, Android 10 through 15, arm or arm64 ABI. Aborts cleanly on x86/x86_64 (AXP.OS does not publish WebView builds for those ABIs), on unknown ABIs, and on devices that ship WebView as an APEX module.
- Drops the signed AOSmium APK into the systemless tree at `system/product/app/AOSmiumWebView/AOSmiumWebView.apk`. Magisk magic mount makes this visible to PackageManager as a preinstalled system app, satisfying the framework `MATCH_FACTORY_ONLY` scan.
- Places a static RRO in the systemless overlay partition. The RRO adds `org.axpos.aosmium_wv` plus the full AXP.OS ECDSA signing certificate to the framework `config_webview_packages` allowlist. Both `com.google.android.webview` and `com.android.webview` are kept in the allowlist as fallbacks.
- After `sys.boot_completed`, `service.sh` runs `cmd webviewupdate set-webview-implementation org.axpos.aosmium_wv` to promote AOSmium to the active provider. Activation is verified by re reading `dumpsys webviewupdate`.

Two bootloop safety layers were added:

- post-fs-data sentinel: drops a boot_pending marker on every boot which service.sh clears on success. A stale marker on the next boot triggers `touch /data/adb/modules/dresoswv/disable`, which Magisk respects to skip the module entirely on subsequent boots.
- Inert mode flag: set automatically by service.sh on any activation failure. Files remain bind mounted by Magisk but no further activation is attempted until the flag is cleared.

After flashing v2.1.0, no manual step in Developer Options is required. Verify activation from adb with `adb shell dumpsys webviewupdate | grep "Current WebView package"`. Expected output: `Current WebView package (name, version): (org.axpos.aosmium_wv, 147.0.7727.49)`.

---

### Compatible Devices Expanded

The Compatible Devices appendix has been updated with the four newly confirmed device and Android version combinations. The list now reads:

- Motorola Moto G32 on LineageOS Android 15
- Motorola ThinkPhone on Stock Android 15
- Motorola Moto G7 Plus on Stock Android 10
- Motorola Moto G7 Plus on LineageOS Android 15
- Samsung Galaxy A05s on Stock Android 10

---

### Security Architecture Update

The Layer 5 (System WebView) section of `SECURITY_ARCHITECTURE.md` has been rewritten to reflect the new v2.1.0 activation pipeline. The diagram now describes:

- Systemless APK placement at `system/product/app/AOSmiumWebView/` (no more system/app vs system/product/app branching)
- Static RRO with AXP.OS ECDSA certificate embedded in `config_webview_packages` allowlist
- OEM WebView retained as a fallback rather than hidden with .replace markers
- `cmd webviewupdate set-webview-implementation` activation with `dumpsys` verification
- post-fs-data sentinel and inert mode flag as the two bootloop safety layers
- Confirmed working device list
- AXP.OS signing certificate SHA-256 fingerprint for verification

---

## Early May 2026 Update

---

### File Management - Replaced ZArchiver Pro and Fossify Files with Amaze File Manager

ZArchiver Pro has been removed from the DresOS app suite entirely. It was sourced from an unofficial GitHub mirror, is not available on F-Droid, and is not fully open-source in the form distributed. Fossify Files has been removed as a separate install because it has no encryption capability.

Both are replaced by **Amaze File Manager**, available directly from F-Droid at `https://f-droid.org/packages/com.amaze.filemanager/`.

Amaze File Manager (GPL-3.0, built by Team Amaze) covers both use cases in one app:

- Full file management with cut, copy, move, delete, compress, extract across internal storage, SD card, and USB OTG
- Built-in AES-256 encryption and decryption of files and folders with .aes output files
- Biometric lock (fingerprint and face unlock) for the entire app
- Root explorer for advanced system access on rooted devices
- Built-in APK reader, installer, and App Manager for backing up or uninstalling apps
- Multiple tabs, bookmarks, search, and SMB/SSH/FTP/SFTP network share support
- No ads, no trackers, no Google dependencies, actively maintained

The file management section in Step 10 and the Part 3 app suite entry have both been updated to reflect this change.

---

### WebView - DresOS Magisk Modules Repo Now Live

The DresOS Magisk Modules repository is now live at `https://github.com/DresOperatingSystems/DresOS-Magisk-Modules`.

The AOSmium WebView module (`dresoswv`) v1.0.0 is the first release. It installs AOSmium (Chromium 147.0.7727.49, hardened with GrapheneOS/Vanadium patches) as the system WebView in a single Magisk flash. The guide now links directly to this repo rather than describing a manual build process.

What the module does:

- Validates device (Android 10+, ARM/ARM64)
- Removes data-partition updates of Chrome, Google WebView, AOSP WebView, TrichromeLibrary, and OEM WebView packages
- Systemlessly hides all competing system WebView packages via Magisk .replace files
- Installs AOSmium to system/app (or system/product/app on LineageOS) and registers via pm install
- Places a compiled RRO overlay that patches the config_webview_packages allowlist so Android accepts AOSmium as a valid WebView provider
- Writes install and activation logs to /data/adb/modules/dresoswv/ for diagnostics

After flashing: Settings -> Developer Options -> WebView implementation -> select AOSmium WebView.

---

### Security Architecture Update

The security architecture document has been updated to reflect:

- WebView layer: install path corrected to system/app (not system/priv-app)
- Threat map: added "Sensitive files on device -> AES-256 encrypted by Amaze File Manager"
