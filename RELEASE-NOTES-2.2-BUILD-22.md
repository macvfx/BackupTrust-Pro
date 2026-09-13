# BackupTrust Pro 2.2 (22)

This testing prerelease adds restoring encrypted backups from inside the app. Encryption remains a Pro feature; the sandboxed BackupTrust Standard edition neither creates nor restores encrypted destinations.

## Highlights

- **Restore Files…** in Settings → Encryption, and a **Restore…** button beside each encrypted destination in the plan editor. Choose the backup folder, its recovery key file, and where the files should go.
- The original folder structure is rebuilt, and restored files keep their original modification dates.
- Files already present in the restore folder are **skipped rather than replaced** unless you ask. A restore must never be the thing that destroys the copy you already had.
- Previous versions under `_BackupVersions` are excluded unless you ask for them.
- The recovery key is checked against the backup's own marker as soon as it is chosen, so a wrong key is refused before anything is written rather than after.
- A pre-flight shows the file count and size and refuses to start if the restore folder lacks room.
- One file failing to decrypt does not stop the rest: it is named in the summary, its partial output is removed, and everything else still restores.
- Progress per file, cancellable, and Reveal in Finder when it finishes.

**Test a Restore…** is unchanged and still does something different: it decrypts a sample in memory and writes nothing, to prove a backup opens. Verify after copy cannot prove that, because a backup run holds no private key by design.

Restoring without BackupTrust — the `restore-encrypted-backup.sh` script that ships inside every encrypted destination — works exactly as before, and remains the answer on a Mac that does not have the app.

## Requirements

- macOS 14 or later on Apple Silicon or Intel.
- The recovery key file for the backup being restored. Nothing else can decrypt it.
- Room in the restore folder for the decrypted files.

## Validation boundary

The universal Developer ID build was notarized by Apple and passed signature, staple, Gatekeeper, version/build, architecture and SHA-256 validation. Automated tests cover the restore path directly: structure and contents, original dates preserved, existing files left alone by default and replaced only on request, versions excluded then included, BackupTrust's own folder ignored, a wrong key refused before writing, one damaged file not stopping the others, cancellation, and the pre-flight counts.

The boundaries from 2.1 still stand. **No run has been made against an SMB NAS or a LucidLink volume**, so per-file latency, mid-run mount loss and the filename-length limit on encrypted NAS shares remain untested in practice. **The encryption and restore interfaces have not yet been exercised by a person**; they are covered by automated tests of the logic beneath them. The restore flow in particular writes files, so try it on a scratch folder first.

See [ENCRYPTION-TESTING.md](ENCRYPTION-TESTING.md) for what to try and what to report.

SHA-256 for `BackupTrust-Pro-2.2.dmg`:

`6f7719a1f04d1bffa4186bb1582050f3d6dc759fab7bb8efd3a94723fd7242f0`
