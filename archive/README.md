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

## Critical Dependency: Search Term Pattern

The **Search Term** parameter is the key dependency for this workbook.

- Default value: `.admin@`
- Purpose: identify likely admin identities in UPN patterns
- Example alternatives: `adm@`, `-admin@`, `_admin@`, or a dedicated admin domain marker

If your environment does not have a clear and consistent admin naming pattern, matching and correlation quality drops significantly.

Use this view to validate and tune the Search Term first:

## Major Revision Coverage

### 1. Main Admin Review Experience

The main view lists likely admin accounts and enables deeper investigation pivots, including sign-in activity, related incidents and alerts, device context, PIM history, and Azure activity.

### 2. Risky User Review (Details)

The risky-user path applies the same investigation flow to users flagged by identity risk signals. This is useful for quickly determining whether risky accounts also show privileged or suspicious operational behavior.

### 3. PIM Activity Review

The PIM section provides broad role activation visibility and trend analysis, including role/category filters and activation patterns.

### 4. PIM Activity Details

The detail view highlights activation-level evidence, including role, category, source IP, and justification comments for deeper governance and compliance review.

## Data Sources Required

For full functionality, your workspace should include:

- `SigninLogs`
- `AuditLogs`
- `IdentityInfo` (if available from MDI/M365 Defender integration)
- `BehaviorAnalytics`
- `SecurityIncident`
- `SecurityAlert`
- `DeviceLogonEvents`
- `DeviceInfo`
- `AzureActivity`

If one or more sources are missing, sections can appear empty.

## Import

1. Open Microsoft Sentinel or Azure Monitor.
2. Go to **Workbooks**.
3. Create a new workbook and open **Advanced Editor**.
4. Switch to **JSON** mode.
5. Paste contents of `AzureAdminReviewWorkbook.json`.
6. Save the workbook.

## Analyst Notes

- Start by tuning **Search Term** until admin candidate detection matches your naming standard.
- Validate correlated user matches before operational action.
- Use PIM comment and source IP context to identify weak or unusual activation behavior.
- Empty widgets usually indicate missing data source coverage, not necessarily workbook errors.

## Files

- `AzureAdminReviewWorkbook.json`

## License

Provided as-is. Validate queries and assumptions in your environment before production use.
