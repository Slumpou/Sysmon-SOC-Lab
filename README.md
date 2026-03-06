# Sysmon SOC Lab

This repository contains a Security Operations Center (SOC) lab using **Sysmon** logs ingested into **Splunk**. It demonstrates basic threat hunting, detection, and log analysis techniques that SOC analysts use to monitor and investigate suspicious activity on Windows hosts.

## Purpose

- Provide a structured lab environment for learning SOC detection techniques
- Demonstrate how Sysmon logs can be ingested, searched, and monitored in Splunk

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
- **Driver & Module Monitoring:** Detect loaded drivers and DLLs for potential malware persistence

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