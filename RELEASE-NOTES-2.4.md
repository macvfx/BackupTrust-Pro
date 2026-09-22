# BackupTrust Pro 2.4 (24) — Beta

> **Beta — for testing only.** BackupTrust Pro is not a commercial product and comes with no support and no warranty. Keep your own independent backup of all data before and while using it. You are responsible for your data.

This beta fixes three ways hidden files were handled wrongly. It matters only for plans with **Include hidden files** turned on, where it matters a great deal: restoring such a backup left files behind.

## Fixes

- **Restoring an encrypted backup skipped every hidden file.** A plan that includes hidden files backs up `.env` as `.env.aea`, which is still hidden, and **Restore Files…** passed over it without saying so. The restore script on the destination always restored them, so the app and the script disagreed. Affects 2.2 and 2.3.
- **Mirror mode never removed hidden files** that had been deleted from the source, so a mirrored destination kept them for good.
- **Hidden files sent to an overflow destination were never reported as reclaimable**, even once the same file was on a main destination, so overflow kept filling up.

## Changes

- What counts as backup content on a destination is now decided in one place, shared by restore, mirror cleanup and overflow reclaim. BackupTrust's own folders, disk metadata such as `.Trashes` and `.Spotlight-V100`, `.DS_Store` and the `._` companion files macOS writes on SMB and exFAT disks are never treated as your files: never restored, never deleted, never counted as failures.
- **Test a Restore…** now samples exactly what a restore would read.
- About and Help show the beta notice above.

## Checking the update

With a plan that includes hidden files, run a backup, then **Restore Files…** into an empty folder and confirm dot-files such as `.env` come back. In mirror mode, delete a hidden file from the source, run again, and confirm it is removed from the destination.

## Validation

- 255 core tests, one skipped, zero failures. Seven are new and cover these fixes.
- Both editions build.
- Signing, notarization, staple, Gatekeeper and checksum results are recorded with the release.

macOS 14 or later. Download the DMG and checksum below.

SHA-256 for `BackupTrust-Pro-2.4.dmg`: recorded when the build is published.
