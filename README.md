# Azure VM – MDE Onboarding Status Workbook

## Overview

**Azure VM – MDE Onboarding Status** is an Azure Monitor Workbook that helps verify **Microsoft Defender for Endpoint (MDE)** onboarding across **Azure virtual machines** and **Azure Arc–enabled servers**.

The workbook correlates Azure activity data with MDE device records to identify which machines are actively reporting and whether they are properly protected. It is designed to surface onboarding gaps that are difficult to detect through extension health or portal views alone.

This workbook is intended for security engineers, cloud administrators, and SOC teams responsible for Defender for Servers and MDE compliance.

---

## Screenshot

![Azure VM – MDE Onboarding Status Workbook](https://github.com/AndrewBlumhardt/workbooks/blob/main/MDE_Workbook.png)

---

## How It Works

Azure resource data and MDE device data cannot be queried together directly. To work around this, the workbook uses a two-stage correlation approach.

### Active VMs with MDE Status

- Shows Azure VMs with recorded Azure activity in the last 30 days  
- Cross-references those systems against MDE device records  
- Highlights:
  - MDE onboarding status
  - Microsoft Defender Antivirus status  
- Supports filtering by status  
- Selecting one or more rows filters the lower table automatically  

This view is optimized for identifying **actively running workloads that are not protected by MDE**.

### All Tenant VMs

- Displays all Azure VMs and Arc-enabled servers visible to the current user  
- Includes inactive or rarely used systems  
- Automatically filtered based on selections from the top table  

This view supports **full inventory validation** and detailed investigation.

---

## Common Issues the Workbook Helps Identify

The workbook is designed to expose conditions related to common MDE onboarding failures, including:

- Defender for Servers not enabled on the subscription
- Missing or broken Azure VM Guest Agent
- Failed or repeatedly failing MDE extensions
- Conflicts between direct onboarding methods and Defender for Cloud
- Local OS or network conditions preventing sensor activation

Guidance for interpreting these scenarios is included in the workbook’s help panel.

---

## Prerequisites

- Access to **Log Analytics** where Defender data is stored  
- Permissions to query:
  - Azure Resource Graph
  - Log Analytics workspaces  
- Microsoft Defender for Endpoint enabled in the tenant  
- Defender for Servers Plan 1 or Plan 2 for full compliance validation  

---

## Importing the Workbook

### Import via Azure Portal

1. Open the **Azure Portal**
2. Navigate to **Azure Monitor → Workbooks**
3. Select **+ New**
4. Open **Advanced Editor**
5. Paste the workbook JSON from this repository
6. Select **Apply**
7. Save the workbook:
   - Choose a resource group
   - Assign a name
   - Optionally share at subscription or resource group scope

### Import from JSON File

1. Download the workbook JSON file from this repository
2. Open **Azure Monitor → Workbooks**
3. Select **+ New**
4. Choose **Template Gallery → Import**
5. Upload the JSON file
6. Save the workbook

---

## Permissions and Data Visibility

- Results are limited to resources visible to the signed-in user  
- Cross-subscription visibility depends on RBAC assignments  
- Activity-based results depend on recent Azure control-plane activity  

---

## Intended Use Cases

- Validate Defender for Servers onboarding at scale
- Identify active VMs missing MDE protection
- Troubleshoot onboarding failures without VM-by-VM inspection
- Support audit and compliance efforts
- Reduce reliance on extension health alone as a signal

---

## Repository Contents

- Azure Monitor Workbook JSON template  
- README documentation  
- Screenshot used in this README  

---

## Disclaimer

This workbook is provided as-is for operational visibility and troubleshooting. Always validate findings against official Azure and Defender portal views before making remediation or compliance decisions.
