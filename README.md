\# Sysmon SOC Lab



This repository contains a Security Operations Center (SOC) lab using \*\*Sysmon\*\* logs ingested into \*\*Splunk\*\*. It demonstrates basic threat hunting, detection, and log analysis techniques that SOC analysts use to monitor and investigate suspicious activity on Windows hosts.



\## Purpose



\- Provide a structured lab environment for learning SOC detection techniques  

\- Demonstrate how Sysmon logs can be ingested, searched, and monitored in Splunk  

&nbsp;



\## Project Structure



```

Sysmon-SOC-Lab/

│

├─ configs/               # Sysmon configuration files

├─ detections/            # Example detection searches and queries

│  └─ example\_detection\_searches.md

├─ README.md              # This overview file

└─ .gitignore             # Git ignore file

```



\## Features



\- \*\*Process Monitoring:\*\* Detect suspicious process creations  

\- \*\*Network Monitoring:\*\* Detect unusual network connections initiated by processes  

\- \*\*Driver \& Module Monitoring:\*\* Detect loaded drivers and DLLs for potential malware persistence  



\## How to Use



1\. Clone the repository:

```bash

git clone https://github.com/Slumpou/Sysmon-SOC-Lab.git

```

2\. Load your Sysmon configuration into a Windows host or lab VM  

3\. Ingest the logs into Splunk (or your SIEM of choice)  

4\. Open `detections/example\_detection\_searches.md` for example searches  

5\. Modify or expand detection searches as needed for your lab or SOC exercises  



\## Notes



\- All detection searches are examples and should be adapted for real-world environments  

\- This lab is intended for learning, practice, and portfolio purposes only  

