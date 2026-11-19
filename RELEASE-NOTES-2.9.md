# BackupTrust Pro 2.9 (30) — Beta

> **Beta — for testing only.** BackupTrust Pro is not a commercial product and comes with no support and no warranty. Keep your own independent backup of all data before and while using it. You are responsible for your data.

This beta adds one filter switch and brings the in-app help and guides up to date with the code-folder filters from 2.8.

## New

- **Skip P5 container blocks.** A new switch in File Filters skips Archiware P5 container block files (`.p5b`), which can run to hundreds of MB. It is off by default, so a plan that backs up a P5 container volume keeps them. Saved plans open with it off.

## Documentation

- **In-app Quick Start help** now points code-project plans at the Xcode & Swift and Python & Dev Tools presets and the filter switches.
- **The user guide gains a "Backing Up Code Folders" section** covering the presets, the four skip switches (credential files, compiler output, P5 container blocks, sparse files), and what a mirror plan does when a skip is turned on.

## Validation

- 271 core tests, one skipped, zero failures. One test is new.
- The Pro edition builds.
- Universal (arm64 + x86_64) DMG: Developer ID signed, Apple-notarized, stapled, and Gatekeeper-accepted.

Not tried on a live scheduled plan: the new switch is tested in the filter code only, not in a plan run through the app.

Version bumped to 2.9 rather than a new build of 2.8, because Munki-managed Macs compare the version number, not the build number.

macOS 14 or later.
