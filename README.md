# Vizor releases

Signed builds of Vizor: dictation (built on OpenSuperWhisper), the notch pill and the
voice in one app. The app updates itself from here (Settings, Updates). The source is
private; this repo only holds the release workflow, the appcast and the downloads.

## First install

Download the newest zip under Releases, unzip it, and move the app to Applications.
The build is signed with a Vizor certificate, not an Apple Developer ID, so macOS
blocks the first start of a downloaded copy. Clear that once:

    xattr -dr com.apple.quarantine /Applications/Vizor.app

Then grant the app Accessibility, Input Monitoring and the microphone. Updates keep
the same signature, so macOS keeps those grants.

## How a release happens

`.github/workflows/release.yml` runs daily and by hand (Actions, release, Run
workflow). It releases when OpenSuperWhisper publishes a new tag or vizor's `main`
moves. The version is `<upstream>.<n>`: `0.12.7.1`, `0.12.7.2`, then `0.12.8.1`.

If the dictation patch no longer applies to a new upstream tag, the run opens an
issue here and publishes nothing.

## Setup (once, by the repo owner)

1. Create this repo as **public**, so the app can read the appcast and the zips
   without a token. Copy `README.md` and `.github/` from `release/vizor-release/`
   in the vizor repo.
2. Add four Actions secrets (Settings, Secrets and variables, Actions):

   | Secret | Value |
   |---|---|
   | `VIZOR_SOURCE_TOKEN` | classic personal access token with `repo` scope that can read electro56435/vizor |
   | `SPARKLE_ED_PRIVATE_KEY` | from the Vizor signing material |
   | `SIGNING_P12_BASE64` | from the Vizor signing material |
   | `SIGNING_P12_PASSWORD` | from the Vizor signing material |

   The last three are handed over privately, never through an issue or a commit. The
   app trusts only updates signed with that Sparkle key.
3. Run the workflow once by hand with `force` checked.
