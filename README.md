# User Provisioning and Group Membership Script

## Overview

This PowerShell script installs and imports the Microsoft Graph PowerShell module, authenticates to Microsoft Graph, validates a user by User Principal Name (UPN), and performs a series of Microsoft Graph and Exchange Online administrative tasks.

The script prompts for user input to determine additional group memberships and distribution list memberships before applying the required changes.

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
- Updates the user's Usage Location to `NZ`.
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

### Modules

- Microsoft.Graph
- Exchange Online PowerShell module

### Permissions

Microsoft Graph scopes requested by the script:

- Group.ReadWrite.All
- User.ReadWrite.All
- Directory.Read.All

Exchange Online permissions must allow execution of:

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

## Output

The script writes status information to the console.

No CSV, log, or report files are generated.

If present, the script reads trusted senders from:

```
C:\temp\trusted_senders.txt
```

## Notes / Limitations

- Microsoft.Graph version 2.32.0 is installed each time the script runs.
- User validation continues until an existing user is entered.
- Group membership is determined by group display name.
- The script waits for an E5 licence assignment for up to 10 attempts with a 15 second delay between checks.
- Trusted senders are only processed if `C:\temp\trusted_senders.txt` exists.
