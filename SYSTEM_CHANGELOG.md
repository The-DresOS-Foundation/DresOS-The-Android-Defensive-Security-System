
# DresOS Android Defensive Security System - Changelog

## May 25, 2026 Update

---

### microG - DresOS microG Module Released as v2.0.0

The DresOS microG Magisk module has been released as v2.0.0 and replaces the older Noogle microG plus LSPosed plus FakeGApps chain in the build guide. It is a single Magisk flash that ships the full microG suite as systemless privileged apps with a bundled Zygisk signature spoof scoped to the microG process only.

Step 5 (Install Magisk Modules) and Step 6 (Set Up microG) in the build guide have been rewritten end to end. The Step 5 module list, the Step 6 setup procedure, the SECURITY_ARCHITECTURE.md Layer 8 entry, the What You Will Need table, and the Magisk Modules Roadmap have all been updated to reflect the new module.

What the module does in one flash:

- Stages microG GmsCore 0.3.7.250932, microG Companion (FakeStore at com.android.vending), microG GsfProxy, microG DroidGuard Helper, Aurora Store, and Aurora Services as systemless privileged apps. APKs and matching privapp permissions XML land in the same partition (`system/product/priv-app` on API 28 plus, `system/priv-app` on API 26 and 27), satisfying Android 11 plus same partition enforcement.
- Bundles a Zygisk based signature spoof, scoped to the microG process only, with a Google certificate read from the module directory. On arm64 and x86_64 LSPosed is NOT required. On armeabi-v7a, armeabi, x86, or riscv64 the bundled hook does not ship a prebuilt and LSPosed plus FakeGApps remains the fallback.
- Detects ROMs that already ship a working upstream signed microG (CalyxOS, LineageOS for microG, iodeOS, /e/OS) by reading the X.509 cert SHA-256 via `cmd package dump`. On those ROMs the bundled microG is skipped and only the Aurora components are staged.
- Hard refuses to install on GrapheneOS, which deliberately blocks signature spoofing and ships its own Sandboxed Google Play.
- Runs all PackageManager state mutations from `service.sh` after `sys.boot_completed` plus a settling delay. `customize.sh` does file work only. This avoids the racy install time PMS work that caused the older Noogle microG chain to be fragile on Pixel and several LineageOS builds.
- Runtime Google debloat via `pm disable-user --user 0`, persisted in `/data/system/users/0/package-restrictions.xml`, reversible by `pm enable` or by uninstalling the module. No directory level `.replace` markers on priv-app dirs, which would have hidden the ART OAT cache on Android 14 plus and missed boot complete.
- Per component bootloop sentinel. If any single piece (the Zygisk hook, the priv-app overlay, or the debloat pass) does not survive three consecutive boots, only that piece is disabled on the next boot and the rest of the module continues to operate.

Known issue in v2.0.0: the Aurora Privileged Extension (`com.aurora.services`) does not always land as a system priv-app on every device, which means users will see the standard Android installer prompt for each Aurora Store install instead of the silent install path through Aurora Services. Aurora Store itself works fine. A targeted fix is the headline feature of v2.1.0.

Coming in upcoming releases:

- v2.1.0: rework Aurora Services staging logic to land as `system_only_enabled` on every supported device, with a dedicated post boot self heal pass that detects `system_with_data_update_enabled`, `system_with_data_update_disabled`, and `data_only` states and remediates each.
- v2.2.0: bundle a Play Integrity Fix so apps requiring attestation (banking apps, Pokemon Go, some streaming services) pass without a separate module flash. SafetyNet self check inside microG will go green.

Diagnostics live at `/data/adb/modules/dresosmicrog/logs/` and runtime state under `/data/adb/dresosmicrog/`. The Action button on the module in the Magisk app reprints the full status dashboard, regrants runtime permissions, and restarts the microG components.

---

### Magisk Modules Roadmap

The DresOS microG module (`dresosmicrog`) has moved from "In active development" to "Released v2.0.0". The next module in active development is the Permissions Hardener (`dresosperms`).

The full roadmap status:

- dresoswv: AOSmium WebView - Released v2.1.0
- dresosmicrog: DresOS microG - Released v2.0.0
- dresosperms: Dangerous permission revocation from system apps - In active development
- dresosdebloat: Core Google app and system bloat removal - Planned
- dresosafwall: AFWall+ pre-configured iptables rules - Planned
- dresosoverlay: System-level telemetry and advertising ID disabling - Planned
- dresosfossify: Fossify suite system app installer - Planned
- dresosheliboard: HeliBoard default keyboard installer - Planned

---

## May 15, 2026 Update

---

### WebView - DresOS AOSmium WebView Module Rewritten to v2.1.0

The AOSmium WebView Magisk module has been rewritten end to end and released as v2.1.0. The previous v1.0.0 bootlooped Pixel 9 and several LineageOS builds, and the v1.2.x intermediates flashed cleanly but never actually activated AOSmium. v2.1.0 fixes all of this with a redesigned activation pipeline and two layers of bootloop safety.

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

### Magisk Modules Roadmap

The MicroG module (`dresosmicrog`) has moved from "Planned" to "In active development" as the next module to ship now that AOSmium WebView is stable. It will replace Google Play Services with MicroG for app compatibility without Google's tracking infrastructure, in a single Magisk flash. Updates will be posted in the Updates tab on the DresOS website.

The full roadmap status:

- dresoswv: AOSmium WebView - Released v2.1.0
- dresosmicrog: Noogle microG installer - In active development
- dresosdebloat: Core Google app and system bloat removal - Planned
- dresosperms: Dangerous permission revocation from system apps - Planned
- dresosafwall: AFWall+ pre-configured iptables rules - Planned
- dresosoverlay: System-level telemetry and advertising ID disabling - Planned
- dresosfossify: Fossify suite system app installer - Planned
- dresosheliboard: HeliBoard default keyboard installer - Planned

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
---

### Magisk Modules Roadmap

The following modules are planned to automate further steps of the DresOS build:

- dresosmicrog: Noogle microG installer
- dresosdebloat: Core Google app and system bloat removal
- dresosperms: Dangerous permission revocation from system apps
- dresosafwall: AFWall+ pre-configured iptables rules
- dresosoverlay: System-level telemetry and advertising ID disabling
- dresosfossify: Fossify suite system app installer
- dresosheliboard: HeliBoard default keyboard installer
