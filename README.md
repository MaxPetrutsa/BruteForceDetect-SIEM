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
![Brute Force Detection](<(Brute Force) screenshots\SplunkDetectionbruteforcelogin.png>)

### Windows Event Viewer - Failed Logins (Event ID 4625)
![Windows Security Log](<(Brute Force) screenshots\EventID4625.png>)

### Splunk Alert Configuration - Brute Force Detection
![Splunk Alert Settings](<(Brute Force) screenshots\Splunk Alert Configuration.png>)
