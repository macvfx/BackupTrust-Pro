# BackupTrust Pro 2.7 (28) — Beta

> **Beta — for testing only.** BackupTrust Pro is not a commercial product and comes with no support and no warranty. Keep your own independent backup of all data before and while using it. You are responsible for your data.

This beta fixes hourly plans that could silently stop running, corrects the "Next:" time in the menu bar, and reorganizes the menu bar buttons. It also covers an internal 2.6 build that was never published.

## Fixes

- **An hourly plan could stop running for a day or more, with nothing in any log.** When several plans shared a destination volume, a plan waiting for its turn could lose the race for the destination lock after being woken, and quietly give up. Because the plan's timer is armed again only when a run finishes, it then never ran again until the app was relaunched or the Mac woke from sleep. A plan that loses the race now waits its turn again.
- **A scheduled run that cannot start now says why and tries again.** Every reason a scheduled run can be skipped (source or destination offline, expired saved access, no destinations, a busy window still active, a failed pre-script, a cancel) is now written to the app diagnostic log, and the plan is retried after 5 minutes instead of being left without a timer.
- **Every scheduled run leaves a line in the log**, so an hourly plan writes one entry an hour: `scheduled run firing`, then either the completed run (with its session log filename) or the reason it was skipped. A plan that never started can now be told apart from one that was not due.
- **The menu bar could show a "Next:" time in the past** (for example yesterday) when runs had not been completing. It now never shows a time earlier than now: an overdue plan shows about 15 seconds from now, as the scheduler actually treats it. The menu bar, the plan list and the Schedule tab all use the same calculation.

## Changes

- **Menu bar buttons reorganized.** Refresh and Back Up All Now are now the two large buttons (Back Up All Now in blue). Settings, Logs, Help and Updates are quiet links underneath, with Quit at the right. A one-line status above the buttons shows "Checking volumes…", "Volumes checked at <time>" after a Refresh, or "Backing up N plans…" while a backup runs. Back Up All Now reads "Backing Up…" and is disabled while any plan is running. Keyboard shortcuts are unchanged.
- **Version bumped to 2.7** rather than a new build of 2.6. This is a packaging change only — Munki-managed Macs compare the version number, not the build number.

## Confirming a scheduled plan is running

Open the app diagnostic log (**Settings → Logs**). Each scheduled run writes `scheduled run firing`, followed by `N copied, M failed | session log: …` when it finishes, or `scheduled run skipped: <reason> — retrying in 300s` if it could not start. Every completed run also has its own session log in the Logs folder. An enabled plan with none of these for several hours needs attention.

## Validation

- 261 core tests, one skipped, zero failures.
- Both editions build.
- Hourly plans sharing one NAS ran every hour for 15 hours, including the case that used to drop a plan silently.
- Universal (arm64 + x86_64) DMG: Developer ID signed, Apple-notarized, stapled, and Gatekeeper-accepted.

Not exercised on a real volume: the retry after a skipped run (the 5-minute retry path was covered by unit tests of the scheduler only).

macOS 14 or later. Download the DMG and checksum below.

SHA-256 for `BackupTrust-Pro-2.7.dmg`:

`1031622e8a75ebac84011efa0c743465c018a5292c24db6fce525ac45ce8b271`
