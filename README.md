# 🛑 Stop Tahoe Update

_A community-led effort to delay, suppress, and safely block unwanted macOS upgrades (e.g. Sequoia → Tahoe)._

[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-blue.svg)](CONTRIBUTING.md)
[![GitHub Discussions](https://img.shields.io/github/discussions/travisvn/stop-tahoe-update?logo=github)](https://github.com/travisvn/stop-tahoe-update/discussions)
[![Build & Validate](https://github.com/travisvn/stop-tahoe-update/actions/workflows/validate.yml/badge.svg)](https://github.com/travisvn/stop-tahoe-update/actions)

> Safe • Transparent • Community-driven  
> Everything here is reversible and off by default.

---

## ⚙️ Why This Project Exists

Apple allows deferring major macOS upgrades (like Sequoia → Tahoe) for **up to 90 days** using MDM or configuration profiles.  
Beyond that, users are on their own — exposed to constant upgrade prompts, badge counts, and surprise downloads.

**Stop Tahoe Update** exists to change that responsibly.  
We’re building a **community-maintained source of truth** for users who want to stay on stable macOS versions **without unsafe hacks**.

---

## 🎯 Project Goals

### ✅ **Immediate Goals**

- Provide `.mobileconfig` profiles for **30 / 60 / 90-day deferrals**
- Include **safe install / uninstall / status scripts**
- Establish a **safety-first governance model**
- Build and maintain a **living documentation hub**

### 🧠 **Community Research Goals**

- 🚫 Prevent _“Install Tonight”_ or _“Install Now”_ prompts
- 🔕 Suppress Settings app badge counts for new macOS upgrades
- 🛡️ Detect & optionally remove unwanted `Install macOS*.app` downloads
- 🌐 Explore early network-detection of upgrade payloads (opt-in)
- 🧩 Track new Apple deferral keys & declarative MDM behavior

> All experimental features are **dry-run by default**.  
> No background services, no silent system modifications.

---

## 🗂️ Repository Structure

```bash
stop-tahoe-update/
├─ profiles/                 # 30, 60, 90-day deferral .mobileconfig files
├─ scripts/                  # Safe install/uninstall/status scripts
├─ plugins/                  # Optional "shield" experiments
│  ├─ installer-watcher/     # Detect & alert on rogue installers
│  └─ update-signal-monitor/ # Early-warning system log monitor
├─ docs/                     # Goals, RFCs, research, compatibility
└─ .github/                  # Governance, CI, review workflows
```

---

## 🔬 Active Experiments

| Plugin                    | Type        | Description                                                               | Default |
| ------------------------- | ----------- | ------------------------------------------------------------------------- | ------- |
| **Installer Watcher**     | LaunchAgent | Detects `/Applications/Install macOS*.app` and offers to move it to Trash | Dry-run |
| **Update Signal Monitor** | LaunchAgent | Monitors unified logs & prefs for upgrade preparation signals             | Dry-run |

> Both run in **user space**, never require `sudo`, and write audit logs under `~/Library/Logs`.

---

## 🧭 Roadmap

| Phase       | Description                                          | Status               |
| ----------- | ---------------------------------------------------- | -------------------- |
| **Phase 1** | 30/60/90-day deferrals + safety scripts              | ✅ Done              |
| **Phase 2** | Fallback shields (Installer Watcher, Signal Monitor) | 🚧 In progress       |
| **Phase 3** | Badge & prompt suppression research                  | 🔍 Community testing |
| **Phase 4** | Declarative + network-layer experiments              | 🧪 Future            |

---

## 🧩 Vision & Philosophy

- **Transparency over magic:** Every line of code is inspectable and reversible
- **Safety first:** No background daemons, no automatic actions
- **Community-led:** Every installable artifact is review-gated
- **Apple-aligned:** Start with supported deferral keys, innovate cautiously beyond them

---

## 🧑‍💻 Contributing

We welcome:

- 🧱 Developers – build and test optional shields
- 🔍 Researchers – document macOS update behavior
- 🧪 Testers – verify reproducibility across versions
- 🧭 Reviewers – audit scripts and profiles for safety

📄 See [CONTRIBUTING.md](./.github/CONTRIBUTING.md) and [docs/GOALS.md](./docs/GOALS.md).

---

## 📖 Guides

- [Remove the Red Badge from the System Settings Dock Icon](./docs/REMOVE_DOCK_BADGE.md) — Get rid of the persistent notification badge on the System Settings icon in your Dock

---

## 🧰 Quick Start

### 1. Clone or download the repo

```bash
git clone https://github.com/travisvn/stop-tahoe-update.git
cd stop-tahoe-update
```

Ensure the scripts are executable
```bash
# Ensure scripts are executable (just in case)
chmod +x ./scripts/*.sh
```

### 2. Apply a 90-day deferral profile

The script will generate unique identifiers to prevent conflicts and attempt to install the profile.

```bash
./scripts/install-profile.sh profiles/deferral-90days.mobileconfig

```

> [!NOTE]
> On recent macOS versions, silent installation may be blocked. If the script opens **System Settings**, please locate the **"Profiles"** (or "Downloaded Profile") notification and click **Install** manually to complete the process.

### 3. Verify status

```bash
./scripts/status.sh
```

### 4. Remove later if needed

```bash
./scripts/uninstall-profile.sh
```

---

<details>
<summary>🔍 What the deferral profiles actually do</summary>

They use Apple’s official `com.apple.applicationaccess` keys:

```xml
<key>forceDelayedMajorSoftwareUpdates</key><true/>
<key>enforcedSoftwareUpdateMajorOSDeferredInstallDelay</key><integer>90</integer>
```

These settings hide major upgrades from Software Update for **up to 90 days**.
After that, the OS may begin prompting again.

</details>

---

## ⚖️ Safety & Governance

- 🧾 **Signed releases only** — no `curl | sh` install methods
- 🧱 **CodeOwners** required for `profiles/`, `scripts/`, and `plugins/`
- 🧪 **CI validation:** XML linting, ShellCheck, SHA-256 hash checks
- 🛡️ **No root operations** unless clearly documented & user-initiated

---

## 🗺️ Beyond Tahoe

While this repo focuses on **Sequoia → Tahoe**, the long-term goal is a more general toolkit:
**StayOnMac** — empowering macOS users to _choose when_ (or _if_) they upgrade.

Future versions may include:

- Broader version targeting (Sonoma, Ventura, etc.)
- GUI wrappers for non-technical users
- Integration with open MDM tools

---

## 💬 Community & Support

- 💭 [GitHub Discussions](https://github.com/travisvn/stop-tahoe-update/discussions): share findings, theories, and test results
- 🧩 `observations` tag: submit verified upgrade triggers or logs
- 🧠 RFCs: propose new shields via `docs/rfcs/`

---

## 🧾 License

**MIT License** — simple, permissive, and open.
All contributions must remain verifiable, reversible, and user-controlled.
