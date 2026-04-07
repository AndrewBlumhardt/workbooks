# Azure VM & Arc Resource - Security Health

## Overview

**Azure VM & Arc Resource - Security Health** is an Azure Monitor Workbook that provides a unified security and operational view of Azure virtual machines, VM scale sets, and Azure Arc-enabled servers.

The workbook merges data from Azure Resource Manager, Microsoft Defender for Endpoint (MDE), and Azure Activity Logs into a single correlated view. This merge-based approach surfaces configuration gaps, security risks, and signs of resource neglect that are not visible from any single data source.

This workbook is intended for security engineers, cloud administrators, and SOC teams responsible for Defender for Servers, MDE compliance, and VM governance.

> A prior version focused exclusively on MDE onboarding status is preserved in the [archive](./archive/) folder.

---

## Screenshot

<p align="center"><img src="https://github.com/AndrewBlumhardt/workbooks/blob/main/images/workkbook.png" width="75%"/></p>

---

## What This Workbook Shows

For each VM, VMSS, or Arc-enabled server, the workbook surfaces:

- **MDE onboarding status** — whether the device is onboarded and the sensor health state
- **Microsoft Defender Antivirus status** — enabled or unknown
- **Azure VM Agent health** — critical for policy enforcement, extension deployment, and cloud resource management
- **Secure boot and auto-update configuration** — key security baseline settings
- **Power status** — running, deallocated, or stopped
- **Last management activity** — identifies the most recent Azure control-plane event, useful for detecting abandoned or orphaned resources
- **Last human admin interaction** — identifies the last person to interact with the resource through Azure Activity Logs; combined with a separate admin account query, this supports finding an owner or admin contact for remediation

Most actively used VMs generate regular Azure Activity Logs during normal operation. VMs with no recent activity (outside of Azure Virtual Desktop hosts, which produce OS-level activity not reflected in control-plane logs) are flagged as potentially inactive or orphaned.

---

## How It Works

The workbook uses the workbook **merge** feature to combine three datasets without requiring a single joined query:

| Source | Data Provided |
| --- | --- |
| **Azure Resource Graph** | Full compute inventory: power status, OS, extensions, secure boot, auto-update, VM Agent health |
| **Azure Activity Logs** | Management activity history; used to detect inactivity and surface the last human admin contact |
| **MDE — DeviceInfo** | Real-world endpoint security posture: MDE onboarding status, antivirus state, sensor health |

---

## Common Issues and Recovery Steps

The workbook's embedded help panel covers the four most common findings and their resolution:

### 1. Missing or Unresponsive Azure VM Agent
**Signals:** VM Agent Health = `Unknown` or `Not Ready`; Power Status = `Running`  
**Impact:** Blocks all Azure-managed extension deployments including MDE.  
**Resolution:** Log into the VM and reinstall `WindowsAzureVmAgent` (Windows) or `walinuxagent` (Linux) per Microsoft documentation.

### 2. Active VM Missing from MDE
**Signals:** Power Status = `Running`; MDE Status = `Not Onboarded` or blank  
**Impact:** Running server with no endpoint security monitoring — critical gap.  
**Resolution:** Confirm VM Agent health first. If healthy, ensure Defender for Servers Plan 1 or 2 is enabled on the subscription. If the MDE extension failed, uninstall and allow Defender for Cloud to redeploy. Run the MDE Client Analyzer on-device if the extension continues to fail.

### 3. Orphaned or Abandoned Resource
**Signals:** Last Activity Date = blank or very old; Last Human Activity = blank or very old  
**Impact:** Resource incurring cost with no active owner and potential unmonitored exposure.  
**Resolution:** Use the Last Human Activity column to identify a contact. If no owner can be found, create a decommission plan.

### 4. MDE Healthy but Azure Extension Shows an Error
**Signals:** MDE Status = `Onboarded`; MDE Sensor Health = `Active`; VM Agent or Extension = `Failed` or `Unknown`  
**Impact:** Device was onboarded directly (script), not via Azure extension. Security posture is intact but VM Agent issue should be resolved.  
**Resolution:** Fix the Azure VM Agent per Finding #1. The extension state will resolve once the agent is healthy.

---

## Required Tables

| Table | Source | Purpose |
| --- | --- | --- |
| `resources` | Azure Resource Graph | VM/Arc inventory, power status, extensions, OS configuration |
| `AzureActivity` | Log Analytics | Management activity history, inactivity detection, last human admin |
| `DeviceInfo` | Log Analytics (MDE) | MDE onboarding status, antivirus state, sensor health |
| `SigninLogs` | Log Analytics (Entra ID) | Admin account correlation for owner identification |

---

## Prerequisites

- Access to a Log Analytics workspace with MDE and Entra ID data
- Read access to the relevant Azure subscriptions
- Microsoft Defender for Endpoint enabled in the tenant
- Defender for Servers Plan 1 or Plan 2 for automated onboarding coverage

---

## Importing the Workbook

1. Open the **Azure Portal**
2. Navigate to **Azure Monitor → Workbooks**
3. Select **+ New**
4. Open **Advanced Editor**
5. Paste the contents of `ResourceSecurityWorkbook.json`
6. Select **Apply** and save the workbook

---

## Permissions and Data Visibility

- Results are scoped to subscriptions and workspaces the signed-in user has read access to
- Cross-subscription visibility depends on RBAC assignments
- Activity-based inactivity detection depends on Azure control-plane events; AVD hosts may appear inactive even when in active use

---

## Files

- `ResourceSecurityWorkbook.json` — current workbook
- `AzureVmMdeOmboardingStatus.json` — prior MDE-focused workbook (current version)
- `archive/` — prior version of the MDE onboarding workbook

---

## Disclaimer

This workbook is provided as-is for operational visibility and troubleshooting. Always validate findings against official Azure and Defender portal views before making remediation or compliance decisions.
