# DresOS Android Defensive Security System

**A Complete Rooted DeGoogling + Operational Security Guide**
**TWRP + Magisk + MicroG + Fossify + InviZible Pro + AFWall+ + DresOS App Suite – For 2026**

[![Please Help fund future projects and keep this one going](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/dresos)

---

> **Project Goal:** Turn any stock Android phone into a fully private, hardened, FOSS-only device with no Google tracking, no Google Services, no hidden data harvesting, and only open-source replacements - then layer on a full defensive security stack to protect against surveillance, malware, censorship, and network-level attacks.
>
> **Why:** Because of all the restrictions Google is bringing into place over the open-source communities, to bring you back your privacy, to stop your data being sold, and to give you full operational security on a device you actually control.

---

## ⚠️ EXTREME WARNING

Rooting and flashing can **brick** your device, void your warranty, trip Knox/SafetyNet, and wipe all data.
**Back up everything** (photos, apps, files) before starting.
This is advanced - if you're not comfortable with ADB/fastboot or device-specific guides, stop and use a non-root method.
Proceed at your own risk.

**Make a full TWRP backup after rooting** (Boot + System + Data + Vendor) before any debloating.

---

## Table of Contents

### Part 1 - DeGoogling (Full Root Method)

* [What You Will Need](#what-you-will-need)
* [Replacement Overview](#replacement-overview)
* [Step 1: Unlock the Bootloader](#step-1-unlock-the-bootloader)
* [Step 2: Install TWRP](#step-2-install-twrp)
* [Step 3: Install Magisk](#step-3-install-magisk)
* [Step 4: Install Core FOSS Apps](#step-4-install-core-foss-apps)
* [Step 5: Install Magisk Modules](#step-5-install-magisk-modules)
* [Step 6: Set Up Noogle microG](#step-6-set-up-noogle-microg)
* [Step 7: Change System WebView to AOSmium](#step-7-change-system-webview-to-aosmium)
* [Step 8: Set Up Shizuku](#step-8-set-up-shizuku)
* [Step 9: Fine-Tune Debloat & Cleanup](#step-9-fine-tune-debloat--cleanup)
* [Step 11: Replace System File Manager, Package Installer & Download Manager](#step-10-replace-system-file-manager-package-installer--download-manager)
* [Step 12: Final Setup & Defaults](#step-11-final-setup--defaults)
* [Step 13: Test & Backup](#step-13-test--backup)

### Part 2 - Operational Security (OPSEC Stack)

* [OPSEC Overview](#opsec-overview)
* [OPSEC Step 1: Configure DNS and MAC Randomization](#opsec-step-1-configure-dns-and-mac-randomization)
* [OPSEC Step 2: Set Up InviZible Pro in Proxy + Root Mode](#opsec-step-2-set-up-invizible-pro-in-proxy--root-mode)
* [OPSEC Step 3: Set Up AFWall+ - Root-Level Kernel Firewall](#opsec-step-3-set-up-afwall--root-level-kernel-firewall)
* [OPSEC Step 4: Set Up Secure Email - Tuta + Duck Addresses](#opsec-step-4-set-up-secure-email--tuta--duck-addresses)
* [OPSEC Step 5: Spoof Location with Fake Traveler](#opsec-step-5-spoof-location-with-fake-traveler)
* [OPSEC Step 6: Final Checks and Hardening Tips](#opsec-step-6-final-checks-and-hardening-tips)

### Part 3 - DresOS Defensive App Suite

* [Apps Overview](#apps-overview)
* [1. Amaze File Manager](#1-amaze-file-manager--file-management--aes-encryption)
* [2. IYPS](#2-iyps--password-strength-analyser--generator)
* [3. RedReader](#3-redreader--private-reddit-client)
* [4. URL Check](#4-url-check--link-scanner)
* [5. Hypatia](#5-hypatia--open-source-antivirus)
* [6. OONI Probe](#6-ooni-probe--network-censorship-detector)
* [7. InviZible Pro](#7-invizible-pro--tor--i2p--dnscrypt-in-proxy--root-mode)
* [8. AFWall+](#8-afwall--root-level-iptables-firewall)
* [9. Metrolist](#9-metrolist--private-youtube-music-client)
* [10. Arcane Chat](#10-arcane-chat--decentralized-encrypted-messaging)
* [11. Aurora Store](#11-aurora-store--anonymous-app-store)
* [12. Fossify Suite](#12-fossify-suite--complete-foss-system-apps)
* [13. HeliBoard](#13-heliboard--offline-keyboard)
* [14. Tuta Mail](#14-tuta-mail--encrypted-email)
* [15. DuckDuckGo Privacy Browser](#15-duckduckgo-privacy-browser--primary-browser)
* [16. AOSmium WebView](#16-aosmium--system-webview-privacy-hardened-webview-engine)

### Appendix

* [Compatible Devices](#compatible-devices)
* [Contact DresOS](#contact-dresoS)

---

---

# PART 1 - DEGOOGLING

## What You Will Need

Download **only** from these official links (2026). Install **F-Droid** first - it is the backbone of your FOSS ecosystem.

| Tool | Purpose | Link |
|---|---|---|
| **F-Droid** | Main FOSS app store | [Download APK](https://f-droid.org/F-Droid.apk) / [Site](https://f-droid.org/) |
| **Inure App Manager** | Debloat + app management | [GitHub Releases](https://github.com/Hamza417/Inure/releases) |
| **Shizuku** | Root-like advanced system permissions | [GitHub Releases](https://github.com/RikkaApps/Shizuku/releases) |
| **TWRP** | Custom recovery (full backups + flashing) | [twrp.me](https://twrp.me/) → search your exact device model |
| **Magisk** | Root + module framework | [GitHub Releases](https://github.com/topjohnwu/Magisk/releases) |
| **De-Bloater** | Fine-tune system app removal | [F-Droid](https://f-droid.org/packages/com.sunilpaulmathew.debloater/) / [GitHub](https://github.com/sunilpaulmathew/De-Bloater) |
| **Noogle microG** | Magisk module - replaces Google Play Services | [GitHub Releases](https://github.com/SelfRef/noogle-magisk/releases) |
| **LSPosed** | Advanced Xposed-style module framework | [GitHub Releases](https://github.com/LSPosed/LSPosed/releases) |
| **DresOS AOSmium WebView Module** | Magisk module v2.1.0 - systemless replacement of Android System WebView with AOSmium via static RRO plus bind mount, activated by cmd webviewupdate after boot, with bootloop sentinel and inert mode fallback | [DresOS-Magisk-Modules releases](https://github.com/DresOperatingSystems/DresOS-Magisk-Modules/releases/latest) |
| **AFWall+** | Root-level iptables firewall - per-app kernel-level rules, mobile data proxy redirect, no VPN slot | [F-Droid](https://f-droid.org/packages/dev.ukanth.ufirewall/) / [GitHub](https://github.com/ukanth/afwall) |
| **SD Maid SE** | Deep system cleaner + remnant finder | [F-Droid](https://f-droid.org/packages/eu.darken.sdmse/) |
| **Fossify Suite** | Phone, Messages, Gallery, Calendar, Launcher, Contacts, Music | [fossify.org/apps](https://www.fossify.org/apps/) (all via F-Droid) |
| **Amaze File Manager** | File manager with built-in AES encryption and root explorer | [F-Droid](https://f-droid.org/packages/com.amaze.filemanager/) |
| **AOSmium** | System WebView - bundled inside the DresOS module ZIP | APK source: [codeberg.org/AXP-OS/app_aosmium/releases](https://codeberg.org/AXP-OS/app_aosmium/releases) |
| **InviZible Pro** | Tor + I2P + DNSCrypt in proxy + root mode - IP anonymisation, encrypted DNS, no VPN slot used | [F-Droid direct APK](https://f-droid.org/repo/pan.alexander.tordnscrypt.stable_26603.apk) / [F-Droid search: InviZible Pro](https://f-droid.org/packages/pan.alexander.tordnscrypt.stable/) |
| **DuckDuckGo Privacy Browser** | Primary browser (Chrome replacement) | [F-Droid](https://f-droid.org/packages/com.duckduckgo.mobile.android/) |
| **Aurora Store** | Anonymous Play Store alternative | [F-Droid](https://f-droid.org/packages/com.aurora.store/) |
| **HeliBoard** | Fully offline keyboard (no Gboard) | [F-Droid](https://f-droid.org/packages/helium314.keyboard/) |
| **Tuta Mail** | End-to-end encrypted email (no Gmail) | [F-Droid](https://f-droid.org/packages/de.tutao.tutanota/) |
| **Gopeed** | FOSS download manager - HTTP/HTTPS/BitTorrent/magnet, hash verification, no Google components | [F-Droid](https://f-droid.org/packages/com.gopeed/) / [gopeed.com](https://gopeed.com/) |
| **SAI** | Split APK Installer - FOSS package installer | [F-Droid](https://f-droid.org/packages/com.aefyr.sai.fdroid/) |
| **Fake Traveler** | GPS location spoofer | [F-Droid](https://f-droid.org/packages/cl.coders.faketraveler/) |

---

## Replacement Overview

Here is exactly what each core app replaces and why it is better for privacy:

| App | Replaces | Why It's Better |
|---|---|---|
| Fossify Phone | Stock Phone / Dialer | Fully open source, zero telemetry |
| Fossify Messages | Stock SMS | Privacy-first, no Google tracking |
| Amaze File Manager | Google Files / Stock File Manager / ZArchiver Pro | Open source file manager with built-in AES-256 encryption, biometric lock, root explorer, and APK management |
| DuckDuckGo Privacy Browser | Chrome | Private search engine, tracker blocking, zero Google telemetry |
| AOSmium WebView | Android System WebView (Google) | Blocks ads & trackers in **every** app that loads web content |
| Tuta Mail | Gmail | End-to-end encrypted email (no Google account or content scanning) |
| HeliBoard | Gboard | Completely offline keyboard, no data sent anywhere, ever |
| Aurora Store | Google Play Store | Anonymous downloads, no Google account required |
| Noogle microG | Google Play Services | Full app compatibility without any Google tracking infrastructure |
| Fossify Gallery / Calendar / Launcher / Contacts / Music | Stock Google equivalents | All open source, no cloud sync or telemetry |
| Gopeed | Google Download Manager | Modern FOSS download manager - HTTP/BitTorrent/magnet, hash verification, actively maintained |
| DresOS AOSmium WebView Module | Manual WebView patching process | Single Magisk flash: places AOSmium in systemless tree, ships a static RRO that allowlists it in config_webview_packages, activates via cmd webviewupdate after boot, includes bootloop sentinel |
| InviZible Pro | Orbot + separate DNS/I2P apps | Combines Tor, I2P, and DNSCrypt in proxy + root mode - no VPN slot used; IP spoofing via Android system proxy; iptables DNS redirect at kernel level |
| AFWall+ | No equivalent in stock Android | Root-level iptables firewall; per-app internet blocking at kernel level; custom rules for mobile data proxy routing; works independently of VPN and proxy layers |

---

## Step 1: Unlock the Bootloader

**Device-specific** - required for TWRP + Magisk. Usually wipes all data.

1. Enable **Developer Options**: Settings → About phone → tap **Build number** 7 times.
2. Turn on **OEM unlocking** + **USB debugging**.
3. Install ADB & Fastboot on your PC (Android Platform Tools from Android Developers).
4. Connect phone via USB.
5. Run: `adb reboot bootloader`
6. Run: `fastboot flashing unlock` (or `fastboot oem unlock` on older devices).
7. Confirm on phone screen - **this will factory reset the device**.

**Samsung:** Use Download Mode + Odin with official or unofficial unlock.
**Huawei / carrier-locked phones:** Use the official unlock code process for your carrier.
**Exact instructions:** Search `[your exact model] unlock bootloader XDA Developers` and follow **that** device's specific guide.

Phone will factory reset. Set it up again and re-enable USB debugging before continuing.

---

## Step 2: Install TWRP (Custom Recovery)

TWRP gives you full device backups and easy module/zip flashing.

1. Go to [twrp.me](https://twrp.me/) and search your **exact** device model and variant.
2. Download the latest `.img` (and `.zip` if available).
3. Follow the **exact instructions** on that device's TWRP page - they are device-specific.

Typical commands:

```
adb reboot bootloader
fastboot boot twrp-xxxx.img           # Temporary boot into TWRP
# Inside TWRP: Install → select TWRP .zip to make it permanent
# Or: Advanced → Install Recovery Ramdisk
```

Reboot to recovery to confirm TWRP is permanently installed.

**No TWRP for your device?** Use Magisk direct patching method only (see Step 3 below) or check XDA for unofficial TWRP builds. Search `[model] unofficial TWRP`.

---

## Step 3: Install Magisk (Root)

Two methods - pick one based on your situation.

### Recommended: Magisk Patching Method (works on almost everything)

1. Download the latest Magisk APK from GitHub.
2. Extract your device's `boot.img` (or `init_boot.img`) from the stock firmware ZIP for your exact model and Android version.
3. Install the Magisk app on your phone.
4. Open Magisk app → **Install** → **Select and Patch a File** → choose `boot.img`.
5. `magisk_patched_xxxx.img` will appear in your Downloads folder.
6. Pull it to your PC: `adb pull /sdcard/Download/magisk_patched_xxxx.img`
7. `adb reboot bootloader`
8. `fastboot flash boot magisk_patched_xxxx.img` (or `init_boot` if your device uses it).
9. On some devices you also need to flash a patched `vbmeta.img` with flags: `fastboot flash vbmeta vbmeta.img --disable-verity --disable-verification`
10. Reboot. Open Magisk app - it should show "Installed."

### Alternative: Via TWRP

1. Rename the Magisk APK file from `.apk` to `.zip`
2. Boot to TWRP → Install → select and flash the `.zip`
3. Reboot system

**Samsung-specific:** Use Recovery mode + Odin with a patched AP.tar file.

Reboot → open Magisk app → you are rooted. Keep Magisk app installed - you need it for everything that follows.

---

## Step 4: Install Core FOSS Apps

1. Transfer the **F-Droid APK** to your phone and install it (allow installs from unknown sources when prompted).
2. Open F-Droid and from the search/browse install **all** of these:
   * **Inure App Manager**
   * **Shizuku**
   * **SD Maid SE**
   * **All Fossify apps** (Phone, Messages, Gallery, Calendar, Launcher, Contacts, Music)
   * **Amaze File Manager** (replaces Fossify Files and ZArchiver Pro)
   * **HeliBoard**
   * **DuckDuckGo Privacy Browser**
   * **Aurora Store**
   * **Tuta Mail**
   * **Gopeed** (download manager)
   * **SAI (Split APK Installer)**
   * **Fake Traveler**
3. Download the **DresOS AOSmium WebView module** from [DresOS-Magisk-Modules releases](https://github.com/DresOperatingSystems/DresOS-Magisk-Modules/releases/latest) - you will flash it in Step 5.

Do **not** open or configure these apps yet - follow each step in order.

---

## Step 5: Install Magisk Modules

Open **Magisk app** → **Modules** → **Install from storage** and flash each of these one at a time, rebooting between them if instructed:

1. **Noogle microG** - replaces Google Play Services
2. **LSPosed** - advanced Xposed module framework (Zygisk version)
3. **DresOS AOSmium WebView Module v2.1.0** - systemless replacement of Android System WebView with AOSmium. Drops the AOSmium APK into the systemless tree at `system/product/app/AOSmiumWebView/`, ships a static RRO that adds AOSmium plus the AXP.OS signing certificate to the framework `config_webview_packages` allowlist, and activates AOSmium via `cmd webviewupdate set-webview-implementation` after boot complete. Includes a post-fs-data bootloop sentinel and an inert mode fallback so a failed activation cannot bootloop the device.
   * **Module repo:** [github.com/DresOperatingSystems/DresOS-Magisk-Modules](https://github.com/DresOperatingSystems/DresOS-Magisk-Modules)
   * **Download:** [DresOS-AOSmium-WebView-v2.1.0.zip](https://github.com/DresOperatingSystems/DresOS-Magisk-Modules/releases/latest)
   * Handles the entire WebView switch in one flash. Full details in Step 7.
   * First module in the DresOS Magisk Modules project. MicroG (dresosmicrog) is in active development as the next module.

Reboot after flashing all three.

---

## Step 6: Set Up Noogle microG (Replaces Google Services)

Noogle microG is a Magisk module that installs a clean, privacy-respecting replacement for Google Play Services. Apps that require Play Services (push notifications, maps, logins) will work through microG with zero Google data harvesting.

1. Open **Magisk** → tap the action button on Noogle microG → grant root permissions.
2. Open the **microG Settings** app (should appear in your app drawer after reboot).
3. Go to **Self Check** → work through every unticked item until all show green/ticked.
4. **Signature spoofing** must say "OK" - this is handled by LSPosed + Zygisk automatically.
5. microG is now active. Apps that previously needed Google Play Services will route through it.

**Note:** If the Self Check still shows issues after LSPosed is installed, reboot once more. If microG fails entirely, try uninstalling the LSPosed module from Magisk and retrying - some devices have compatibility quirks.

---

## Step 7: Install AOSmium as System WebView

### What is the System WebView?

The **Android System WebView** is a browser engine baked into Android itself. It is not a browser you open - it powers web content rendering inside hundreds of apps: social media feeds, email clients, news apps, microG sign-in screens, shopping apps, and more. Every time any of those apps display a webpage internally, the System WebView is doing the rendering. Replacing Google's default with AOSmium means all of that content runs through a privacy-hardened, Google-free engine rather than Google's WebView.

**AOSmium** (built by the [AXP.OS Project](https://axpos.org/)) is a Chromium fork hardened with GrapheneOS/Vanadium security patches, with Google services and anti-features stripped throughout.

Your everyday browser is **DuckDuckGo Privacy Browser**. AOSmium runs silently in the background as the engine all other apps use to render web content.

---

### DresOS AOSmium WebView Module v2.1.0

**Module repo:** [github.com/DresOperatingSystems/DresOS-Magisk-Modules](https://github.com/DresOperatingSystems/DresOS-Magisk-Modules)
**Download:** [DresOS-AOSmium-WebView-v2.1.0.zip](https://github.com/DresOperatingSystems/DresOS-Magisk-Modules/releases/latest)

**What the module does in one flash:**

1. Validates the host environment: Magisk 24.0 or newer, Android 10 through 15, arm or arm64 ABI. Aborts cleanly on x86/x86_64 (AXP.OS does not publish WebView builds for those ABIs), on unknown ABIs, and on devices that ship WebView as an APEX module.
2. Selects the correct AOSmium APK for the device ABI from the two bundled in the zip (webview64-signed.apk for arm64, webview32-signed.apk for arm).
3. Drops the signed AOSmium APK into the systemless tree at `system/product/app/AOSmiumWebView/AOSmiumWebView.apk`. Magisk magic mount makes this visible to PackageManager as a preinstalled system app, satisfying the framework `MATCH_FACTORY_ONLY` scan that `WebViewUpdateService` performs.
4. Places a static RRO in the systemless overlay partition. The RRO adds `org.axpos.aosmium_wv` plus the full AXP.OS ECDSA signing certificate to the framework `config_webview_packages` resource, which is the canonical allowlist that `WebViewUpdateService` reads at boot. Both `com.google.android.webview` and `com.android.webview` are kept in the allowlist as fallbacks so removing AOSmium cannot leave the device without a valid provider.
5. After `sys.boot_completed`, `service.sh` calls `cmd webviewupdate set-webview-implementation org.axpos.aosmium_wv` to promote AOSmium to the active provider, with a `settings put global webview_provider` fallback write for redundancy.
6. Activation is verified by re reading `dumpsys webviewupdate`. If the active provider is not AOSmium, the module flips itself into inert mode and stays out of the way until the user investigates.

**Bootloop safety:**

The module ships with two layers of protection so a failed activation cannot brick the device.

- **post-fs-data sentinel:** drops a `boot_pending` marker on every boot which `service.sh` clears on successful activation. A stale marker on the next boot signals a previous crash and the module auto-disables itself by touching `/data/adb/modules/dresoswv/disable`. Magisk recognises this file and skips the module entirely on subsequent boots.
- **Inert mode flag:** set automatically by `service.sh` on any activation failure (RRO did not register, APK bind mount did not land, framework refused to select AOSmium). The module's files remain bind mounted by Magisk but no further activation is attempted on subsequent boots, preventing retry storms.

**Package details:**
- Package: `org.axpos.aosmium_wv`
- Chromium version: `147.0.7727.49`
- Architectures: `arm64-v8a` + `armeabi-v7a` (bundled in the same zip, picked at install time based on device ABI)
- Signing cert SHA-256: `005C9805D501BF50C1A8BFD3204B6908843088581FDCF3DB8AB4F688FFC0E7B6` (AXP.OS ECDSA P-521)

**Confirmed working devices:**

| Device | Android version |
|--------|----------------|
| Motorola Moto G32 | LineageOS Android 15 |
| Motorola ThinkPhone | Stock Android 15 |
| Motorola Moto G7 Plus | Stock Android 10 |
| Motorola Moto G7 Plus | LineageOS Android 15 |
| Samsung Galaxy A05s | Stock Android 10 |

If the module works on your device, open an issue on the DresOS Magisk Modules repository so it can be added to the confirmed list. Include device model, Android version, and the output of `adb shell dumpsys webviewupdate | grep Current` after activation.

### Flash Instructions

1. Download `DresOS-AOSmium-WebView-v2.1.0.zip` from the releases link above
2. Open **Magisk → Modules → Install from storage**
3. Select the ZIP and wait for installation to complete
4. Tap **Reboot**

### After Reboot

No manual step is required. The module activates AOSmium automatically after boot complete. To verify activation, from adb:

```
adb shell dumpsys webviewupdate | grep "Current WebView package"
```

Expected output:

```
Current WebView package (name, version): (org.axpos.aosmium_wv, 147.0.7727.49)
```

If the output shows the OEM provider instead of AOSmium, check the activation log to diagnose:

```
adb shell cat /data/adb/modules/dresoswv/webview_activation.log
```

The Developer Options WebView picker may still show the OEM provider as selected on some OEM ROMs even when AOSmium is actually active. Trust `dumpsys` over the UI.

### Diagnostic Logs

If something goes wrong check these files via adb or a terminal app:

```
/data/adb/modules/dresoswv/logs/install.log
/data/adb/modules/dresoswv/logs/boot.log
/data/adb/modules/dresoswv/logs/service.log
/data/adb/modules/dresoswv/webview_activation.log
```

### Uninstalling

Disable or remove the module in **Magisk → Modules** and reboot. The `uninstall.sh` script clears the framework's saved WebView provider selection and restores either `com.google.android.webview` (on GMS devices) or `com.android.webview` (on AOSP/LineageOS builds) as the active provider.

---

## Step 8: Set Up Shizuku

Shizuku gives apps like **Inure App Manager** powerful system-level access - specifically the ability to deeply debloat and manage system apps - without requiring every individual operation to go through a full root prompt. It is safer, more granular, and lighter than running everything root, and it works perfectly alongside your existing Magisk root.

### Why Use Shizuku?

* Lets Inure and other tools debloat safely and deeply at the system level.
* Survives reboots when configured correctly.
* Uses Android's ADB-equivalent permission layer - no need to expose full root for every operation.
* Works with several other FOSS tools that support it.

### Step-by-Step Root Setup (Recommended - Device Already Rooted)

1. Open the **Shizuku** app.
2. When Magisk prompts for root permission, grant it to Shizuku **permanently**.
3. On the main Shizuku screen where it shows the root option, tap **Start** (the big button).
4. You will briefly see a black screen with white text confirming the service has started.
5. Go to **Shizuku settings** → enable **Auto start on boot** and **Keep alive**.

### Grant Permissions to Apps

* When you open **Inure App Manager** for the first time it will automatically request Shizuku access - approve it and grant **all** requested permissions.
* You can view and manage all apps that have been granted Shizuku access under Shizuku → **Authorized apps**.

**Test:** Reboot your phone. Open Shizuku - it should auto-start and show "Running" with root status. If it shows "Not running," open Shizuku and tap Start again.

You are now ready for the deepest, safest debloating possible.

---

## Step 9: Fine-Tune Debloat & Cleanup

Now we remove every trace of Google and system bloat safely.

### 9.1 Inure App Manager (Recommended Bulk Removal)

1. Open **Inure App Manager**.
2. Grant it **full access** - choose **Root** and **Shizuku**. Approve **everything** it asks for.
3. Go to the **Debloat** tab.
4. Tap the **select all** icon.
5. Select the **Recommended** debloat list.
6. This automatically selects everything your device does not need: Google bloat, carrier apps, unused system services, etc.
7. Review the list carefully. Uncheck anything you want to keep. Then tap **Debloat** to remove everything.
8. Restart the phone.

> **Note:** If Inure looks like it is doing nothing after you tap Debloat - leave it running. It is digging through your system to safely remove everything. This can take several minutes.

### 9.2 De-Bloater App (Fine-Tuning Pass)

1. Open the **De-Bloater** app (it will detect Magisk root automatically).
2. Scan system apps.
3. Remove **all** remaining Google system apps, including:
   * Gmail
   * Google Search
   * Chrome
   * Google Play Services remnants (anything not replaced by microG)
   * Any other carrier or OEM bloat
4. Apply changes and reboot.

### ⚠️ CRITICAL WARNING

You **must** have installed all replacement apps **before** removing their Google equivalents. Specifically:
* Install **Tuta Mail** before removing Gmail
* Install **Fossify Phone** before removing the stock dialer
* Install **Fossify Messages** before removing the stock SMS app
* Install **AOSmium** (via the DresOS module) before removing Chrome and the Google WebView
* Install **Gopeed** before removing Google Download Manager
* Install **SAI** before removing the Google Package Installer

Removing a system app without a replacement can break core phone functions until you restore a TWRP backup.

### 9.3 Final Cleanup with SD Maid SE

Open **SD Maid SE** → run **Full Scan** + **CorpseFinder** → delete every Google remnant, leftover data folder, and orphaned system file.

---

## Step 10: Install AFWall+ - Root-Level Firewall

**Download:** F-Droid - [dev.ukanth.ufirewall](https://f-droid.org/packages/dev.ukanth.ufirewall/)

AFWall+ (Android Firewall+) is a root-level firewall that uses **iptables** - the same Linux kernel-level packet filtering used in professional firewalls and servers. Unlike app-based firewalls that operate through the Android VPN slot, AFWall+ works at the kernel level, meaning it intercepts and controls network traffic before any app can touch it. No VPN slot is used. No app can bypass it. It runs silently in the background and survives reboots via Magisk root.

This is your system-wide network policy layer. InviZible Pro (covered in Part 2) handles routing your traffic through Tor/I2P/DNSCrypt via proxy. AFWall+ handles which apps are permitted to reach the network at all - including blocking apps that would try to bypass the proxy entirely.

### Why AFWall+ Instead of InviZible Pro's Internal Firewall

InviZible Pro is configured in **proxy mode** (not VPN mode) so the Android VPN slot remains free for DuckDuckGo App Tracking Protection. In proxy mode, InviZible Pro cannot enforce firewall rules on apps that ignore or bypass the system proxy - some apps do exactly this. AFWall+ operates at a level those apps cannot reach: the kernel's iptables chains, which sit below the entire Android app layer.

### Setup

1. Install AFWall+ from F-Droid.
2. Open AFWall+ - it will request root via Magisk. Grant it permanently.
3. The main screen shows every installed app as a row with toggle switches for:
   * **WiFi** (left column) - allow/block on Wi-Fi networks
   * **Data** (middle column) - allow/block on mobile data
   * **VPN/Roaming** (right column, if shown) - allow/block on VPN or roaming
4. **Set the default policy first:**
   * Tap the **≡ menu → Set Default Policy → Deny (White List)**
   * This means **all apps are blocked by default** - you explicitly whitelist only what needs internet. This is the most secure posture.

### Recommended Rules

Enable internet for these apps (tap their toggles to green for Wi-Fi and Data):

| App | Allow | Notes |
|---|---|---|
| DuckDuckGo | ✅ WiFi + Data | Your browser - needs full access |
| InviZible Pro | ✅ WiFi + Data | Proxy engine - must reach Tor/I2P/DNS servers |
| Tuta Mail | ✅ WiFi + Data | Email push notifications |
| F-Droid | ✅ WiFi only | App updates - Wi-Fi only to avoid data leaks |
| Aurora Store | ✅ WiFi only | App updates - Wi-Fi only |
| Arcane Chat | ✅ WiFi + Data | Messaging - needs constant connectivity |
| OONI Probe | ✅ WiFi + Data | Network tests |
| Hypatia | ✅ WiFi only | Signature updates only |
| microG (com.google.android.gms) | ✅ WiFi + Data | If using microG for push notifications |
| Fossify Phone | ✅ Data | Calls over mobile data |
| Metrolist | ✅ WiFi + Data | Music streaming |
| RedReader | ✅ WiFi + Data | Reddit client |

Block everything else - games, offline tools, system apps that have no reason to phone home, and any remaining Google service remnants.

### Advanced: Force Apps Through InviZible Pro Proxy via iptables

On rooted devices, AFWall+ can redirect traffic at the kernel level to ensure apps route through InviZible Pro's local Tor proxy even if they do not respect Android's system proxy setting. Go to:

**AFWall+ → ≡ menu → Custom Scripts → Startup Script**

Add the following rule to redirect all outgoing HTTP/HTTPS traffic through InviZible Pro's Tor HTTP proxy on port 8118 (replace `UID` with the app's Android user ID from Settings → Apps → [App] → App info):

```bash
iptables -t nat -A OUTPUT -p tcp -m owner --uid-owner UID -j DNAT --to-destination 127.0.0.1:8118
```

> For most users, the combination of InviZible Pro system proxy + DuckDuckGo App Tracking Protection + AFWall+ default-deny whitelist is sufficient without custom iptables rules. The custom script approach is for advanced users who want guaranteed per-app Tor enforcement regardless of system proxy compliance.

### Logs

AFWall+ → **≡ menu → Logs** shows you every blocked connection attempt in real time - which app tried to reach which IP, when, and whether it was blocked. This is an excellent way to spot apps trying to phone home that you did not expect.

---

## Step 11: Replace System File Manager, Package Installer & Download Manager

A truly de-Googled phone requires replacing not just apps but the system-level utilities Google embeds for file management, app installation, and downloading. Here is the full FOSS replacement stack - zero Google components:

### File Management and Encryption: Amaze File Manager

**Amaze File Manager** replaces both Fossify Files and ZArchiver Pro in a single app. It is a fully open-source file manager (GPL-3.0) built by Team Amaze with Material Design, available on F-Droid with no Google dependencies.

Install from F-Droid: `https://f-droid.org/packages/com.amaze.filemanager/`

Key features relevant to the DresOS build:

**File management:** Full local file browsing across internal storage, SD card, and USB OTG. Cut, copy, move, delete, rename, create folders. Multiple tabs open simultaneously. Bookmark frequently used directories. Built-in text editor, ZIP/RAR reader, APK reader, and database reader.

**AES-256 encryption:** Amaze has built-in AES encryption for files and folders - no separate archiving app needed. To encrypt a file: long-press it, tap the overflow menu, select Encrypt, set a password. The encrypted file gets an `.aes` extension. To decrypt: tap the `.aes` file, enter the password. Encrypted files cannot be read without the password even with full physical access to the device.

**Biometric lock:** Lock the entire app behind fingerprint or face unlock via Settings → Security → Fingerprint. This prevents anyone who picks up your phone from accessing your files.

**Root explorer:** On your rooted device, enable root mode in Settings → Root Explorer. This lets you browse and manage system directories directly - useful for advanced DresOS maintenance.

**APK management:** Long-press any APK file to install it directly, or use the built-in App Manager to backup, view, or uninstall installed apps.

Set **Amaze File Manager** as your default file manager in Settings → Apps → Default apps → Files.

> **Why Amaze instead of Fossify Files + ZArchiver Pro separately:** Fossify Files is a clean everyday browser but has no encryption. ZArchiver Pro is not fully open-source and requires a separate download from an unofficial GitHub mirror. Amaze is GPL-3.0, on F-Droid, actively maintained, and covers both use cases in one install.

### Package Installer: SAI (Split APK Installer)

**SAI** (Split APK Installer) is a fully open-source replacement for Google's PackageInstaller. It supports:

* Standard APK installation
* Split APK packages (.apks / .xapk files) - the format many apps use
* APK signature verification before installing
* Batch installation from ZIP archives
* Root and Shizuku installation modes for seamless installs without confirmation prompts
* Zero Google components, zero network calls home

Install from F-Droid: `https://f-droid.org/packages/com.aefyr.sai.fdroid/`

Set SAI as the default app opener for `.apk` files in Settings → Apps → Default apps → Opening links, or by long-pressing an APK in Amaze File Manager and selecting SAI.

### Download Manager: Gopeed

**Gopeed** is a modern, actively maintained open-source download manager that replaces Google's hidden Download Manager service entirely. It is under active development with regular releases and supports a broader range of protocols than older Android download managers.

Install from F-Droid: `https://f-droid.org/packages/com.gopeed/`
Or download from [gopeed.com](https://gopeed.com/)

Key features:
- HTTP, HTTPS, BitTorrent, magnet link, and ed2k support
- Multi-threaded downloads with resumable transfers
- File hash verification (SHA-256/MD5) to confirm download integrity
- Wi-Fi only mode - restrict downloads to unmetered connections
- Clean modern interface with download queue management
- Zero Google dependencies, zero tracking

After installing, go to **Settings → Apps → Default apps → Opening links** and ensure Gopeed handles download intents. In **DuckDuckGo Privacy Browser**, set Gopeed as the external download handler if prompted.

> **Why This Matters:** Google's Download Manager is a background service that silently handles every file download on the phone and can log metadata. Gopeed replaces it entirely with a transparent, open-source alternative that is actively maintained.

### APK Manager: F-Droid + Aurora Store

For sourcing and updating apps:

* **F-Droid** handles all open-source apps from its repository and any added repos (like AXP.OS for AOSmium). F-Droid verifies all APK signatures against developer keys.
* **Aurora Store** provides anonymous access to the Google Play Store catalogue without a Google account. Use **Anonymous** login mode only.

Together, these two replace the Google Play Store ecosystem completely.

---

## Step 12: Final Setup & Defaults

Go to **Settings → Apps → Default apps** and set:

| Function | Default App |
|---|---|
| Browser | **DuckDuckGo Privacy Browser** |
| Keyboard / Input method | **HeliBoard** |
| Home / Launcher | **Fossify Launcher** |
| Phone app | **Fossify Phone** |
| SMS / Messages | **Fossify Messages** |
| Email | **Tuta Mail** (also set for `mailto:` links) |
| File manager | **Amaze File Manager** |
| Package installer | **SAI** |
| Download manager | **Gopeed** |

**Aurora Store:** Open it → Settings → set **Anonymous** login mode. Do not log in with any Google account.

**DuckDuckGo Privacy Browser:** Open it and complete initial setup. **App Tracking Protection can and should be enabled** - InviZible Pro runs in proxy + root mode and does not occupy the Android VPN slot, so there is no conflict. Enable it in DuckDuckGo → Settings → App Tracking Protection → Enable. Android will prompt you to allow a VPN connection for DuckDuckGo - confirm it. This gives you both InviZible Pro's proxy-level Tor/DNSCrypt routing AND DuckDuckGo's in-app tracker blocking running simultaneously.

**LSPosed modules (optional hardening):**
* **Hide My Applist** - prevents apps from detecting other apps installed on your phone
* **Play Integrity Fix** - helps microG pass SafetyNet/Play Integrity checks for apps that require it

---

## Step 13: Test & Backup

* Make and receive a test phone call via **Fossify Phone**
* Send and receive a test SMS via **Fossify Messages**
* Send and receive a test email via **Tuta Mail**
* Open a website in **DuckDuckGo Privacy Browser** - confirm it is working and App Tracking Protection shows active
* Verify AOSmium is the active WebView provider via `adb shell dumpsys webviewupdate | grep "Current WebView package"` - the output should show `org.axpos.aosmium_wv`
* Open **AFWall+** - confirm firewall rules are applied and iptables is active
* Open **Aurora Store** - verify apps load and anonymous mode works
* Open an APK via **SAI** - confirm it installs correctly
* Download a file in **DuckDuckGo** - confirm it opens in **Gopeed**
* Everything working through microG and AOSmium WebView as expected

**TWRP Backup (if not done already):** Boot to TWRP → Backup → select Boot + System + Data + Vendor → swipe to back up. This is your emergency restore point. Store the backup folder in a safe location off-device.

**If anything breaks:** Boot to TWRP → Restore → select your backup → swipe. Your full system is back in minutes.

---

---

# PART 2 - OPERATIONAL SECURITY (OPSEC STACK)

## OPSEC Overview

This repository provides a guide to enhancing operational security (OPSEC) and cybersecurity on Android devices. It focuses on protecting against phishing, malware, IP tracking, data mining, device fingerprinting, censorship detection, and network surveillance - using free, open-source tools only.

This method creates a layered privacy setup: encrypted DNS at the OS level (Quad9 DNS-over-TLS), Tor routing + DNSCrypt + I2P at the network level via InviZible Pro in proxy + root mode (which leaves the VPN slot free), a root-level iptables firewall via AFWall+ enforcing per-app access control at the kernel level, app-layer tracker blocking via DuckDuckGo App Tracking Protection in the VPN slot, and location spoofing at the sensor level via Fake Traveler. The result is an extremely hardened device with multiple independent, non-conflicting privacy layers operating simultaneously.

**Compatibility:** Android 10 or later. The degoogling steps in Part 1 are recommended but most OPSEC steps work on stock Android too.

> **Architecture Note:** InviZible Pro runs in **proxy + root mode** - not VPN mode. It exposes Tor as an HTTP proxy on `127.0.0.1:8118`, and uses iptables (via Magisk root) to redirect all DNS to DNSCrypt at the kernel level. The Android system proxy setting routes HTTP/HTTPS traffic from all cooperating apps through Tor. The **Android VPN slot remains completely free**, meaning DuckDuckGo's **App Tracking Protection can and should be fully enabled** to run alongside InviZible Pro. **AFWall+** adds a second independent kernel-level firewall layer - per-app access control and mobile data proxy redirect - that operates entirely outside of both the proxy and VPN systems.

---

## OPSEC Step 1: Configure DNS and MAC Randomization

This is the baseline privacy layer - applied at the OS level, before any apps run.

### DNS over TLS (Quad9)

1. Go to **Settings → Network & Internet → Private DNS**.
2. Select **Private DNS provider hostname**.
3. Enter: `dns.quad9.net`
4. Save.

**What this does:** Quad9 DNS blocks malicious domains, protects against malware and phishing at the DNS resolution level, and does not log or collect user data - unlike Google DNS (8.8.8.8) or Cloudflare DNS (1.1.1.1) which both log queries. Quad9 is operated by a Swiss nonprofit. All DNS queries are now encrypted via DNS-over-TLS and filtered against Quad9's threat intelligence database.

> Note: Once InviZible Pro is running in proxy mode (Step 2), it takes over DNS resolution via DNSCrypt, which is more private than system-level DNS-over-TLS. The Quad9 setting here serves as a fallback for when InviZible Pro is not active.

### MAC Address Randomization

1. Go to **Settings → Network & Internet → Internet** → tap your Wi-Fi network.
2. Tap **Network usage** → set to **Treat as unmetered** (reduces certain monitoring behaviors).
3. Go to **Privacy** (within the same network settings).
4. Enable **Use randomized MAC address**.
5. Disable **Send device name**.

**What this does:** Every Wi-Fi device has a hardware MAC address that uniquely identifies it. By default, this allows any network operator to track your device across time even if you change your IP. Randomized MAC changes your device's hardware identifier on each network, preventing this form of persistent tracking. Your device might appear as a completely random hardware identifier - preventing physical location tracking via Wi-Fi network logs.

### Developer Options Hardening

1. Go to **Settings → About phone** → tap **Build number** seven times to unlock Developer Options.
2. In **Developer options**:
   * Enable **Non-persistent MAC** (complements the randomization above across reboots)
   * Enable **Tethering hardware acceleration** (improves performance when hotspot is used)

---

## OPSEC Step 2: Set Up InviZible Pro in Proxy + Root Mode

**InviZible Pro** (the app listed in stores as **iVizblePro**) is the network privacy engine of the DresOS stack. It combines **Tor**, **I2P**, and **DNSCrypt** into a single application. In this setup it runs in **proxy mode with root**, which means:

* It runs as a local proxy server on your device - the **Android VPN slot is not used at all**
* DuckDuckGo's **App Tracking Protection** can be fully enabled alongside it with zero conflict
* Magisk root lets InviZible Pro redirect DNS at the iptables level without any VPN interface
* Android's built-in system proxy setting routes your traffic through InviZible Pro's Tor HTTP proxy - your apparent IP address to every website and service becomes a Tor exit node IP, not your real IP

**Download:** [pan.alexander.tordnscrypt.stable_26603.apk](https://f-droid.org/repo/pan.alexander.tordnscrypt.stable_26603.apk)
Or search **InviZible Pro** in F-Droid.

---

### What Each Component Does

**DNSCrypt:**
DNS converts domain names like `example.com` into IP addresses. By default this happens in plain text - your ISP, network operator, and any middlebox on the network can see every domain you visit, even when page content is HTTPS-encrypted. DNSCrypt encrypts and cryptographically authenticates every DNS query. In proxy + root mode, InviZible Pro uses iptables to redirect all DNS traffic (UDP and TCP port 53) to its local DNSCrypt listener at port 5354 - no app can bypass it, no VPN is needed.

**Tor (The Onion Router):**
Tor routes your traffic through three volunteer-operated relays. Each relay only knows the immediately previous and next hop - no single relay can identify both your identity and your destination. InviZible Pro exposes Tor as a local HTTP proxy on port `8118`. When you set Android's system proxy to `127.0.0.1:8118`, all apps that respect the system proxy route their traffic through Tor. From the perspective of every website and service you connect to, your IP address is the Tor exit node's IP - a relay in another country, different from your real IP, rotating every 10 minutes.

**I2P (Invisible Internet Project):**
A separate anonymising network using garlic routing (bundles multiple encrypted messages per hop). Independent of Tor, runs on its own relay network. Suited for accessing `.i2p` hidden services (eepsites) and provides a different anonymity model from Tor that resists certain traffic correlation attacks differently. InviZible Pro exposes the I2P HTTP proxy locally on port `4444`.

**Root Mode / iptables DNS Redirection:**
With Magisk root, InviZible Pro writes iptables rules at the kernel level to redirect all outbound DNS queries (port 53 → port 5354) to DNSCrypt - regardless of which app generates them. No VPN slot needed. This runs below the application layer: it cannot be bypassed by any app.

---

### Full Setup: Proxy + Root Mode with Tor + I2P + DNSCrypt

Follow these steps in order. Do not start any service until Phase 6.

#### Phase 1: Install and Open

1. Install InviZible Pro via F-Droid or the direct APK link above (install with SAI).
2. Open **InviZible Pro**.
3. Allow notification permissions when prompted.
4. Do not tap Start on anything yet.

#### Phase 2: Set Connection Mode to Proxy + Enable Root

1. Tap the **≡ hamburger menu** (top-left corner) → **Settings** → **Common settings**.
2. Find **Run mode** or **Connection method** → select **Proxy mode** (not VPN mode, not Root-only mode).
3. Under **Root settings** in the same screen:
   * Enable **Run modules with root** - allows InviZible Pro to use your Magisk root for iptables and more reliable process control
   * Enable **Redirect DNS to DNSCrypt** - this is the iptables rule that forces all port-53 DNS from every app through DNSCrypt on port 5354
   * Enable **Fix TTL for tethering** - useful if you share your connection via hotspot
4. Under **Proxy settings** (still in Common settings):
   * Confirm **SOCKS5 proxy port:** `9050` (Tor SOCKS5)
   * Confirm **HTTP proxy port:** `8118` (Tor HTTP CONNECT proxy - this is what you will enter into Android's Wi-Fi proxy setting)
5. Enable **Start on boot** - restores all services and iptables rules automatically after every reboot.
6. Go back to the main screen.

#### Phase 3: Configure DNSCrypt

1. Tap the **DNSCrypt** tab.
2. Tap the **settings gear** icon.
3. Under **DNS servers**, select resolvers that meet all of these criteria:
   * `nolog` - server does not log your queries
   * `nodump` - server does not share data with third parties
   * `dnssec` - server validates DNS responses cryptographically
   * **Recommended:** `quad9-dnscrypt-ip4-filter-pri` (Quad9, Swiss non-profit, malware domain blocking), `mullvad-adblock` (Mullvad VPN's resolver, strict no-log, Swedish jurisdiction)
   * **Always avoid:** Any `cloudflare` entry (logs queries), any Google resolver
4. Select 2–3 qualifying servers for redundancy.
5. Under **DNSCrypt settings**, enable:
   * **DNSSEC** - cryptographic validation of all DNS responses; rejects spoofed or tampered answers
   * **Require DNSSEC** - filters server list to DNSSEC-supporting resolvers only
   * **Require no logging** - filters list to strictly non-logging resolvers only
   * **Block IPv6** - reduces attack surface if you do not actively use IPv6 services
   * **Bootstrap resolvers** - allows initial encrypted resolver list fetch; only contacts a plain resolver once for bootstrapping, then stays encrypted
6. Go back.

#### Phase 4: Configure Tor

1. Tap the **Tor** tab.
2. Tap the **settings gear** icon.
3. Under **Tor settings**, enable:
   * **Isolate destination addresses** - forces each destination domain to use a separate Tor circuit; prevents one service from correlating your traffic to another
   * **Isolate destination ports** - additional circuit isolation per port number
   * **Isolate SOCKS auth** - further isolation if apps use SOCKS5 authentication
4. Note the HTTP proxy port is `8118` (used in Phase 7 below).
5. Under **Bridges** - enable this if you are in a country where Tor is blocked, or for extra obfuscation on any network:
   * Enable **Use bridges**
   * Select bridge type: **obfs4** - disguises Tor traffic as ordinary HTTPS, bypasses deep packet inspection (DPI)
   * Tap **Request new bridges** (fetches fresh bridges from Tor Project) or enter bridges manually from [bridges.torproject.org](https://bridges.torproject.org/)
6. Go back.

#### Phase 5: Configure I2P

1. Tap the **I2P** tab.
2. Tap the **settings gear** icon.
3. For most users, default settings are correct. If you plan to access I2P eepsites, confirm the I2P HTTP proxy is enabled on port `4444`.
4. Enable **Tunnel bandwidth** limiting if your device is battery-sensitive.
5. Go back.
6. Note: I2P takes 10–20 minutes to fully build its routing table on first run - this is expected behaviour, not an error.

#### Phase 6: Start All Services (In Order)

From the InviZible Pro main screen, start services in this exact order:

1. **DNSCrypt** → tap **Start** → wait for status: **Running**
2. **Tor** → tap **Start** → wait for bootstrap to reach **100%** (30–90 seconds)
3. **I2P** → tap **Start** → bootstrap begins (10–20 min first run; much faster after that)

InviZible Pro is now running. The iptables DNS redirect is active (all DNS → DNSCrypt). Your local Tor HTTP proxy is live at `127.0.0.1:8118`.

#### Phase 7: Configure Android System Proxy for IP Spoofing via Tor

This is how you route app traffic through InviZible Pro's Tor proxy using Android's built-in system proxy - **no VPN slot used**. Apps that respect the system proxy will have their HTTP/HTTPS traffic exit through a Tor exit node. Your real IP is not visible to any destination.

**Repeat this for every Wi-Fi network you connect to:**

1. **Settings → Network & Internet → Wi-Fi**
2. Long-press your network name → **Modify network** (or tap the pencil icon)
3. Expand **Advanced options**
4. Under **Proxy** → select **Manual**
5. Enter:
   * **Proxy hostname:** `127.0.0.1`
   * **Proxy port:** `8118`
   * **Bypass proxy for:** `localhost,127.0.0.1`
6. Tap **Save**

> **Mobile data:** Android's system proxy setting is Wi-Fi only in standard UI. For cellular data, AFWall+ (OPSEC Step 3) with custom iptables rules can enforce routing at the kernel level. For most users, Wi-Fi proxy coverage is sufficient.

#### Phase 8: Enable DuckDuckGo App Tracking Protection

Now that the VPN slot is free:

1. Open **DuckDuckGo Privacy Browser**
2. Tap **≡ menu → Settings → App Tracking Protection → Enable**
3. Android prompts to allow DuckDuckGo to create a VPN connection - tap **OK**
4. DuckDuckGo App Tracking Protection is now running in the VPN slot, blocking trackers inside all other apps

This gives you two independent protection layers running simultaneously:
* **InviZible Pro** - handles network-level anonymity (Tor IP spoofing, DNSCrypt, I2P) via proxy + iptables
* **DuckDuckGo App Tracking Protection** - handles app-layer tracker blocking via the VPN slot

#### Phase 9: Verify Everything Is Working

1. InviZible Pro main screen - all three services: **Running** (green)
2. Open **DuckDuckGo Privacy Browser**
3. Visit `https://check.torproject.org` → should confirm **"Congratulations. This browser is configured to use Tor."**
4. Visit `https://dnsleaktest.com` → run Extended Test → DNS should resolve through your DNSCrypt server, not your ISP
5. Visit `https://whatismyipaddress.com` → IP shown should be a Tor exit node IP, not your real IP or ISP IP
6. All three checks passing = DNS encrypted, traffic anonymised through Tor, real IP hidden

#### Ongoing Usage Tips

* InviZible Pro restores services and iptables rules on every boot automatically.
* If Tor is slow: stop I2P temporarily to free bandwidth for Tor circuits. Restart I2P when you need eepsites.
* Check **InviZible Pro → Logs** tab to see active Tor circuits, DNSCrypt queries, and any errors.
* Update InviZible Pro via F-Droid regularly - outdated Tor binaries can have known vulnerabilities.
* Do not log into personal accounts (Google, Facebook, etc.) while routing through Tor - account-level identification defeats network-level anonymity.

---

## OPSEC Step 3: Set Up AFWall+ - Root-Level Kernel Firewall

**AFWall+** (Android Firewall+) is a root-level firewall for Android that uses **iptables** - the Linux kernel's built-in packet filtering framework - to control exactly which apps can access the internet and on which interfaces. Unlike InviZible Pro's proxy (which apps must cooperate with) or DuckDuckGo's App Tracking Protection (which works at the VPN layer), AFWall+ operates at the kernel level: it is enforced before any app or VPN stack even sees the traffic. An app cannot circumvent it.

**Download:** F-Droid - [dev.ukanth.ufirewall](https://f-droid.org/packages/dev.ukanth.ufirewall/) / [GitHub](https://github.com/ukanth/afwall)

---

### Why AFWall+ Is a Separate Layer From InviZible Pro

InviZible Pro's internal firewall (when running in proxy + root mode) handles routing decisions - which traffic goes through Tor, which goes through I2P. AFWall+ handles **access control** - which apps are allowed to communicate at all, on which network interfaces (Wi-Fi, mobile data, VPN), and to which addresses. These are complementary, not redundant:

* InviZible Pro routes and anonymises traffic
* AFWall+ decides at the kernel level which apps are even allowed to generate network traffic in the first place

Running both gives you defence-in-depth: even if InviZible Pro's proxy routing fails or an app bypasses the system proxy, AFWall+'s iptables rules are still enforced at the kernel.

---

### Setup and Configuration

#### Install and Grant Root

1. Install AFWall+ from F-Droid.
2. Open AFWall+ → it will request root access via Magisk - grant it permanently.
3. AFWall+ confirms it can control iptables.

#### Enable the Firewall

1. Tap the **≡ menu** (top-left) → **Preferences** → **Firewall rules**.
2. Confirm **iptables** mode is selected (not ip6tables-only - enable both for full coverage).
3. Go back to the main screen.
4. Tap **Enable firewall** (the toggle at the top) → confirm.

#### Configure Per-App Rules

AFWall+ shows all installed apps with toggles for each interface:

* **Wi-Fi** column - whether this app can use Wi-Fi
* **Data** column - whether this app can use mobile/cellular data
* **VPN** column - whether this app can use VPN tunnel traffic (relevant for DuckDuckGo App Tracking Protection)
* **LAN** column - whether this app can access local network (optional column)

**Recommended starting rules:**

| App | Wi-Fi | Mobile Data | Notes |
|---|---|---|---|
| DuckDuckGo Privacy Browser | ✓ | ✓ | Primary browser, allow all |
| InviZible Pro | ✓ | ✓ | Must have access to reach Tor network |
| Tuta Mail | ✓ | ✓ | Email, allow all |
| Arcane Chat | ✓ | ✓ | Messaging, allow all |
| Aurora Store | ✓ | ✗ | Updates via Wi-Fi only to save data |
| Hypatia | ✓ | ✗ | Signature updates via Wi-Fi only |
| OONI Probe | ✓ | ✓ | Needs both for network testing |
| F-Droid | ✓ | ✗ | Update repo via Wi-Fi only |
| Fossify Phone | ✓ | ✓ | Calls use data |
| Fossify Messages | ✓ | ✓ | MMS uses data |
| Metrolist | ✓ | ✗ | Stream via Wi-Fi only |
| Games / offline apps | ✗ | ✗ | Block all - no reason for internet access |
| Any Google remnant | ✗ | ✗ | Block completely |

#### Custom iptables Rules for Mobile Data Proxy Routing

Android's system proxy setting only applies to Wi-Fi. On mobile data, apps bypass the system proxy. To enforce Tor proxy routing on mobile data as well, add these custom iptables rules in AFWall+:

1. Tap **≡ menu → Custom rules**
2. Add the following (routes HTTP and HTTPS from all apps through InviZible Pro's local Tor HTTP proxy on mobile data):

```
# Redirect HTTP traffic to InviZible Pro Tor proxy (mobile data)
-A OUTPUT -p tcp --dport 80 -o rmnet+ -j REDIRECT --to-port 8118
-A OUTPUT -p tcp --dport 443 -o rmnet+ -j REDIRECT --to-port 8118
```

> **Note:** `rmnet+` matches mobile data interfaces on most Android devices. On some devices this may be `rmnet_data+` - check your device's interface names in a terminal emulator if the rule does not apply.

3. Save and apply.

#### Enable Logging

1. **≡ menu → Log → Enable log** - AFWall+ will log all blocked connection attempts.
2. Check the log periodically - blocked entries reveal apps attempting to phone home, connect to ad networks, or bypass your privacy stack.

#### Set a PIN Lock on AFWall+

1. **≡ menu → Preferences → UI preferences → Password protection** → set a PIN.
2. This prevents any app or accidental tap from disabling your firewall.

---

## OPSEC Step 4: Set Up Secure Email - Tuta + Duck Addresses

This two-layer setup gives you the strongest available email privacy. Tuta Mail is your real, encrypted inbox. Duck Addresses are disposable forwarding aliases you hand out to every app, website, and service instead of your real address - so your actual Tuta inbox is never exposed.

### Part A - Set Up Tuta Mail

1. Open **Tuta Mail** and create a new account.
2. Use a `.de` domain address (e.g., `yourname@tuta.de`) - German privacy laws (DSGVO/GDPR) provide strong legal protections for accounts hosted in Germany.
3. **No phone number is required.** Tuta only asks for an email address and an **encryption key / recovery code** - write this down and store it somewhere physically safe. This recovery code is how you regain access to your account if you ever lose access. Guard it the same way you would guard a physical key to your home.
4. Enable **biometric lock** or a strong PIN within Tuta Mail's security settings.
5. All emails between Tuta accounts are automatically end-to-end encrypted. Emails to external recipients (Gmail, etc.) can be password-encrypted using Tuta's "secure external password" feature.

**What Tuta does:** End-to-end encrypted email with zero trackers, zero ads, and zero data mining. Unlike Gmail, Tuta cannot read your emails. Unlike most providers, Tuta also encrypts subject lines and metadata - not just the body. Your encryption key means only you can access your account, even if Tuta's servers were ever compromised.

### Part B - Set Up Duck Addresses (DuckDuckGo Email Protection)

**Duck Addresses** are free, private email aliases provided by DuckDuckGo. The idea is simple: instead of giving any website, app, or service your real Tuta address, you give them a Duck Address like `yourname@duck.com`. DuckDuckGo strips all email trackers from messages and forwards them clean to your real Tuta inbox. If a service gets breached, sells your data, or starts spamming you - you just deactivate that alias. Your real Tuta address is never exposed.

**Setup:**

1. Open **DuckDuckGo Privacy Browser**.
2. Tap the **≡ menu** → **Settings** → **Email Protection**.
3. Tap **Get Started** and follow the setup flow.
4. When asked for your forwarding address, enter your **Tuta Mail address** - all forwarded emails will arrive in your Tuta inbox, stripped of trackers.
5. You will receive a **Duck Address** (e.g., `yourname@duck.com`). This is your primary alias. **No phone number required** - just your Tuta address to receive forwarded mail.
6. You will also be given an **encryption key / personal Duck Key** - store this safely alongside your Tuta recovery code. This is how you manage your account if needed.

**Using Duck Addresses everywhere:**

* Whenever any app, website, or form asks for your email address, tap the DuckDuckGo keyboard suggestion **"Use Duck Address"** - or manually enter your `@duck.com` address
* DuckDuckGo can also **auto-generate unique one-time aliases** for each service (e.g., `yourname_amazon_5k3j@duck.com`) - these appear as autofill suggestions in DuckDuckGo browser when you tap an email field
* Each unique alias forwards to your Tuta inbox with all trackers removed
* If a specific service starts sending spam or your alias appears in a breach, go to **DuckDuckGo Settings → Email Protection → Manage** and deactivate just that alias - without touching your real address

**What this stops:**
* **Phishers** - they only have a disposable alias, never your real Tuta address
* **Doxxers** - your real inbox cannot be searched for, looked up, or correlated across services
* **Data brokers** - even if five different companies share your email address, they each have a different alias and cannot link your accounts together
* **Email trackers** - DuckDuckGo strips tracking pixels, spy links, and hidden tracking code from every forwarded email before it reaches Tuta

### Part C - Auto-Generated Passwords in DuckDuckGo

DuckDuckGo's browser also includes a **built-in password manager** that integrates with Duck Address setup. Once you have Email Protection enabled:

* When you create a new account on any website in the DuckDuckGo browser, it will **automatically suggest a strong, randomly generated password** for the password field - tap to accept and it is saved to DuckDuckGo's encrypted local password storage
* Saved passwords are stored on-device, encrypted, and sync only to other devices you control via your Duck account (optional)
* On returning to a site, DuckDuckGo autofills both the Duck Address alias and your saved password
* This means for most sign-ups you never manually type or choose a password - the browser generates, saves, and fills it all automatically

**Use IYPS** (App #2) to analyse the strength of any existing passwords you want to migrate, then replace weak ones with DuckDuckGo-generated passwords going forward.

**The complete email security chain:**
```
Website/App → Duck Address alias (unique per service)
    → DuckDuckGo strips email trackers
        → Forwards clean to your Tuta Mail address
            → Tuta decrypts with your encryption key
                → You read it
```
Your real address never appears anywhere outside of the DuckDuckGo Email Protection settings on your own device.

---

## OPSEC Step 5: Spoof Location with Fake Traveler

Physical location is one of the most sensitive data points your phone broadcasts constantly. Fake Traveler lets you replace your real GPS coordinates with any location in the world.

1. Open **Fake Traveler**.
2. Browse the map and select a fake location (tap to pin it, or search for a city/place).
3. Tap **Apply** or **Start mocking**.
4. Go to **Settings → About phone** → tap **Build number** seven times to ensure Developer Options is open.
5. In **Developer Options** → scroll to **Select mock location app** → choose **Fake Traveler**.
6. Your device's GPS will now report the fake location to all apps that request it.

**What this does:** Spoofs your device's geolocation, preventing apps from determining your real position. Combined with InviZible Pro's Tor routing (which masks your IP address from network observers), this gives you both network-level and sensor-level location protection simultaneously.

---

## OPSEC Step 6: Final Checks and Hardening Tips

**InviZible Pro / proxy setup:**
* Confirm InviZible Pro is running (all three services green) and the Android system proxy is set to `127.0.0.1:8118` on your Wi-Fi network.
* Allow InviZible Pro background network access and disable any battery optimization that would kill it (Settings → Battery → Battery optimization → InviZible Pro → Not optimized).
* Allow background network and battery usage for Tuta Mail (for push notifications) the same way.

**AFWall+ check:**
* Open AFWall+ and confirm the default policy is **Deny (White List)** and only the apps you explicitly approved are green.
* Check the Logs tab - you should see blocked connection attempts from apps you did not whitelist. This confirms it is working.

**DuckDuckGo App Tracking Protection:**
* Since InviZible Pro uses proxy mode and does not occupy the Android VPN slot, DuckDuckGo's App Tracking Protection **can and should be enabled**.
* Open DuckDuckGo → ≡ menu → App Tracking Protection → Enable. Android will prompt you to confirm the VPN connection - this is DuckDuckGo using the VPN slot to monitor other apps' tracker traffic. Confirm it.
* Both InviZible Pro (proxy mode) and DuckDuckGo App Tracking Protection will now run simultaneously without conflict.

**Network preference:**
* Use Wi-Fi over mobile data where possible - Tor performance is significantly better on Wi-Fi, and the system proxy applies automatically to saved Wi-Fi networks.
* On public Wi-Fi, AFWall+'s whitelist ensures no unexpected apps phone home even on untrusted networks.

**Periodic maintenance:**
* Run **Hypatia** weekly to scan for malware.
* Run **OONI Probe** to monitor for censorship or network manipulation in your area.
* Run **SD Maid SE** monthly to clear leftover data from removed apps.
* Update all apps via F-Droid regularly - security patches are critical.

**Your device now has a hardened, layered cybersecurity posture reducing risks from surveillance, phishing, malware, censorship, and network-level attacks.**

---

---

# PART 3 - DRESOS DEFENSIVE APP SUITE

## Apps Overview

The following apps form the complete DresOS defensive security application suite. Every single one is open-source, has zero Google dependencies, and is sourced directly from verified developer releases or F-Droid. Together they cover encrypted file storage, password management, private social media, link safety scanning, malware detection, network censorship monitoring, privacy routing, private media, and secure communications - building a complete defensive security ecosystem on your device.

---

## 1. Amaze File Manager - File Management & AES Encryption

**Download:** [F-Droid](https://f-droid.org/packages/com.amaze.filemanager/) / [Direct APK](https://f-droid.org/repo/com.amaze.filemanager_124.apk)

### What It Is

Amaze File Manager is a fully open-source (GPL-3.0) file manager built by Team Amaze. It replaces both a standard file manager and a separate encryption tool in a single app. Available on F-Droid with no Google dependencies, actively maintained, and supports Android 5.0 and up.

### Key Features

- Full file management: cut, copy, move, delete, rename, compress, extract
- Built-in AES-256 encryption and decryption of files and folders
- Biometric lock (fingerprint and face unlock) for the entire app
- Multiple tabs open simultaneously
- Root explorer for advanced system access on rooted devices
- Built-in APK reader, installer, and App Manager
- Built-in ZIP/RAR reader, text editor, and database viewer
- SMB, SSH, FTP and SFTP support for network shares
- No ads, no trackers, no Google dependencies

### Setup and Usage

1. Install via F-Droid.
2. Open Amaze File Manager and grant storage permission.
3. Set as default file manager: Settings > Apps > Default apps > Files > Amaze.

**Enabling biometric lock:**
Go to Amaze Settings > Security > Fingerprint lock. Enable it and enroll your fingerprint.

**Encrypting a file or folder:**
1. Long-press the file or folder you want to protect
2. Tap the overflow menu (three dots) > Encrypt
3. Set a strong password
4. Amaze encrypts it and adds an .aes extension
5. The original is replaced by the encrypted version

**Decrypting:**
1. Tap the .aes file
2. Enter the password
3. Amaze decrypts it in place

**Root explorer:**
Go to Amaze Settings > Root Explorer > Enable. Browse and manage system directories directly.

**App Manager:**
Tap the drawer icon > App Manager. View, backup as APK, or uninstall any installed app directly from Amaze.

### Why It Replaced ZArchiver Pro and Fossify Files

ZArchiver Pro was sourced from an unofficial GitHub mirror and is not available on F-Droid. Fossify Files has no encryption. Amaze covers both everyday file management and AES file encryption in a single GPL-3.0 app on F-Droid.

---

## 2. IYPS - Password Strength Analyser & Generator

**Download:** [IYPS_v1.5.6.apk](https://github.com/StellarSand/IYPS/releases/download/v1.5.6/IYPS_v1.5.6.apk)

### What It Is

IYPS (It's Your Password Security) is an open-source password strength analyser and generator. It does **not** store passwords - that job is handled by DuckDuckGo's built-in password manager (covered in App #14). What IYPS does is tell you exactly how strong or weak any given password is, with real-world crack-time estimates across different attack scenarios, and generate strong random passwords on demand.

### Key Features

* **Password strength analyser** - paste any existing password and get a detailed breakdown: entropy score, estimated crack time under online attack, offline attack, and distributed cracking scenarios
* **Password generator** - creates strong, random passwords with customisable length and character sets
* **Completely offline** - no password ever leaves your device during analysis
* No storage, no vault, no sync - purely an analysis and generation tool
* No ads, no trackers, no telemetry

### Setup and Usage

1. Install via SAI from the direct APK link above.
2. Open IYPS - no account or setup required.
3. **Check an existing password:**
   * Paste it into the analyser field
   * IYPS shows the entropy score and estimated crack time across attack scenarios (online throttled, offline fast hash, distributed brute-force)
   * Anything under "centuries" for an offline attack is worth replacing
4. **Generate a strong password:**
   * Go to the **Generator** tab
   * Set desired length (20+ characters recommended) and character set (uppercase, lowercase, numbers, symbols)
   * Tap generate - copy the result and save it in DuckDuckGo's password manager (see App #14)

### Why It's in the DresOS Suite

Most people massively overestimate how strong their passwords are. IYPS shows you in concrete terms exactly how quickly your password would fall - making it a practical tool for auditing your existing passwords before migrating them to DuckDuckGo's password manager. Use IYPS to generate and vet, DuckDuckGo to store.

---

## 3. RedReader - Private Reddit Client

**Download:** [RedReader-v1.25.1.apk](https://github.com/QuantumBadger/RedReader/releases/download/v1.25.1/RedReader-v1.25.1.apk)

### What It Is

RedReader is a free, open-source Reddit client that removes all advertising, tracking, data collection, and analytics present in the official Reddit app and other third-party clients.

### Key Features

* **No ads whatsoever** - not even Reddit's own promoted posts
* **No tracking or analytics** - no telemetry sent to Reddit or any third party
* **No account required** to browse - full anonymous access to Reddit content
* Optional account login for commenting/voting without Reddit app telemetry
* Clean, fast, highly readable interface
* Full image, GIF, and video support
* Offline reading mode - cache posts for reading without connectivity
* Night mode and customizable text sizes

### Setup and Usage

1. Install via SAI from the direct APK link above.
2. Open RedReader - no setup required to start reading anonymously.
3. **Browse without account:** You can immediately browse subreddits, search, and read posts without logging in.
4. **Optional account login:**
   * Settings → Accounts → Add Account
   * Log in with your Reddit credentials - RedReader does not store these beyond the session token
5. **Customize feed:**
   * Add subreddits to your front page by searching (magnifying glass) and subscribing
   * If not logged in, subreddits are stored locally in the app

### Why It's in the DresOS Suite

The official Reddit app is one of the most aggressive data-harvesting apps available - it requests extensive device permissions, tracks your reading behavior, and feeds data to advertising networks. RedReader provides the same content access with none of the surveillance.

---

## 4. URL Check - Link Scanner

**Download:** [URLCheck-3.4.1.apk](https://github.com/TrianguloY/URLCheck/releases/download/v3.4.1/URLCheck-3.4.1.apk)

### What It Is

URL Check is an open-source link analysis and scanning tool. Before opening any link, URL Check intercepts it and scans it for IP loggers, tracking parameters, malware indicators, phishing patterns, and URL redirects - showing you exactly where a link actually leads before you visit it.

### Key Features

* **IP logger detection** - identifies links from known IP-logging services (grabify, iplogger, etc.)
* **Phishing detection** - flags domains that impersonate legitimate services
* **Malware link scanning** - checks against known malicious URL databases
* **URL unshortener** - expands bit.ly, t.co, and all other shorteners to reveal the real destination
* **Tracking parameter stripping** - removes `utm_source`, `fbclid`, `gclid`, and all other tracking tags from URLs automatically
* **Redirect following** - shows the full redirect chain so you can see every server a link passes through
* VirusTotal integration (optional) - submit URLs to VirusTotal's multi-engine scanner
* Clipboard monitoring (optional) - automatically checks any URL you copy

### Setup and Usage

1. Install via SAI from the direct APK link above.
2. Open URL Check → go through the initial permissions setup (allow it to handle link intents).
3. **Set as default link opener:**
   * In URL Check settings → enable "Intercept links"
   * Android will prompt you to set URL Check as the handler for links - confirm
4. **How it works in practice:**
   * Someone sends you a link in a chat app or email
   * Instead of opening directly in the browser, URL Check intercepts it
   * A screen appears showing: the original URL, expanded URL, any trackers detected, any malware flags
   * Tap **Open** to proceed (if clean) or **Copy** to share the cleaned URL
5. **Tracking removal:**
   * Enable "Remove tracking parameters" in settings
   * All UTM tags, Facebook click IDs, Google analytics parameters, etc. are stripped automatically

### Why It's in the DresOS Suite

Phishing and IP logging are two of the most common attack vectors in targeted surveillance. A single click on an IP logger reveals your real IP address, approximate location, device type, and OS - even if you are using a VPN. URL Check prevents this by scanning every link before it is opened.

---

## 5. Hypatia - Open-Source Antivirus

**Download:** [hypatia_v3.18.apk](https://github.com/MaintainTeam/Hypatia/releases/download/v3.18/hypatia_v3.18.apk)

### What It Is

Hypatia is a fully open-source antivirus and malware scanner for Android, powered by ClamAV signature databases. It scans APKs before installation, scans files on your storage, and can perform full-device scans to detect known malware, trojans, ransomware, and other threats.

### Key Features

* **ClamAV-powered** - uses the same signature database as the industry-standard ClamAV engine trusted by organizations worldwide
* **APK scanning** - scans any APK file before you install it, including those from Aurora Store and direct downloads
* **File scanning** - scans documents, archives, downloaded files, and any other content on your storage
* **Full-device scan** - scans all installed apps and accessible storage in one pass
* **Real-time protection** (optional) - monitors new files as they are created or downloaded
* **Offline capable** - signatures are downloaded locally; scanning does not require internet once signatures are updated
* Zero telemetry, no cloud scanning, no data sent to any server during scans
* Regular signature updates via the app (or F-Droid)

### Setup and Usage

1. Install via SAI from the direct APK link above.
2. Open Hypatia → grant storage permission.
3. **Update signatures first:**
   * Settings → Update signatures → tap Update All
   * Wait for ClamAV, MSRT, and other signature databases to download
4. **Scan a specific APK before installing:**
   * Download an APK via Gopeed
   * Open Hypatia → Scan file → navigate to the downloaded APK → scan
   * If clean: green result → install with SAI
   * If flagged: do not install - delete the file
5. **Full device scan:**
   * Tap the **Scan** button on the main screen → select Full Scan
   * Hypatia scans all installed apps and storage
6. **Real-time monitoring:**
   * Settings → Real-time protection → Enable
   * Hypatia monitors new files as they land on your device and alerts you if anything malicious is detected
7. **Scheduled scans:**
   * Set up weekly scans in Settings → Schedule to run automatically

### Why It's in the DresOS Suite

Side-loading APKs (which is necessary on a de-Googled phone without Play Protect) requires you to take responsibility for your own malware screening. Hypatia with ClamAV signatures provides robust detection of known threats in every APK you install - a critical piece of the defensive stack.

---

## 6. OONI Probe - Network Censorship Detector

**Download:** [ooniprobe_274.apk](https://f-droid.org/repo/org.openobservatory.ooniprobe_274.apk)

### What It Is

OONI Probe (Open Observatory of Network Interference) is a network measurement tool developed by the Tor Project and independent researchers. It detects censorship, surveillance infrastructure, and network manipulation on your internet connection - telling you exactly what your ISP or government is blocking or tampering with.

### Key Features

* **Website blocking detection** - tests whether specific websites are blocked on your network
* **Censorship measurement** - detects DNS manipulation, HTTP blocking, TCP/IP blocking, and TLS interference
* **Surveillance infrastructure detection** - identifies middleboxes (equipment used for network surveillance)
* **VPN and Tor reachability** - tests whether Tor, VPNs, and circumvention tools are blocked
* **Global network data** - contributes your test results to OONI's public database, helping document censorship worldwide
* Runs automatic tests at scheduled intervals
* Completely open-source (Tor Project affiliated)

### Setup and Usage

1. Install from F-Droid using the link above (or search "OONI Probe" in F-Droid).
2. Open OONI Probe → complete the onboarding (agree to data sharing terms - your test results are contributed to the public OONI database anonymously).
3. **Run immediate tests:**
   * Tap **Run** on the main dashboard
   * OONI Probe runs the full test suite: websites, instant messaging, circumvention tools, middleboxes
4. **View results:**
   * Results appear under the **Test Results** tab
   * Green checkmarks = no blocking detected
   * Red X = blocking or manipulation detected
   * Orange warning = anomaly (possible interference)
5. **Automated testing:**
   * Settings → Automated testing → Enable
   * Set a schedule (e.g., daily at 3am on Wi-Fi only)
6. **Interpret results:**
   * If websites you expect to be accessible show as blocked → your ISP/government is censoring them → use InviZible Pro's Tor routing to bypass
   * If circumvention tools (Tor, VPNs) show as blocked → use InviZible Pro's Tor bridge mode with obfs4 bridges (see OPSEC Step 2)

### Why It's in the DresOS Suite

Understanding your threat environment is the first step in defending against it. OONI Probe tells you exactly what your network operator is doing to your traffic - whether you are in a heavily censored country or simply want to know if your ISP is silently blocking certain content.

---

## 7. InviZible Pro - Tor + I2P + DNSCrypt in Proxy + Root Mode

**App Store Name:** InviZible Pro
**Download:** [pan.alexander.tordnscrypt.stable_26603.apk](https://f-droid.org/repo/pan.alexander.tordnscrypt.stable_26603.apk)

*(Full step-by-step setup in Part 2 - OPSEC Step 2)*

### What It Is

**InviZible Pro** is the network privacy engine of the DresOS stack. It is a single application combining **Tor**, **I2P**, and **DNSCrypt** - running in **proxy + root mode**, which means it does not use the Android VPN slot and can run alongside DuckDuckGo App Tracking Protection simultaneously. It routes your traffic through Tor via Android's system proxy setting, giving every app that respects the system proxy a Tor exit node as its apparent IP address. Root access via Magisk lets it redirect all DNS at the iptables level - nothing bypasses DNSCrypt.

### What It Replaces

| Without InviZible Pro | With InviZible Pro |
|---|---|
| Orbot (Tor client - separate app) | Built-in Tor with isolation settings |
| Standalone DNSCrypt app | Built-in DNSCrypt with DNSSEC |
| Standalone I2P client | Built-in I2P with garlic routing |
| Plain-text DNS leaking to ISP | All DNS encrypted via iptables redirect |
| Real IP visible to websites | Tor exit node IP visible instead |

### Summary of Features

* **Tor** - three-hop onion routing for strong anonymity; circuit isolation per destination; obfs4 bridge support for censored networks
* **I2P** - independent garlic-routing network for I2P eepsites and a distinct anonymity model from Tor
* **DNSCrypt** - encrypts and authenticates all DNS queries via a local resolver on port 5354; prevents ISP-level domain surveillance
* **DNSSEC** - cryptographically validates every DNS response; blocks spoofed DNS answers
* **Root / iptables DNS redirect** - forces all device DNS (port 53 → 5354) through DNSCrypt at the kernel level; no app can bypass it
* **Proxy mode** - exposes Tor as HTTP proxy on `127.0.0.1:8118` and SOCKS5 on `127.0.0.1:9050`; Android system proxy routes all compatible app traffic through Tor without touching the VPN slot
* **Start on boot** - restores all services and iptables rules automatically after every reboot
* Works alongside DuckDuckGo App Tracking Protection (VPN slot is completely free)

*(Full setup guide covering all 9 phases - proxy mode config, DNS server selection, Tor isolation settings, I2P bootstrap, root iptables activation, Android system proxy IP spoofing, DuckDuckGo ATP enablement, and verification - is in Part 2 - OPSEC Step 2.)*

---

## 8. AFWall+ - Root-Level iptables Firewall

**Download:** F-Droid - [dev.ukanth.ufirewall](https://f-droid.org/packages/dev.ukanth.ufirewall/) / [GitHub](https://github.com/ukanth/afwall)

*(Full setup and custom rules in Part 2 - OPSEC Step 3)*

### What It Is

**AFWall+** (Android Firewall+) is a root-level firewall that uses **iptables** - the Linux kernel's native packet filtering framework - to control internet access per app, per network interface. It operates at the kernel level, below all apps, VPNs, and proxy layers. An app cannot circumvent it by any means: iptables rules are enforced by the kernel before traffic ever reaches a network socket.

AFWall+ is a completely separate and complementary layer to InviZible Pro. InviZible Pro handles routing and anonymisation - AFWall+ handles access control. Together they give defence in depth: even if InviZible Pro's proxy routing is temporarily unavailable or an app ignores the system proxy, AFWall+ still enforces which apps are allowed to communicate at all.

### Key Features

* **Per-app, per-interface rules** - separately control Wi-Fi, mobile data, and VPN for every installed app
* **Whitelist mode** - default deny; only explicitly approved apps can reach the internet
* **Custom iptables rules** - write raw iptables rules for advanced control (e.g. mobile data proxy redirect to port 8118)
* **Logging** - logs every blocked connection attempt; reveals apps trying to phone home
* **PIN lock** - locks the firewall UI so no app or accidental tap can disable rules
* **LAN control** - optionally block apps from accessing your local network
* Works on any rooted Android device with Magisk; no VPN slot consumed

### Why It's in the DresOS Suite

Every de-Googled phone still has apps that attempt to reach the internet - analytics libraries embedded in third-party APKs, microG service pings, update checkers, background sync services. InviZible Pro routes traffic anonymously but does not block apps from connecting. AFWall+ fills this gap: apps you have not explicitly approved cannot generate any network traffic at all. It also solves the mobile data proxy gap - Android's system proxy applies to Wi-Fi only, but AFWall+'s custom iptables rules can redirect HTTP/HTTPS traffic on mobile data interfaces through InviZible Pro's Tor proxy on port 8118 as well.

*(Full configuration, recommended per-app rules table, and custom iptables rules for mobile data proxy routing are in Part 2 - OPSEC Step 3.)*

---

## 9. Metrolist - Private YouTube Music Client

**Download:** [Metrolist.apk](https://github.com/MetrolistGroup/Metrolist/releases/download/v13.3.0/Metrolist.apk)

### What It Is

Metrolist is a privacy-focused, modded YouTube Music client. It provides access to YouTube Music's full catalogue - including all your existing playlists and library if you have a YouTube Music account - without Google's tracking, advertising, or data collection.

### Key Features

* **Ad-free YouTube Music** - no pre-roll ads, no mid-song interruptions, no audio ads
* **No tracking** - Google Analytics, advertising identifiers, and telemetry calls are removed
* **Background playback** - music plays when the screen is off or when using other apps (normally requires a paid YouTube Music Premium subscription)
* **Audio quality control** - select specific bitrates and codec preferences
* **Offline capability** - cache songs locally for offline listening
* **Account support** - optionally log in with a YouTube/Google account to access your playlists and liked songs (note: if you log in, Google still knows what you are listening to - use without account for maximum privacy)
* Custom theme support, AMOLED dark mode

### Setup and Usage

1. Install via SAI from the direct APK link above.
2. Open Metrolist.
3. **Without account (maximum privacy):**
   * Browse and search the full YouTube Music catalogue freely
   * Your listening history is stored locally only
4. **With account (access your library):**
   * Tap the account icon → sign in via **DuckDuckGo Privacy Browser** with your Google account
   * Your playlists, liked songs, and subscriptions will sync
   * Understand that YouTube/Google will still log your listening activity server-side when you are logged in
5. **Background playback:**
   * Just minimize the app - music continues playing without needing any subscription
6. **Cache for offline listening:**
   * Long-press a song or playlist → Download → select audio quality
   * Downloaded files are stored in your chosen directory (manage with Amaze File Manager)

### Why It's in the DresOS Suite

Music streaming should not come with a surveillance apparatus. Metrolist gives you full access to YouTube Music's catalogue without surrendering your listening habits to Google's advertising engine.

---

## 10. Arcane Chat - Decentralized Encrypted Messaging

**Download:** [ArcaneChat-foss-arm64-v8a-2.43.0.apk](https://github.com/ArcaneChat/android/releases/download/v2.43.0/ArcaneChat-foss-arm64-v8a-2.43.0.apk)

### What It Is

Arcane Chat is a fully decentralized, end-to-end encrypted messaging application built on the **Delta Chat** protocol, which uses **email as the transport layer** with **OpenPGP end-to-end encryption**. This means your messages are transmitted over email infrastructure - making them extremely difficult to block, and giving you full control of your identity (your email address) without depending on any centralized messaging server.

### Key Features

* **End-to-end encryption** via OpenPGP (Autocrypt standard) - messages are encrypted on your device before transmission and can only be decrypted by the recipient
* **Decentralized** - no central Arcane Chat server; messages route through standard email servers. There is no single point of failure or surveillance
* **No phone number required** - your identity is your email address (use your Tuta Mail address for full privacy)
* **No metadata harvesting** - unlike Signal, WhatsApp, or Telegram, there is no central server collecting who you talk to, when, or how frequently
* **Censorship resistant** - email infrastructure is nearly impossible to block globally; even in censored regions, email typically works
* Group chats, image/file sharing, voice messages
* Multiple accounts (use different email addresses for different contexts)
* FOSS (fully open-source), no ads, no tracking

### Setup and Usage

1. Install via SAI from the direct APK link above.
2. Open Arcane Chat.
3. **Configure with Tuta Mail (recommended):**
   * Tap **Add Account**
   * Enter your Tuta Mail address
   * Arcane Chat will attempt auto-configuration; for Tuta you may need to enter IMAP/SMTP settings manually:
     * IMAP server: `imap.tuta.com` | Port: `993` | Security: TLS
     * SMTP server: `smtp.tuta.com` | Port: `465` | Security: TLS
   * Enter your Tuta password → Connect
4. **Start a conversation:**
   * Tap the **+** button → enter a contact's email address → write message → send
   * If the recipient also uses Arcane Chat (or any Autocrypt-compatible app), messages are automatically end-to-end encrypted - look for the **lock icon**
5. **Verify encryption:**
   * Open a chat → tap the contact name → **Encryption info**
   * Verify the displayed fingerprint matches your contact's (do this via a separate channel, e.g., in person or via phone call)
6. **Group chats:**
   * Tap + → New Group → add contacts by email → name the group → create
7. **Disappearing messages:**
   * Open a chat → Settings → Disappearing Messages → set a timer

### Why It's in the DresOS Suite

Most encrypted messengers require you to trust a central server (Signal's servers, Telegram's servers, WhatsApp/Meta's servers). Arcane Chat removes this trust requirement entirely - your messages flow over email infrastructure you choose, encrypted with OpenPGP that only you and your recipient hold keys for. Using it with Tuta Mail combines the strongest available email privacy with the strongest available decentralized messaging protocol.

---

## 11. Aurora Store - Anonymous App Store

**Download:** Install via F-Droid - search "Aurora Store" or go to `https://f-droid.org/packages/com.aurora.store/`

### What It Is

Aurora Store is an open-source, unofficial Google Play Store client that lets you browse, download, and update apps from Google's Play catalogue **without a Google account** and without any Google Play Services telemetry. It is a full Google Play replacement with complete anonymity.

### Key Features

* **Anonymous mode** - browse and download apps using randomly generated anonymous credentials. Google sees an anonymous device, not your account.
* No Google account login required
* Full Play Store catalogue access - any app available on Google Play can be downloaded
* App update management - check for updates for all Play-sourced apps
* **Exodus Privacy integration** - shows you a detailed tracker and permission report for every app before you install it
* Block automatic app updates for specific apps
* Spoofed device profile - can present as a different Android device to access region-locked apps
* No ads, no telemetry, zero Google components in the Aurora app itself

### Setup and Usage

1. Install via F-Droid.
2. Open Aurora Store → select **Anonymous** mode during setup. **Do not log in with a Google account.**
3. Browse, search, and install apps exactly as you would on the Play Store.
4. **Before installing any app, check its tracker report:**
   * Tap the app → scroll down to **Privacy** → check the Exodus Privacy report
   * Apps with many trackers (advertising, analytics, fingerprinting) should be avoided or replaced with FOSS alternatives
5. **Manage updates:**
   * Aurora Store → Updates tab → review available updates
   * Update FOSS apps via F-Droid instead (F-Droid verifies signatures; Aurora Store does not independently verify)

### Why It's in the DresOS Suite

Completely removing Play Store access is impractical for many users - some apps (banking, 2FA, essential services) only exist on Google Play. Aurora Store provides access to this catalogue without surrendering your identity or using Google Play Services. The Exodus Privacy integration helps you make informed decisions about which apps are safe to install.

---

## 12. Fossify Suite - Complete FOSS System Apps

**Download:** All via F-Droid - search each app name or visit [fossify.org/apps](https://www.fossify.org/apps/)

### What It Is

Fossify is a suite of open-source replacements for every core Android system app that Google ships with proprietary, tracking-laden stock versions. Together, the Fossify suite replaces the entire Google-provided app layer with fully open-source, zero-telemetry equivalents.

### Apps in the Suite

| App | Replaces | Features |
|---|---|---|
| **Fossify Phone** | Google/Stock Dialer | Open-source dialer, call blocking, call recording (where legal), no analytics |
| **Fossify Messages** | Google Messages | Open-source SMS/MMS, no Google RCS, no message scanning |
| **Amaze File Manager** | Files by Google | Local file manager, no cloud sync, no Google One integration |
| **Fossify Gallery** | Google Photos | Local gallery, no cloud backup to Google, no AI face scanning |
| **Fossify Calendar** | Google Calendar | Local calendar, optional CalDAV sync with your own server |
| **Fossify Launcher** | Pixel Launcher | Open-source home screen, no Google Feed integration |
| **Fossify Contacts** | Google Contacts | Local contacts, no sync to Google's servers |
| **Fossify Music** | YouTube Music / Play Music | Fully local music player, supports all common audio formats |
| **Fossify Clock** | Google Clock | Alarm, timer, stopwatch - zero telemetry |
| **Fossify Notes** | Google Keep | Local encrypted notes, no cloud sync |

### Setup

1. Install all Fossify apps via F-Droid (search individually or visit the Fossify F-Droid page).
2. Set each as the default for its function in **Settings → Apps → Default apps**.
3. Transfer your data:
   * **Contacts:** Export from old app as `.vcf` → import into Fossify Contacts
   * **Calendar:** Export as `.ics` → import into Fossify Calendar
   * **Photos:** Already on your storage - open with Fossify Gallery
   * **Music:** Already on your storage - open with Fossify Music

### Why It's in the DresOS Suite

Every stock Google app (Dialer, Messages, Files, Gallery, Calendar) contains telemetry that reports usage data, contact metadata, and behavioral information back to Google's servers. Fossify apps are clean replacements with identical functionality and zero reporting.

---

## 13. HeliBoard - Offline Keyboard

**Download:** F-Droid - search "HeliBoard" or visit `https://f-droid.org/packages/helium314.keyboard/`

### What It Is

HeliBoard is a fully open-source, completely offline keyboard for Android. It is a direct replacement for Google's Gboard, which is one of the most invasive data-collection tools on Android - it can capture everything you type (passwords, messages, searches) and transmit it to Google's servers for "personalization" and other undisclosed purposes.

### Key Features

* **Completely offline** - zero network access, zero data transmission
* Full multilingual support with offline dictionaries
* Swipe typing (gesture input)
* Auto-correction and predictive text (fully local)
* Customizable themes, layouts, and key sizes
* Voice input support via local speech-to-text (no cloud)
* No analytics, no crash reporting, no permissions beyond keyboard input
* Actively developed and maintained

### Setup and Usage

1. Install via F-Droid.
2. Open HeliBoard → follow the setup wizard to enable it as a system keyboard.
3. Go to **Settings → General management → Keyboard list and default** → select HeliBoard.
4. Disable Gboard (or remove it during the debloating step).
5. Download offline language dictionaries for your languages within HeliBoard settings.

---

## 14. Tuta Mail - Encrypted Email

**Download:** F-Droid - `https://f-droid.org/packages/de.tutao.tutanota/`

*(Full setup in Part 2 - OPSEC Step 3)*

### What It Is

Tuta Mail (formerly Tutanota) is an end-to-end encrypted email service operated by a German company under German and EU privacy law. It is a full Gmail replacement with zero data mining, zero ads, and full content encryption.

### Key Features

* **End-to-end encryption** for all emails (both subject and body, not just body like most providers)
* **Zero-knowledge** - Tuta cannot read your emails; they are encrypted before leaving your device
* **Encrypted contacts and calendar** included in the free plan
* Encrypted search (searches happen locally on your device, never on the server)
* **Email aliases** for compartmentalization (use different aliases for different purposes)
* Secure external email (password-protected messages to non-Tuta recipients)
* German/EU jurisdiction - GDPR compliance, no US FISA court exposure
* Free plan available; paid plans add more aliases and storage
* Fully open-source client (iOS and Android)

---

## 15. DuckDuckGo Privacy Browser - Primary Browser

**Download:** F-Droid - `https://f-droid.org/packages/com.duckduckgo.mobile.android/`

### What It Is

DuckDuckGo Privacy Browser is a privacy-focused web browser with built-in tracker blocking, a private search engine as default, a one-tap Fire Button to wipe all browsing data, a built-in password manager, and **Duck Address** email protection. It is the primary browser in the DresOS setup, replacing Chrome entirely.

### Key Features

* **Private search by default** - DuckDuckGo search engine, no user profiles, no search history tracking
* **Tracker blocking** - blocks known advertising and analytics trackers on every page
* **HTTPS upgrading** - automatically upgrades connections to HTTPS where available
* **Fire Button** - one tap destroys all tabs, cookies, and browsing history instantly
* **Cookie pop-up management** - automatically handles GDPR cookie banners
* **Duck Address / Email Protection** - generates private `@duck.com` forwarding aliases; strips trackers from forwarded emails before they reach your Tuta inbox; no phone number required, just your encryption key
* **Built-in password manager** - stores encrypted passwords locally; auto-generates strong unique passwords when you create accounts on websites
* **GPC (Global Privacy Control)** - broadcasts a legal opt-out signal to websites
* No Google account required, no telemetry

### App Tracking Protection - Enable This

DuckDuckGo's **App Tracking Protection** monitors tracker activity inside all other apps on your phone using the Android VPN slot. **Enable this feature.** InviZible Pro runs in proxy + root mode and does not use the Android VPN slot at all - there is zero conflict. Both run simultaneously and independently:

* **InviZible Pro** (proxy + iptables) → handles network-level anonymity: Tor IP routing, DNSCrypt, I2P
* **DuckDuckGo App Tracking Protection** (VPN slot) → handles app-layer tracker blocking: stops trackers inside installed apps from phoning home

You get both layers active at the same time, which is the full DresOS defensive stack.

### Setup and Usage

1. Install via F-Droid.
2. Open DuckDuckGo → complete onboarding.
3. When it asks about App Tracking Protection → **Enable it**. Android will prompt you to allow a VPN connection for DuckDuckGo - tap OK. InviZible Pro uses proxy + root mode and does not occupy the VPN slot, so there is no conflict.
4. Set as default browser: Settings → Apps → Default apps → Browser → DuckDuckGo.

**Setting up Duck Address (Email Protection):**

5. Tap **≡ menu → Settings → Email Protection → Get Started**.
6. When prompted for a forwarding address, enter your **Tuta Mail address**.
7. You will receive a `@duck.com` alias. **No phone number required** - your account is secured with an encryption key only. Write that key down and store it safely alongside your Tuta recovery code.
8. From now on, use your Duck Address (or auto-generated per-site aliases) for every sign-up on every app and website - see full setup guide in OPSEC Step 4.

**Using the built-in password manager:**

9. When you create an account on any website, DuckDuckGo will **automatically suggest a strong generated password** in the password field - tap to accept.
10. The password is saved to DuckDuckGo's encrypted on-device storage automatically.
11. Next time you visit that site, DuckDuckGo autofills both your Duck Address alias and your saved password - you never have to remember or type either one.
12. Use **IYPS** (App #2) to analyse the strength of any older passwords you want to check before migrating them.

**Fire Button:**

13. Tap the flame icon at any time to wipe all tabs, cookies, and browsing history instantly - useful before handing your phone to someone or when switching contexts.

---

## 16. AOSmium - System WebView (Privacy-Hardened WebView Engine)

**Installed via:** [DresOS AOSmium WebView Module v2.1.0](https://github.com/DresOperatingSystems/DresOS-Magisk-Modules/releases/latest) - flash in Magisk, handles everything automatically

**APK source:** [codeberg.org/AXP-OS/app_aosmium/releases](https://codeberg.org/AXP-OS/app_aosmium/releases)

*(Full setup in Part 1 - Step 7)*

### What It Is

AOSmium is the **System WebView** in the DresOS stack - not a browser you open directly. Built by the AXP.OS Project, it is a Chromium fork hardened with GrapheneOS/Vanadium security patches throughout, with Google services and anti-features stripped out.

The **Android System WebView** is used internally by hundreds of apps whenever they render web content - social media feeds, email clients, news apps, microG sign-in screens, and more. Replacing it with AOSmium means all of that embedded web content runs through a privacy-hardened, Google-free engine.

**DuckDuckGo Privacy Browser** is your everyday browser. **AOSmium** runs silently as the engine powering web content inside every other app on the device.

### Installation

The DresOS Magisk module handles the full installation:
- Drops the signed AOSmium APK into the systemless tree at `system/product/app/AOSmiumWebView/` via Magisk magic mount
- Places a static RRO in the systemless overlay partition that allowlists AOSmium plus the AXP.OS signing certificate in the framework `config_webview_packages` resource
- Activates AOSmium via `cmd webviewupdate set-webview-implementation` after boot complete, with `dumpsys` verification

No manual step is required after the reboot. To confirm activation, run `adb shell dumpsys webviewupdate | grep "Current WebView package"`. The expected output is `org.axpos.aosmium_wv`.

### Features

* Chromium-based with Vanadium (GrapheneOS) security patches
* Google anti-features stripped throughout
* Hardens web content rendering for every app on the device simultaneously
* Package: `org.axpos.aosmium_wv` - unique name, no package conflicts with any stock WebView
* The OEM WebView (`com.google.android.webview` on Pixel/GMS, `com.android.webview` on AOSP/LineageOS) is kept intact as a fallback in the RRO allowlist, so removing the module returns the device to its original provider with no manual cleanup

---

---

# MAGISK MODULES - AUTOMATION ROADMAP

The DresOS Magisk Modules project ([github.com/DresOperatingSystems/DresOS-Magisk-Modules](https://github.com/DresOperatingSystems/DresOS-Magisk-Modules)) is building towards automating the full system build with flashable modules. The goal is to eventually replace most manual steps in this guide with a single sequence of Magisk flashes.

| Module | What it automates | Status |
|--------|-------------------|--------|
| `dresoswv` - AOSmium WebView | System WebView replacement - drops AOSmium into systemless tree, allowlists it via static RRO, activates via cmd webviewupdate, includes bootloop sentinel | ✅ Released v2.1.0 |
| `dresosmicrog` - Noogle microG | Google Play Services replacement | 🔨 In active development |
| `dresosdebloat` - DresOS Debloater | Core Google app and system bloat removal | 🔨 Planned |
| `dresosperms` - Permissions Hardener | Revoke dangerous permissions from system apps at flash time | 🔨 Planned |
| `dresosafwall` - AFWall+ Bootstrap | Pre-configured iptables default-deny whitelist rules | 🔨 Planned |
| `dresosoverlay` - Privacy Overlay | Disable telemetry flags, advertising IDs, and unwanted sensors at system level | 🔨 Planned |
| `dresosfossify` - Fossify Installer | Install Fossify suite as system apps | 🔨 Planned |
| `dresosheliboard` - HeliBoard System | Install HeliBoard as the default system keyboard | 🔨 Planned |

Each module will be independently flashable so users can pick exactly what they need, or flash the full sequence for a complete automated DresOS build.

Follow the module repo for updates, and open an issue there to request a module or report problems.

---

---

# APPENDIX

## Compatible Devices

Tested and confirmed working for the full DeGoogling + OPSEC stack:

```
- Motorola Moto G32 on LineageOS Android 15 (custom TWRP recovery available on DresOS GitHub profile)
- Motorola ThinkPhone on Stock Android 15
- Motorola Moto G7 Plus on Stock Android 10
- Motorola Moto G7 Plus on LineageOS Android 15
- Samsung Galaxy A05s on Stock Android 10
```

**If this works on your device, please report it with proof to the DresOS team** so we can add it to the official compatibility list. Include: device model, Android version, TWRP version used, which steps you completed, and the output of `adb shell dumpsys webviewupdate | grep Current` after the AOSmium module is active.

Device-specific debloat lists and flashable ZIPs are available for the above devices. Drop your exact model in the Issues tab and the DresOS team will add a custom guide or resource for your device.

---

## Contact DresOS

* **Website:** https://dresoperatingsystems.github.io/
* **Issues & Device Requests:** Use the GitHub Issues tab on this repository

---

## License

MIT License - see [LICENSE](LICENSE) file.

---

## Made with ❤️ for privacy by DresOS

**You now have a 100% de-Googled, operationally secure Android device** - no Google apps, no Google Services, AOSmium (or Vanadium) system WebView, Tor + I2P + DNSCrypt via InviZible Pro proxy + root mode, root-level iptables firewall via AFWall+, app-layer tracker blocking via DuckDuckGo App Tracking Protection, and a complete defensive security app suite. Every tool is open-source, every APK is sourced directly from verified developers, and every component is designed to give you full control of your own device, your own data, and your own privacy.

---

*Topics: android · open-source · foss · twrp · magisk · degoogle · degoogled-android · opsec · cybersecurity · android-security · privacy · defensive-security · tor · i2p · dnscrypt · dresoS*
