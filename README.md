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
