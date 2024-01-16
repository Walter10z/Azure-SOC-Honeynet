# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In the course of this project, I constructed a compact honeynet within the Azure platform and brought in log sources from diverse origins into a Log Analytics workspace. Subsequently, Microsoft Sentinel was employed to construct attack maps, initiate alerts, and formulate incidents based on the ingested logs. I gauged certain security metrics within the vulnerable environment for a duration of 24 hours, implemented a set of security controls to fortify the environment, re-evaluated metrics for an additional 24 hours, and present the outcomes below. The metrics to be presented include:

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

- Virtual Machines (2 windows, 1 linux)
- Virtual Network (VNet)
- Network Security Group (NSG)
- Azure Key Vault
- Azure Storage Account
- Log Analytics Workspace
- Microsoft Sentinel
- Microsoft Defender

In the initial "BEFORE" metrics phase, all resources were initially deployed and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls configured with broad accessibility, and all other resources were deployed with public endpoints that were visible to the Internet, meaning there was no utilization of Private Endpoints.

In the subsequent "AFTER" metrics phase, the Network Security Groups underwent reinforcement by blocking ALL traffic, except for that originating from my admin workstation. Additionally, all other resources were safeguarded by their built-in firewalls, and Private Endpoints were utilized for added protection.

## Attack Maps Before Hardening / Security Controls
![before windows-rdp-auth-fail](https://github.com/Walter10z/Azure-SOC-Honeynet/assets/102203609/3e373354-30aa-467e-a68d-2cc4a23e3ce4)<br>
![before linux-ssh-auth-fail](https://github.com/Walter10z/Azure-SOC-Honeynet/assets/102203609/6def3b3e-2bec-482b-bd96-c608cb164b90) <br>
![before nsg-malicious-allowed-in](https://github.com/Walter10z/Azure-SOC-Honeynet/assets/102203609/7c446a52-6ec7-46f3-8fee-3dab83e58980) <br>


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-01-11 06:29:29
Stop Time 2024-01-12 16:29:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 21670
| Syslog                   | 3048
| SecurityAlert            | 3
| SecurityIncident         | 236
| AzureNetworkAnalytics_CL | 70163

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-01-13 18:47:18
Stop Time	2024-01-14 18:47:18

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8922
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a small-scale honeynet was set up on Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was utilized to activate alerts and generate incidents based on the received logs. Furthermore, metrics were assessed in the insecure environment both before implementing security controls and after their application. It is significant to highlight that the implementation of security measures led to a significant reduction in the number of security events and incidents, showcasing their effectiveness.

It is important to mention that if the resources within the network were extensively used by regular users, there is a likelihood that more security events and alerts could have been generated within the 24-hour period following the implementation of the security controls.
