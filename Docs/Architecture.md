# Architecture Overview

## System Components

### Authentication

- Microsoft Graph authentication using `Connect-MgGraph`
- Exchange Online authentication using `Connect-ExchangeOnline`

### Microsoft Graph

The script uses Microsoft Graph cmdlets to:

- Retrieve users
- Retrieve groups
- Add users to groups
- Update user properties
- Retrieve licence details
- Assign licences

### Exchange Online

The script uses Exchange Online cmdlets to:

- Add distribution group members
- Update mailbox trusted senders

### Processing Logic

The script:

1. Installs Microsoft.Graph.
2. Imports Microsoft.Graph.
3. Connects to Microsoft Graph.
4. Validates a user.
5. Prompts for additional group selections.
6. Adds Microsoft Entra ID group memberships.
7. Updates Usage Location.
8. Waits for an E5 licence assignment.
9. Assigns a Viva Insights licence if required.
10. Connects to Exchange Online.
11. Adds Exchange distribution group memberships.
12. Applies trusted sender entries if available.
13. Displays completion status.

### Output Handling

Output is written to the console using `Write-Host`.

## Data Flow

1. Install Microsoft.Graph.
2. Import Microsoft.Graph.
3. Authenticate to Microsoft Graph.
4. Prompt for a UPN.
5. Validate UPN format.
6. Retrieve the user.
7. Build lists of Microsoft Entra ID groups and Exchange distribution lists.
8. Retrieve each group by display name.
9. Add the user to each Microsoft Entra ID group.
10. Update the user's Usage Location.
11. Poll licence assignments until the E5 licence is detected or the retry limit is reached.
12. Assign the Viva Insights licence if required.
13. Disconnect from Microsoft Graph.
14. Connect to Exchange Online.
15. Add the user to each distribution list.
16. Read trusted senders from `C:\temp\trusted_senders.txt` if available.
17. Add each trusted sender to the user's mailbox configuration.
18. Display completion status.

## Dependencies

Modules explicitly used:

- Microsoft.Graph
- Exchange Online PowerShell module

## Authentication Model

Microsoft Graph authentication:

- Connect-MgGraph
- Scopes:
  - Group.ReadWrite.All
  - User.ReadWrite.All
  - Directory.Read.All

Exchange Online authentication:

- Connect-ExchangeOnline

## Security Considerations

- User input is validated using a regular expression before Microsoft Graph lookup.
- Microsoft Graph authentication requests delegated permissions using explicit scopes.
- Trusted senders are read from a local file located at:

```
C:\temp\trusted_senders.txt
```

## Limitations

- Group lookup is performed using group display name.
- Distribution lists are identified by name.
- The script expects `C:\temp\trusted_senders.txt` if trusted senders are to be applied.
- The E5 licence check stops after 10 attempts.
