# Sysmon SOC Lab

This repository contains a Security Operations Center (SOC) lab using **Sysmon** logs ingested into **Splunk**. It demonstrates basic threat hunting, detection, and log analysis techniques that SOC analysts use to monitor and investigate suspicious activity on Windows hosts.

## Purpose

- Provide a structured lab environment for learning SOC detection techniques
- Demonstrate how Sysmon logs can be ingested, searched, and monitored in Splunk

## Lab Goal

The goal of this lab was to learn how system activity from a Windows machine can be collected and reviewed using Splunk. I installed and configured Sysmon to generate detailed event logs from the system.

After confirming that the logs were successfully appearing in Splunk, I used simple searches to review things like process creation, network connections, and PowerShell activity. These are the types of events a SOC analyst would normally review when monitoring systems for suspicious behavior.

## Tools Used

Splunk Enterprise – log ingestion and search platform  
Sysmon – Windows system monitoring and event generation  
Windows Event Logs – telemetry source for host activity  
PowerShell – used to generate test events such as launching Notepad


## Project Structure

```
Sysmon-SOC-Lab/
│
├─ configs/               # Sysmon configuration files
├─ detections/            # Example detection searches and queries
│  └─ example_detection_searches.md
├─ README.md              # This overview file
└─ .gitignore             # Git ignore file
```

## Features

- **Process Monitoring:** Detect suspicious process creations
- **Network Monitoring:** Detect unusual network connections initiated by processes
- **Driver & Module Monitoring:** Detect loaded drivers for potential malware persistence


## How to Use

1. Clone the repository:
```bash
git clone https://github.com/Slumpou/Sysmon-SOC-Lab.git
```
2. Load your Sysmon configuration into a Windows host or lab VM
3. Ingest the logs into Splunk (or your SIEM of choice)
4. Open `detections/example_detection_searches.md` for example searches
5. Modify or expand detection searches as needed for your lab or SOC exercises

## Notes

- All detection searches are examples and should be adapted for real-world environments
- This lab is intended for learning, practice, and portfolio purposes only


