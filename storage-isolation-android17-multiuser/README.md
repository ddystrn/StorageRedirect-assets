# Storage Isolation — Android 17 multi-user fix

This module supports Storage Isolation `8.8.1` with Enhanced Mode/Zygisk `v29.0.1` on Android 17 arm64-v8a, for a personal user and a managed work profile.

## What it fixes

- Android 17 `PackageInfoList` compatibility in the Enhanced Mode package hook.
- Separate App rule management rows for each installed user/profile.
- Correct `User 10` label and work-profile badge.
- Correct per-profile rule state in the app list: configured rules show **On**; unrelated apps remain **Off**.

The official Storage Isolation APK is not modified or re-signed. This is one Zygisk module; it does not require LSPosed or a second module.

## Install

1. Confirm Storage Isolation core service version is `355` and the device ABI is `arm64-v8a`.
2. In KernelSU or Magisk, install `storage-isolation-enhanced-multiuser-v29.0.1.4.zip`. It replaces the existing Storage Isolation Enhanced Mode module.
3. Reboot.
4. Open Storage Isolation and confirm **Using Enhanced mode v29.0.1 (Zygisk)**.
5. Open **App rule management**. Profile-specific rows show the user suffix and work badge.

## How it works

The module keeps the Android 17 package-list compatibility hook and carries user identity from the package UID into the daemon list response. Its boot service restores the patched daemon after Storage Isolation extracts its stock daemon, so the fix survives reboot.

## Scope and limits

- Built only for Storage Isolation core service `355`, Enhanced Mode `v29.0.1`, and Android 17 `arm64-v8a`.
- It preserves `/data/adb/storage-isolation/configuration.json`; existing rules are not removed.
- The module skips its daemon patch when the core-service version is not `355`; do not use it with another version.
- Tested with personal user `0` and managed profile user `10`.
