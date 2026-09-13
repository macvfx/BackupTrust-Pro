# BackupTrust Pro 2.1 (21)

This testing prerelease adds per-file encryption of chosen destinations. Encryption is a Pro feature; the sandboxed BackupTrust Standard edition does not offer it.

## Highlights

- A destination can be assigned an encryption key. Files written there become Apple Encrypted Archives with an added `.aea` extension, readable only with that key's recovery key file.
- Encryption is chosen per destination, including the overflow destination, so one plan can write plaintext to a local drive and encrypted files to a NAS in the same run.
- BackupTrust stores only the public half of a key. A scheduled run therefore needs no password and cannot be blocked by a locked Keychain, and the Mac that made a backup cannot read it.
- The recovery key file saved when a key is created is the only way to decrypt those backups. A key is not added until that file has been saved and the confirmation ticked.
- Each encrypted destination describes itself: a marker naming the key, plain-text restore instructions, and an executable restore script that needs only the `aea` tool built into macOS.
- **Test a Restore** decrypts a sample with the recovery key and names anything that failed, decrypting in memory so no plaintext is written. Verify after copy cannot prove an encrypted backup decrypts; it confirms a complete, readable archive landed.
- Turning encryption on for a destination that already holds backups re-copies everything encrypted and leaves the unencrypted copies in place, with a warning naming how many. Mirroring does not remove them.
- Importing a plan into BackupTrust Standard has any encrypted destination refused rather than written unencrypted; other destinations in the plan still run.

## Requirements

- macOS 14 or later on Apple Silicon or Intel.
- macOS 12 or later on whichever Mac performs a restore, for the built-in `aea` tool. BackupTrust does not need to be installed to restore.
- Somewhere to keep the recovery key file that is not the backup destination.

## Validation boundary

The universal Developer ID build was notarized by Apple and passed signature, staple, Gatekeeper, version/build, architecture and SHA-256 validation. Automated tests cover encryption round trips across archive block boundaries including empty files, rejection of tampered, truncated and wrong-key files, mirror behaviour over successive runs, incremental runs, mixed encrypted and plaintext destinations, refused destinations not stopping sound ones, migration of an existing destination, encrypted overflow, and restoring with the stock `aea` tool.

Two boundaries are worth stating plainly. **No run has been made against an SMB NAS or a LucidLink volume** — all measurements so far are local disks, so per-file latency, mid-run mount loss and the filename-length limit on encrypted NAS shares are untested in practice. **The encryption interface has not yet been exercised by a person**; it is covered by automated tests of the logic beneath it.

On Synology DSM 7 encrypted shared folders the filename component limit of about 143 bytes drops to 139 for encrypted destinations, because the added extension spends four bytes.

See [ENCRYPTION-TESTING.md](ENCRYPTION-TESTING.md) for what to try and what to report.

SHA-256 for `BackupTrust-Pro-2.1.dmg`:

`32513dc5fdf21cab17bc0d06be96e7594534d7f92a1d38c48569949cc5919342`
