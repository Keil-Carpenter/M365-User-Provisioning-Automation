
# Microsoft 365 User Provisioning Automation

Interactive PowerShell tool for Microsoft 365 user provisioning using Microsoft Graph and Exchange Online PowerShell in a managed services environment.


## Overview

The script performs the following onboarding tasks:

- Validates user exists in Microsoft Entra ID
- Collects user input (UPN, reporting structure, location)
- Assigns security group membership in Microsoft Entra ID
- Assigns distribution group membership in Exchange Online
- Sets Microsoft Entra usage location
- Applies Microsoft 365 licensing (E5 via group-based licensing, then Viva Insights)
- Configures mailbox trusted senders (optional)

## Key Features

- Fully interactive onboarding workflow
- Microsoft Graph-based identity management
- Exchange Online distribution group automation
- Conditional licensing logic (E5 + Viva Insights)
- Input validation and retry handling
- Optional mailbox configuration via external file
- Idempotent design (safe to re-run per user)


## How it works

### Step 1 – Enter User Principal Name (UPN)
The script prompts for a UPN and validates it exists in Microsoft Entra ID.

### Step 2 – Select Reporting Structure
Choose whether the user:
- Reports to CEO
- Has direct reports
- Has no direct reports

This determines security group assignment.

### Step 3 – Select Location
Choose user location:
- Wellington
- Auckland
- Christchurch

This determines distribution group assignment.

### Step 4 – License Validation
The script checks for E5 license assignment via group-based licensing before applying Viva Insights.

### Step 5 – Provisioning Execution
The script applies:
- Security group membership
- Distribution group membership
- Usage location (NZ)
- Licensing configuration
- Mailbox trusted sender configuration (if configuration file exists)


## Screenshots

### Step 1 – Enter UPN
<img width="1481" height="662" alt="STEP 1 - Enter UPN" src="https://github.com/user-attachments/assets/bd269fbf-36a3-4ab7-849d-9d63af2829d6" />

### Step 2 – Reporting Structure
<img width="1482" height="666" alt="STEP 2 - Choose direct reports" src="https://github.com/user-attachments/assets/f5d6f260-4dc1-4d49-b7f7-3c8b3c8b80fc" />

### Step 3 – Location Selection
<img width="1486" height="663" alt="STEP 3 - Choose location" src="https://github.com/user-attachments/assets/69838a0a-6ac0-4cca-bdd6-4dac7d88a602" />

### Step 4 – License Check
<img width="1481" height="667" alt="STEP 4 - Verify License Assignment" src="https://github.com/user-attachments/assets/fec17b52-1a70-4a14-a3d8-35a169dd62e5" />

### Step 5 – Completion
<img width="1480" height="662" alt="STEP 5 - Final Step" src="https://github.com/user-attachments/assets/6a136b81-490d-4b40-b878-e78bc26d8f0a" />


## Requirements

- PowerShell 5.1 or later
- Microsoft Graph PowerShell SDK
- Exchange Online PowerShell

### Required permissions:
- User.ReadWrite.All
- Group.ReadWrite.All
- Directory.Read.All

## Usage

```powershell
# Navigate to source directory
Set-Location ./src

# Run provisioning script
.\Begin-Provisioning.ps1
```

## Optional Configuration

If the following file exists, trusted senders will be applied:

```text
C:\temp\trusted_senders.txt
```

Each entry is added to the mailbox Trusted Senders and Domains list.

## Notes
- Automatically installs and imports required modules if missing
- Authenticates to Microsoft Graph on execution
- Designed for interactive per-user provisioning (not batch automation)
- Uses group-based licensing for Microsoft 365 license assignment




