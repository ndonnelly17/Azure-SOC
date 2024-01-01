# Building a SOC + Honeynet in Azure (Live Traffic)
Cloud Honeynet / SOC![image](https://github.com/ndonnelly17/Azure-SOC/assets/65242449/8f91f9ca-7b85-48bc-94f4-e16679af1d08)


## Introduction

In this cybersecurity project I set up a SOC and a honeynet to capture live traffic.  Log sources from different resources were ingested into a Log Analytics workspace which is used by Microsoft Sentinel to make attack maps, trigger alerts, and create incidents.  The way this was accomplished was by having the NSGs open to be able to measure activity in this insecure setup before applying any security measures.  Then, some security measures were applied, and more measurements were taken to see the difference in outcomes.  Below, you will be able to see the environment before any security measures were applied and then after security measures were applied measured by the metrics below:

•	SecurityEvent (Windows Event Logs)
  
•	Syslog (Linux Event Logs)

•	SecurityAlert (Log Analytics Alerts Triggered)

•	SecurityIncident (Incidents created by Sentinel)

•	AzureNetworkAnalytics_CL (Malicious Flows allowed into honeynet)

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
NSG Allowed Inbound Malicious Flows![image](https://github.com/ndonnelly17/Azure-SOC/assets/65242449/5aa50b9f-7c58-4840-a86e-d26b45082d19)<br>
Linux Syslog Auth Failures![image](https://github.com/ndonnelly17/Azure-SOC/assets/65242449/d2161f26-c13b-4bce-aa42-b5e41e04ec5e)<br>
Windows RDP/SMB Auth Failures![image](https://github.com/ndonnelly17/Azure-SOC/assets/65242449/3441f50a-b8d6-4ce0-bec7-f415ae0990bf)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
