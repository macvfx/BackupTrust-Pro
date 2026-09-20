# BackupTrust Pro — Workflows

Current testing release: **2.3 (23)**.

These workflows cover direct access, scripts, and proactive SMB preparation in BackupTrust Pro.

## Lucid Classic Source to SMB NAS: Check After an Upgrade

1. Keep the saved source and destination if they are still correct. For example, the source may be `/Volumes/Example_Cloud/Example_Filespace/Docs` and the destination `/Volumes/Example_Archive/Backups/Docs`.
2. Open the plan's **Diagnostics** section and click **Refresh Paths**. Read the available-path count, then inspect **Logs → App Diagnostics** for the saved paths, matched mount and Lucid status.
3. If the paths appear available, click **Back Up Now** and inspect the resulting session log. Zero copied can mean all files were already up to date; read the scan and completion details.
4. If the run still cannot start, use **Run Diagnostics**. This performs temporary write/copy probes on the destination and attempts cleanup. Retain both its log and the rejected-run entries.
5. Use **Change…** only if the folder location needs changing. Repeatedly selecting the same folder will not repair a disconnected client or an unavailable NAS.

Version 2.3 fixes recognition of nested mounts without treating stale mount-point directories as mounted filesystems. The affected Lucid Classic-to-SMB workflow was reported working by an operator after updating to the signed 2.3 build; this does not establish acceptance for every storage configuration or encryption/restore workflow.

## Unattended SMB Source to Two Destinations

Fictional plan:

- source: `/Volumes/Example_Source/Current`;
- primary destination: `/Volumes/Example_Archive/Backups/Current`;
- secondary destination: `/Volumes/Example_Local/Backups/Current`.

In SMB Connect 0.7.4 or later, configure `Example_Source` and `Example_Archive` as unique Mount Names, save their credentials, and enable **Allow apps to request this volume**. Enable Pro's default-off preparation setting in **Settings → General**.

At run start, Pro requests each missing SMB Mount Name once, serializing the two different volumes. It waits for the complete stored folders, then applies its normal source and destination gates. If `Example_Local` is reachable while `Example_Archive` remains unavailable, the run can continue to the reachable destination and names the unavailable destination in the UI and logs. If the source or every destination remains unavailable, the run stops safely.

## Same SMB Volume Across Concurrent Plans

If two plans simultaneously require `/Volumes/Example_Archive/...`, they share one in-flight Mount Name request. They independently refresh their own required paths afterward. A real mounted `Example_Archive` is never requested again merely because one plan's relative folder is missing.

## Scripted Preparation and Follow-Up

Use a reviewed pre-script for local preparation that BackupTrust itself should not perform, such as creating a disposable database snapshot. Enable **Abort on failure** when copying without that snapshot would be misleading. Use a post-script only for an intentional follow-up after successful backup completion.

Do not place SMB credentials in scripts. SMB Connect retains endpoints, credentials, Keychain access, share selection, and mounting authority.

## Failure and Recovery

- **No URL handler:** open or install SMB Connect 0.7.4 or later; Pro does not send a request.
- **Filesystem timeout:** review the correlation UUID in diagnostics; the timeout is not a refusal response.
- **Mounted volume, missing folder:** correct the saved folder or create it through the normal operator workflow; Pro does not remount the volume.
- **Cancellation:** BackupTrust stops waiting, but a request already delivered to SMB Connect cannot be recalled.
- **Destination drops mid-run:** the existing reconnect wait protects partial progress for up to three minutes; this is separate from pre-run preparation.

Use only disposable test shares for disruptive reconnect testing, and obtain explicit approval before force-unmounting or changing live NAS state.
