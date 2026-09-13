# BackupTrust Pro — User Guide

Current testing release: **2.0 (20)**.

BackupTrust Pro provides scheduled folder backups, filters, verification, overflow handling, destination locking, reconnect waiting, diagnostics, and logs. The sections below cover its direct-access, script, update, and proactive SMB preparation behavior.

## Direct Filesystem Access

Pro is not App Sandbox-enabled and does not activate security-scoped access during a backup run. The operator must still have normal macOS read permission for the source and write permission for each destination. If a stored folder moves, use **Change…** in the plan editor and select the correct folder again.

This direct-access model is intended for managed storage environments where sandbox bookmark behavior is unsuitable. It does not bypass filesystem permissions, share ACLs, Full Disk Access requirements, or offline storage.

## Pre- and Post-Backup Scripts

Each plan may run an optional zsh script before or after the backup. Configure scripts in **Settings → Plans → Scripts**.

- A pre-script can abort the backup when **Abort on failure** is enabled.
- Each script has a bounded timeout from 30 seconds to 30 minutes.
- **Test Run** executes the configured script intentionally and reports its result.
- Script output and outcome are recorded in the plan session log.

Scripts run with the signed-in user's inherited environment and authority. Review every script before enabling it, avoid embedding credentials, and use fictional or disposable fixtures for tests.

## Prepare Offline SMB Volumes

In SMB Connect 0.7.4 or later, configure every required share with:

1. a unique **Mount Name** matching the first component of the stored `/Volumes` path;
2. saved credentials;
3. **Allow apps to request this volume** enabled.

Then enable **Settings → General → Prepare offline SMB volumes before backups**. The setting is off by default and applies to both manual and scheduled runs.

For the fictional destination `/Volumes/Example_Archive/Backups/Example_Project`, BackupTrust sends only `Example_Archive` and a correlation UUID using `smbconnect://v1/mount`. It does not receive the server address or credentials. It waits for the complete stored path and refreshes normal availability afterward.

BackupTrust groups requirements by Mount Name, coalesces concurrent requests for the same volume, and serializes different-volume requests. It sends at most one request per missing volume. A stale empty directory under `/Volumes` is not treated as a mount. A genuinely mounted volume is not remounted merely because the required relative folder is absent.

For an eligible offline path, use **Mount with SMB Connect** or **Try Again** in the menu bar. **Cancel Mount** stops BackupTrust's bounded wait, but cannot recall a URL already delivered to SMB Connect. Logs record the Mount Name, UUID, elapsed time, and factual outcome.

A timeout means BackupTrust did not observe the required filesystem path within the wait period. SMB Connect v1 has no refusal response, so do not interpret or report the timeout as a refusal. Review both apps' diagnostics.

After preparation, a missing source still aborts. A plan with no reachable destination still aborts. If at least one destination is reachable, existing partial-destination behavior is preserved and unavailable destinations remain explicit in the UI and logs.

## Reconnect Waiting Is Separate

Proactive preparation occurs before a run reaches its normal availability gates. The existing three-minute reconnect wait applies only when a destination disappears after copying has started. SMB Connect auto-reconnect may restore that active mount, but it is a different workflow from the Pro pre-run request.

## Encryption

A destination can be encrypted, so files written there are unreadable without its
recovery key. Each file becomes an Apple Encrypted Archive with an added `.aea`
extension. The choice is per destination, so one plan can copy to a local drive in
the clear and to a NAS encrypted in the same run.

Pro holds only the public half of the key, which is all that encrypting needs. So a
scheduled run needs no password and cannot be stopped by a locked Keychain, this
Mac cannot read its own encrypted backups, and **the recovery key file saved when
the key is created is the only way to decrypt them.** Lose it and the backups are
unrecoverable by anyone.

**Setting up:** Settings → Encryption → **Create Key…**, save the recovery key file
somewhere separate from the backup, then choose that key from the lock menu under a
destination in the plan editor. **Encrypt all destinations with…** applies one key
across a plan, including the overflow destination — assign it there too, or
oversized and long-named files land in the clear, which the editor warns about.

**On the destination**, a `BackupTrust` folder appears holding `encryption.json`
(which key this folder uses, by name and fingerprint; no secret in it),
`README-RESTORE.txt`, and an executable `restore-encrypted-backup.sh` that restores
the folder with only `aea`, part of macOS 12 and later.

**Restoring:** Settings → Encryption → **Restore Files…**, or the **Restore…**
button beside an encrypted destination in the plan editor. Choose the backup, its
recovery key file and a destination folder; the structure is rebuilt and restored
files keep their original dates. Existing files are skipped rather than replaced
unless you ask, previous versions are excluded unless you ask, and a file that
fails to decrypt is named while the rest still restore.

**Checking a backup opens** without restoring anything: Settings → Encryption →
**Test a Restore…** decrypts a
sample with the recovery key and names anything that fails, decrypting in memory so
no plaintext is written. This is not the same as **Verify after copy**, which on an
encrypted destination can only confirm that a complete, readable archive landed —
all a machine holding no private key can confirm. Run Test Restore after setting up
an encrypted plan, and occasionally afterwards.

**Checking a backup opens** without restoring anything: Settings → Encryption →
**Test a Restore…** decrypts a
sample with the recovery key and names anything that fails, decrypting in memory so
no plaintext is written. This is not the same as **Verify after copy**, which on an
encrypted destination can only confirm that a complete, readable archive landed —
all a machine holding no private key can confirm. Run Test Restore after setting up
an encrypted plan, and occasionally afterwards.

**What is not hidden:** filenames, folder structure, file sizes and modification
dates stay visible on the destination. Only contents are encrypted.

**Turning encryption on for a destination that already holds backups** re-copies
everything in encrypted form and leaves the unencrypted copies in place, with a
warning naming how many. Mirroring will not remove them; delete them yourself once
the encrypted copies check out.

**Two limits:** a folder holds one key — pointing a different key at a folder that
already has encrypted files is refused, so use a different folder. And on Synology
DSM 7 encrypted shares the ~143-byte filename component limit drops to 139 for
encrypted destinations, because `.aea` spends four bytes.

What to try in this testing build, and what to report, is in
[ENCRYPTION-TESTING.md](ENCRYPTION-TESTING.md).

## Updates and Support

BackupTrust Pro includes **Check for Updates…** because it is distributed outside the Mac App Store. Nothing is downloaded or installed automatically. Use **Settings → Logs** and the correlated session and diagnostic entries when troubleshooting.

See [BackupTrust Pro Workflows](BackupTrust-Workflows.md) for setup examples.
