# Example Detection Searches for Sysmon Logs

This file contains example Splunk detection searches applied to Sysmon Operational logs ingested into Splunk Enterprise. These searches demonstrate how to monitor Windows hosts for suspicious activity and highlight basic threat hunting techniques commonly used in a Security Operations Center (SOC).

Each search includes:
- The EventCode or behavior it monitors
- Why it matters in a SOC environment
- Key fields to investigate

---

## 1. Process Creation — Detect Suspicious Processes

This search lists recently created processes on the host. Monitoring process creation is fundamental because attackers often execute unusual binaries, scripts, or malware. This query can help identify suspicious activity, such as PowerShell scripts, unknown executables, or tools used for lateral movement.

```spl
index=main sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=1
| sort -_time | head 50
```

**Useful Fields for Investigation:**
- `Image` — path to the executable  
- `CommandLine` — arguments used; can reveal encoded or suspicious commands  
- `ParentImage` — parent process, useful for spotting process injection or spawn chains  
- `User` — which user executed the process  

---

## 2. Network Connections — Detect Suspicious Network Activity

This search monitors network connections made by processes. Network activity is a common vector for malware communication and lateral movement. Reviewing unusual destinations or high-volume connections can help identify suspicious behavior.

```spl
index=main sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=3
| sort -_time | head 20
```

**Useful Fields for Investigation:**
- `Image` — which process initiated the connection  
- `DestinationIp` / `DestinationPort` — where the process connected  
- `Protocol` — TCP/UDP  
- `User` — which account initiated the connection  

---

## 3. Driver or Image Loads — Monitor System Modules

This search identifies driver or module loads on the system. Monitoring loaded drivers and DLLs can help detect suspicious persistence mechanisms or malware modifying the kernel or system libraries.

```spl
index=main sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=6
| sort -_time | head 20
```

**Useful Fields for Investigation:**
- `ImageLoaded` — the driver or module path  
- `Hashes` — hash values, useful for detecting known malicious files  
- `Signed` — digital signature status, can indicate tampering  
- `User` — which account loaded the driver or module  

---

**Quick Reference for EventCodes:**
- `EventCode=1` → process creation  
- `EventCode=3` → network connections  
- `EventCode=6` → driver/image loads