# Archive: Entra Admin and Risky User Review Workbook

> **This is an archived version.** The workbook has been significantly revised and is actively maintained at [AndrewBlumhardt/EntraAdminReviewWorkbook](https://github.com/AndrewBlumhardt/EntraAdminReviewWorkbook). Use that version for new deployments.

---

This workbook supports structured analyst review of administrative and risky users in Microsoft Entra ID. It correlates Entra sign-ins, Sentinel incidents and alerts, UEBA, MDE device context, PIM activation history, and Azure administrative activity in one place.

## Why Admin Account Hunting Matters

The hardest attacker to stop is a legitimate admin using their own assigned credentials. Whether disgruntled, coerced, compensated, or placed deliberately, an admin acting within their authorized scope generates activity that looks normal because it is technically normal. Standard detection tools are not built to distinguish an admin doing their job from one doing harm.

The signal is in the deviation. This workbook looks for:

- **Unusual sign-in patterns** before successful authentication: failed attempts, atypical locations, unfamiliar devices, or sequences that do not match the account's history
- **PIM activation anomalies**: role activations at unusual times, thin or missing justifications, or frequency that does not match prior behavior
- **Admin change patterns**: bursts of privileged changes or modifications outside normal operational windows
- **Behavioral drift**: UEBA signals, risky user flags, and device context suggesting an account is operating differently than it has historically

## Coverage

- Main admin review with sign-in, incident, alert, device, and PIM pivots
- Risky user investigation path using the same correlation flow
- PIM activity trends and activation-level detail including role, source IP, and justification

## Data Sources Required

- `SigninLogs`
- `AuditLogs`
- `IdentityInfo` (if available from MDI/M365 Defender integration)
- `BehaviorAnalytics`
- `SecurityIncident`
- `SecurityAlert`
- `DeviceLogonEvents`
- `DeviceInfo`
- `AzureActivity`

## Import

1. Open Microsoft Sentinel or Azure Monitor.
2. Go to **Workbooks**.
3. Create a new workbook and open **Advanced Editor**.
4. Switch to **JSON** mode.
5. Paste contents of `AzureAdminReviewWorkbook.json`.
6. Save the workbook.

## Critical Dependency: Search Term Pattern

The **Search Term** parameter identifies likely admin identities by UPN pattern. Default value is `.admin@`. Tune this first to match your environment's naming standard before relying on any correlation results.

## Notes

- Validate correlated user matches before operational action
- Use PIM comment and source IP context to identify weak or unusual activation behavior
- Empty widgets usually indicate missing data source coverage, not workbook errors
- The current version at [AndrewBlumhardt/EntraAdminReviewWorkbook](https://github.com/AndrewBlumhardt/EntraAdminReviewWorkbook) includes additional refinements and is the recommended starting point
