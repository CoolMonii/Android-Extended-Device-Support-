# Android Extended Device Support (AEDS)

Android Extended Device Support (AEDS) is a custom, lightweight Android ROM designed specifically to breathe new life into legacy and low-RAM devices. Built on top of an optimized **Android 12/13 (Android Go)** base, AEDS eliminates modern OS bloat, strips background telemetry, and introduces custom framework-level optimizations to prioritize frame-pacing, low latency, and battery longevity over flashy aesthetics.

---

## Core Objectives

* **Ultra-Optimized for Legacy Hardware:** Specifically tuned for devices with limited CPU clusters and low RAM (1GB - 3GB).
* **True Android Go Integration:** Native enforcement of low-RAM behaviors to minimize the overall system footprint.
* **Zero Bloat & Enhanced Privacy:** No Google services pre-installed, completely stripped of background analytics, but maintains full support for optional GApps flashing.
* **Performance Over Benchmarks:** Focused strictly on UI smoothness, instantaneous touch responsiveness, and aggressive power saving.

---

## Technical Architecture & Custom Enhancements

AEDS utilizes a hybrid architecture—leveraging the robust hardware compatibility layer of LineageOS while aggressively modifying the core AOSP framework.

#### Framework Hook Points:
* `NotificationManagerService`: Automatic importance downgrading for low-priority tiers to prevent rendering overhead.
* `AlarmManagerService` & `JobSchedulerService`: Deep batching and deferral of background wakeups.
* `AppStandbyController`: Direct enforcement of restricted standby buckets upon app focus loss.

### Memory & Graphics Optimizations
* **Low RAM Enforcement:** `ro.config.low_ram=true` enabled globally to optimize memory allocations, simplify the Recents panel, and disable heavy multi-window processes.
* **Aggressive LMKD:** Low Memory Killer Daemon retuned via `PSI` (Process Stall Information) to eliminate UI stuttering caused by memory thrashing.
* **Zero-Blur UI:** Translucencies and background blurs are explicitly disabled (`ro.sf.blurs_are_expensive=1`) to drastically reduce GPU overdraw.
* **Optimized Swap Management:** ZRAM configured optimally with modern compression (`lz4`/`zstd`) to maximize physical RAM efficiency.

---

## What's Included (And What's Not)

| Included Clean & Fast | Stripped / Removed |
| :--- | :--- |
| Minimalist AOSP UI | Google Play Services / GMS (Optional via Flash) |
| Core Essential Apps (Dialer, SMS, Files) | LineageOS SDK Extra UI Tweaks & Custom Themes |
| ANPS Management Engine | Background Telemetry & Analytics (`LineageStats`, etc.) |
| Upstream Android 12 Security Patches | Heavy App Animations & Window Blurs |

---

## How to Build

### 1. Environment Setup
Initialize your build environment according to standard Android 12 / LineageOS 19.1 prerequisites. 

### 2. Initialize AEDS Manifest
Initialize the repository using the AEDS manifest local manifests to pull the stripped frameworks:

```bash
repo init -u [https://github.com/Android-Extended-Device-Support/android_manifest.git](https://github.com/AEDS-Project/android_manifest.git) -b aeds-12.1
repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
