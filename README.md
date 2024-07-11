# Building a SOC + Honeynet in Azure (Live Traffic)
![SOC _ Honeynet Diagram](https://github.com/LauraKellerLY/Azure-SOC/assets/175071141/bf93c676-171b-44d5-a719-17c0cd4639f9)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Before](https://github.com/LauraKellerLY/Azure-SOC/assets/175071141/25753eb3-15e9-4aea-98ca-e917762e3629)


## Architecture After Hardening / Security Controls
![Architecture After](https://github.com/LauraKellerLY/Azure-SOC/assets/175071141/8297eadc-fc22-49f7-bebf-d2dcb9dbb576)


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
![(before)nsg_malicious_allowed_in](https://github.com/LauraKellerLY/Azure-SOC/assets/175071141/cc8f2a3b-b2e6-4d61-a0f7-ed3b4d5414dc)
<br>
![(before)linux_auth_fail](https://github.com/LauraKellerLY/Azure-SOC/assets/175071141/1a7c3ca9-7449-41cd-9732-defae084d6ba)
<br>
![(before)windows_rdp_auth_fail](https://github.com/LauraKellerLY/Azure-SOC/assets/175071141/53b3e0e5-7076-4092-bbc2-d3fece7b89b4)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-06-01 04:34 AM
Stop Time 2024-06-02 04:34 AM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 36383
| Syslog                   | 5368
| SecurityAlert            | 5
| SecurityIncident         | 198
| AzureNetworkAnalytics_CL | 2066

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-06-28 11:12 PM
Stop Time	2024-06-29 11:12 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9485
| Syslog                   | 7
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
