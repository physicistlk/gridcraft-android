# GridCraft for Android (arm64-v8a)

An unofficial Android port of **[GridCraft](https://github.com/storytold/gridcraft)** — the real engine
and egui interface, packaged as a NativeActivity APK for sideloading on arm64 devices.

| | |
|---|---|
| Upstream project | [`storytold/gridcraft`](https://github.com/storytold/gridcraft) — Apache-2.0 OR MIT |
| Pinned commit | `476448d7d9f3a5f8e93fcf7d98b47fd52293fe7f` (upstream version `0.19.1`) |
| Android package | `ai.storyteller.gridcraft` |
| Published APK | `gridcraft-0.19.1-arm64-v8a-20261112.apk` — 16.4 MB, arm64-v8a, signed (locally, 2026-11-12) |
| APK sha256 | `91754013a44f640ddcbc467287ddd22bad8c0e32798e03610c7d6fc5ff0ee6a7` |
| Upstream files changed | 9 files (1 of them new) — see [`patches/`](patches/) |
| New port crate | `apps/gridcraft-android/` — 2300 lines of Rust plus resources |

## What this repository is

This is **not a fork** of GridCraft. It holds only the Android layer, so it stays small, readable,
and obviously separable from upstream:

```
apps/gridcraft-android/     the new shell crate — copy it into your checkout of upstream
patches/                the diff against upstream — apply it after checking out the pin
docs/PORT-NOTES.md      what was changed and why, fix by fix, with how each one was checked
scripts/                cross-compile environment, build and verify scripts (from shared-android-port)
CHANGELOG.md            what is in the published APK
```

The Android layer is not a reimplementation. It is a shell crate that fills the same `Services`
struct the desktop and web shells fill, so the real engine and the real egui UI run unchanged.

## Build it

Prerequisites: Rust (rustup) with the `aarch64-linux-android` target, Android SDK + NDK r27, a
JDK 21, and `cargo install cargo-apk`.

```bash
git clone https://github.com/storytold/gridcraft.git
cd gridcraft
git fetch --depth 1 origin 476448d7
git checkout FETCH_HEAD                        # the exact commit this port targets
git apply -3 /path/to/0001-gridcraft-android-port.patch
cp -r /path/to/gridcraft-android apps/             # the shell crate from this repository

# any keystore of your own signs an installable APK; a throwaway test key is fine for sideloading
keytool -genkeypair -keystore ks.jks -alias gridcraft -keyalg RSA -keysize 2048 -validity 10000 \
        -storepass android -keypass android -dname "CN=Local Test, O=Local, C=US"
export CARGO_APK_RELEASE_KEYSTORE="$PWD/ks.jks" CARGO_APK_RELEASE_KEYSTORE_PASSWORD=android
cargo apk build --release -p gridcraft-android
```

`scripts/android-env.sh` is the cross-compile environment this port was built with (the NDK linker
and the C compiler the native dependencies need), `scripts/build-apk.sh` wraps the build, and
`scripts/verify-apk.sh` checks the artifact: `aapt2 dump badging`, `apksigner verify`, and `llvm-nm`
on the `.so` inside the APK.

## Install

```bash
adb install -r gridcraft-0.19.1-arm64-v8a-20261112.apk
```
or copy the APK to the device and open it (installing from unknown sources must be allowed).

On first run, grant **Settings ▸ Apps ▸ GridCraft ▸ Special access ▸ All files access** if you want
the in-app picker to reach files outside the app's own folders. Without it, Android 10+ refuses a
`read_dir` of shared storage by path, and the picker can only offer the app's own directory.

## Verification status

Everything below was checked on the machine this port was published from, in November 2026.
A tick means the check was *run*, not inferred; the wording says how far it goes.

| Claim | How it was checked | Result |
|---|---|---|
| `patches/…patch` applies to a clean checkout of the pin | `git archive v0.19.1` into a pristine tree + `git apply --check` (then applied and `diff -r` against the built tree) | ✅ applies cleanly; trees identical |
| The assembled tree equals the tree the APK was built from | `diff -r` over the whole tree (patched files and every port-crate file) | ✅ no differences |
| The port crate compiles for Android | `cargo check -p gridcraft-android --target aarch64-linux-android`, run in a tree assembled from scratch (fresh `git fetch` of the pin, patch applied, port crate copied in) | ✅ exit 0 |
| AutoSum number-format and Format Painter fixes | `cargo test -p gridcraft-engine` (host) | ✅ 4/4 pass; the whole engine lib suite passes 207/207 |
| The port crate's own tests | `cargo test -p gridcraft-android` in the freshly assembled tree | ✅ 15/15 pass |
| APK is signed and is the right artifact | `apksigner verify --verbose`, `aapt2 dump badging`, sha256 | ✅ v2/v3 signatures true, `native-code: 'arm64-v8a'`, package `ai.storyteller.gridcraft` |
| Behaviour on real hardware | hand-testing on the porter's arm64 tablet | ⚠️ the fixes below were found and driven by that testing; each published binary is not individually re-verified here |

**Before trusting the APK:** the source side is verified as listed above; runtime behaviour only as far
as [`docs/PORT-NOTES.md`](docs/PORT-NOTES.md) says per fix, plus the tablet testing that produced those
fixes. There is no automated device test in this repository.

## Known gaps

- **Runtime verification is hand-testing only.** The engine tests were run here (207/207), but no
  device test exists.
- **Format Painter applies on click and drag only.** There is no double-click-to-lock (sticky) mode
  beyond the engine's own one-shot behaviour.
- **All files access is a manual grant**; without it the picker offers only the app's own directory.
- **`Ctrl` shortcuts work only because the shell synthesises modifiers** — if a device's IME swallows
  a key event entirely, that modifier cannot be seen.
- **The system clipboard is text only.** Copy/Paste crossed the app boundary for text;
  pictures and HTML on the clipboard are still out of reach (`ClipData` carries them, but this
  port does not negotiate those formats yet).


## The rest of this set

Every port in this set is built from the same patched upstream tree and published by
[@physicistlk](https://github.com/physicistlk). Each link goes to that port's latest
release, which carries the signed `arm64-v8a` APK:

- [cadcraft-android](https://github.com/physicistlk/cadcraft-android/releases/latest) — CadCraft 0.20.1
- [deckcraft-android](https://github.com/physicistlk/deckcraft-android/releases/latest) — DeckCraft 0.18.1
- [lightcraft-android](https://github.com/physicistlk/lightcraft-android/releases/latest) — LightCraft 0.21.1
- [pdfcraft-android](https://github.com/physicistlk/pdfcraft-android/releases/latest) — PdfCraft 0.21.1
- [photocraft-android](https://github.com/physicistlk/photocraft-android/releases/latest) — PhotoCraft 0.23.1
- [vectorcraft-android](https://github.com/physicistlk/vectorcraft-android/releases/latest) — VectorCraft 0.24.1
- [wordcraft-android](https://github.com/physicistlk/wordcraft-android/releases/latest) — WordCraft 0.18.1

## Credits and licence

GridCraft is © its authors at [`storytold/gridcraft`](https://github.com/storytold/gridcraft) and is
dual-licensed Apache-2.0 / MIT; both licence texts are kept here. The Android layer in this
repository is an unofficial modification by [@kanaaka](https://github.com/kanaaka), carrying the
same licence. Not affiliated with or endorsed by the upstream project. See
[`ATTRIBUTION.md`](ATTRIBUTION.md).


## Android 16 build (physicistlk fork)

This fork carries the Android 16 port (arm64-v8a, 16 KB page-aligned) plus the
tablet-compatibility milestones (system clipboard, open-with/share intents, flush on
suspend, back-gesture closes dialog, trackpad wheel + pen hover, stylus pressure for
PhotoCraft, PhotoCraft crash-recovery autosave).

A signed APK for the current state is attached to the
[v2026.10.30 release](https://github.com/physicistlk/gridcraft-android/releases/tag/v2026.10.30).

See the upstream `kanaaka` repository for the full port notes and verification details.
