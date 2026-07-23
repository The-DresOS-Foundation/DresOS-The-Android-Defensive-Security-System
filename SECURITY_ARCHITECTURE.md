# DresOS Android Defensive Security System
## Full Security Architecture

---

```
╔══════════════════════════════════════════════════════════════════╗
║              DRESOS SECURITY STACK - FULL ARCHITECTURE           ║
╚══════════════════════════════════════════════════════════════════╝

┌─────────────────────────────────────────────────────────────────┐
│  LAYER 1 - KERNEL (AFWall+)                                     │
│                                                                  │
│  AFWall+ - iptables firewall running at Linux kernel level      │
│  ├── Default-deny whitelist: all apps blocked unless approved   │
│  ├── Per-app rules: separate Wi-Fi / mobile data / VPN toggle   │
│  ├── Custom iptables: redirects mobile data HTTP/HTTPS          │
│  │   traffic -> 127.0.0.1:8118 (InviZible Pro Tor proxy)        │
│  ├── Logs every blocked connection attempt                      │
│  └── PIN-locked so nothing can disable it accidentally          │
│                                                                  │
│  Cannot be bypassed by any app. Works below all other layers.   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 2 - NETWORK (InviZible Pro - Proxy + Root Mode)          │
│                                                                  │
│  DNS - DNSCrypt                                                  │
│  ├── iptables rule: ALL outbound DNS (port 53) -> port 5354      │
│  ├── DNSCrypt encrypts and authenticates every DNS query        │
│  ├── DNSSEC: cryptographic validation - rejects spoofed answers │
│  ├── Resolvers: nolog + nodump + dnssec required                │
│  │   Recommended: Quad9 (Swiss non-profit) / Mullvad            │
│  │   Blocked: Cloudflare, Google DNS                            │
│  └── Your ISP cannot see any domain you visit                   │
│                                                                  │
│  Traffic Anonymisation - Tor                                     │
│  ├── Local HTTP proxy: 127.0.0.1:8118                           │
│  ├── Local SOCKS5 proxy: 127.0.0.1:9050                         │
│  ├── Three-hop onion routing: entry -> middle -> exit             │
│  ├── Each hop knows only previous + next - no full picture      │
│  ├── Circuit isolation: each destination = separate circuit     │
│  ├── Your IP to all websites = Tor exit node IP                 │
│  ├── Real IP: hidden. ISP IP: hidden. Location: hidden.         │
│  └── obfs4 bridges available for censored networks              │
│                                                                  │
│  Hidden Services - I2P                                          │
│  ├── Local HTTP proxy: 127.0.0.1:4444                           │
│  ├── Garlic routing: independent of Tor network                 │
│  ├── Access .i2p hidden services (eepsites)                     │
│  └── Different anonymity model - resists timing correlation     │
│                                                                  │
│  No VPN slot used. Android VPN slot remains completely free.    │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 3 - ANDROID SYSTEM PROXY                                 │
│                                                                  │
│  Wi-Fi proxy: hostname 127.0.0.1 / port 8118                   │
│  └── All apps respecting system proxy route through Tor         │
│                                                                  │
│  Mobile data: covered by AFWall+ custom iptables (Layer 1)      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 4 - VPN SLOT (DuckDuckGo App Tracking Protection)        │
│                                                                  │
│  ├── Uses Android VPN slot (free because InviZible Pro doesn't) │
│  ├── Monitors all installed apps for tracker connections        │
│  ├── Blocks advertising, analytics, and fingerprinting trackers │
│  │   inside apps before they can phone home                     │
│  └── Runs simultaneously with InviZible Pro - no conflict       │
│                                                                  │
│  Independent of Layers 1–3. Adds app-layer tracker blocking     │
│  on top of network-level anonymity.                             │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 5 - SYSTEM WEBVIEW                                       │
│                                                                  │
│  What it is: the browser engine used by every app that displays │
│  web content internally - news apps, social media, email        │
│  clients, shopping apps, microG sign-in pages, and hundreds     │
│  more. Replaced entirely via a single Magisk module flash.      │
│                                                                  │
│  INSTALLED BY - DresOS WebView Module v2.2.0                    │
│  ├── Repo: github.com/The-DresOS-Foundation/DresOS-Magisk-Modules│
│  ├── Single flash handles entire installation process:          │
│  │   ├── Validates Magisk 29.0+, Android 10-16, arm64           │
│  │   ├── Aborts on x86/x86_64 and 32-bit arm (arm64 only)       │
│  │   ├── Aborts on APEX WebView devices (forward-looking guard) │
│  │   ├── Single signed arm64 APK, no per-ABI APK                │
│  │   │   selection (DresOS WebView is arm64 only)                │
│  │   ├── Drops APK into systemless tree at                       │
│  │   │   system/product/app/DresOSWebView/ via magic mount      │
│  │   ├── Places static RRO in systemless overlay partition that │
│  │   │   adds org.dresos.webview + DresOS RSA certificate       │
│  │   │   to framework config_webview_packages allowlist         │
│  │   ├── OEM WebView (com.google.android.webview /              │
│  │   │   com.android.webview) kept as fallback in allowlist     │
│  │   ├── service.sh runs cmd webviewupdate set-webview-          │
│  │   │   implementation after sys.boot_completed                │
│  │   ├── Activation verified via dumpsys webviewupdate          │
│  │   └── No pm install. No .replace markers on OEM WebView.     │
│  │                                                               │
│  ├── BOOTLOOP SAFETY - two independent layers:                   │
│  │   ├── post-fs-data sentinel: boot_pending marker dropped     │
│  │   │   every boot, cleared by service.sh on success. Stale    │
│  │   │   marker on next boot = previous crash, module auto-     │
│  │   │   disables via /data/adb/modules/dresoswv/disable         │
│  │   └── Inert mode: activation failure touches inert flag,     │
│  │       prevents retry storms on subsequent boots              │
│  │                                                               │
│  └── Logs:                                                       │
│      ├── /data/adb/modules/dresoswv/logs/install.log            │
│      ├── /data/adb/modules/dresoswv/logs/boot.log               │
│      ├── /data/adb/modules/dresoswv/logs/service.log            │
│      └── /data/adb/modules/dresoswv/webview_activation.log      │
│                                                                  │
│  WEBVIEW ENGINE - DresOS WebView (Cromite based)                │
│  ├── Package: org.dresos.webview                                │
│  ├── Chromium 145.0.7632.120 (Cromite base)                     │
│  ├── Cromite privacy and security hardening applied             │
│  ├── Google services and anti-features stripped throughout      │
│  └── Cert SHA-256: B7815F746C3183C66CAD631079CB669C0494A8BE54   │
│      2A2A96E5975D8FABCD92DE (DresOS RSA 4096)                   │
│                                                                  │
│  LICENSE - Module: GPL-3.0 | DresOS WebView: GPL-3.0            │
│                                                                  │
│  After flash and reboot: activation is automatic. Verify with    │
│  adb shell dumpsys webviewupdate | grep "Current WebView"        │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 6 - BROWSER (DuckDuckGo Privacy Browser)                 │
│                                                                  │
│  ├── Private search by default - no user profiles               │
│  ├── Tracker blocking on every page                             │
│  ├── HTTPS upgrading - forces secure connections                │
│  ├── App Tracking Protection - enabled (uses VPN slot)          │
│  ├── Duck Address: @duck.com aliases -> forward to Tuta Mail     │
│  │   └── Real email address never given to any service          │
│  ├── Built-in password manager: auto-generates strong passwords │
│  │   per site, encrypted local storage, autofill               │
│  ├── Fire Button: one tap destroys all tabs + browsing data     │
│  └── GPC (Global Privacy Control) signal sent to all sites      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 7 - EMAIL (Tuta Mail + Duck Addresses)                   │
│                                                                  │
│  Tuta Mail                                                       │
│  ├── End-to-end encrypted: subject + body + metadata            │
│  ├── Zero-knowledge: Tuta cannot read your emails               │
│  ├── German jurisdiction (DSGVO/GDPR)                           │
│  ├── No phone number required - secured by encryption key only  │
│  └── Recovery: encryption key stored physically off-device      │
│                                                                  │
│  Duck Address (DuckDuckGo Email Protection)                     │
│  ├── Unique @duck.com alias generated per service               │
│  ├── Real Tuta address never given to any website or app        │
│  ├── DuckDuckGo strips email trackers before forwarding         │
│  ├── Any alias can be deactivated independently if compromised  │
│  └── No phone number required - secured by encryption key only  │
│                                                                  │
│  Flow:                                                           │
│  Website -> Duck alias -> tracker stripped -> Tuta inbox           │
│  └── Real address never exposed anywhere                        │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 8 - OS BASELINE                                          │
│                                                                  │
│  Private DNS                                                     │
│  └── Quad9 DNS-over-TLS (dns.quad9.net) - fallback when        │
│      InviZible Pro is not active                                │
│                                                                  │
│  MAC Randomisation                                               │
│  ├── Randomised MAC per Wi-Fi network                           │
│  ├── Non-persistent MAC enabled in Developer Options            │
│  └── Device name broadcast disabled                             │
│                                                                  │
│  GPS Spoofing - DresSecureComms                                  │
│  ├── Mock GPS location set to anywhere in the world             │
│  └── All apps requesting location receive the fake coordinates  │
│                                                                  │
│  Google Services - DresOS microG Module v3.0.0                  │
│  ├── Full replacement for Google Play Services                  │
│  ├── Apps requiring Play Services work without Google tracking  │
│  ├── Pure file overlay: officially signed microG in priv-app    │
│  │   GmsCore 0.3.15, Companion, GsfProxy, DroidGuard,           │
│  │   Aurora Store, Aurora Services                              │
│  ├── Signature spoofing provided by the ROM via the official    │
│  │   microG key. No Zygisk, no Xposed, no LSPosed, any ABI      │
│  ├── Cert verified self heal: X.509 SHA-256 from cmd package    │
│  │   dump, no .RSA blob hashing                                 │
│  ├── No boot-time PackageManager work, cannot bootloop          │
│  ├── Aurora Services priv-app enables silent app installs       │
│  └── ROMs that already ship microG (Calyx, iode, eOS, Lineage   │
│      for microG) detected by cert match, ROM copy preserved     │
└─────────────────────────────────────────────────────────────────┘


╔══════════════════════════════════════════════════════════════════╗
║                    RESILIENCE MODEL                              ║
╚══════════════════════════════════════════════════════════════════╝

Every layer is independent. If one layer is temporarily
unavailable, all others continue protecting you.

  AFWall+ down?          -> InviZible Pro + DDG ATP still active
  InviZible Pro down?    -> AFWall+ still blocking unauthorised apps
                           Quad9 DNS-over-TLS still active as fallback
  DDG ATP down?          -> InviZible Pro Tor routing still active
  Wi-Fi proxy bypassed?  -> AFWall+ iptables catches it at kernel level
  WebView breached?      -> Network layers contain any damage
  Email alias exposed?   -> Deactivate alias, real address untouched

No single point of failure in the stack.


╔══════════════════════════════════════════════════════════════════╗
║                  WHAT EACH THREAT HITS                          ║
╚══════════════════════════════════════════════════════════════════╝

  Your real IP address        -> Blocked by InviZible Pro (Tor)
  DNS surveillance            -> Blocked by DNSCrypt + DNSSEC (iptables)
  In-app trackers             -> Blocked by DDG App Tracking Protection
  WebView data harvesting     -> Blocked by DresOS WebView (DresOS WebView Module)
  GPS tracking                -> Blocked by DresSecureComms Geo Spoofer
  Wi-Fi MAC tracking          -> Blocked by MAC randomisation
  Phishing via email          -> Blocked by Duck Address aliases
  Email tracker pixels        -> Stripped by DuckDuckGo before delivery
  Unauthorised app internet   -> Blocked by AFWall+ whitelist
  Weak/reused passwords       -> Prevented by DDG password manager
  Google Play Services spying -> Replaced by DresOS microG Module
  Malware in APKs             -> Scanned by DresSecureComms File Scan
  Sensitive files on device   -> AES-256 encrypted by Amaze File Manager
  Censorship / Tor blocking   -> Bypassed by obfs4 bridges
  IP logging links            -> Scanned by DresSecureComms Threat Scan
```

---

**Full setup guide:** https://dresos.org
**The DresOS Team**
