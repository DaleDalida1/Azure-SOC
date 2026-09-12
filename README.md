# Building a SOC and Honeynet in Azure

![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Project Summary

I built a small honeynet in Microsoft Azure and connected multiple log sources to a Log Analytics workspace. Microsoft Sentinel was used to visualize activity, generate alerts, and create incidents. I recorded activity before applying security controls and then recorded a second observation window after hardening the environment.

This project demonstrates cloud-security monitoring, SIEM deployment, log ingestion, security-control validation, and evidence-based analysis.

## Environment

- Microsoft Azure
- Microsoft Sentinel
- Log Analytics Workspace
- Virtual Network (VNet)
- Network Security Groups (NSGs)
- Two Windows virtual machines
- One Linux virtual machine
- Azure Key Vault
- Azure Storage Account

## Data Sources

- **SecurityEvent:** Windows Security Event logs
- **Syslog:** Linux system logs
- **SecurityAlert:** alerts available in Log Analytics
- **SecurityIncident:** incidents created in Microsoft Sentinel
- **AzureNetworkAnalytics_CL:** network-flow data collected for the honeynet

## Architecture Before Hardening

![Architecture before security controls](https://i.imgur.com/aBDwnKb.jpg)

During the initial observation window, the virtual machines were exposed to the internet with permissive NSG and host-firewall configurations. Other resources used public endpoints rather than private endpoints.

## Architecture After Hardening

![Architecture after security controls](https://i.imgur.com/YQNa9Pp.jpg)

The hardened configuration restricted inbound access to the administrative workstation, applied host-based firewall protections, and protected other resources with firewall rules and private endpoints.

## Activity Observed Before Hardening

Recorded window: **July 7, 2024 at 14:48 through July 8, 2024 at 14:48 (24 hours)**

| Metric | Recorded count |
| --- | ---: |
| SecurityEvent | 19,661 |
| Syslog | 7,381 |
| SecurityAlert | 10 |
| SecurityIncident | 195 |
| AzureNetworkAnalytics_CL | 1 |

### Attack Maps

![Attack-map result 1](https://github.com/user-attachments/assets/5292b603-8dbc-4ea7-b11d-1bad9d27cefb)

![Attack-map result 2](https://github.com/user-attachments/assets/4ad01fea-6370-4637-b061-789141a35b4c)

![Attack-map result 3](https://github.com/user-attachments/assets/8f2d0f87-424d-44c3-9b84-379ab2fe9a35)

## Activity Observed After Hardening

Recorded window: **July 26, 2024 at 22:51 through July 28, 2024 at 22:51 (48 hours)**

| Metric | Recorded count |
| --- | ---: |
| SecurityEvent | 28,452 |
| Syslog | 11 |
| SecurityAlert | 0 |
| SecurityIncident | 0 |
| AzureNetworkAnalytics_CL | 0 |

The post-hardening attack-map queries returned no results during the recorded window.

## Analysis

The raw results show that Sentinel alerts, Sentinel incidents, Linux Syslog records, and the recorded malicious-flow result were lower after hardening. Windows SecurityEvent records increased from 19,661 to 28,452.

A Windows SecurityEvent count represents logged activity and should not automatically be interpreted as malicious activity. In addition, the two observation windows were different lengths—24 hours before hardening and 48 hours after hardening—so the raw counts are not normalized for a direct rate comparison. These limitations should be considered when interpreting the results.

Even with those limitations, the absence of Sentinel alerts and incidents during the second observation window is consistent with the environment being less exposed after the controls were applied. A longer experiment with equal observation windows and normalized event rates would provide stronger evidence.

## Skills Demonstrated

- Microsoft Sentinel deployment and monitoring
- Log-source integration with Log Analytics
- Windows and Linux security-log analysis
- Azure network-security configuration
- NSG and firewall hardening
- Public-versus-private endpoint analysis
- Alert and incident review
- Before-and-after control validation
- Technical documentation and analytical reporting

## Future Improvements

- Repeat both tests using equal observation windows
- Normalize results as events per hour
- Document the KQL queries used for each metric and map
- Add a sample alert-triage investigation
- Map observed activity to relevant MITRE ATT&CK techniques
- Document false-positive considerations and escalation criteria
