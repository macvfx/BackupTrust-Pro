# BackupTrust Pro 2.3 (23) — Beta

> **Beta — for testing only.** BackupTrust Pro is not a commercial product and comes with no support and no warranty. Keep your own independent backup of all data before and while using it. You are responsible for your data.

This beta fixes false offline detection for nested LucidLink mounts and improves path troubleshooting. Pro remains unsandboxed, and existing saved folders do not need to be recreated to apply the fix.

## Changes

- Match the actual mounted ancestor of a selected folder, including nested Lucid mounts, while rejecting stale mount directories.
- Use saved paths directly in Pro instead of substituting bookmark-resolved paths.
- Add **Refresh Paths** in the plan editor's Diagnostics section: check availability without starting a backup or requesting mounts, with results in App Diagnostics.
- For detected or saved Lucid paths, check installed `lucid` and `lucid2` versions and status, and verify the reported mount contains the selected path and exists in the OS mount list. An inactive other client does not invalidate a working client.
- Log saved paths and matched mounts when a run is rejected as unavailable, and include availability context in diagnostic sessions.
- Apply nested mount matching to destination reconnection and check the required destination folder.

## Checking the update

Open the affected plan and choose **Diagnostics → Refresh Paths**, then **Back Up Now**. If the run cannot start, inspect **Logs → App Diagnostics** and use **Run Diagnostics** for source reads and temporary destination write/copy probes. The Logs Refresh button only reloads log files.

CLI status is diagnostic evidence, not a new backup veto. Missing CLI tools, timeouts and unmatched/default-instance status remain warnings; multiple-instance configurations may need inspection. The general offline label still does not distinguish every filesystem error from a timeout.

## Validation

- 248 core tests completed, with one skipped and zero failures.
- Both Pro and Standard Debug builds passed; the Pro Release archive contains Apple Silicon and Intel binaries.
- Developer ID signature, Apple notarization, staple, Gatekeeper, version/build and checksum checks passed.
- An operator reported the affected Lucid Classic-to-SMB NAS workflow working with the signed 2.3 build. This is an operator report, not an independent exhaustive acceptance test. It does not expand the encryption/restore acceptance claims from earlier releases.

macOS 14 or later. Download the DMG and checksum below. Application source and private release receipts are not included.

SHA-256 for `BackupTrust-Pro-2.3.dmg`:

`3dd52cb1f0368ca84c4256a19d11bc87fc51f82d57654538664eeb0fd43879cc`
