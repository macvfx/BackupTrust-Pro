# BackupTrust Pro

BackupTrust Pro is the direct-access edition of BackupTrust for scheduled, incremental folder backup on macOS. The current testing release is **2.2 (22)**.

It includes backup plans and schedules, verification, overflow routing, destination locking, reconnect waiting, diagnostics, logs, per-file encryption of chosen destinations, pre- and post-backup scripts, self-managed update checking, and opt-in proactive SMB preparation through SMB Connect 0.7.4 or later.

BackupTrust Pro has its own bundle identifier, display name, update behavior, and operator guidance. It does not replace or alter the sandboxed BackupTrust Standard edition.

## Requirements

- macOS 14 or later on Apple Silicon or Intel;
- normal macOS filesystem permissions for every selected path;
- for proactive SMB preparation, SMB Connect 0.7.4 or later with a unique Mount Name, saved credentials, and **Allow apps to request this volume** enabled for each share.

BackupTrust Pro never reads SMB endpoints, passwords, or SMB Connect Keychain items and never mounts a share directly. It sends only the validated Mount Name and a correlation UUID, then observes its stored `/Volumes/<Mount Name>/<relative folder>` path.

## Documentation

- [BackupTrust Pro User Guide](BackupTrust-Pro-UserGuide.md)
- [BackupTrust Pro Workflows](BackupTrust-Workflows.md)
- [Release notes](RELEASE-NOTES-2.2-BUILD-22.md)
- [Encryption testing guide](ENCRYPTION-TESTING.md)
- Earlier release notes: [2.1 (21)](RELEASE-NOTES-2.1-BUILD-21.md), [2.0 (20)](RELEASE-NOTES-2.0-BUILD-20.md)
- [Changelog](CHANGELOG.md)

## Distribution

This public repository contains operator documentation and signed testing builds. BackupTrust Pro application source code, signing material, notarization receipts, and internal engineering records are not published here.
