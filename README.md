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


## Observations

After generating several Sysmon events and reviewing them in Splunk, I was able to confirm that the system was correctly logging process activity.

For example, when launching Notepad from PowerShell, a Sysmon EventCode 1 entry appeared showing the new process along with useful investigation fields such as the command line, parent process, and the user account that executed it.

This type of logging is important because it allows analysts to trace how programs start on a system and identify unusual behavior, such as scripts launching unexpected tools or suspicious parent-child process relationships.


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

---

During the process of setting up this project, I ran into several challenges getting Sysmon logs to appear in Splunk. Even though Sysmon was installed and running, I wasn’t seeing any events at first. After a lot of trial and error, I realized the issue was with the log file path—not being accessible by Splunk. Once I moved the log location to a public user folder and adjusted permissions, events started flowing. I also had to rethink my search logic: initially, I was filtering too narrowly by event code, so I broadened the search, looked at the raw events, and identified the patterns I needed. This taught me a lot about field extraction, indexing, and how to iteratively troubleshoot issues, which are all crucial skills in a SOC environment.

![Splunk Event Screenshot](splunk-eventid1.png)

This screenshot displays a Sysmon Event ID 1, which I triggered by launching Notepad. It shows key details like the process name and user, confirming that Sysmon successfully logged the action. By reviewing these fields, I ensured that the process creation events were being captured as expected, validating the integration between Sysmon and Splunk."


## Example Investigation Notes

During testing of Sysmon process creation logs (EventCode=1), I generated events by launching simple applications such as Notepad through PowerShell.

This allowed me to confirm that Sysmon logs were successfully being forwarded and indexed by Splunk.

Example query used:

index=main sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=1
| sort -_time | head 20

From the event details, several important investigation fields can be reviewed:

Image – shows the executable that was launched  
CommandLine – reveals how the process was started  
ParentImage – identifies the process responsible for spawning the new process  
User – indicates which account executed the process

In a real SOC environment, these fields help analysts determine whether a process was expected or potentially suspicious. For example, unusual parent-child process relationships or encoded PowerShell commands could indicate malicious activity.


