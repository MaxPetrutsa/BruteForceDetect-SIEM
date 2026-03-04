# Brute Force Attack Detection - Splunk SIEM

Project Overview

Developed a brute force attack detection system using Splunk SIEM.
Designed and implemented a solution to monitor Windows Event Logs for failed login attempts (Event ID 4625).
Configured a real-time alert to trigger when five or more failed login attempts occur within five minutes.
Demonstrated expertise in security log analysis, automated threat detection, and SIEM configuration.

Tools and Technologies

Splunk
Windows Event Logs
Event ID 4625 (Failed Login Attempts)
PowerShell for attack simulation
Security Information and Event Management (SIEM)
Implementation

Integrated Windows Security Logs into Splunk to capture failed authentication attempts.
Developed a custom search query to extract and analyze brute force login patterns:
  index=windows_logs sourcetype=WinEventLog:Security EventCode=4625

Engineered a real-time alerting mechanism to detect and notify security teams of potential brute force activity.
Conducted validation and testing through simulated brute force attacks using PowerShell:
  runas /user:fakeuser cmd

Verified the accuracy and efficiency of alert triggers and log event tracking in Splunk.

Key Contributions

Strengthened security monitoring capabilities by optimizing log analysis and alerting efficiency.

Enhanced automated incident detection through custom SIEM rule configurations.

Provided insights into brute force attack patterns and mitigation strategies.

Potential Enhancements

Expanding detection logic to include additional attack vectors and anomaly detection.

Implementing email notifications or automated remediation workflows.

Developing a Splunk dashboard for visual representation of attack trends.

Conclusion

Successfully deployed a real-time brute force attack detection system leveraging Splunk.

This project highlights my ability to configure SIEM tools, analyze security logs, and develop automated threat detection solutions.


------------------------------------------------------------------------
------------------------------------------------------------------------


### Splunk Query Detecting Brute Force Attacks
![Brute Force Detection](<(Brute Force) screenshots/SplunkDetectionbruteforcelogin.png>)

### Windows Event Viewer - Failed Logins (Event ID 4625)
![Windows Security Log](<(Brute Force) screenshots/EventID4625.png>)

### Splunk Alert Configuration - Brute Force Detection
![Splunk Alert Settings](<(Brute Force) screenshots/Splunk Alert Configuration.png>)

# Active Directory Password Spraying Detection Lab

## Overview
This project simulates a password spraying attack against an Active Directory domain and detects it using Splunk.

## Lab Environment
- Windows 10 (Domain Joined)
- Windows Server (Domain Controller - SOC-SRV01)
- Splunk Enterprise
- Splunk Universal Forwarder
- VirtualBox Host-Only Network

## Attack Simulation
Simulated password spraying using repeated failed SMB authentication attempts.

## Attack Execution

The following command was used to simulate a password spraying attack against the Domain Controller:

```cmd
for /L %i in (1,1,15) do net use \\SOC-SRV01\IPC$ /user:SOCLAB\sprayuser%i WrongPass123
```

![Attack Execution](<(Password Spraying) screenshots/01-attack-execution.png>)

## Failed Logon Events (Event ID 4625)

The simulated password spray generated multiple failed network authentication events (Event ID 4625) on the Domain Controller.

![Failed Logon Events](<(Password Spraying) screenshots/02-4625-events.png>)

## Detection Query

To detect password spraying behavior, distinct failed usernames were counted per workstation:

```spl
index=wineventlog host=SOC-SRV01 EventCode=4625 Logon_Type=3
| stats dc(Account_Name) as unique_users by Workstation_Name
```

The query identifies a high number of unique failed logon attempts originating from a single workstation.

![Detection Query Results](<(Password Spraying) screenshots/03-detection-query.png>)

## Alert Configuration

A scheduled Splunk alert was created to detect password spraying activity when more than 10 unique failed network logons (Event ID 4625) occur from a single workstation.

```spl
index=wineventlog host=SOC-SRV01 EventCode=4625 Logon_Type=3
| stats dc(Account_Name) as unique_users by Workstation_Name
| where unique_users > 10
```

![Alert Logic](<(Password Spraying) screenshots/04-alert-logic.png>)

![Alert Configuration](<(Password Spraying) screenshots/05-alert-configuration.png>)

## Incident Summary

A simulated password spraying attack was conducted against the Active Directory domain using repeated SMB authentication attempts.

The attack generated multiple failed logon events (Event ID 4625) on the Domain Controller. Splunk successfully ingested the logs via the Universal Forwarder.

Detection logic was developed to identify abnormal authentication behavior by counting distinct failed usernames per workstation. When the threshold exceeded 10 unique failed logons within the defined time window, a scheduled alert triggered.

This project demonstrates:
- Active Directory attack simulation
- Windows Event Log analysis
- SPL detection engineering
- Threshold-based alerting
- MITRE ATT&CK mapping (T1110.003)


------------------------------------------------------------------------
------------------------------------------------------------------------


# Port Scan Detection in Elastic SIEM (Sysmon-Based)

This project demonstrates the design, implementation, and validation of
a behavioral detection rule for identifying port scanning activity using
**Elastic Security** and **Sysmon Event ID 3 logs**.

The objective was to simulate reconnaissance activity in a controlled
lab environment and engineer a production-style detection rule capable
of identifying abnormal network behavior.

This repository includes:

-   Attack simulation using Nmap\
-   Log analysis using Sysmon\
-   Threshold-based detection rule creation\
-   Alert validation and timeline investigation\
-   Industry-style SOC incident report\
-   Defensive recommendations

------------------------------------------------------------------------

## Lab Architecture

**Attacker:** Kali Linux\
**Target + SIEM Host:** Windows 11\
**SIEM Platform:** Elastic Stack (Main System deployment)\
**Log Forwarder:** Winlogbeat\
**Log Source:** Sysmon Event ID 3 (Network Connection)

### Network Flow

    Kali Linux (Attacker)
            ↓
    Port Scanning (Nmap)
            ↓
    Windows 11 (Victim + Elastic Host)
            ↓
    Sysmon Event ID 3 Logs
            ↓
    Winlogbeat
            ↓
    Elasticsearch
            ↓
    Elastic Security Detection Rule

------------------------------------------------------------------------

## Detection Objective

Detect abnormal port scanning behavior defined as:

-   Multiple network connection attempts\
-   Same source IP\
-   Rapid change in destination ports\
-   Threshold exceeded within defined time window

Detection is based on behavioral correlation, not static signatures.

------------------------------------------------------------------------

## Detection Logic

**Log Source:** Sysmon Event ID 3

### KQL Query

event.code: 3

### Rule Configuration

-   Rule Type: Threshold Rule\
-   Group By: source.ip\
-   Threshold: ≥ 10 events\
-   Time Window: 1 minute\
-   Severity: Medium\
-   Risk Score: 50

### MITRE ATT&CK Mapping

-   Tactic: Reconnaissance\
-   Technique: T1046 -- Network Service Discovery

------------------------------------------------------------------------

## Attack Simulation

The following Nmap scans were executed from Kali:

-   TCP SYN Scan (-sS)\
-   TCP Connect Scan (-sT)\
-   Service Version Detection (-sV)

These scans generated multiple network connection events across various
destination ports.

------------------------------------------------------------------------

## Alert Investigation

After rule activation:

-   Alert triggered successfully\
-   Source IP identified\
-   Multiple destination ports confirmed\
-   Timeline analysis validated reconnaissance behavior

No exploitation or privilege escalation activity was observed.

------------------------------------------------------------------------

## Incident Timeline

  Time       Event
  ---------- -------------------------------------
  12:17:45   First connection attempt observed
  12:18:17   Multiple port connections initiated
  12:25:01   Detection threshold met
  12:25:26   Alert generated in Elastic

Total Duration: \~7 minutes 41 seconds\
Threat Stage: Reconnaissance\
Severity: Medium

------------------------------------------------------------------------


## Skills Demonstrated

-   Detection Engineering\
-   KQL Query Development\
-   Sysmon Log Analysis\
-   Behavioral Threat Detection\
-   MITRE ATT&CK Mapping\
-   SOC Alert Investigation Workflow\
-   Incident Documentation Standards

------------------------------------------------------------------------

## Defensive Recommendations

-   Restrict unnecessary open ports\
-   Implement firewall hardening\
-   Monitor abnormal connection bursts\
-   Correlate reconnaissance with authentication activity\
-   Consider IDS/IPS integration

------------------------------------------------------------------------

## Learning Outcomes

This project reinforced:

-   Importance of proper log validation\
-   Behavioral detection over signature-based detection\
-   Threshold tuning considerations\
-   Structured SOC investigation methodology\
-   Professional incident reporting standards
