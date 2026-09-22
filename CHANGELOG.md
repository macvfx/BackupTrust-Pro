# BackupTrust Pro Changelog

## 2.4 (24) — 2026-09-22 — Beta

- Restoring an encrypted backup no longer skips hidden files. A plan that includes hidden files backs `.env` up as `.env.aea`, which is still hidden, and **Restore Files…** passed over it silently while the restore script on the destination restored it. Affects 2.2 and 2.3.
- Mirror mode now removes hidden files that were deleted from the source.
- Hidden files on an overflow destination are now reported as reclaimable once the same file is on a main destination.
- What counts as backup content on a destination is decided in one place, shared by restore, mirror cleanup and overflow reclaim: BackupTrust's own folders, disk metadata, `.DS_Store` and AppleDouble `._` files are never restored, deleted or counted as failures.
- **Test a Restore…** samples exactly what a restore would read.
- About and Help state that Pro is a beta for testing only, with no support or warranty, and that keeping independent backups is the user's responsibility.

Validation: 255 core tests, one skipped, zero failures, seven of them new for these fixes; both editions built. The universal Pro 2.4 (24) DMG passed Developer ID signing, Apple notarization, stapling, Gatekeeper and checksum checks. The beta notice was checked on screen in About, Help and the Check for Updates alert. See [2.4 release notes](RELEASE-NOTES-2.4.md).

## 2.3 (23) — 2026-09-20 — Testing prerelease

- Match the actual mounted ancestor of a selected folder, including nested Lucid mounts, while rejecting stale mount directories.
- Use saved paths directly in Pro instead of substituting bookmark-resolved paths.
- Add **Refresh Paths** in the plan editor's Diagnostics section: check availability without starting a backup or requesting mounts, with results in App Diagnostics.
- For detected or saved Lucid paths, check installed `lucid` and `lucid2` versions and status, and verify the reported mount contains the selected path and exists in the OS mount list. An inactive other client does not invalidate a working client.
- Log saved paths and matched mounts when a run is rejected as unavailable, and include availability context in diagnostic sessions.
- Apply nested mount matching to destination reconnection and check the required destination folder.

Automated validation: 248 core tests, one skipped, zero failures; both editions built successfully. The universal Pro DMG passed signature, notarization, staple, Gatekeeper and checksum checks. An operator reported the affected Lucid Classic-to-SMB workflow working. See [2.3 release notes](RELEASE-NOTES-2.3-BUILD-23.md) for limitations and troubleshooting.

## 2.2 (22) — 2026-09-14 — Testing prerelease

- Added restoring encrypted backups from inside the app: **Restore Files…** in Settings → Encryption, and a **Restore…** button beside each encrypted destination in the plan editor. Until now this needed the restore script and a Terminal window.
- The original folder structure is rebuilt and restored files keep their original modification dates.
- Files already present in the restore folder are skipped rather than replaced unless asked, and previous versions under `_BackupVersions` are excluded unless asked for.
- The recovery key is checked against the backup's marker when chosen, so a wrong key is refused before anything is written.
- A pre-flight reports the file count and size and refuses to start when the restore folder lacks room. One file failing to decrypt is named in the summary while the rest still restore.
- **Test a Restore…** is unchanged and still writes nothing: it decrypts a sample in memory to prove a backup opens.

Automated validation covered the restore path directly: structure and contents, original dates, existing files left alone by default and replaced only on request, versions excluded then included, a wrong key refused before writing, one damaged file not stopping the others, cancellation, and the pre-flight counts. No run was made against an SMB NAS or a LucidLink volume, and neither the encryption nor the restore interface has yet been exercised by a person.

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
