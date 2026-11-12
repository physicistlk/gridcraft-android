# Changelog

## 2026-11-12 — Re-pinned to upstream 0.19.1

| | |
|---|---|
| APK | `gridcraft-0.19.1-arm64-v8a-20261112.apk` — sha256 `91754013a44f640ddcbc467287ddd22bad8c0e32798e03610c7d6fc5ff0ee6a7` (built 2026-11-12, locally signed) |
| Upstream pin | `476448d7d9f3a5f8e93fcf7d98b47fd52293fe7f` — tag `v0.19.1` (upstream `0.19.1`) |
| Package | `ai.storyteller.gridcraft` — versionName `0.19.1`, versionCode `16782081`, minSdk 26, targetSdk 35 |
| ABI | `arm64-v8a` only |
| ELF page alignment | `0x4000` (16 KB) — Android 16 compatible |

### Changed
- **The upstream pin moved from 0.18.0 to 0.19.1** — the release carries the 0.19.0 minor bump
  (workspace version, engine/UI changes) plus a patch on top. The port patch was re-derived against
  the `v0.19.1` tree; every hunk applied unchanged and no file had to be re-ported by hand. The
  only content difference in the resubmitted patch is the `Cargo.lock` entry for
  `gridcraft-android`, whose version follows the workspace, plus the usual line offsets/blob ids.
- **The shipped `patches/0001-gridcraft-android-port.patch` was complete this round.** It was
  checked before use against the previous tree's `git diff HEAD` backup: it held all nine files
  (`Cargo.toml` with the `winit → vendor/winit` switch, `Cargo.lock`, the seven `crates/ui-egui`
  files, `save_dialogs.rs` as the one new file) and applied to `v0.19.1` with no conflicts.
- **Verification:** `git apply --check` against a pristine `git archive v0.19.1` tree (port crate
  and `vendor/winit` copied in as directories) — the regenerated patch applies cleanly, and after
  applying it `diff -r` over the assembled tree and the tree the APK was built from shows no
  differences. `cargo check -p gridcraft-android --target aarch64-linux-android` passes (warnings
  only). The signed APK was verified with the port's `scripts/verify-apk-linux.sh`:
  `versionName 0.19.1`, `native-code: 'arm64-v8a'`, v2/v3 signatures verify, `android_main` and
  `ANativeActivity_onCreate` exported, and every `LOAD` segment `0x4000`-aligned.


## 2026-11-09 — Re-pinned to upstream 0.18.0

| | |
|---|---|
| APK | `gridcraft-0.18.0-arm64-v8a-20261109.apk` — sha256 `b43d19d3ba19d1a17109bf823928b935332de381a7e1f83e18d054c85ac6c472` (built 2026-11-09, locally signed) |
| Upstream pin | `17cb3c83d9e73df6a88ed174cec612f1de2e8244` — tag `v0.18.0` (upstream `0.18.0`) |
| Package | `ai.storyteller.gridcraft` — versionName `0.18.0`, versionCode `16781824`, minSdk 26, targetSdk 35 |
| ABI | `arm64-v8a` only |
| ELF page alignment | `0x4000` (16 KB) — Android 16 compatible |

### Changed
- **The upstream pin moved from 0.17.0 to 0.18.0.** The port patch was re-derived against the
  `v0.18.0` tree. All hunks applied unchanged; the only edit needed was the `Cargo.lock` entry for
  the `gridcraft-android` package, whose version follows the workspace (`0.18.0`).
- **`patches/0001-gridcraft-android-port.patch` carries the whole port again.** The 0.17.0
  re-port produced the patch with plain `git diff`, which dropped every staged hunk and, because
  `save_dialogs.rs` was untracked, the module and its wiring as well: the file had shrunk to 78
  lines and the 0.17.0 tree was missing the Save As path. It is now generated with `git diff HEAD`
  after an intent-to-add of the new file, and holds all nine files: `Cargo.toml` (vendored
  winit), `Cargo.lock`, and the seven `crates/ui-egui` files, with `save_dialogs.rs` as the one
  new file.
- **Verification:** the patch was checked with `git apply --check` against a pristine
  `git archive v0.18.0` tree (the port crate and `vendor/winit` copied in as directories), applied,
  and `diff -r` shows the assembled tree and the built tree are identical. `cargo check -p
  gridcraft-android --target aarch64-linux-android` passes; the signed APK was verified with
  `aapt2 dump badging` (`versionName 0.18.0`, `native-code: 'arm64-v8a'`), `apksigner verify`,
  and `llvm-readelf` (every `LOAD` segment `0x4000`-aligned).


## 2026-11-05 — Re-pinned to upstream 0.16.1

| | |
|---|---|
| APK | `gridcraft-0.16.1-arm64-v8a-20261105.apk` — sha256 `3ea831afe07586c125cae540f7c4eeb542802393b69927e5b07775025d9194e3` (built 2026-11-05, locally signed) |
| Upstream pin | v0.16.1 (upstream `0.16.1`) |
| Package | `ai.storyteller.gridcraft` — versionName `0.16.1`, versionCode `16781313`, minSdk 26, targetSdk 35 |
| ABI | `arm64-v8a` only |
| ELF page alignment | `0x4000` (16 KB) — Android 16 compatible |


## 2026-11-04 — Re-pinned to upstream 0.16.0

| | |
|---|---|
| APK | `gridcraft-0.16.0-arm64-v8a-20261104.apk` — sha256 `cebb633ad0daaae79d839de64b6aadf5e1cfcc04a9142dd2ca71e6632709119c` (built 2026-11-04, locally signed — not a published release) |
| Upstream pin | `85e51e009c24fad47575f04d3c08f12342615d82` (upstream `0.16.0`) — was `43284377` (`0.13.0`) |
| Package | `ai.storyteller.gridcraft` — versionName `0.16.0`, versionCode `16781312`, minSdk 26, targetSdk 35 |
| ABI | `arm64-v8a` only |
| ELF page alignment | `0x4000` (16 KB) — Android 16 compatible |

### Changed
- **The upstream pin moved from 0.13.0 to 0.16.0.** The canonical patch describes against
  `0.13.0` applied cleanly to the `0.16.0` tree — the only update needed was re-deriving it
  against the `0.16.0` tree (new line numbers/index lines) and carrying over the previously
  un-patched `crates/ui-egui/src/save_dialogs.rs` creation, which the previous re-port left
  as an untracked file. `patches/0001-gridcraft-android-port.patch` now carries all of:
  `Cargo.toml` (vendored winit), `Cargo.lock` (regional allocation), and the seven
  `crates/ui-egui` files, with `save_dialogs.rs` as the one new file.
- **Verification:** `scripts/verify-apk-linux.sh` — package id, `native-code: 'arm64-v8a'`,
  v2+v3 signatures, AArch64 `.so`, every LOAD segment `0x4000`-aligned, `android_main` and
  `ANativeActivity_onCreate` exported. `cargo check -p gridcraft-android --target
  aarch64-linux-android` also passes on the reassembled tree.

## 2026-10-30 — The system clipboard (text)

| | |
|---|---|
| APK | `gridcraft-0.13.0-arm64-v8a-20261030.apk` — sha256 in `SHA256SUMS.txt` beside the build |
| Package | `ai.storyteller.gridcraft` — versionName `0.13.0`, minSdk 26, targetSdk 35 |
| ABI | `arm64-v8a` only |

### Changed
- **Copy and paste crossed the app boundary for text (`src/clipboard.rs`).** Ctrl+C/X/V now produce
  `Event::Copy`/`Cut`/`Paste` (the Android backend never reports modifiers, so `egui-winit` never
  did), Paste reads `ClipboardManager` through JNI, and `OutputCommand::CopyText` is written back
  with `ClipData.newPlainText`. The ribbon and menu Paste read the same clipboard through
  `Services::paste_text`. Plain text only: HTML/formatting is not negotiated yet.

## Unreleased — re-pinned to upstream 0.13.0, Android 16 (16 KB page size)

| | |
|---|---|
| Upstream pin | `43284377d2466037628e57d8ad0c891778fdb45c` (upstream `0.13.0`) — was `a513787` (`0.5.0`) |
| Package | `ai.storyteller.gridcraft` — versionName `0.13.0`, minSdk 26, targetSdk 35 |
| ABI | `arm64-v8a` only |
| ELF page alignment | `0x4000` (16 KB) — Android 16 compatible |

### Changed
- **The upstream pin moved from 0.5.0 to 0.13.0.** The old seven-file patch did not apply: upstream
  moved `Services` into its own module, `SheetApp::new` into `app.rs`, the `file.saveAs` command
  into `app/commands.rs` and the dialog rendering into `dialogs/show.rs`. The patch was re-derived
  against the new tree, not forced onto it.
- **A relative path in "Save a copy" still asks for a folder**, as upstream does — the old port
  patch had dropped that.
- **Android 16 page-size compatibility.** The native library is linked with
  `-Wl,-z,max-page-size=16384`, so every `LOAD` segment is 16 KB aligned.

### Dropped
- **The Format Painter delivery backport.** Upstream 0.13.0 delivers it itself now.
- **The `AutoSum` number-format backport.** It was an upstream engine feature the port carried, not
  an Android change; upstream 0.13.0's `auto_sum` has been refactored since, and carrying it would
  make the Android build diverge from the desktop release. The old patch is kept in
  `patches/archive/0001-gridcraft-android-port-v005.patch`.


## v0.1.2 — 2026-10-13 (Ctrl shortcuts)

| | |
|---|---|
| APK | `gridcraft-0.5.0-arm64-v8a-20261013b.apk` (11.1 MB) — supersedes the v0.1.1 asset, same signing key |
| sha256 | `930bec5b49369e12579d2b4e2f299f71df07fb28a7ce08932f1fc8d7005b645f` |
| Built from | upstream `storytold/gridcraft` @ `a5137872c15fac7bf260fa2d25ea8f7e66b19dc2` + this repository at the commit that released it |
| Package | `ai.storyteller.gridcraft` — versionName `0.5.0`, versionCode `16778496`, minSdk 26, targetSdk 35 |
| ABI | `arm64-v8a` only |

### Fixed
- **Ctrl+anything did nothing, while Shift+click and Shift+arrow worked.** winit's Android
  logical-key table names `Shift` and `Alt` but **not** `Control` — so a Ctrl press arrives with an
  unidentified *logical* key (egui-winit falls back to the physical one) while its physical key is
  correctly `ControlLeft`/`ControlRight`. `src/modifiers.rs` matched the logical key alone, so it
  never saw Ctrl go down: Ctrl+A, Ctrl+C, Ctrl+V, Ctrl+S, Ctrl+B and every other modifier shortcut
  was dead, and only the modifiers winit happens to name worked. The tracker now reads the physical
  key as well as the logical one, on both left and right, and keeps everything else: the per-event
  modifier copies (so a press and a click in one frame stay ordered) and `ModifiersChanged` as the
  authority (so a modifier held while the app goes to the background does not stick).
  The same tracker is in all four ports of this set; it is verified end-to-end in WordCraft
  (`Ctrl+A` selects the text, `Ctrl+B` emboldens it). This app's own shortcuts are the tablet check.

## v0.1.1 — 2026-10-13 (scrolling, modifiers, fonts, picker wording)

| | |
|---|---|
| APK | `gridcraft-0.5.0-arm64-v8a-20261013.apk` (11.1 MB) — supersedes the v0.1.0 asset, same signing key |
| sha256 | `a2012e67571d098a2c39834f6683040b0269cb4da516d3fa7dbd163be92683ae` |
| Built from | upstream `storytold/gridcraft` @ `a5137872c15fac7bf260fa2d25ea8f7e66b19dc2` + this repository at the commit that released it |
| Package | `ai.storyteller.gridcraft` — versionName `0.5.0`, versionCode `16778496`, minSdk 26, targetSdk 35 |
| ABI | `arm64-v8a` only |

### Changed
- **Two-finger scrolling.** A two-finger drag now scrolls the sheet (synthesised `MouseWheel` events
  from the touch centroid, with the lift event that starts the kinetic coast); one finger still selects
  and a pinch still zooms. Ctrl+wheel zooms.
- **A *Fonts* folder** at `Android/media/<package>/Fonts` (with `.fonts` and `.local/share/fonts`
  symlinked to it), so a font copied in over a USB cable is picked up.
- **Picker wording corrected.** An empty folder in shared storage no longer tells a user who has
  already granted *All files access* to grant it, and the USB tip now matches the folder
  (`Android/data` is unreachable over a cable, `Android/media` is not).

### Notes
The build was made from exactly the source in this repository (upstream pin + the patch + the port
crate) and signed with the same local test key as v0.1.0, so it installs over it.


## v0.1.0 — first published port (GridCraft 0.5.0)

| | |
|---|---|
| APK | `gridcraft-0.5.0-arm64-v8a-v3.apk` (11.0 MB) |
| sha256 | `f9439c0c7e47db3ecf240e7ceee016f3bcad711051d29ab7fb4a670231b376e7` |
| Built from | upstream `storytold/gridcraft` @ `a5137872c15fac7bf260fa2d25ea8f7e66b19dc2` + `patches/0001-gridcraft-android-port.patch` + `apps/gridcraft-android/` |
| Package | `ai.storyteller.gridcraft` — versionName `0.5.0`, minSdk 26, targetSdk 35 |
| ABI | `arm64-v8a` only |
| Signature | throwaway local test key, not an official key |

### In this release
- Android shell crate: NativeActivity entry point, `Services` implementation, file logger, preferences
  written while the app runs (atomic), modifier-key synthesis, Activity not recreated on configuration
  change.
- **Save dialogs that do not hold the frame** (`pick_save_async` + `save_dialogs`): a saved workbook
  learns its path, so the title bar follows, `Ctrl+S` stops asking, closing stops prompting, and the
  file enters the recent list.
- **Recent documents updated on save** — Save As no longer leaves the file out of the Start screen.
- **Format Painter works**: click paints a cell, drag paints the dragged range.
- **AutoSum totals take the number format of the data** they sum (currency column → currency total),
  with four tests added beside it.
- In-app picker for Open and Save As, with cached listings, "Show all files", hidden-file count and
  self-explaining locked roots.
- Launcher icon, storage permissions, edge-to-edge theme variant.


### Known issues
See *Known gaps* in the [README](README.md) and [`docs/PORT-NOTES.md`](docs/PORT-NOTES.md).
