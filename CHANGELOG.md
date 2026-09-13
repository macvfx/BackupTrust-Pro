# BackupTrust Pro Changelog

## 2.1 (21) — 2026-09-13 — Testing prerelease

- Added per-file encryption of chosen destinations, including the overflow destination. Files written to an encrypted destination become Apple Encrypted Archives with an added `.aea` extension.
- BackupTrust stores only the public half of an encryption key, so a scheduled run holds no secret and the Mac that wrote a backup cannot read it. The recovery key file saved when the key is created is the only way to decrypt those backups.
- A key is added only after its recovery key file has been saved and confirmed, and removing a key is refused while a plan still uses it.
- Each encrypted destination carries a marker naming its key, plain-text restore instructions, and an executable restore script that needs only the `aea` tool built into macOS.
- Added **Test a Restore**, which decrypts a sample with the recovery key and names anything that fails. Verify after copy cannot prove an encrypted backup decrypts; it confirms a complete, readable archive landed.
- Turning encryption on for a destination that already holds backups re-copies everything encrypted and leaves the unencrypted copies in place with a warning; mirroring does not remove them.
- Encryption is Pro only. Importing a plan into BackupTrust Standard has any encrypted destination refused rather than written unencrypted, while other destinations still run.

Automated validation covered encryption round trips across archive block boundaries including empty files, rejection of tampered, truncated and wrong-key files, mirror behaviour over successive runs, incremental runs, mixed encrypted and plaintext destinations, migration of an existing destination, encrypted overflow, and restoring with the stock `aea` tool. No run was made against an SMB NAS or a LucidLink volume, and the encryption interface has not yet been exercised by a person.

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
