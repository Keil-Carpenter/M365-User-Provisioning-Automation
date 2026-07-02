# M365 User Provisioning Automation

## Contents

- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Usage](#usage)
- [Example Execution](#example-execution)
- [Output](#output)
- [Notes / Limitations](#notes--limitations)

## Overview

This PowerShell script installs and imports the Microsoft Graph PowerShell module, authenticates to Microsoft Graph, validates a user by User Principal Name (UPN), and performs user provisioning tasks using Microsoft Graph and Exchange Online.

## Features

- Installs Microsoft.Graph version 2.32.0.
- Imports the Microsoft.Graph module.
- Authenticates to Microsoft Graph.
- Prompts for a user's UPN.
- Validates the UPN format using a regular expression.
- Checks whether the user exists in Microsoft Graph.
- Re-prompts until a valid user is found.
- Adds the user to a predefined list of Microsoft Entra ID groups.
- Prompts for a DoneSafe group selection and adds the selected group.
- Prompts for a location and adds the corresponding Exchange Online distribution list.
- Updates the user's usage Location to `NZ`.
- Waits for an E5 licence assignment by checking assigned licence SKUs.
- Assigns a Microsoft Viva Insights licence if it is not already assigned.
- Disconnects from Microsoft Graph.
- Connects to Exchange Online.
- Adds the user to predefined Exchange Online distribution lists.
- Imports trusted senders from `C:\temp\trusted_senders.txt` if the file exists.
- Adds each trusted sender to the user's mailbox junk email configuration.
- Displays status messages throughout execution.

## Requirements

### PowerShell

- PowerShell
- Microsoft.Graph module version 2.32.0
- Exchange Online PowerShell module (required for Exchange Online cmdlets used by the script)

### Permissions

Microsoft Graph scopes requested by the script:

- Group.ReadWrite.All
- User.ReadWrite.All
- Directory.Read.All

Exchange Online permissions must allow:
- Add-DistributionGroupMember
- Set-MailboxJunkEmailConfiguration

## Usage

Run the script:

```powershell
.\Begin-Provisioning.ps1
```

The script prompts for:

- User Principal Name (UPN)
- DoneSafe group selection
- User location

## Example Execution

The following screenshots show the interactive prompts and console output during script execution.

### Step 1: Enter User Principal Name

<img width="1481" alt="Enter User Principal Name (UPN)" src="https://github.com/user-attachments/assets/bd269fbf-36a3-4ab7-849d-9d63af2829d6" />

### Step 2: Select Reporting Structure

<img width="1482" alt="Reporting Structure Selection" src="https://github.com/user-attachments/assets/f5d6f260-4dc1-4d49-b7f7-3c8b3c8b80fc" />

### Step 3: Select Location

<img width="1486" alt="Location Selection" src="https://github.com/user-attachments/assets/69838a0a-6ac0-4cca-bdd6-4dac7d88a602" />

### Step 4: Verify Licence Assignment

<img width="1481" alt="Licence Assignment Check" src="https://github.com/user-attachments/assets/fec17b52-1a70-4a14-a3d8-35a169dd62e5" />

### Step 5: Provisioning Complete

<img width="1480" alt="Provisioning Complete" src="https://github.com/user-attachments/assets/6a136b81-490d-4b40-b878-e78bc26d8f0a" />

The prompts shown above represent the interactive stages of the provisioning process before the script completes.

## Output

The script writes progress and status information to the console.

No CSV, log, or report files are generated.

If `C:\temp\trusted_senders.txt` exists, the script imports each entry and adds it to the user's mailbox junk email trusted senders list.

## Notes / Limitations

- Microsoft.Graph version 2.32.0 is installed each time the script runs.
- User validation continues until an existing user is entered.
- Group membership is determined by group display name.
- The script waits for an E5 licence assignment for up to 10 attempts with a 15 second delay between checks.
- Trusted senders are only processed if `C:\temp\trusted_senders.txt` exists.
