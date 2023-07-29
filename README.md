
## Introduction

For this project, I constructed a mini honeynet within the Azure platform and collected log data from diverse sources, all of which were fed into a Log Analytics workspace. Microsoft Sentinel was utilized to process this data, enabling the creation of attack maps, alert triggers, and incident records. To gauge security levels, I conducted measurements in the vulnerable environment for a 24-hour period. Subsequently, security controls were implemented to fortify the environment, after which another 24-hour metric assessment was carried out. The outcomes of these measurements are presented below The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

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
NSG Allowed Inbound Malicious Flows<br>
![image](https://github.com/jdrz/Azure_SOC_Honeynet/assets/42385929/1fb5b811-08f5-449f-8aa0-51185b61a265)

Linux Syslog Auth Failures<br>
![image](https://github.com/jdrz/Azure_SOC_Honeynet/assets/42385929/1f348e09-0479-4b99-98cc-07f4b29aba45)

Windows RDP/SMB Auth Failures<br>
![image](https://github.com/jdrz/Azure_SOC_Honeynet/assets/42385929/bb47fb0d-ce09-405e-8ca0-a37920cd8b23)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-07-22T18:36:13.2357901Z
Stop Time 2023-07-23T18:36:13.2357901Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 87674
| Syslog                   | 4575
| SecurityAlert            | 5
| SecurityIncident         | 249
| NSG Inbound Flow Allowed | 949 

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-07-24T00:07:48.0214759Z
Stop Time	2023-07-25T00:07:48.0214759Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 1627
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| NSG Inbound Flow Allowed | 0

## Conclusion

This project involved setting up a mini honeynet using Microsoft Azure and integrating log sources into a Log Analytics workspace. Microsoft Sentinel was utilized to activate alerts and generate incidents based on the collected logs. Furthermore, security metrics were measured in the vulnerable environment both before and after implementing security controls. Notably, the implementation of security measures led to a significant reduction in the number of security events and incidents, highlighting their effectiveness.

Importantly, it should be acknowledged that in a scenario where network resources are extensively utilized by regular users, the 24-hour period following the implementation of security controls might result in a higher volume of security events and alerts.
