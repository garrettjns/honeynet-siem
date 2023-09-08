# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

Within this project, I established a compact honeynet infrastructure within Azure. I collected log data from diverse sources and funneled it into a dedicated Log Analytics workspace. Microsoft Sentinel leveraged this data to construct comprehensive attack visualizations, initiate alert mechanisms, and generate incident reports. The project unfolded through distinct phases:

Initial Security Assessment: I commenced by quantifying essential security metrics within the initially vulnerable environment, diligently monitoring them over a continuous 24-hour interval.

Security Enhancement Initiative: Following this assessment, I instituted a series of robust security controls to fortify the environment, significantly bolstering its defensive measures.

Reassessment of Security Metrics: Subsequently, I conducted a subsequent 24-hour evaluation of security metrics, providing a comparative analysis of the environment's improved security posture.

Log sources include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/garrettjns/garrettjns/blob/main/maps_before/nsg-malicious-allowed-in.png)<br>
![Linux Syslog Auth Failures](https://github.com/garrettjns/garrettjns/blob/main/maps_before/linux-ssh-auth-fail.png)<br>
![Windows RDP/SMB Auth Failures](https://github.com/garrettjns/garrettjns/blob/main/maps_before/windows-rdp-smb-auth-fail.png)<br>
![MSSQL Authentication Failuers](https://github.com/garrettjns/garrettjns/blob/main/maps_before/mssql-auth-failures.png)<br>
## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-06-29 19:17:53
Stop Time 2023-06-30 19:17:53

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 37356
| Syslog                   | 2859
| SecurityAlert            | 10
| SecurityIncident         | 178
| AzureNetworkAnalytics_CL | 657

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 7/4/2023 1:55:26
Stop Time	7/5/2023, 1:55:26

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 17108
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
