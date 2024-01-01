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
Start Time 2023-11-30 01:32:37
Stop Time 2023-12-01 01:32:37

| Metric                              | Count
| ------------------------            | -----
| SecurityEvent                       | 25280
| Syslog                              | 609
| SecurityAlert                       | 5
| SecurityIncident                    | 123
| NSG Inbound Malicious Flows Allowed | 2116

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-12-02 01:03
Stop Time	2023-12-03 01:03

| Metric                              | Count
| ------------------------            | -----
| SecurityEvent                       | 110
| Syslog                              | 344
| SecurityAlert                       | 1
| SecurityIncident                    | 2
| NSG Inbound Malicious Flows Allowed | 0

## Conclusion

This project used Microsoft Azure to create a honeynet in which log sources were integrated into a Log Analytics workspace.  Microsoft Sentinel was taken advantage of to have alerts activated and have incidents made that were based on the ingested logs.  The insecure environment had metrics taken before security controls were implemented, and then metrics were taken again after the environment was secured.  Please see the table below to see noticeable difference in the reduction security events and incidents after security controls were applied. 

| Results                                      | Change After Security Controls Implemented
| ------------------------                     | -----
| SecurityEvents (Windows VM)                  | 99.56%
| Syslog (Linux VMs)                           | 43.51%
| SecurityAlert (Microsoft Defender for Cloud) | 80.00%
| SecurityIncident (Sentinel Incidents)        | 98.37%
| NSG Inbound Malicious Flows Allowed          | 100%

As an end-note it is worth mentioning that if this network had regular users, there would be most likely be more security events and alerts that would have been discovered in the 24-hour time frame following the implementation of the security controls.
