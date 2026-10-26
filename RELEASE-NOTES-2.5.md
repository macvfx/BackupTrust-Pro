# BackupTrust Pro 2.5 (25) — Beta

> **Beta — for testing only.** BackupTrust Pro is not a commercial product and comes with no support and no warranty. Keep your own independent backup of all data before and while using it. You are responsible for your data.

This beta fixes a stale-status problem: a plan that briefly lost its destination could keep showing "No destination volumes are currently available" long after the volume reconnected.

## Fixes

- **A plan's destination-unavailable error outlived the outage that caused it.** The error banner is set when a run is refused, but until now nothing cleared it except running the plan again — on a daily schedule, up to 24 hours later. The app's own periodic availability check already knew the volume was back; it just never looked at the error banner sitting on top of it. The banner now clears itself as soon as the condition that set it resolves. A run that failed partway through a copy still stays visible until you rerun the plan — the volume being reachable again says nothing about whether those files are correct.

## Changes

- **Added a Refresh button** to the menu bar, next to Back Up All Now, so you can check volume availability immediately instead of waiting for the automatic check.
- **Version bumped to 2.5** rather than a new build of 2.4. This is a packaging change only — Munki-managed Macs compare the version number, not the build number, so a build-only bump would be invisible to them.

## Checking the update

Point a plan at a destination, disconnect the share, trigger a failed run (or wait for a scheduled one to fail), then reconnect the share. The plan's status should clear on its own within about 15 seconds, without needing to run the plan again. The Refresh button in the menu bar forces the same check immediately.

## Validation

- 255 core tests, one skipped, zero failures.
- Both editions build.
- Signing, notarization, staple, Gatekeeper and checksum results are recorded with the release.

macOS 14 or later. Download the DMG and checksum below.
