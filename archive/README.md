# Archive: Azure VM – MDE Onboarding Status Workbook (Prior Version)

> **This is a prior version.** The workbook has been significantly revised and is actively maintained as [AzureVmMdeOmboardingStatus.json](../AzureVmMdeOmboardingStatus.json) in the root of this repository. Use that version for new deployments.

---

This version of the workbook was built to verify Microsoft Defender for Endpoint (MDE) onboarding across Azure virtual machines. It correlates Azure activity data with MDE device records to identify machines that are actively running but not properly reporting to MDE.

The current version expanded coverage to include Azure Arc-enabled servers, added a secondary search method for environments where the primary correlation method returns incomplete results, and significantly revised filters, help content, and visual layout.

## What This Version Covers

- Azure VMs with recorded activity in the selected time range
- Cross-reference against MDE device records
- MDE onboarding status per device
- Microsoft Defender Antivirus status

## Data Sources Required

- `AzureActivity` — used to identify active VMs; recommended time range 14–30 days
- Azure Resource Graph — `resources` query to enumerate VMs and Arc servers
- MDE device table via Microsoft Defender portal data connector

## Import

1. Open the Azure Portal.
2. Navigate to **Azure Monitor → Workbooks**.
3. Select **+ New**.
4. Open **Advanced Editor**.
5. Switch to **JSON** mode.
6. Paste the contents of `AzureVmMdeOmboardingStatus_prior.json`.
7. Select **Apply** and save the workbook.

## Notes

- This version does not include Arc-enabled server support.
- The secondary search method introduced in the Feb 2026 update is not present.
- Use the current version in the repo root for any new deployment.

## Files

- `AzureVmMdeOmboardingStatus_prior.json`

## License

Provided as-is. Validate in your environment before production use.
