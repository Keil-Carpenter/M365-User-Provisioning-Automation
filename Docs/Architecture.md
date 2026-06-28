This document describes the internal design and execution flow of the Microsoft 365 User Onboarding Automation script.

# Architecture – Microsoft 365 User Onboarding Automation

## Overview

This script is an interactive PowerShell provisioning tool used to manage Microsoft 365 user onboarding.

It integrates Microsoft Graph and Exchange Online PowerShell to apply identity configuration, group membership, and licensing based on operator input.

The process is sequential and dependency-driven to ensure identity, licensing, and messaging configuration are applied in the correct order.


## Core Components

### 1. Input and Validation Layer
- Collects user input (UPN, reporting structure, location)
- Validates UPN format using regex
- Confirms user exists in Microsoft Entra ID using `Get-MgUser`
- Re-prompts until a valid user is found


### 2. Microsoft Graph Integration Layer
Used for:
- User lookup (`Get-MgUser`)
- Group lookup (`Get-MgGroup`)
- Security group membership assignment (`New-MgGroupMemberByRef`)
- Updating usage location (`Update-MgUser`)
- License state retrieval (`Get-MgUserLicenseDetail`)
- License assignment (`Set-MgUserLicense`)


### 3. Exchange Online Integration Layer
Used for:
- Distribution group membership (`Add-DistributionGroupMember`)
- Mailbox junk email configuration (Trusted Senders list)


## Execution Flow

The script follows a fixed sequential workflow:

1. Module install and import (Microsoft Graph)
2. Connect to Microsoft Graph
3. Validate user existence (loop until valid UPN provided)
4. Collect reporting structure input
5. Collect location input
6. Build security group and distribution group arrays
7. Assign security groups via Graph
8. Set Microsoft Entra usage location (NZ)
9. Wait for E5 license assignment via group-based licensing
10. Apply Viva Insights license if not present
11. Connect to Exchange Online
12. Assign distribution groups
13. Apply trusted senders configuration if file exists
14. Disconnect and complete execution


## Decision Logic

### Reporting Structure Mapping
User input determines security group assignment:

- CEO reporting → Executive security group
- Direct reports → People leader group
- No direct reports → Standard user group

Groups are resolved dynamically using `Get-MgGroup`.


### Location Mapping
User input determines distribution group assignment:

- Wellington → DL WEL Users
- Auckland → DL Te Whare Rama
- Christchurch → DL CHC Users


### Licensing Logic
- Script waits for E5 license assignment via group-based licensing
- Polling loop checks license state every 15 seconds (max 10 attempts)
- If E5 is present, Viva Insights license is applied (if missing)


### Optional Configuration

During execution, the script checks for the following configuration file:

```text
C:\temp\trusted_senders.txt
```

If the file is present, each entry is applied to the target mailbox's **Trusted Senders and Domains** list using Exchange Online PowerShell.


## Error Handling and Resilience

- User validation uses retry loop until valid UPN is provided
- Group assignment includes try/catch handling for existing memberships
- License polling includes timeout and fallback messaging
- Exchange group assignment handles duplicate membership scenarios


## Dependencies
- Microsoft Graph PowerShell SDK
- Exchange Online PowerShell

### Required Permissions
- User.ReadWrite.All
- Group.ReadWrite.All
- Directory.Read.All


## Design Characteristics

- Fully interactive (no batch processing)
- Sequential execution model with dependency order
- Relies on group-based licensing already configured in tenant
- Hybrid identity and messaging configuration support
