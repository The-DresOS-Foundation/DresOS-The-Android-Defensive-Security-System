# DresOS Android Defensive Security System

---

> **Project Goal:** Turn any Android phone into a fully private, hardened, FOSS only device with no Google tracking, no Google Services, no hidden data harvesting, and only open source replacements then layer on a full defensive security stack to protect against surveillance, malware, censorship, and network level attacks.

> **See it running:** Watch the full DresOS Android 16 demo on a Motorola Moto g32, every layer active, on [Ko-fi](https://ko-fi.com/post/DresOS-Android-16-Motorola-Moto-g32-Video-Demo-F5R420TKRN) or directly at [streamable.com/rg3tsm](https://streamable.com/rg3tsm).

> **Why:** Because of all the restrictions Google is bringing into place over the open source communities, to bring you back your privacy, to stop your data being sold, and to give you full operational security on a device you actually control.

---

## EXTREME WARNING

Rooting and flashing can **brick** your device, void your warranty, trip Knox/SafetyNet, and wipe all data.
**Back up everything** (photos, apps, files) before starting.
This is advanced, if you're not comfortable with ADB/fastboot or device specific guides, stop and use a non root method.
Proceed at your own risk.

**Make a full TWRP backup after rooting** (Boot + System + Data + Vendor) before any debloating.

---

## Table of Contents

### Part 1: DeGoogling (Full Root Method)

* [What You Will Need](#what-you-will-need)
* [Replacement Overview](#replacement-overview)
* [Step 1: Unlock the Bootloader](#step-1-unlock-the-bootloader)
* [Step 2: Install TWRP (Custom Recovery)](#step-2-install-twrp-custom-recovery)
* [Step 3: Install Magisk (Root)](#step-3-install-magisk-root)
* [Step 4: Install Core FOSS Apps](#step-4-install-core-foss-apps)
* [Step 5: Install Magisk Modules](#step-5-install-magisk-modules)
* [Step 6: Set Up DresOS microG (Replaces Google Services)](#step-6-set-up-dresos-microg-replaces-google-services)
* [Step 7: Install DresOS WebView as System WebView](#step-7-install-dresos-webview-as-system-webview)
* [Step 8: Set Up Shizuku](#step-8-set-up-shizuku)
* [Step 9: Fine Tune Debloat & Cleanup](#step-9-fine-tune-debloat--cleanup)
* [Step 10: Replace System File Manager, Package Installer & Download Manager](#step-10-replace-system-file-manager-package-installer--download-manager)
* [Step 11: Final Setup & Defaults](#step-11-final-setup--defaults)

### Part 2: Operational Security (OPSEC Stack)

* [OPSEC Overview](#opsec-overview)
* [OPSEC Step 1: Configure DNS and MAC Randomization](#opsec-step-1-configure-dns-and-mac-randomization)
* [OPSEC Step 2: Set Up InviZible Pro](#opsec-step-2-set-up-invizible-pro)
* [OPSEC Step 3: OONI Probe Network Censorship Detector](#opsec-step-3-ooni-probe-network-censorship-detector)
* [OPSEC Step 4: Install AFWall+ Root Level Firewall](#opsec-step-4-install-afwall-root-level-firewall)
* [OPSEC Step 5: Set Up The Tuta & DuckDuckGo Suite](#opsec-step-5-set-up-the-tuta--duckduckgo-suite)
* [OPSEC Step 6: Set up DresSecureComms](#opsec-step-6-set-up-dressecurecomms)
* [OPSEC Step 7: Final Checks and Hardening Tips](#opsec-step-7-final-checks-and-hardening-tips)

### Part 3: Fossify Suite

* [Apps in the Suite](#apps-in-the-suite)

### Part 4: Additional Apps and Core Components

* [Apps Overview](#apps-overview)
* [1. RedReader, Private Reddit Client](#1-redreader-private-reddit-client)
* [2. Hypatia, Open Source Antivirus](#2-hypatia-open-source-antivirus)
* [3. Metrolist, Private YouTube Music Client](#3-metrolist-private-youtube-music-client)
* [4. Arcane Chat, Decentralized Encrypted Messaging](#4-arcane-chat-decentralized-encrypted-messaging)
* [5. HeliBoard, Offline Keyboard](#5-heliboard-offline-keyboard)
* [6. Aves Libre, Gallery and Photo Manager](#6-aves-libre-gallery-and-photo-manager)
* [7. Stratum, 2FA Authenticator](#7-stratum-2fa-authenticator)

#### Confirmed working devices:

**DresOS is built for universal compatibility: the degoogling and OPSEC steps work on any rooted Android 10 or newer device, and the Magisk modules are written to run on any ABI and any OEM partition layout. The combinations below are the ones confirmed working end to end so far**

| Device | Android version |
| --- | --- |
| Motorola Moto G32 | LineageOS Android 15 and 16 |
| Motorola ThinkPhone | Stock Android 15 |
| Motorola Moto G7 Plus | Stock Android 10 |
| Motorola Moto G7 Plus | LineageOS Android 15 |
| Samsung Galaxy A05s | Stock Android 10 |
| Samsung Galaxy A02s | Stock Android 12 |

If you run DresOS on a device that is not listed, a confirmation report is welcome so the list can grow.

* [License](#license)

---

## Part 1: DeGoogling (Full Root Method)

### What You Will Need

Download **only** from these official links (2026). Install **F-Droid** first, it is the backbone of your FOSS ecosystem.

| Tool | Purpose | Link |
| --- | --- | --- |
| **F-Droid** | Main FOSS app store | [Download APK](https://f-droid.org/F-Droid.apk) / [Site](https://f-droid.org/) |
| **Inure App Manager** | Debloat + app management | [GitHub Releases](https://github.com/Hamza417/Inure/releases) |
| **Shizuku** | Root like advanced system permissions | [GitHub Releases](https://github.com/RikkaApps/Shizuku/releases) |
| **TWRP** | Custom recovery (full backups + flashing) | [twrp.me](https://twrp.me/) search your exact device model |
| **Magisk** | Root + module framework | [GitHub Releases](https://github.com/topjohnwu/Magisk/releases) |
| **DresOS microG Module** | Magisk module v3.1.2, universal systemless microG suite. Pure file overlay: no Zygisk payload, no Xposed, no boot scripts, so it cannot bootloop the device. Signature spoofing is provided by the ROM because the microG APKs carry the official microG key. | [DresOS Magisk Modules releases](https://github.com/The-DresOS-Foundation/DresOS-Magisk-Modules/releases/tag/microg-v3.1.2) |
| **DresOS WebView Module** | Magisk module v2.2.0, systemless replacement of Android System WebView with DresOS WebView via static RRO plus bind mount, activated by cmd webviewupdate after boot, with bootloop sentinel and inert mode fallback | [DresOS Magisk Modules releases](https://github.com/DresOperatingSystems/DresOS-Magisk-Modules/releases/download/webview-v2.2.0/DresOS-WebView-v2_2_0.zip) |
| **AFWall+** | Root level iptables firewall, per app kernel level rules, mobile data proxy redirect, no VPN slot | [F-Droid](https://f-droid.org/packages/dev.ukanth.ufirewall/) / [GitHub](https://github.com/ukanth/afwall) |
| **SD Maid SE** | Deep system cleaner + remnant finder | [F-Droid](https://f-droid.org/packages/eu.darken.sdmse/) |
| **DresSecureComms** | DresOS secure communications: SMS, dialer, contacts, link scanning, metadata wipe, geo spoofer (replaces Fossify Phone, Messages, Contacts, URL Checker and Fake Traveler) | [GitHub releases](https://github.com/DresOperatingSystems/DresSecureComms/releases) |
| **Fossify Apps** | Launcher, Clock and Notes | [fossify.org/apps](https://www.fossify.org/apps/) (via F-Droid) |
| **Aves Libre** | Gallery, photo and video manager (replaces Fossify Gallery and Google Photos) | [F-Droid](https://f-droid.org/packages/deckers.thibault.aves.libre/) |
| **Amaze File Manager** | File manager with built in AES encryption and root explorer | [F-Droid](https://f-droid.org/packages/com.amaze.filemanager/) |
| **DresOS WebView** | System WebView, bundled inside the DresOS module ZIP | APK source: [https://github.com/The-DresOS-Foundation/DresOS-WebView/releases/tag/v1.0.0](https://github.com/The-DresOS-Foundation/DresOS-WebView/releases/tag/v1.0.0) |
| **InviZible Pro** | Tor + I2P + DNSCrypt in proxy + root mode, IP anonymisation, encrypted DNS, no VPN slot used | [F-Droid](https://f-droid.org/packages/pan.alexander.tordnscrypt.stable/) |
| **DuckDuckGo Privacy Browser** | Primary browser (Chrome replacement) | [F-Droid](https://f-droid.org/packages/com.duckduckgo.mobile.android/) |
| **Aurora Store** | Anonymous Play Store alternative | [F-Droid](https://f-droid.org/packages/com.aurora.store/) |
| **HeliBoard** | Fully offline keyboard (no Gboard) | [F-Droid](https://f-droid.org/packages/helium314.keyboard/) |
| **Tuta Mail** | End to end encrypted email (no Gmail) | [F-Droid](https://f-droid.org/packages/de.tutao.tutanota/) |
| **Tuta Calendar** | End to end encrypted calendar, syncs with your Tuta account (no Google Calendar) | [F-Droid](https://f-droid.org/packages/de.tutao.calendar/) |
| **Stratum** | Offline two factor authenticator (TOTP/HOTP), encrypted backups, no internet, no cloud | [stratumauth.com/download](https://stratumauth.com/download) / [GitHub](https://github.com/stratumauth/app) |
| **Gopeed** | FOSS download manager, HTTP/HTTPS/BitTorrent/magnet, hash verification, no Google components | [F-Droid](https://f-droid.org/packages/com.gopeed/) / [gopeed.com](https://gopeed.com/) |
| **SAI** | Split APK Installer, FOSS package installer | [F-Droid](https://f-droid.org/packages/com.aefyr.sai.fdroid/) |

---

### Replacement Overview

Here is exactly what each core app replaces and why it is better for privacy:

| App | Replaces | Why It's Better |
| --- | --- | --- |
| DresSecureComms | Stock Phone, Dialer, SMS, Contacts | DresOS secure comms app: private SMS with optional encryption, full dialer, caller ID and spam screening, encrypted contacts vault, no Google services and comes with link scanning, file scanning, mock location |
| Amaze File Manager | Google Files / Stock File Manager / ZArchiver Pro | Open source file manager with built in AES 256 encryption, biometric lock, root explorer, and APK management |
| DuckDuckGo Privacy Browser | Chrome | Private search engine, tracker blocking, zero Google telemetry |
| DresOS WebView | Android System WebView (Google) | Blocks ads & trackers in **every** app that loads web content |
| Tuta Mail | Gmail | End to end encrypted email (no Google account or content scanning) |
| Tuta Calendar | Google Calendar | End to end encrypted calendar, syncs across your devices through your Tuta account, zero knowledge |
| HeliBoard | Gboard | Completely offline keyboard, no data sent anywhere, ever |
| Aurora Store | Google Play Store | Anonymous downloads, no Google account required |
| DresOS microG | Google Play Services | Full app compatibility without any Google tracking infrastructure |
| Fossify Launcher / Clock / Notes | Stock Google equivalents | open source system app replacements |
| Aves Libre | Google Photos / Stock Gallery | Open source photo and video gallery, metadata viewer, map and tag organisation, no cloud, no trackers |
| Stratum | Google Authenticator / Authy | Offline open source 2FA, encrypted exportable backups, no cloud lock in, single permission |
| Gopeed | Google Download Manager | Modern FOSS download manager, HTTP/BitTorrent/magnet, hash verification, actively maintained |
| DresOS WebView Module | Manual WebView patching process | Single Magisk flash: places DresOS WebView in systemless tree, ships a static RRO that allowlists it in config_webview_packages, activates via cmd webviewupdate after boot, includes bootloop sentinel |
| InviZible Pro | Orbot + separate DNS/I2P apps | Combines Tor, I2P, and DNSCrypt in proxy + root mode, no VPN slot used; IP spoofing via Android system proxy; iptables DNS redirect at kernel level |
| AFWall+ | No equivalent in stock Android | Root level iptables firewall; per app internet blocking at kernel level; custom rules for mobile data proxy routing; works independently of VPN and proxy layers |

---

### Step 1: Unlock the Bootloader

**Device specific**, required for TWRP + Magisk. Usually wipes all data.

1. Enable **Developer Options**: Settings, About phone, tap **Build number** 7 times.
2. Turn on **OEM unlocking** + **USB debugging**.
3. Install ADB & Fastboot on your PC (Android Platform Tools from Android Developers).
4. Connect phone via USB.
5. Run: `adb reboot bootloader`
6. Run: `fastboot flashing unlock` (or `fastboot oem unlock` on older devices).
7. Confirm on phone screen, **this will factory reset the device**.

**Samsung:** Use Download Mode + Odin with official or unofficial unlock.
**Huawei / carrier locked phones:** Use the official unlock code process for your carrier.
**Exact instructions:** Search `[your exact model] unlock bootloader XDA Developers` and follow **that** device's specific guide.

Phone will factory reset. Set it up again and re enable USB debugging before continuing.

---

### Step 2: Install TWRP (Custom Recovery)

TWRP gives you full device backups and easy module/zip flashing.

1. Go to [twrp.me](https://twrp.me/) and search your **exact** device model and variant.
2. Download the latest `.img` (and `.zip` if available).
3. Follow the **exact instructions** on that device's TWRP page, they are device specific.

Typical commands:

```
adb reboot bootloader
fastboot boot twrp-xxxx.img           # Temporary boot into TWRP
# Inside TWRP: Install, select TWRP .zip to make it permanent
# Or: Advanced, Install Recovery Ramdisk
```

Reboot to recovery to confirm TWRP is permanently installed.

**No TWRP for your device?** Use Magisk direct patching method only (see Step 3 below) or check XDA for unofficial TWRP builds. Search `[model] unofficial TWRP`.

**TWRP Backup (if not done already):** Boot to TWRP, Backup, select Boot + System + Data + Vendor, swipe to back up. This is your emergency restore point. Store the backup folder in a safe location off device.

**If anything breaks:** Boot to TWRP, Restore, select your backup, swipe. Your full system is back in minutes.

---

### Step 3: Install Magisk (Root)

Two methods, pick one based on your situation.

#### Recommended: Magisk Patching Method (works on almost everything)

1. Download the latest Magisk APK from GitHub.
2. Extract your device's `boot.img` (or `init_boot.img`) from the stock firmware ZIP for your exact model and Android version.
3. Install the Magisk app on your phone.
4. Open Magisk app, **Install**, **Select and Patch a File**, choose `boot.img`.
5. `magisk_patched_xxxx.img` will appear in your Downloads folder.
6. Pull it to your PC: `adb pull /sdcard/Download/magisk_patched_xxxx.img`
7. `adb reboot bootloader`
8. `fastboot flash boot magisk_patched_xxxx.img` (or `init_boot` if your device uses it).
9. On some devices you also need to flash a patched `vbmeta.img` with flags: `fastboot flash vbmeta vbmeta.img --disable-verity --disable-verification`
10. Reboot. Open Magisk app, it should show "Installed."

#### Alternative: Via TWRP

1. Rename the Magisk APK file from `.apk` to `.zip`
2. Boot to TWRP, Install, select and flash the `.zip`
3. Reboot system

**Samsung specific:** Use Recovery mode + Odin with a patched AP.tar file.

Reboot, open Magisk app, you are rooted. Keep Magisk app installed, you need it for everything that follows.

---

### Step 4: Install Core FOSS Apps

1. Transfer the **F-Droid APK** to your phone and install it (allow installs from unknown sources when prompted).
2. Open F-Droid and from the search/browse install **all** of these:
   * **Inure App Manager**
   * **Shizuku**
   * **SD Maid SE**
   * **Fossify Launcher, Clock, and Notes**
   * **Aves Libre**
   * **Amaze File Manager**
   * **HeliBoard**
   * **DuckDuckGo Privacy Browser**
   * **Aurora Store**
   * **Tuta Mail**
   * **Tuta Calendar**
   * **Stratum**
   * **Gopeed**
   * **SAI (Split APK Installer)**
3. Install **DresSecureComms** from its [GitHub releases](https://github.com/DresOperatingSystems/DresSecureComms/releases). Set it as your default SMS and phone app after first launch.
4. Download the **DresOS WebView module** from [DresOS Magisk Modules releases](https://github.com/DresOperatingSystems/DresOS-Magisk-Modules/releases/latest), you will flash it in Step 5.

Do **not** open or configure these apps yet, follow each step in order.

---

### Step 5: Install Magisk Modules

Open **Magisk app**, **Modules**, **Install from storage** and flash each of these one at a time, rebooting between them if instructed:

1. **DresOS microG Module v3.1.2** (`dresosmicrog`), universal systemless microG suite, single flash. Ships the officially signed microG GmsCore 0.3.15, Companion (FakeStore), GsfProxy, DroidGuard Helper, Aurora Store, and Aurora Services as privileged system apps under product, with a privapp permissions allowlist generated from the bundled manifests. It is a pure file overlay: no Zygisk payload, no Xposed, no runtime PackageManager work, and it never touches other modules, so it cannot bootloop the device and coexists cleanly with the DresOS WebView module. Signature spoofing is provided by the ROM: because the microG APKs carry the official microG key, any ROM with microG signature spoofing support spoofs them automatically once placed in priv app.

   * Full details in Step 6.
2. **DresOS WebView Module v2.2.0** (`dresoswv`), systemless replacement of Android System WebView with DresOS WebView. Drops the signed DresOS WebView APK into the systemless tree at `system/product/app/DresOSWebView/`, ships a static RRO that adds `org.dresos.webview` plus the DresOS signing certificate to the framework `config_webview_packages` allowlist, and activates DresOS WebView via `cmd webviewupdate set-webview-implementation` after boot complete. Updating over the AOSmium build swaps it out in place. Includes a post fs data bootloop sentinel and an inert mode fallback so a failed activation cannot bootloop the device.
   * Handles the entire WebView switch in one flash. Full details in Step 7.

Reboot after flashing both modules above.

---

### Step 6: Set Up DresOS microG (Replaces Google Services)

The DresOS microG module installs a clean, privacy respecting replacement for Google Play Services as systemless privileged apps. Apps that require Play Services (push notifications, maps, logins, app compatibility) will work through microG with zero Google data harvesting.

What it stages:

- **microG GmsCore** (`com.google.android.gms`) 0.3.15.250932, the Play Services replacement core
- **microG Companion** (`com.android.vending`), FakeStore stub so apps that check for Play Store presence see something legitimate
- **microG GsfProxy** (`com.google.android.gsf`), Google Services Framework proxy
- **microG DroidGuard Helper** (`org.microg.gms.droidguard`), DroidGuard runtime stub
- **Aurora Store** (`com.aurora.store`), anonymous Play Store alternative
- **Aurora Services** (`com.aurora.services`), Aurora privileged extension for silent, prompt free app installs

Signature spoofing is provided by the ROM, not by the module. Because the bundled microG APKs are signed with the official microG key, any ROM that supports microG signature spoofing (LineageOS from 2024 02 26 onward, /e/OS, CalyxOS, iodeOS, DivestOS and similar) activates spoofing automatically once the apps are placed in priv app. There is no Zygisk hook and no Xposed or LSPosed framework involved on any ABI.

#### Activation

1. Open **Magisk app**, **Modules**. Confirm `DresOS microG` is listed and enabled.
2. Tap the **Action** button on the DresOS microG entry. This prints the status dashboard, regrants runtime permissions to GmsCore, Companion, GSF, and DroidGuard, and restarts the microG components. Grant root when prompted.
3. Open the **microG Settings** app from your app drawer.
4. Go to **Self Check**. The signature spoofing item should read green because the ROM is spoofing the officially signed microG. Every other item should tick, except SafetyNet and Play Integrity, which are planned for a later release.
5. If signature spoofing reads as failing, reboot once more and re check.

#### Logs and diagnostics

- `/data/adb/modules/dresosmicrog/logs/install.log`
- `/data/adb/modules/dresosmicrog/logs/boot.log`
- `/data/adb/modules/dresosmicrog/logs/service.log`
- `/data/adb/dresosmicrog/` (state dir, decision records, bootloop counter)

#### On ROMs that already ship microG

LineageOS for microG, CalyxOS, iodeOS, /e/OS already provide native signature spoofing and may ship microG built in. The installer detects an already present, upstream signed microG by reading the X.509 certificate SHA 256 via `cmd package dump` and leaves the ROM's microG in place, staging only the Aurora components on those ROMs so there is no duplicate.

GrapheneOS deliberately blocks native signature spoofing and ships its own Sandboxed Google Play. The module detects GrapheneOS and aborts the install.

#### Aurora Store an Anonymous App Store

Aurora Store is an open source, unofficial Google Play Store client that lets you browse, download, and update apps from Google's Play catalogue **without a Google account** and without any Google Play Services telemetry. It is a full Google Play replacement with complete anonymity.

**Source Code:** no need for download link as the module covers that [https://github.com/whyorean/AuroraStore](https://github.com/whyorean/AuroraStore)

Open Aurora Store once the module is active. Sign in with **Anonymous** (no Google account). You can now install apps from the Play Store catalogue without a Google account.

**Aurora Services:** v3.0.0 stages the Aurora privileged extension (`com.aurora.services`) as a system priv app with its own permissions allowlist, so silent prompt free installs through Aurora Store work without the standard Android installer prompt appearing for every app.

#### Key Features

* **Anonymous mode**, browse and download apps using randomly generated anonymous credentials. Google sees an anonymous device, not your account.
* No Google account login required
* Full Play Store catalogue access, any app available on Google Play can be downloaded
* App update management, check for updates for all Play sourced apps
* **Exodus Privacy integration**, shows you a detailed tracker and permission report for every app before you install it
* Block automatic app updates for specific apps
* Spoofed device profile, can present as a different Android device to access region locked apps
* No ads, no telemetry, zero Google components in the Aurora app itself

#### Setup and Usage

1. Installed via our Microg module.
2. Open Aurora Store, select **Anonymous** mode during setup. **Do not log in with a Google account.**
3. Browse, search, and install apps exactly as you would on the Play Store.
4. **Before installing any app, check its tracker report:**
   * Tap the app, scroll down to **Privacy**, check the Exodus Privacy report
   * Apps with many trackers (advertising, analytics, fingerprinting) should be avoided or replaced with FOSS alternatives
5. **Manage updates:**
   * Aurora Store, Updates tab, review available updates
   * Update FOSS apps via F-Droid instead (F-Droid verifies signatures; Aurora Store does not independently verify)

---

### Step 7: Install DresOS WebView as System WebView

#### What is the System WebView?

The **Android System WebView** is a browser engine baked into Android itself. It is not a browser you open it powers web content rendering inside hundreds of apps: social media feeds, email clients, news apps, microG sign in screens, shopping apps, and more. Every time any of those apps display a webpage internally, the System WebView is doing the rendering. Replacing Google's default with DresOS WebView means all of that content runs through a privacy hardened, Google free engine rather than Google's WebView.

#### Why It's in the DresOS Suite

**DresOS WebView** (built by DresOperatingSystems from [Cromite](https://github.com/uazo/cromite)) is a Chromium engine carrying Cromite's privacy and security hardening, with Google services and telemetry stripped throughout.

**Source Code** [https://github.com/The-DresOS-Foundation/DresOS-WebView](https://github.com/The-DresOS-Foundation/DresOS-WebView)

#### What It Is

DresOS WebView, built by DresOperatingSystems from Cromite, is a Chromium engine carrying Cromite's privacy and security hardening throughout. The Android System WebView is the rendering engine hundreds of apps use internally any time they show web content, from social feeds to sign in screens to in app articles. By default that engine is Google's, which phones home on every render. DresOS WebView replaces it system wide, so every app that draws web content does so through a Google free, privacy hardened engine. It is invisible: you never open DresOS WebView directly, and your actual browser is DuckDuckGo.

#### Key Features

* Chromium engine built from Cromite with its privacy and security hardening throughout
* Replaces Google's Android System WebView for every app on the device at once
* No Google services, no telemetry, no anti features
* Installed systemlessly through the DresOS WebView Module, so it is bootloop safe and fully reversible

Your everyday browser is **DuckDuckGo Privacy Browser**. DresOS WebView runs silently in the background as the engine all other apps use to render web content.

#### Setup and Usage

DresOS WebView is not installed by hand. The DresOS WebView Module v2.2.0 you flash drops the signed APK into the systemless tree, registers it through a static RRO, and promotes it to the active WebView provider with `cmd webviewupdate set-webview-implementation` after boot. Full flashing and verification steps, including the bootloop sentinel and inert mode fallback. Confirm it is active with `adb shell dumpsys webviewupdate | grep "Current WebView package"`, which should report `org.dresos.webview`.

---

#### DresOS WebView Module v2.2.0

**What the module does in one flash:**

1. Validates the host environment: Magisk 29.0 or newer, Android 10 through 16, arm64. Aborts cleanly on 32 bit arm and on x86/x86_64 (DresOS WebView is currently built for arm64 only), on unknown ABIs, and on devices that ship WebView as an APEX module.
2. Drops the signed DresOS WebView APK into the systemless tree at `system/product/app/DresOSWebView/DresOSWebView.apk`. Magisk magic mount makes this visible to PackageManager as a preinstalled system app, satisfying the framework `MATCH_FACTORY_ONLY` scan that `WebViewUpdateService` performs.
3. Places a static RRO in the systemless overlay partition. The RRO adds `org.dresos.webview` plus the full DresOS signing certificate to the framework `config_webview_packages` resource, which is the canonical allowlist that `WebViewUpdateService` reads at boot. Both `com.google.android.webview` and `com.android.webview` are kept in the allowlist as fallbacks so removing DresOS WebView cannot leave the device without a valid provider.
4. After `sys.boot_completed`, `service.sh` calls `cmd webviewupdate set-webview-implementation org.dresos.webview` to promote DresOS WebView to the active provider, with a `settings put global webview_provider` fallback write for redundancy.
5. Activation is verified by re reading `dumpsys webviewupdate`. If the active provider is not DresOS WebView, the module flips itself into inert mode and stays out of the way until the user investigates.

**Bootloop safety:**

The module ships with two layers of protection so a failed activation cannot brick the device.

- **post fs data sentinel:** drops a `boot_pending` marker on every boot which `service.sh` clears on successful activation. A stale marker on the next boot signals a previous crash and the module auto disables itself by touching `/data/adb/modules/dresoswv/disable`. Magisk recognises this file and skips the module entirely on subsequent boots.
- **Inert mode flag:** set automatically by `service.sh` on any activation failure (RRO did not register, APK bind mount did not land, framework refused to select DresOS WebView). The module's files remain bind mounted by Magisk but no further activation is attempted on subsequent boots, preventing retry storms.

**Package details:**
- Package: `org.dresos.webview`
- Engine: Chromium `145.0.7632.120` (Cromite build)
- Architecture: `arm64 v8a`
- Signing cert SHA 256: `B7815F746C3183C66CAD631079CB669C0494A8BE542A2A96E5975D8FABCD92DE` (DresOS RSA 4096)

#### Flash Instructions

1. Download `DresOS WebView v2_2_0.zip` from the releases link above
2. Open **Magisk, Modules, Install from storage**
3. Select the ZIP and wait for installation to complete
4. Tap **Reboot**

#### After Reboot

No manual step is required. The module activates DresOS WebView automatically after boot complete. To verify activation, from adb:

```
adb shell dumpsys webviewupdate | grep "Current WebView package"
```

Expected output:

```
Current WebView package (name, version): (org.dresos.webview, 145.0.7632.120)
```

If the output shows the OEM provider instead of DresOS WebView, check the activation log to diagnose:

```
adb shell cat /data/adb/modules/dresoswv/webview_activation.log
```

The Developer Options WebView picker may still show the OEM provider as selected on some OEM ROMs even when DresOS WebView is actually active. Trust `dumpsys` over the UI.

#### Diagnostic Logs

If something goes wrong check these files via adb or a terminal app:

```
/data/adb/modules/dresoswv/logs/install.log
/data/adb/modules/dresoswv/logs/boot.log
/data/adb/modules/dresoswv/logs/service.log
/data/adb/modules/dresoswv/webview_activation.log
```

#### Uninstalling

Disable or remove the module in **Magisk, Modules** and reboot. The `uninstall.sh` script clears the framework's saved WebView provider selection and restores either `com.google.android.webview` (on GMS devices) or `com.android.webview` (on AOSP/LineageOS builds) as the active provider.

---

### Step 8: Set Up Shizuku

Shizuku gives apps like **Inure App Manager** powerful system level access, specifically the ability to deeply debloat and manage system apps, without requiring every individual operation to go through a full root prompt. It is safer, more granular, and lighter than running everything root, and it works perfectly alongside your existing Magisk root.

#### Why Use Shizuku?

* Lets Inure and other tools debloat safely and deeply at the system level.
* Survives reboots when configured correctly.
* Uses Android's ADB equivalent permission layer, no need to expose full root for every operation.
* Works with several other FOSS tools that support it.

#### Step by Step Root Setup (Recommended As Device Already Rooted)

1. Open the **Shizuku** app.
2. Click start root.
3. When Magisk prompts for root permission, grant it to Shizuku **permanently**.
4. You will briefly see a black screen with white text confirming the service has started.
5. Go to **Shizuku settings** and enable **Auto start on boot**.

#### Grant Permissions to Apps

* When you open **Inure App Manager** for the first time it will automatically request Shizuku access, approve it and grant **all** requested permissions.
* You can view and manage all apps that have been granted Shizuku access under Shizuku via **Authorized apps**.

**Test:** Reboot your phone. Open Shizuku, it should auto start and show "Running" with root status. If it shows "Not running," open Shizuku and tap Start again.

You are now ready for the deepest, safest debloating possible.

---

### Step 9: Fine Tune Debloat & Cleanup

Now we remove every trace of Google and system bloat safely.

#### 9.1 Inure App Manager (Recommended Bulk Removal)

1. Open **Inure App Manager**.
2. Grant it **full access**, choose **Root** and **Shizuku**. Approve **everything** it asks for.
3. Go to the **Debloat** tab.
4. Tap the **select all** icon.
5. Select the **Recommended** debloat list.
6. This automatically selects everything your device does not need: Google bloat, carrier apps, unused system services, etc.
7. Review the list carefully. Uncheck anything you want to keep. Then tap **Debloat** then **uninstall** to remove everything.
8. Restart the phone.

#### 9.2 (Fine Tuning Pass)

Only do this if you have installed the replacement apps

1. Reopen **inure**
2. Go back to the Debloat tab.
3. Remove **all** remaining Google system apps, including:
   * Gmail
   * Google Search
   * Chrome
   * Google Play Services remnants (anything not replaced by microG)
   * Stock Apps (phone, messages, clock, gallery, camera etc)
   * Any other carrier or OEM bloat that is safe to remove (inure will explain what the proccess/app is for and if its safe or not to remove and will warn you if removal causes bootloop)
4. Apply changes and reboot.

> **Note:** If Inure looks like it is doing nothing after you tap Debloat, leave it running. It is digging through your system to safely remove everything. This can take several minutes. Remember every device is different so be careful when fine tuning.

#### CRITICAL WARNING

You **must** have installed all replacement apps **before** removing their Google equivalents. Specifically:
* Install **Tuta Mail** before removing Gmail
* Install **DresSecureComms** and set it as the default phone and SMS app before removing the stock dialer and SMS app
* Install **DresOS WebView** (via the DresOS module) before removing Chrome and Google WebView
* Install **Gopeed** before removing Google Download Manager
* Install **SAI** before removing the Google Package Installer

Removing a system app without a replacement can break core phone functions until you restore a backup.

#### 9.3 Final Cleanup with SD Maid SE

Open **SD Maid SE**, run **Full Scan** + **CorpseFinder**, delete every Google remnant, leftover data folder, and orphaned system file.

---

### Step 10: Replace System File Manager, Package Installer & Download Manager

A truly de Googled phone requires replacing not just apps but the system level utilities Google embeds for file management, app installation, and downloading. Here is the full FOSS replacement stack, zero Google components:

#### File Management and Encryption: Amaze File Manager

**Download:** [F-Droid](https://f-droid.org/packages/com.amaze.filemanager/)

Amaze File Manager is a fully open source (GPL 3.0) file manager built by Team Amaze. It replaces both a standard file manager and a separate encryption tool in a single app. Available on F-Droid with no Google dependencies, actively maintained, and supports Android 5.0 and up.

#### Key Features

- Full file management: cut, copy, move, delete, rename, compress, extract
- Built in AES 256 encryption and decryption of files and folders
- Biometric lock (fingerprint and face unlock) for the entire app
- Multiple tabs open simultaneously
- Root explorer for advanced system access on rooted devices
- Built in APK reader, installer, and App Manager
- Built in ZIP/RAR reader, text editor, and database viewer
- SMB, SSH, FTP and SFTP support for network shares
- No ads, no trackers, no Google dependencies

#### Setup and Usage

1. Install via F-Droid.
2. Open Amaze File Manager and grant storage permission.
3. Set as default file manager: Settings, Apps, Default apps, Files, Amaze.

Key features relevant to the DresOS build:

**File management:** Full local file browsing across internal storage, SD card, and USB OTG. Cut, copy, move, delete, rename, create folders. Multiple tabs open simultaneously. Bookmark frequently used directories. Built in text editor, ZIP/RAR reader, APK reader, and database reader.

**AES 256 encryption:** Amaze has built in AES encryption for files and folders, no separate archiving app needed. To encrypt a file: Press the options menu on the right hand side of the file, select Encrypt, set a password. The encrypted file gets an `.aes` extension. To decrypt: tap the `.aes` file, enter the password. Encrypted files cannot be read without the password even with full physical access to the device.

**Biometric lock:** Lock the entire app behind fingerprint or face unlock via Settings, Security, Fingerprint. This prevents anyone who picks up your phone from accessing your files.

**Root explorer:** On your rooted device, enable root mode in Settings, Root Explorer. This lets you browse and manage system directories directly, useful for advanced DresOS maintenance.

**APK management:** Long press any APK file to install it directly, or use the built in App Manager to backup, view, or uninstall installed apps.

Set **Amaze File Manager** as your default file manager in Settings, Apps, Default apps, Files.

#### Package Installer: SAI (Split APK Installer)

**SAI** (Split APK Installer) is a fully open source replacement for Google's PackageInstaller. It supports:

* Standard APK installation
* Split APK packages (.apks / .xapk files), the format many apps use
* APK signature verification before installing
* Batch installation from ZIP archives
* Root and Shizuku installation modes for seamless installs without confirmation prompts
* Zero Google components, zero network calls home

Install from F-Droid: `https://f-droid.org/packages/com.aefyr.sai.fdroid/`

Set SAI as the default app opener for `.apk` files in Settings, Apps, Default apps, Opening links, or by long pressing an APK in Amaze File Manager and selecting SAI.

#### Download Manager: Gopeed

**Gopeed** is a modern, actively maintained open source download manager that replaces Google's hidden Download Manager service entirely. It is under active development with regular releases and supports a broader range of protocols than older Android download managers.

Install from F-Droid: `https://f-droid.org/packages/com.gopeed/`
Or download from [gopeed.com](https://gopeed.com/)

Key features:
- HTTP, HTTPS, BitTorrent, magnet link, and ed2k support
- Multi threaded downloads with resumable transfers
- File hash verification (SHA 256/MD5) to confirm download integrity
- Wi Fi only mode, restrict downloads to unmetered connections
- Clean modern interface with download queue management
- Zero Google dependencies, zero tracking

After installing, go to **Settings, Apps, Default apps, Opening links** and ensure Gopeed handles download intents. In **DuckDuckGo Privacy Browser**, set Gopeed as the external download handler if prompted.

> **Why This Matters:** Google's Download Manager is a background service that silently handles every file download on the phone and can log metadata. Gopeed replaces it entirely with a transparent, open source alternative that is actively maintained.

#### APK Manager: F-Droid + Aurora Store

For sourcing and updating apps:

* **F-Droid** handles all open source apps from its repository and any added repos (like IzzyOnDroid for DresSecureComms). F-Droid verifies all APK signatures against developer keys.
* **Aurora Store** provides anonymous access to the Google Play Store catalogue without a Google account. Use **Anonymous** login mode only.

Together, these two replace the Google Play Store ecosystem completely.

---

### Step 11: Final Setup & Defaults

Go to **Settings, Apps, Default apps** and set:

| Function | Default App |
| --- | --- |
| Browser | **DuckDuckGo Privacy Browser** |
| Gallery | **Aves Libre** |
| Keyboard / Input method | **HeliBoard** |
| Home / Launcher | **Fossify Launcher** |
| Phone app | **DresSecureComms** |
| SMS / Messages | **DresSecureComms** |
| Email | **Tuta Mail** (also set for `mailto:` links) |
| File manager | **Amaze File Manager** |
| Package installer | **SAI** |
| Download manager | **Gopeed** |

**DuckDuckGo Privacy Browser:** Open it and complete initial setup. **App Tracking Protection can and should be enabled**, InviZible Pro runs in proxy mode and does not occupy the Android VPN slot, so there is no conflict. Enable it in DuckDuckGo, Settings, App Tracking Protection, Enable. Android will prompt you to allow a VPN connection for DuckDuckGo, confirm it. This gives you both InviZible Pro's proxy level Tor/DNSCrypt routing AND DuckDuckGo's in app tracker blocking running simultaneously.

**Optional hardening (advanced):**
* **Stratum**, set up your two factor authentication accounts now; it works fully offline and you can block it from the network in AFWall+
* **Hide My Applist**, prevents apps from detecting other apps installed on your phone
* **Play Integrity**, microG passing SafetyNet/Play Integrity for banking and similar apps is planned for a later DresOS microG release rather than the current pure overlay build

---

## Part 2: Operational Security (OPSEC Stack)

### OPSEC Overview

This repository provides a guide to enhancing operational security (OPSEC) and cybersecurity on Android devices. It focuses on protecting against phishing, malware, IP tracking, data mining, device fingerprinting, censorship detection, and network surveillance, using free, open source tools only.

This method creates a layered privacy setup: encrypted DNS at the OS level (Quad9 DNS over TLS), Tor routing + DNSCrypt + I2P at the network level via InviZible Pro in proxy + root mode (which leaves the VPN slot free), a root level iptables firewall via AFWall+ enforcing per app access control at the kernel level, app layer tracker blocking via DuckDuckGo App Tracking Protection in the VPN slot, and location spoofing at the sensor level via Fake Traveler. The result is an extremely hardened device with multiple independent, non conflicting privacy layers operating simultaneously.

---

### OPSEC Step 1: Configure DNS and MAC Randomization

This is the baseline privacy layer, applied at the OS level, before any apps run.

#### DNS over TLS (Quad9)

1. Go to **Settings, Network & Internet, Private DNS**.
2. Select **Private DNS provider hostname**.
3. Enter: `dns.quad9.net`
4. Save.

**What this does:** Quad9 DNS blocks malicious domains, protects against malware and phishing at the DNS resolution level, and does not log or collect user data, unlike Google DNS (8.8.8.8) or Cloudflare DNS (1.1.1.1) which both log queries. Quad9 is operated by a Swiss nonprofit. All DNS queries are now encrypted via DNS over TLS and filtered against Quad9's threat intelligence database.

> Note: Once InviZible Pro is running in proxy mode (Step 2), it takes over DNS resolution via DNSCrypt, which is more private than system level DNS over TLS. The Quad9 setting here serves as a fallback if InviZible Pro fails.

#### MAC Address Randomization

1. Go to **Settings, Network & Internet, Internet**, tap your Wi Fi network.
2. Tap **Network usage**, set to **Treat as unmetered** (reduces certain monitoring behaviors).
3. Go to **Privacy** (within the same network settings).
4. Enable **Use randomized MAC address**.
5. Disable **Send device name**.
6. Go to **Developer options**:
7. Enable **Non persistent MAC** (complements the randomization above across reboots)
8. Enable **Tethering hardware acceleration** (improves performance when hotspot is used)

**What this does:** Every Wi Fi device has a hardware MAC address that uniquely identifies it. By default, this allows any network operator to track your device across time even if you change your IP. Randomized MAC changes your device's hardware identifier on each network, preventing this form of persistent tracking. Your device might appear as a completely random hardware identifier, preventing physical location tracking via Wi Fi network logs.

---

### OPSEC Step 2: Set Up InviZible Pro

**InviZiblePro** is the network privacy engine of the DresOS stack. It combines **Tor**, **I2P**, and **DNSCrypt** into a single application. In this setup it runs in **proxy mode with root**, which means:

* It runs as a local proxy server on your device, the **Android VPN slot is not used at all**
* DuckDuckGo's **App Tracking Protection** can be fully enabled alongside it with zero conflict
* Magisk root lets InviZible Pro redirect DNS at the iptables level without any VPN interface
* Android's built in system proxy setting routes your traffic through InviZible Pro's Tor HTTP proxy, your apparent IP address to every website and service becomes a Tor exit node IP, not your real IP

---

#### What Each Component Does

**DNSCrypt:**
DNS converts domain names like `example.com` into IP addresses. By default this happens in plain text, your ISP, network operator, and any middlebox on the network can see every domain you visit, even when page content is HTTPS encrypted. DNSCrypt encrypts and cryptographically authenticates every DNS query. In proxy + root mode, InviZible Pro uses iptables to redirect all DNS traffic (UDP and TCP port 53) to its local DNSCrypt listener at port 5354, no app can bypass it, no VPN is needed.

**Tor (The Onion Router):**
Tor routes your traffic through three volunteer operated relays. Each relay only knows the immediately previous and next hop, no single relay can identify both your identity and your destination. InviZible Pro exposes Tor as a local HTTP proxy on port `8118`. When you set Android's system proxy to `127.0.0.1:8118`, all apps that respect the system proxy route their traffic through Tor. From the perspective of every website and service you connect to, your IP address is the Tor exit node's IP, a relay in another country, different from your real IP, rotating every 10 minutes.

**I2P (Invisible Internet Project):**
A separate anonymising network using garlic routing (bundles multiple encrypted messages per hop). Independent of Tor, runs on its own relay network. Suited for accessing `.i2p` hidden services (eepsites) and provides a different anonymity model from Tor that resists certain traffic correlation attacks differently. InviZible Pro exposes the I2P HTTP proxy locally on port `4444`.

**Root Mode / iptables DNS Redirection:**
With Magisk root, InviZible Pro writes iptables rules at the kernel level to redirect all outbound DNS queries (port 53 to port 5354) to DNSCrypt, regardless of which app generates them. No VPN slot needed. This runs below the application layer: it cannot be bypassed by any app.

* **Proxy mode**, exposes Tor as HTTP proxy on `127.0.0.1:8118` and SOCKS5 on `127.0.0.1:9050`; Android system proxy routes all compatible app traffic through Tor without touching the VPN slot. **Works alongside DuckDuckGo App Tracking Protection (VPN slot is completely free).**

---

#### Full Setup: Proxy + Root Mode with Tor + I2P + DNSCrypt

Follow these steps in order. Do not start any service until Phase 6.

##### Phase 1: Install and Open

1. Install InviZible Pro via F-Droid or the direct APK link above (install with SAI).
2. Open **InviZible Pro**.
3. Allow notification permissions when prompted.
4. Do not tap Start on anything yet.

##### Phase 2: Set Connection Mode to Proxy + Enable Root

1. Tap the **menu**, top right corner, **Settings**, **Common settings**.
2. Select **Proxy mode** (not VPN mode, not Root only mode).
3. Enable InviZble Pro in Magisk
4. Under settings in the top left:
   * Confirm **SOCKS5 proxy port:** `9050` (Tor SOCKS5)
   * Confirm **HTTP proxy port:** `8118` (Tor HTTP CONNECT proxy, this is what you will enter into Android's Wi Fi proxy setting)
5. Go to fast settings Enable All, restores all services and iptables rules automatically after every reboot.
6. Go back to the main screen.

##### Phase 3: Configure DNSCrypt

1. Tap the **settings gear** icon.
2. In **DNS settings**, enable all of these:
   * `suspicious logging`, can reveal presence of malware etc
   * `require no log`, server does not share data with third parties
   * `require dnssec`, server validates DNS responses cryptographically
3. **Block IPv6**, reduces attack surface if you do not actively use IPv6 services
4. Click **Bootstrap resolvers**, allows initial encrypted resolver list fetch; only contacts a plain resolver once for bootstrapping, then stays encrypted
5. Go back.

##### Phase 4: Configure Tor

1. Tap the **settings gear** icon.
2. Under **Tor settings**, enable:
   * **Isolate dest addr**, forces each destination domain to use a separate Tor circuit; prevents one service from correlating your traffic to another
   * **Isolate dest port**, additional circuit isolation per port number
3. Note the HTTP Tunnel port is `8118` (used in Phase 7 below).
4. Go Back
5. Do this if you are in a country where Tor is blocked:
   * Go to fast settings and click Use bridges
   * Tap **Request bridges** click the bridges you want then it will get them for you automatically (fetches fresh bridges from Tor Project) or enter bridges manually from [bridges.torproject.org](https://bridges.torproject.org/)
6. Go back.

##### Phase 5: Configure I2P

For most users the default settings are correct. If you plan to access I2P eepsites, go into settings and confirm the I2P HTTP proxy is enabled on port `4444`.

Note: I2P takes 10 to 20 minutes to fully build its routing table on first run, this is expected behaviour, not an error.

##### Phase 6: Start All Services

On the main screen tick all 3 Boxes then click the big start buttom.

InviZible Pro is now running. The iptables DNS redirect is active (all DNS to DNSCrypt). Your local Tor HTTP proxy is live at `127.0.0.1:8118`.

##### Phase 7: Configure Android System Proxy for IP Spoofing via Tor

This is how you route app traffic through InviZible Pro's Tor proxy using Android's built in system proxy, no VPN slot used. Apps that respect the system proxy will have their HTTP/HTTPS traffic exit through a Tor exit node. Your real IP is not visible to any destination.

**Repeat this for every Wi Fi network you connect to:**

1. **Settings, Network & Internet, Wi Fi**
2. Press the settings icon on the network you're connected to
3. Press the pencil in the upper right hand corner
4. Expand **Advanced options**
5. Under **Proxy**, select **Manual**
6. Enter:
   * **Proxy hostname:** `127.0.0.1`
   * **Proxy port:** `8118`
   * **Bypass proxy for:** `localhost,127.0.0.1`
7. Tap **Save**

> **Mobile data:** Android's system proxy setting is Wi Fi only in standard UI. For cellular data, AFWall+ (OPSEC Step 4) with custom iptables rules can enforce routing at the kernel level. For most users, Wi Fi proxy coverage is sufficient.

##### Phase 9: Verify Everything Is Working

1. InviZible Pro main screen, all three services: **Running** (green)
2. Open **DuckDuckGo Privacy Browser**
3. Visit `https://check.torproject.org`, should confirm **"Congratulations. This browser is configured to use Tor."**
4. Visit `https://dnsleaktest.com`, run Extended Test, DNS should resolve through your DNSCrypt server, not your ISP
5. Visit `https://whatismyipaddress.com`, IP shown should be a Tor exit node IP, not your real IP or ISP IP
6. All three checks passing = DNS encrypted, traffic anonymised through Tor, real IP hidden

##### Ongoing Usage Tips

* InviZible Pro restores services and iptables rules on every boot automatically.
* If Tor is slow: stop I2P temporarily to free bandwidth for Tor circuits. Restart I2P when you need eepsites.
* Check **InviZible Pro Logs** to see active Tor circuits, DNSCrypt queries, and any errors.
* Update InviZible Pro via F-Droid regularly, outdated Tor binaries can have known vulnerabilities.
* Do not log into personal accounts (Google, Facebook, etc.) while routing through Tor, account level identification defeats network level anonymity.

To Check whats being blocked and censored in your proxy network then follow the steps below to add OONI Probe to your device

---

### OPSEC Step 3: OONI Probe Network Censorship Detector

**Download:** [F-Droid](https://f-droid.org/packages/org.openobservatory.ooniprobe/)

OONI Probe (Open Observatory of Network Interference) is a network measurement tool developed by the Tor Project and independent researchers. It detects censorship, surveillance infrastructure, and network manipulation on your internet connection, telling you exactly what your ISP or government is blocking or tampering with.

#### Key Features

* **Website blocking detection**, tests whether specific websites are blocked on your network
* **Censorship measurement**, detects DNS manipulation, HTTP blocking, TCP/IP blocking, and TLS interference
* **Surveillance infrastructure detection**, identifies middleboxes (equipment used for network surveillance)
* **VPN and Tor reachability**, tests whether Tor, VPNs, and circumvention tools are blocked
* **Global network data**, contributes your test results to OONI's public database, helping document censorship worldwide
* Runs automatic tests at scheduled intervals
* Completely open source (Tor Project affiliated)

#### Setup and Usage

1. Install from F-Droid using the link above (or search "OONI Probe" in F-Droid).
2. Open OONI Probe complete the onboarding by answering the questions (both are true).
3. Go into settings then into test options, toggle off both things that are toggled on and if you want to run tests automatically
4. **Run immediate tests:**
   * Tap **Run** on the main dashboard
   * OONI Probe runs the full test suite: websites, instant messaging, circumvention tools, middleboxes
5. **View results:**
   * Results appear under the **Test** tab
   * Green checkmarks = no blocking detected
   * Red X = blocking or manipulation detected
   * Orange warning = anomaly (possible interference)
6. **Interpret results:**
   * If websites you expect to be accessible show as blocked, your ISP/government is censoring them, use InviZible Pro's Tor routing to bypass
   * If circumvention tools (Tor, VPNs) show as blocked, use InviZible Pro's Tor bridge mode with obfs4 bridges (see OPSEC Step 2)

Understanding your threat environment is the first step in defending against it. OONI Probe tells you exactly what your network operator is doing to your traffic whether you are in a heavily censored country or simply want to know if your ISP is silently blocking certain content. Along with telling you how free your proxy network is.

---

### OPSEC Step 4: Install AFWall+ Root Level Firewall

**Download:** F-Droid, [dev.ukanth.ufirewall](https://f-droid.org/packages/dev.ukanth.ufirewall/)

AFWall+ (Android Firewall+) is a root level firewall that uses **iptables**, the same Linux kernel level packet filtering used in professional firewalls and servers. Unlike app based firewalls that operate through the Android VPN slot, AFWall+ works at the kernel level, meaning it intercepts and controls network traffic before any app can touch it. No VPN slot is used. No app can bypass it. It runs silently in the background and survives reboots via Magisk root.

This is your system wide network policy layer. InviZible Pro (covered in Part 2) handles routing your traffic through Tor/I2P/DNSCrypt via proxy. AFWall+ handles which apps are permitted to reach the network at all, including blocking apps that would try to bypass the proxy entirely.

#### Key Features

* **Per app, per interface rules**, separately control Wi Fi, mobile data, and VPN for every installed app
* **Whitelist mode**, default deny; only explicitly approved apps can reach the internet
* **Custom iptables rules**, write raw iptables rules for advanced control (e.g. mobile data proxy redirect to port 8118)
* **Logging**, logs every blocked connection attempt; reveals apps trying to phone home
* **PIN lock**, locks the firewall UI so no app or accidental tap can disable rules
* **LAN control**, optionally block apps from accessing your local network
* Works on any rooted Android device with Magisk; no VPN slot consumed

#### Why AFWall+ Is a Separate Layer From InviZible Pro

InviZible Pro's internal firewall (when running in proxy mode) handles routing decisions which traffic goes through Tor, which goes through I2P. AFWall+ handles **access control** which apps are allowed to communicate at all, on which network interfaces (Wi Fi, mobile data, VPN), and to which addresses. These are complementary, not redundant:

* InviZible Pro routes and anonymises traffic
* AFWall+ decides at the kernel level which apps are even allowed to generate network traffic in the first place

Running both gives you defence in depth: even if InviZible Pro's proxy routing fails or an app bypasses the system proxy, AFWall+'s iptables rules are still enforced at the kernel.

#### Setup

1. Install AFWall+ from F-Droid.
2. Open AFWall+ it will request root via Magisk. Grant it permanently.
3. The main screen shows every installed app as a row with toggle switches for:
   * **WiFi** (left column), allow/block on Wi Fi networks
   * **Data** (middle column), allow/block on mobile data
   * **VPN/Roaming** (right column, if shown), allow/block on VPN or roaming
4. Tick all the apps then click the options menu in the right hand corner then press apply.

Block everything else like games, offline tools, system apps that have no reason to phone home, and any remaining Google service remnants if you dont use internet with them.

##### Configure Per App Rules

AFWall+ shows all installed apps with toggles for each interface:

* **Wi Fi** column, whether this app can use Wi Fi
* **Data** column, whether this app can use mobile/cellular data

**Recommended starting rules:**

| App | Wi Fi | Mobile Data | Notes |
| --- | --- | --- | --- |
| DuckDuckGo Privacy Browser | ✓ | ✓ | Primary browser, allow all |
| InviZible Pro | ✓ | ✓ | Must have access to reach Tor network |
| Tuta Mail | ✓ | ✓ | Email, allow all |
| Arcane Chat | ✓ | ✓ | Messaging, allow all |
| Aurora Store | ✓ | ✗ | Updates via Wi Fi only to save data |
| Hypatia | ✓ | ✗ | Signature updates via Wi Fi only |
| OONI Probe | ✓ | ✓ | Needs both for network testing |
| F-Droid | ✓ | ✗ | Update repo via Wi Fi only |
| DresSecureComms | ✓ | ✓ | Calls and SMS use data |
| Metrolist | ✓ | ✗ | Stream via Wi Fi only |
| Games / offline apps | ✗ | ✗ | Block all, no reason for internet access |
| Any Google remnant | ✗ | ✗ | Block completely |

##### Enable Logging

1. **menu, Log, Enable log**, AFWall+ will log all blocked connection attempts.
2. Check the log periodically, blocked entries reveal apps attempting to phone home, connect to ad networks, or bypass your privacy stack.

##### Set a PIN Lock on AFWall+

1. **menu, Preferences, security, enable protection**, set a PIN.
2. This prevents any app or accidental tap from disabling your firewall.

#### ADVANCED: Force Apps Through InviZible Pro Proxy via iptables

On rooted devices, AFWall+ can redirect traffic at the kernel level to ensure apps route through InviZible Pro's local Tor proxy even if they do not respect Android's system proxy setting. Go to:

**AFWall+, menu, Custom Scripts, Startup Script**

Add the following rule to redirect all outgoing HTTP/HTTPS traffic through InviZible Pro's Tor HTTP proxy on port 8118 (replace `UID` with the app's Android user ID from Settings, Apps, [App], App info):

```bash
iptables -t nat -A OUTPUT -p tcp -m owner --uid-owner UID -j DNAT --to-destination 127.0.0.1:8118
```

> For most users, the combination of InviZible Pro system proxy + DuckDuckGo App Tracking Protection + AFWall+ default deny whitelist is sufficient without custom iptables rules. The custom script approach is for advanced users who want guaranteed per app Tor enforcement regardless of system proxy compliance.

---

### OPSEC Step 5: Set Up The Tuta & DuckDuckGo Suite

This two layer setup gives you the strongest available email privacy. Tuta Mail is your real, encrypted inbox. Duck Addresses are disposable forwarding aliases you hand out to every app, website, and service instead of your real address, so your actual Tuta inbox is never exposed.

#### Part A1, Set Up Tuta Mail

1. Open **Tuta Mail** and create a new account.
2. Use a `.de` domain address (e.g., `yourname@tuta.de`), German privacy laws (DSGVO/GDPR) provide strong legal protections for accounts hosted in Germany.
3. **No phone number is required.** Tuta only asks for an email address and gives you an **encryption key / recovery code**, write this down and store it somewhere physically safe. This recovery code is how you regain access to your account if you ever lose access. Guard it the same way you would guard a physical key to your home.
4. Enable **biometric lock** or a strong PIN within Tuta Mail's security settings.
5. All emails between Tuta accounts are automatically end to end encrypted. Emails to external recipients (Gmail, etc.) can be password encrypted using Tuta's "secure external password" feature.

#### Key Features

* **End to end encryption** for all emails (both subject and body, not just body like most providers)
* **Zero knowledge**, Tuta cannot read your emails; they are encrypted before leaving your device
* **Encrypted contacts and calendar** included in the free plan
* Encrypted search (searches happen locally on your device, never on the server)
* **Email aliases** for compartmentalization (use different aliases for different purposes)
* Secure external email (password protected messages to non Tuta recipients)
* German and EU jurisdiction with GDPR protections
* Free plan available; paid plans add more aliases and storage
* Fully open source client

---

#### Part A2: Set up Tuta Calendar

Tuta Calendar is the standalone end to end encrypted calendar app from the Tuta team, split out from the Tuta Mail app into its own package. Your events, descriptions, attendees, and notifications are encrypted before they leave the device, and they sync across all your devices through the same Tuta account that holds your encrypted mail. It is the calendar counterpart to Tuta Mail and the DresOS choice when you want an encrypted calendar that syncs, rather than a purely local one.

#### Key Features

* End to end encrypted events, including titles, descriptions, and attendees
* Zero knowledge: Tuta cannot read your calendar
* Syncs across devices through your existing Tuta account, no Google account
* Encrypted reminders and recurring events
* German and EU jurisdiction with GDPR protections
* Fully open source client

#### Setup and Usage

1. Install Tuta Calendar from F-Droid.
2. Sign in with the same Tuta account you created in Part A1. Your encrypted calendar is available immediately and stays in sync with Tuta Mail.
3. In AFWall+, allow Tuta Calendar on Wi Fi and Data, since it needs the network to sync your encrypted calendar.

---

#### Part A3, Setting Up DuckDuckGo,

1. Install DuckDuckGo from F-Droid.
2. When it offers App Tracking Protection, enable it. Android will ask you to allow a VPN connection for DuckDuckGo; confirm it. InviZible Pro uses proxy plus root mode and leaves the VPN slot free, so there is no conflict.
3. Set it as your default browser in **Settings, Apps, Default apps, Browser**.
4. Set up your Duck Address: **menu, Settings, Email Protection, Get Started**, enter your Tuta Mail address as the forwarding address, and store the generated key alongside your Tuta recovery code.
5. Allow DuckDuckGo on Wi Fi and Data in AFWall+, since it is your browser and needs full access.
6. For YouTube, turn on Duck Player in **menu, Settings, Duck Player** and set it to open YouTube videos automatically. Videos then play in a clean, distraction free window with targeted ads stripped and without YouTube tying the session to your watch history or recommendation profile.

**Duck Addresses** are free, private email aliases provided by DuckDuckGo. The idea is simple: instead of giving any website, app, or service your real Tuta address, you give them a Duck Address like `yourname@duck.com`. DuckDuckGo strips all email trackers from messages and forwards them clean to your real Tuta inbox. If a service gets breached, sells your data, or starts spamming you, you just deactivate that alias. Your real Tuta address is never exposed.

**Full Email Setup:**

1. Open **DuckDuckGo Privacy Browser**.
2. Tap the **menu**, **Settings**, **Email Protection**.
3. Tap **Get Started** and follow the setup flow.
4. When asked for your forwarding address, enter your **Tuta Mail address**, all forwarded emails will arrive in your Tuta inbox, stripped of trackers.
5. You will receive a **Duck Address** (e.g., `yourname@duck.com`). This is your primary alias. **No phone number required**, just your Tuta address to receive forwarded mail.
6. You will also be given an **encryption key / personal Duck Key**, store this safely alongside your Tuta recovery code. This is how you manage your account if needed.

**Using Duck Addresses everywhere:**

* Whenever any app, website, or form asks for your email address, tap the DuckDuckGo keyboard suggestion **"Use Duck Address"**, or manually enter your `@duck.com` address
* DuckDuckGo can also **auto generate unique one time aliases** for each service (e.g., `yourname_amazon_5k3j@duck.com`), these appear as autofill suggestions in DuckDuckGo browser when you tap an email field
* Each unique alias forwards to your Tuta inbox with all trackers removed
* If a specific service starts sending spam or your alias appears in a breach, go to **DuckDuckGo Settings, Email Protection, Manage** and deactivate just that alias, without touching your real address

**What this stops:**
* **Phishers**, they only have a disposable alias, never your real Tuta address
* **Doxxers**, your real inbox cannot be searched for, looked up, or correlated across services
* **Data brokers**, even if five different companies share your email address, they each have a different alias and cannot link your accounts together
* **Email trackers**, DuckDuckGo strips tracking pixels, spy links, and hidden tracking code from every forwarded email before it reaches Tuta

**The complete email security chain:**
```
Website/App to Duck Address alias (unique per service)
    to DuckDuckGo strips email trackers
        to Forwards clean to your Tuta Mail address
            to Tuta decrypts with your encryption key
                to You read it
```
Your real address never appears anywhere outside of the DuckDuckGo Email Protection settings on your own device.

---

#### Part A4, Auto Generated Passwords in DuckDuckGo

DuckDuckGo's browser also includes a **built in password manager** that integrates with Duck Address setup. Once you have Email Protection enabled:

* When you create a new account on any website in the DuckDuckGo browser, it will **automatically suggest a strong, randomly generated password** for the password field tap to accept and it is saved to DuckDuckGo's encrypted local password storage
* Saved passwords are stored on device, encrypted, and sync only to other devices you control via your Duck account (optional)
* On returning to a site, DuckDuckGo autofills both the Duck Address alias and your saved password
* This means for most sign ups you never manually type or choose a password the browser generates, saves, and fills it all automatically

**Use IYPS** (App #2) to analyse the strength of any existing passwords you want to migrate, then replace weak ones with DuckDuckGo generated or IYPS passwords going forward.

IYPS (It's Your Password Security) is an open source password strength analyser and generator. It does **not** store passwords that job is handled by DuckDuckGo's built in password manager. What IYPS does is tell you exactly how strong or weak any given password is, with real world crack time estimates across different attack scenarios, and generate strong random passwords on demand.

#### Key Features

* **Password strength analyser**, paste any existing password and get a detailed breakdown: entropy score, estimated crack time under online attack, offline attack, and distributed cracking scenarios
* **Password generator**, creates strong, random passwords with customisable length and character sets
* **Completely offline**, no password ever leaves your device during analysis
* No storage, no vault, no sync, purely an analysis and generation tool
* No ads, no trackers, no telemetry

#### Setup and Usage

1. Install via SAI from the direct APK link **Here:** [GitHub Releases (latest)](https://github.com/StellarSand/IYPS/releases/latest).
2. Open IYPS, no account or setup required.
3. **Check an existing password:**
   * Paste it into the analyser field
   * IYPS shows the entropy score and estimated crack time across attack scenarios (online throttled, offline fast hash, distributed brute force)
   * Anything under "centuries" for an offline attack is worth replacing
4. **Generate a strong password:**
   * Go to the **Generator** tab
   * Set desired length (20+ characters recommended) and character set (uppercase, lowercase, numbers, symbols)
   * Tap generate copy the result and save it in DuckDuckGo's password manager.

---

### OPSEC Step 6: Set up DresSecureComms

DresSecureComms is our in house FOSS, fully de Googled application that handles private messaging, calling, contacts, link threat scanning, photo metadata wiping, and location spoofing. It is built and signed by us The DresOS Team and is designed to be set as your default SMS and phone app.

It puts the most sensitive parts of the device, your texts, calls, contacts, and the links you open, inside one audited, de Googled app built by DresOS. Nothing leaves the device except the VirusTotal lookups you choose to run.

#### Key Features

* Private offline SMS client with optional per message AES 256 GCM encryption, using a shared passphrase and readable only by another DresSecureComms user
* Full default phone app with its own in call screen (mute, speaker, keypad, hold, add call) and a deletable call history
* Caller ID and spam detection coming soon
* An encrypted on device contacts vault with add, edit (name, number, email), and import
* Threat Scan, which checks any link against VirusTotal and returns a clear safe, suspicious, or dangerous verdict
* Metadata Wipe, which strips GPS and EXIF data from photos before you share them
* Geo Spoofer, which sets a fixed or random mock GPS location
* App lock (fingerprint or device PIN) and an app wide block screenshots mode
* No Google services, no trackers, no analytics; the only network call is the VirusTotal lookup you trigger yourself
* File scanning engine built up from Hypatias coming soon

#### Setup and Usage

1. Open the app then go into settings and set as defualt if it says it has been restricted then Open Settings, Apps, DresSecureComms, the options menu, then Allow restricted settings. This is required for sideloaded SMS and phone apps.
2. For encrypted SMS, set a shared key in Settings and share it by hand with your contact.
3. For link scanning get a Virus Total api key and set it in settings via the Threat scanning block.

#### Setting up mock location

Physical location is one of the most sensitive data points your phone broadcasts constantly. DresSecureComms lets you replace your real GPS coordinates with any location in the world.

1. Open **DresSecureComms**.
2. Go into settings
3. Click Randomize location then press apply
4. Go to **Settings**, About phone, tap **Build number** seven times to ensure Developer Options is open.
5. In Developer Options scroll down to **Select mock location app** then choose **DresSecureComms**.
6. Your device's GPS will now report the fake location to all apps that request it.

**What this does:** Spoofs your device's geolocation, preventing apps from determining your real position. Combined with InviZible Pro's Tor routing (which masks your IP address from network observers), this gives you both network level and sensor level location protection simultaneously.

---

### OPSEC Step 7: Final Checks and Hardening Tips

**Test & Backup:**
* Make and receive a test phone call via **DresSecureComms**
* Send and receive a test SMS via **DresSecureComms**
* Send and receive a test email via **Tuta Mail**
* Open a website in **DuckDuckGo Privacy Browser**, confirm it is working and App Tracking Protection shows active
* Verify DresOS WebView is the active WebView provider via `adb shell dumpsys webviewupdate | grep "Current WebView package"`, the output should show `org.dresos.webview`
* Open **AFWall+**, confirm firewall rules are applied and iptables is active
* Open **Aurora Store**, verify apps load and anonymous mode works
* Open an APK via **SAI**, confirm it installs correctly
* Download a file in **DuckDuckGo**, confirm it opens in **Gopeed**
* Everything working through microG and DresOS WebView as expected

**InviZible Pro / proxy setup:**
* Confirm InviZible Pro is running (all three services green) and the Android system proxy is set to `127.0.0.1:8118` on your Wi Fi network.
* Allow InviZible Pro background network access and disable any battery optimization that would kill it (Settings, Battery, Battery optimization, InviZible Pro, Not optimized).
* Allow background network and battery usage for Tuta Mail (for push notifications) the same way.

**DuckDuckGo App Tracking Protection:**
* Since InviZible Pro uses proxy mode and does not occupy the Android VPN slot, DuckDuckGo's App Tracking Protection should show up as a **VPN**.

**Network preference:**
* Use Wi Fi over mobile data where possible, Tor performance is significantly better on Wi Fi, and the system proxy applies automatically to saved Wi Fi networks.
* On public Wi Fi, AFWall+'s whitelist ensures no unexpected apps phone home even on untrusted networks.

**Periodic maintenance:**
* Run **Hypatia** weekly to scan for malware.
* Run **OONI Probe** to monitor for censorship or network manipulation in your area.
* Run **SD Maid SE** monthly to clear leftover data from removed apps.
* Update all apps via F-Droid regularly, security patches are critical.

**Your device is now a hardened security software Android system reducing risks from surveillance, phishing, malware, censorship, and network level attacks.**

---

## Part 3: Fossify Suite

**Download:** All via F-Droid, search each app name or visit [fossify.org/apps](https://www.fossify.org/apps/)

Fossify is a suite of open source replacements for core Android system apps. In DresOS several of these are already covered: phone, SMS, and contacts by DresSecureComms, the gallery by Aves Libre, files by Amaze, and the calendar by Tuta Calendar. From Fossify you only need the system apps that nothing else covers, listed below.

### Apps in the Suite

| App | Replaces | Features |
| --- | --- | --- |
| **Fossify Launcher** | Pixel Launcher | Open source home screen, no Google Feed integration |
| **Fossify Clock** | Google Clock | Alarm, timer, stopwatch, zero telemetry |
| **Fossify Notes** | Google Keep | Local encrypted notes, no cloud sync |

### Setup

1. Install Fossify Launcher, Clock, and Notes via F-Droid.
2. Set Fossify Launcher as your default home app in **Settings, Apps, Default apps, Home app**.
3. Open Notes and Clock and just allow the permissions they ask for.

---

## Part 4: Additional Apps and Core Components

### Apps Overview

The following apps form the complete DresOS defensive security system. Every single one is open source, has zero Google dependencies, and is sourced directly from verified developer releases or F-Droid.

---

### 1. RedReader, Private Reddit Client

**Download:** [F-Droid](https://f-droid.org/packages/org.quantumbadger.redreader/)

RedReader is a free, open source Reddit client that removes all advertising, tracking, data collection, and analytics present in the official Reddit app and other third party clients.

#### Key Features

* **No ads whatsoever**, not even Reddit's own promoted posts
* **No tracking or analytics**, no telemetry sent to Reddit or any third party
* **No account required** to browse, full anonymous access to Reddit content
* Optional account login for commenting/voting without Reddit app telemetry
* Clean, fast, highly readable interface
* Full image, GIF, and video support
* Offline reading mode cache posts for reading without connectivity
* Night mode and customizable text sizes

#### Setup and Usage

1. Install via SAI from the direct APK link above.
2. Open RedReader, no setup required to start reading anonymously.
3. **Browse without account:** You can immediately browse subreddits, search, and read posts without logging in.
4. **Optional account login:**
   * Settings, Accounts, Add Account
   * Log in with your Reddit credentials, RedReader does not store these beyond the session token
5. **Customize feed:**
   * Add subreddits to your front page by searching (magnifying glass) and subscribing
   * If not logged in, subreddits are stored locally in the app

---

### 2. Hypatia, Open Source Antivirus

> **Coming to DresSecureComms.** A future DresSecureComms update will scan files, media, APKs, and ZIPs in app, taking over this antivirus role. Until then, Hypatia is the malware layer.

**Download:** [GitHub Releases (latest)](https://github.com/MaintainTeam/Hypatia/releases/latest)

Hypatia is a fully open source antivirus and malware scanner for Android, powered by ClamAV signature databases. It scans APKs before installation, scans files on your storage, and can perform full device scans to detect known malware, trojans, ransomware, and other threats.

#### Key Features

* **ClamAV powered**, uses the same signature database as the industry standard ClamAV engine trusted by organizations worldwide
* **APK scanning**, scans any APK file before you install it, including those from Aurora Store and direct downloads
* **File scanning**, scans documents, archives, downloaded files, and any other content on your storage
* **Full device scan**, scans all installed apps and accessible storage in one pass
* **Real time protection** (optional), monitors new files as they are created or downloaded
* **Offline capable**, signatures are downloaded locally; scanning does not require internet once signatures are updated
* Zero telemetry, no cloud scanning, no data sent to any server during scans
* Regular signature updates via the app (or F-Droid)

#### Setup and Usage

1. Install via SAI from the direct APK link above.
2. Open Hypatia, grant storage permission.
3. **Update databases first:**
   * Tap the options menu then update databases
   * Wait for ClamAV, MSRT, and other signature databases to download
4. **Full device scan:**
   * Tap the **Scan** button on the main screen, select Full Scan
   * Hypatia scans all installed apps and storage
5. **Real time monitoring:**
   * Tap the options menu, Tick Realtime scanner.
   * Hypatia monitors new files as they land on your device and alerts you if anything malicious is detected

#### Why It's in the DresOS Suite

Side loading APKs (which is necessary on a de Googled phone without Play Protect) requires you to take responsibility for your own malware screening. Hypatia with ClamAV signatures provides robust detection of known threats in every APK you install, a critical piece of the defensive stack.

---

### 3. Metrolist, Private YouTube Music Client

**Download:** [GitHub Releases (latest)](https://github.com/MetrolistGroup/Metrolist/releases/latest)

#### What It Is

Metrolist is a privacy focused, modded YouTube Music client. It provides access to YouTube Music's full catalogue, including all your existing playlists and library if you have a YouTube Music account, without Google's tracking, advertising, or data collection.

#### Key Features

* **Ad free YouTube Music**, no pre roll ads, no mid song interruptions, no audio ads
* **No tracking**, Google Analytics, advertising identifiers, and telemetry calls are removed
* **Background playback**, music plays when the screen is off or when using other apps (normally requires a paid YouTube Music Premium subscription)
* **Audio quality control**, select specific bitrates and codec preferences
* **Offline capability**, cache songs locally for offline listening
* **Account support**, optionally log in with a YouTube/Google account to access your playlists and liked songs (note: if you log in, Google still knows what you are listening to, use without account for maximum privacy)
* Custom theme support, AMOLED dark mode

#### Setup and Usage

1. Install via SAI from the direct APK link above.
2. Open Metrolist.
3. **Without account (maximum privacy):**
   * Browse and search the full YouTube Music catalogue freely
   * Your listening history is stored locally only
   * make playlists
4. **With account (access your library):**
   * Tap the account icon, sign in via **DuckDuckGo Privacy Browser** with your Google account
   * Your playlists, liked songs, and subscriptions will sync
   * Understand that YouTube/Google will still log your listening activity server side when you are logged in
5. **Background playback:**
   * Just minimize the app, music continues playing without needing any subscription
6. **Cache for offline listening:**
   * Long press a song or playlist, Download, select audio quality
   * Downloaded files are stored in your chosen directory (manage with Amaze File Manager)

---

### 4. Arcane Chat, Decentralized Encrypted Messaging

**Download:** [GitHub Releases (latest)](https://github.com/ArcaneChat/android/releases/latest)

Arcane Chat is a fully decentralized, end to end encrypted messaging application built on the **Delta Chat** protocol, which uses **email as the transport layer** with **OpenPGP end to end encryption**. This means your messages are transmitted over email infrastructure, making them extremely difficult to block, and giving you full control of your identity (your email address) without depending on any centralized messaging server.

#### Key Features

* **End to end encryption** via OpenPGP (Autocrypt standard), messages are encrypted on your device before transmission and can only be decrypted by the recipient
* **Decentralized**, no central Arcane Chat server; messages route through standard email servers. There is no single point of failure or surveillance
* **No phone number required**, your identity is your email address (use your Tuta Mail address for full privacy)
* **No metadata harvesting**, unlike Messenger, WhatsApp, and Signal, there is no central server collecting who you talk to, when, or how frequently
* **Censorship resistant**, email infrastructure is nearly impossible to block globally; even in censored regions, email typically works
* Group chats, image/file sharing, voice messages
* Multiple accounts (use different email addresses for different contexts)
* FOSS (fully open source), no ads, no tracking

#### Setup and Usage

1. Install via SAI from the direct APK link above.
2. Open Arcane Chat.
   * Tap **Create new profile**
   * Enter your name
   * Click agree and create profile
3. To talk to someome tap the plus button and scan the qr code they gave you or send the other person a link to connect to your profile.
4. **Group chats:**
   * Tap + then New Group give it a name, add members then click continue
5. **Disappearing messages:**
   * Open a chat press the options menu click Disappearing Messages and set a timer

---

### 5. HeliBoard, Offline Keyboard

**Download:** F-Droid, search "HeliBoard" or visit `https://f-droid.org/packages/helium314.keyboard/`

HeliBoard is a fully open source, completely offline keyboard for Android. It is a direct replacement for Google's Gboard, which is one of the most invasive data collection tools on Android it can capture everything you type (passwords, messages, searches) and transmit it to Google's servers for "personalization" and other undisclosed purposes.

#### Key Features

* **Completely offline**, zero network access, zero data transmission
* Full multilingual support with offline dictionaries
* Swipe typing (gesture input)
* Auto correction and predictive text (fully local)
* Customizable themes, layouts, and key sizes
* No analytics, no crash reporting, no permissions beyond keyboard input
* Actively developed and maintained

#### Setup and Usage

1. Install via F-Droid.
2. Open HeliBoard, follow the setup wizard to enable it as a system keyboard.
3. Remove Gboard during the debloating step.
4. Download offline language dictionaries for your languages within HeliBoard settings.

#### DresOS HeliBoard Design (One Tap Restore)

We worked up a ready made DresOS HeliBoard configuration so you do not have to tune the keyboard by hand. It is a privacy tuned setup with always incognito mode on, contacts and personalized dictionaries off, sound off, and the DresOS theme with key borders, a number row, and narrow key gaps. You restore it as a single backup file and the keyboard is fully set up.

1. Download the design file: [DresOS HeliBoard Design.zip](https://github.com/DresOperatingSystems/DresOS-The-Android-Defensive-Security-System/raw/main/assets/DresOS-HeliBoard-Design.zip)
2. Open HeliBoard then click advanced then scroll down to Backup and restore.
3. Tap **Restore**, select the downloaded **DresOS HeliBoard Design.zip**, and confirm.
4. HeliBoard applies the full DresOS configuration immediately. No further setup is needed.

---

### 6. Aves Libre, Gallery and Photo Manager

**Download:** [F-Droid](https://f-droid.org/packages/deckers.thibault.aves.libre/)

Aves Libre is a fully open source gallery and photo manager for Android. It handles photos and videos of every common format, reads and displays full image metadata, and organises your library by album, country, place, tag, and date, all locally on the device with no cloud, no account, and no trackers.

#### Key Features

* Photos and videos, including raw, GIF, SVG, and many other formats
* Map view and place or country grouping from on device metadata only
* Tags, favourites, and powerful search across your local library
* Full EXIF and metadata viewer and editor
* No cloud sync, no Google Photos backup, no AI face scanning, no telemetry
* Works fully offline; you can block it from the network in AFWall+

#### Setup and Usage

1. Install Aves Libre from F-Droid.
2. Grant it media and storage access when prompted.
3. In AFWall+, leave Aves Libre blocked on both Wi Fi and Data, since a local gallery has no reason to reach the internet.

---

### 7. Stratum, 2FA Authenticator

**Download:** [stratumauth.com/download](https://stratumauth.com/download) / [GitHub](https://github.com/stratumauth/app)

Stratum (formerly Authenticator Pro) is a free and open source two factor authentication app, released under GPL 3.0. It generates the rolling login codes that protect your accounts, and unlike Google Authenticator or Authy it does not lock your data in or talk to any server. Everything stays on the device, encrypted, and you can export it at any time.

#### Key Features

* TOTP, HOTP, mOTP and more, compatible with almost every provider
* Fully offline: it requires a single permission and never needs internet access
* Encrypted, exportable backups so you can move to a new phone without being locked out
* Password and biometric lock on the code list
* Import from other authenticator apps
* Icons, categories, and Material You theming

#### Setup and Usage

1. Install Stratum from the official download page or GitHub.
2. Open it and set a password plus biometric unlock in the security settings.
3. For each account choose add, then scan the provider's QR code or paste the secret. Stratum starts generating codes immediately.
4. Make an encrypted backup from the backup menu and store it somewhere safe, for example inside an AES 256 archive, so you can restore your 2FA if you lose the phone.
5. In AFWall+, block Stratum on both Wi Fi and Data. It works entirely offline, so denying it the network is a free privacy and security win.

---

### Made with love for privacy by DresOS

You now have a fully de Googled, secure Android device: no Google apps and no Google Services, DresOS WebView as the system WebView through the DresOS WebView Module v2.2.0, Google Play compatibility through the DresOS microG Module v3.1.2 with the ROM providing signature spoofing, Tor plus DNSCrypt through InviZible Pro in proxy mode, a kernel level iptables firewall through AFWall+, app tracker blocking through DuckDuckGo App Tracking Protection, and the full DresOS defensive app suite on top, from encrypted Tuta mail and calendar to offline two factor with Stratum, a local Aves Libre gallery, and an offline HeliBoard keyboard. Every tool is open source, every download is sourced directly from a verified developer, and every component is designed to hand control of your device, your data, and your privacy back to you.

---

### License

This project is released under the Creative Commons Attribution Non Commercial Share Alike 4.0 International License (CC BY NC SA 4.0). See the [LICENSE](LICENSE) file for the full terms.

---

### Donate

> **Help fund future development.** DresOS is built by a small open source team in our spare time. If our guide, Magisk modules or app saved you a weekend of research, please tip the jar. Funds go to test devices, dev stations, and developer time on updates and future projects.

[![Please Help fund future projects and keep this one going](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/dresos)
