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

## Updates and Support

BackupTrust Pro includes **Check for Updates…** because it is distributed outside the Mac App Store. Nothing is downloaded or installed automatically. Use **Settings → Logs** and the correlated session and diagnostic entries when troubleshooting.

See [BackupTrust Pro Workflows](BackupTrust-Workflows.md) for setup examples.
