# BackupTrust Pro Changelog

## 2.0 (20) — 2026-08-28 — Testing prerelease

- Corrected the in-app **Open Workflows Doc** action to open this dedicated Pro documentation repository.
- BackupTrust Standard continues to use its separate Standard documentation.

## 2.0 (19) — 2026-08-28

- Added default-off proactive preparation of missing saved SMB volumes before manual and scheduled runs through SMB Connect 0.7.4 or later.
- Added **Mount with SMB Connect**, **Try Again**, preparation status, and cancellation controls for eligible offline SMB paths.
- Added validated Mount Name and correlation UUID requests, bounded filesystem observation, same-volume coalescing, different-volume serialization, and correlated diagnostics.
- Distinguished true mounted filesystems from stale directories below `/Volumes` and rejected unsafe or duplicate-style mount identities.
- Preserved existing partial-destination behavior while making unavailable destinations explicit.
- Kept proactive pre-run preparation separate from the existing mid-run reconnect wait.

Automated validation covered the SMB request contract, path validation, mocked no-handler behavior, one-shot timeout, cancellation, stale mount points, concurrency, and both application editions. No destructive unmount or live NAS mutation was performed.
