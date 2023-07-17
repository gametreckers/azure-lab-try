
# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

This project focuses on setting up a small-scale honeynet in Azure to monitor and analyze live traffic. It involves collecting log data from various sources and storing it in a Log Analytics workspace. Microsoft Sentinel is utilized to generate attack maps, trigger alerts, and manage security incidents. The project measures security metrics in an insecure environment for 24 hours, implements security controls to fortify the system, and then compares the metrics before and after the enhancements. The key metrics include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Triggered Log Analytics Alerts)
- SecurityIncident (Sentinel Incidents Created)
- AzureNetworkAnalytics_CL (Malicious Flows Allowed into the Honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources were deployed with public endpoints visible to the Internet; no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics measured in the insecure environment for 24 hours:
Start Time: 2023-03-15 17:04:29
Stop Time: 2023-03-16 17:04:29

| Metric                   | Count |
| ------------------------ | ----- |
| SecurityEvent            | 19470 |
| Syslog                   | 3028  |
| SecurityAlert            | 10    |
| SecurityIncident         | 348   |
| AzureNetworkAnalytics_CL | 843   |

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24-hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics measured in the environment for another 24 hours after implementing security controls:
Start Time: 2023-03-18 15:37
Stop Time: 2023-03-19 15:37

| Metric                   | Count |
| ------------------------ | ----- |
| SecurityEvent            | 8778  |
| Syslog                   | 25    |
| SecurityAlert            | 0     |
| SecurityIncident         | 0     |
| AzureNetworkAnalytics_CL | 0     |

## Conclusion

This project involves the construction of a mini honeynet in Microsoft Azure, integrating log sources into a Log Analytics workspace, and utilizing Microsoft Sentinel for alert triggering and incident management. The implementation of security controls resulted in a significant reduction in the number of security events and incidents, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
