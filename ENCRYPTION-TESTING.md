# BackupTrust Pro 2.1 (21) — Encryption Test Build

For whoever installs this build to try the new encryption. Pro only: the App Store
edition does not offer encryption and refuses a destination that asks for it.

This is the operator's side: what to set up, what to try, and what to report.

---

## Install

Open the DMG, drag **BackupTrust Pro** to Applications, launch it. The build is
signed and notarized, so Gatekeeper should not complain.

It is published as a **pre-release**, so the in-app update check will not offer it
to anyone — the checker only ever sees a release marked latest.

Existing plans are untouched by this build, and encryption is off unless a
destination is explicitly given a key.

## Before you start

**Test on a scratch folder first, not on real work.** Encryption is the one part of
a backup app where a mistake is unrecoverable: if the recovery key file is lost,
the backup cannot be decrypted by anyone.

Make two throwaway folders, put a handful of files in the first, and use those as
source and destination until the flow is familiar.

## Setting it up

1. **Settings → Encryption → Create Key…** Name it, then save the recovery key
   file. The key is not added until that file is saved and the checkbox ticked.
   Keep the file somewhere other than the backup destination.
2. Create a plan with your scratch source and destination.
3. In the plan editor, click the lock menu under the destination and choose the
   key. The row should read **Encrypted · <key name>**.
4. **Back Up Now.**

The destination should now hold `.aea` files plus a `BackupTrust` folder containing
`encryption.json`, `README-RESTORE.txt` and `restore-encrypted-backup.sh`.

## Worth trying

- **Test a Restore…** (Settings → Encryption) against that destination with the
  recovery key. It should report decrypting a sample. Try it with a *different*
  key file too: it should refuse before decrypting anything.
- **Restore for real without the app**, which is the point of the design:

  ```
  "<destination>/BackupTrust/restore-encrypted-backup.sh" \
      -k /path/to/recovery.key -o ~/Desktop/restore-test
  ```

  Add `--dry-run` first if you prefer, or `--compare <source folder>` to check
  every file byte for byte.
- **A second run** should copy nothing.
- **Mirror mode:** delete a file from the source and run again. Only that file's
  counterpart should disappear from the destination.
- **A mixed plan:** one destination encrypted, one not. Both should work, and the
  plan row should show a half-open lock rather than a closed one.
- **Overflow:** set an overflow destination and leave it unencrypted while the
  primary is encrypted. The editor should warn that those files land in the clear.
- **Re-grant access** (`Change…`) on an encrypted destination, then run again. The
  backup should keep working — the key should survive the folder being re-selected.

## What to report

- Anything refused with a message you cannot act on, or a refusal you disagree with.
- Any case where a file lands **unencrypted** on a destination that has a key.
- Wording that is unclear, especially in the key-creation sheet: it has to make
  plain that the recovery file is the only copy.
- Throughput against a NAS or LucidLink volume, with rough file counts and sizes.
  This is the one thing we have not measured.
- Filename-length warnings on Synology DSM 7 encrypted shares. The limit drops
  from about 143 bytes to 139 for encrypted destinations, because of `.aea`.

Session logs are in **Settings → Logs**; the per-run log names each destination's
key fingerprint and any refusal. Include the relevant lines when reporting
anything, and say which storage the destination was on.

## Known limits in this build

- **Not tested against a real SMB NAS or LucidLink volume.** Local disks only.
- **A folder holds one key.** Pointing a different key at a folder that already
  has encrypted files is refused; use a different folder.
- **Turning encryption on for a destination that already holds backups** re-copies
  everything encrypted and leaves the old unencrypted copies in place. There is no
  button to remove them; mirroring will not, by design.
- **No passphrase option.** Recovery is by key file only.
- **Verify after copy cannot prove an encrypted backup decrypts** — it confirms a
  complete, readable archive landed. Test Restore is the proof.
