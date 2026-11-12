# Source changes vs the iOS tree

This tree starts from iOS commit `361180b` and only adds Mac Catalyst support. Original iOS repo is not modified.

| File | Reason |
| --- | --- |
| `SecureChat/SecureChatApp.swift` | On Mac Catalyst, `UIDevice.current.name` is a generic “iPad”. Default display name now uses the computer host name (with `.local` stripped). Window also gets a 960×640 default size and an 800×560 minimum. |
| `SecureChat/Info.plist` | Added `LSMinimumSystemVersion = 13.0` and `NSHighResolutionCapable`. `CFBundleExecutable` was already present. UIKit APIs (`UIPasteboard`, share sheet, document picker, `UIImage`) are left as-is because they work under Catalyst. |
| `SecureChat/Assets.xcassets/AppIcon.appiconset/Contents.json` | Added a macOS 512×512@2x icon entry pointing at the existing 1024px image. |
| `project.yml` | Enabled `SUPPORTS_MACCATALYST`, disabled “Designed for iPhone/iPad”, set `MACOSX_DEPLOYMENT_TARGET = 13.0`, and `DERIVE_MACCATALYST_PRODUCT_BUNDLE_IDENTIFIER`. |
| `SecureChat.xcodeproj/project.pbxproj` | Same Catalyst flags as `project.yml`. Release also sets `ARCHS = arm64 x86_64` and `ONLY_ACTIVE_ARCH = NO` so the Intel runner produces a universal binary. |
| `.github/workflows/build.yml` | Replaced the iOS IPA workflow with a Mac Catalyst Release build, ad-hoc `codesign`, `lipo` / plist / signature checks, a short launch smoke test, and a `ditto` zip artifact. Runners: `macos-15-intel` (primary) and `macos-15` (parallel). |
| `README.md`, `README.zh-CN.md` | macOS download and Gatekeeper notes. |

No App Sandbox and no hardened runtime. The workflow signs the `.app` ad-hoc (`codesign -s -`) after the build; `CODE_SIGNING_REQUIRED=NO` alone is not enough to launch on Apple Silicon.
