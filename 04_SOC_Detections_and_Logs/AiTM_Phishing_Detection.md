# Threat Detection & Response: AiTM Phishing Campaign

## Scenario Overview
In the TeleHealth Connect Security Risk Assessment (SRA), **Phishing & Credential Staining** was identified as a critical risk (Score: 16). To defend against advanced Adversary-in-the-Middle (AiTM) phishing attacks that can bypass standard MFA, the following detection logic and triage workflow was developed for our SIEM (Microsoft Sentinel).

## 1. Detection Logic (KQL)
This KQL query hunts for suspicious session hijacking behaviors. It identifies instances where a successful login from an unmanaged device or risky IP address is immediately followed by the creation of a suspicious inbox forwarding rule—a common post-compromise action to hide data exfiltration or password resets.

```kql
// Hunt for suspicious inbox rules created immediately after a high-risk sign-in
let timeframe = 24h;
SigninLogs
| where TimeGenerated >= ago(timeframe)
| where ResultType == "0" // Successful sign-in
| where RiskLevelDuringSignIn in ("High", "Medium") or isempty(DeviceDetail.deviceId) // Risky or unmanaged device
| project SignInTime = TimeGenerated, UserPrincipalName, IPAddress, Location
| join kind=inner (
    AuditLogs
    | where TimeGenerated >= ago(timeframe)
    | where OperationName has "Create Inbox Rule" or OperationName has "Set-Mailbox"
    | extend RuleName = tostring(parse_json(tostring(InitiatedBy.user)).userPrincipalName)
    | project RuleCreationTime = TimeGenerated, UserPrincipalName = tostring(TargetResources[0].userPrincipalName), OperationName
) on UserPrincipalName
| where RuleCreationTime between (SignInTime .. (SignInTime + 2h))
| project SignInTime, RuleCreationTime, UserPrincipalName, IPAddress, Location, OperationName
| sort by SignInTime desc
