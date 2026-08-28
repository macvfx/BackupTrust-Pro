# BackupTrust Pro 2.0 (20)

This testing prerelease adds proactive SMB preparation through SMB Connect 0.7.4 or later and corrects the Pro documentation link.

## Highlights

- Optional pre-run requests for missing SMB volumes before manual or scheduled backups.
- One request per missing Mount Name, with same-volume coalescing and different-volume serialization.
- Menu-bar **Mount with SMB Connect**, **Try Again**, status, and cancellation controls.
- Factual timeout guidance and correlated Mount Name, UUID, elapsed-time, and outcome logging.
- Protection against stale `/Volumes` directories, unsafe Mount Names, and duplicate-style `-1` or `-2` paths.
- BackupTrust Standard behavior remains unchanged.

## Requirements

- macOS 14 or later on Apple Silicon or Intel.
- SMB Connect 0.7.4 or later for proactive SMB preparation.
- A unique Mount Name, saved credentials, and **Allow apps to request this volume** enabled for each requested share.

## Validation boundary

The universal Developer ID build was notarized by Apple and passed signature, staple, Gatekeeper, version/build, architecture, and SHA-256 validation. Automated tests covered the SMB request and availability logic. This prerelease has not performed destructive unmount or live NAS mutation testing.

SHA-256 for `BackupTrust Pro.dmg`:

`2b9e72f09ff34f00cf58be5dffd33d6bd1d87712a38def9ab108b6e54c878ba0`
