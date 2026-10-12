# macOS development
- Don't use xcode directly to develop
- Use pure swift 5.8 code to make the app. No interface builder. No SwiftUI.
- Aim for compact code. Within methods, don't have groups of statements separated with newlines. No inline comments for simple code. Instead, split statements into sub-methods.
- Use guard closes as much as possible to separate the happy-path under them
- Organize source files into folders. Folders should group files that change together, at the same pace (e.g. one feature)
- Favor low latency and responsiveness. Reuse objects, avoid wasting memory or I/O.

# Workflow
- Copy commands from ai/build.sh and run them, to confirm compilation works after you're done with implementing a change. Prefix them with `DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer` when `xcode-select -p` points to CommandLineTools:
  `DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer xcodebuild -workspace alt-tab-macos.xcworkspace -scheme Debug -configuration Debug -derivedDataPath ~/git/alt-tab-macos/DerivedData 2>&1 | grep -E "error:|BUILD (SUCCEEDED|FAILED)"`
- The Debug build is signed with `Apple Development: yang.xb@outlook.com (45236GPQ77)` in config/debug.xcconfig. Don't switch back to `Local Self-Signed` (scripts/codesign/setup_local.sh) or ad-hoc signing for builds that get installed: both lose the Accessibility and Screen Recording grants.
- To install a build (only when asked): back up /Applications/myAltTab.app, `pkill -x myAltTab`, replace it with `ditto` from ~/git/alt-tab-macos/DerivedData/Build/Products/Debug/myAltTab.app, then `open` it and check `pgrep -fl myAltTab`.
- Building against the Xcode 26 SDK, the switcher uses `NSGlassEffectView` on macOS 26. Don't add `NO_LIQUID_GLASS` back to config/debug.xcconfig.
