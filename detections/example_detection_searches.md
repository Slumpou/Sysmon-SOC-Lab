# Example Detection Searches for Sysmon Logs

This file contains example Splunk detection searches used with Sysmon Operational logs that were ingested into Splunk Enterprise during this lab.

The goal of these searches is to demonstrate some basic threat-hunting techniques that a SOC analyst might use when monitoring Windows systems. While these examples are simple, they show how Sysmon data can be used to identify potentially suspicious activity such as unusual processes, unexpected network connections, or abnormal PowerShell usage.

Each search below includes:

* The Sysmon EventCode being analyzed
* A short explanation of why it is relevant in a SOC environment
* Some useful fields that would normally be reviewed during an investigation

## 1. Process Creation – Monitoring Newly Launched Processes

This search shows recently created processes on the host. Monitoring process creation is one of the most important things analysts do because attackers often run tools, scripts, or malware that appear as new processes on the system.

During testing in this lab, I generated EventCode 1 logs by launching simple applications such as Notepad through PowerShell. This confirmed that Sysmon events were being successfully collected and indexed by Splunk.

```spl
index=main sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=1
| sort -_time | head 50
```

Useful fields to review during investigation:

`Image` – path of the executable that was launched
`CommandLine` – arguments used to start the process (useful for spotting suspicious flags or encoded commands)
`ParentImage` – the parent process that launched the executable
`User` – which account executed the process

## 2. Network Connections – Monitoring Outbound Activity

This search shows network connections created by processes on the system. Monitoring network activity can help identify malware communicating with external servers, unusual ports being used, or unexpected applications connecting to the internet.

```spl
index=main sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=3
| sort -_time | head 20
```

Useful fields to review:

`Image` – which process initiated the connection
`DestinationIp` / `DestinationPort` – the destination of the connection
`Protocol` – typically TCP or UDP
`User` – the account associated with the process

## 3. Driver or Image Loads – Monitoring System Modules

This search looks for drivers or modules loaded on the system. While many of these events are normal, attackers sometimes load malicious drivers or tamper with system libraries as a persistence technique.

```spl
index=main sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=6
| sort -_time | head 20
```

Useful fields to review:

`ImageLoaded` – the driver or module path
`Hashes` – file hashes that can be checked against threat intelligence sources
`Signed` – whether the file has a valid digital signature
`User` – which account loaded the module

## Quick Reference for EventCodes

EventCode=1 – Process creation
EventCode=3 – Network connections
EventCode=6 – Driver or image loads

## 4. Suspicious PowerShell Execution – Identifying Encoded Commands

PowerShell is a legitimate administrative tool in Windows, but it is also commonly abused by attackers. One technique frequently used is encoding commands so that they are harder to read or detect.

This search looks for PowerShell executions that include the `-EncodedCommand` flag.

```spl
index=main sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=1
| search Image="*powershell.exe" AND CommandLine="*-EncodedCommand*"
| sort -_time | head 20
```

Why this matters:

Encoded commands can hide the real behavior of a script. Monitoring for this type of activity can help analysts quickly identify suspicious PowerShell usage and investigate further before the activity escalates.

Useful fields to review:

`Image` – usually powershell.exe or pwsh.exe
`CommandLine` – reveals the encoded flag or other suspicious arguments
`User` – which account executed the command
`ParentImage` – shows what launched PowerShell
