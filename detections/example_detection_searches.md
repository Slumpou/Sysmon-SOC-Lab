example\_detection\_searches.md:



```markdown id="q1gk3a"

\# Example Detection Searches for Sysmon Logs



\## 1. Process Creation

Search for new processes created on the host:



```spl

index=main sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=1 | sort -\_time | head 20

````



\## 2. Network Connections



Search for network connections made by processes:



```spl

index=main sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=3 | sort -\_time | head 20

```



\## 3. Driver or Image Loads



Search for driver or module loads:



```spl

index=main sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=6 | sort -\_time | head 20

```



```



\- `EventCode=1` → process creation  

\- `EventCode=3` → network connections  

\- `EventCode=6` → driver/image loads  



