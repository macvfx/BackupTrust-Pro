# BackupTrust Pro 2.8 (29) — Beta

> **Beta — for testing only.** BackupTrust Pro is not a commercial product and comes with no support and no warranty. Keep your own independent backup of all data before and while using it. You are responsible for your data.

This beta adds filters for backing up folders that hold code, so a backup of a projects folder stops carrying build output that Xcode rebuilds. One Xcode compilation cache reported 24 GB while using a few KB of disk.

## New

- **The Xcode & Swift preset covers current Xcode build data.** It now skips folders ending `.noindex` (compilation cache, index, module cache, intermediates), the precompiled-module caches, `XCBuildData`, `SourcePackages`, and folders ending `.dSYM` or `.xcarchive`. The `build` rule is now on in the preset. Matching is by folder name and ignores case, so it also skips `Build`, and a source folder with that name is skipped too.
- **A new Python & Dev Tools preset** skips `__pycache__`, `.mypy_cache`, `.pytest_cache`, `.tox`, `.venv`, `venv` and folders starting `.venv-`. `.claude` is listed but off, because it also holds settings.
- **Three new switches in the filter editor, all off by default:**
  - Skip credential files: `signing.env`, `.sentryclirc`, `.env`, `.p12`, `.mobileprovision` and SSH keys. In a mirror plan, turning it on removes copies already on the destination.
  - Skip compiler output files: `.o`, `.swiftmodule`, `.pcm` and similar, which are rebuilt from source.
  - Skip sparse files: files of 1 GB or more that use under a tenth of that on disk. This also skips sparse virtual machine images, which is why it is a switch.

## What does not change

Existing plans keep their rules. Enable the preset on a plan to get the new ones. Saved plans open with the three switches off.

## Limits

Folder rules match names only. A folder called `BuildSomething` is not skipped by name, because a prefix rule would also hit real source folders. Its cache folders are skipped, which leaves the built app copy.

## Validation

- 270 core tests, one skipped, zero failures. Nine tests are new.
- The Pro edition builds.
- Universal (arm64 + x86_64) DMG: Developer ID signed, Apple-notarized, stapled, and Gatekeeper-accepted.

Not tried on a live scheduled plan: the new switches were tested in the filter and scanner code, and a real 2 GB sparse file, but not in a plan run through the app.

Version bumped to 2.8 rather than a new build of 2.7, because Munki-managed Macs compare the version number, not the build number.

macOS 14 or later.
