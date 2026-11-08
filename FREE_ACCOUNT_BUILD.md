# Free Apple account build (personal fork notes)

This branch (`free-account-build`) builds Trio v1.0.2 with a **free** Apple developer account.

## Changes vs upstream `main`
- `Trio/Resources/Trio.entitlements`: removed `aps-environment` (push), NFC tag reading and
  time-sensitive notifications. Personal teams can't use these.
  Effect: no remote commands via push, no Libre NFC pairing, alerts don't break through Focus.
- `Trio.xcodeproj/xcshareddata/xcschemes/Trio.xcscheme`: Thread Sanitizer off (it breaks device builds).

When moving to a **paid** account, restore the entitlements:
`git checkout main -- Trio/Resources/Trio.entitlements`

## Setup on a new Mac
```sh
git clone --recurse-submodules -b free-account-build https://github.com/gkardeepblue/Trio.git
cd Trio
echo 'DEVELOPER_TEAM = 55DWTBC2JC' > ConfigOverride.xcconfig   # git-ignored
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```
Sign in to Xcode (Settings → Accounts) with the same Apple ID, open `Trio.xcworkspace`,
pick the Trio scheme + your iPhone, and Run.

## Gotchas
- Never pass `-derivedDataPath` inside the repo: the SwiftFormat build phase formats everything
  under the repo, including downloaded packages, and breaks the build. Use the default DerivedData.
- Building from the command line: prefix with `CI=true` to skip the SwiftFormat phase.
- Free-account apps expire after 7 days; rebuild to renew.
