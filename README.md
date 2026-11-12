# SecureChat for macOS

Unofficial **Mac Catalyst** build of [SecureChat](https://github.com/kiritoX0914/SecureChat). Same LAN end-to-end encrypted messenger, packaged as a native `.app` that runs on **macOS 13 and later**, including **macOS 15.x**, on both **Apple Silicon** and **Intel**.

This repository is a copy of the iOS project with the smallest set of changes needed to build for Mac. The original iOS app is unchanged.

## Download

Grab `SecureChat-macOS.zip` from the latest [Actions](https://github.com/kiritoX0914/SecureChat-macOS/actions) run (artifact `SecureChat-macOS-macos-15-intel`) or from [Releases](https://github.com/kiritoX0914/SecureChat-macOS/releases) if one has been published.

1. Unzip the archive. You should get `SecureChat.app`.
2. Because the app is **ad-hoc signed** (not notarized with an Apple Developer ID), macOS Gatekeeper may block the first launch. Use one of:
   - Right-click `SecureChat.app` → **Open** → Open
   - Or in Terminal: `xattr -cr SecureChat.app && open SecureChat.app`
3. Grant **Local Network** permission when asked so peers can be discovered over Bonjour.

The binary is universal (`arm64` + `x86_64`). Minimum system version is **macOS 13.0**.

## What this app is

SecureChat talks **only on your local network**. Messages use the Signal Protocol (X3DH + Double Ratchet) with AES-256-GCM. There is no server, no account, and no internet requirement.

## Build

Built in GitHub Actions on `macos-15-intel` (and `macos-15` when a runner is available) with:

```bash
xcodebuild build \
  -project SecureChat.xcodeproj \
  -scheme SecureChat \
  -destination 'generic/platform=macOS,variant=Mac Catalyst' \
  -configuration Release \
  ARCHS="arm64 x86_64" \
  ONLY_ACTIVE_ARCH=NO

codesign --force --deep --sign - SecureChat.app
ditto -c -k --keepParent SecureChat.app SecureChat-macOS.zip
```

See `MACOS_CHANGES.md` for the source edits relative to the iOS tree.
