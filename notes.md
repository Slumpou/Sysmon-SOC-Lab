\# Example Investigation Notes



During testing of Sysmon process creation logs (EventCode=1), I generated events by launching simple applications such as Notepad through PowerShell.



This allowed me to confirm that Sysmon logs were successfully being forwarded and indexed by Splunk.



\*\*Example query used:\*\*



```spl

index=main sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational EventCode=1

| sort -\_time | head 20



From the event details, several important investigation fields can be reviewed:



Image – shows the executable that was launched

CommandLine – reveals how the process was started

ParentImage – identifies the process responsible for spawning the new process

User – indicates which account executed the process



In a real SOC environment, these fields help analysts determine whether a process was expected or potentially suspicious. For example, unusual parent-child process relationships or encoded PowerShell commands could indicate malicious activity.





\## Observations



After generating several Sysmon events and reviewing them in Splunk, I was able to confirm that the system was correctly logging process activity.



For example, when launching Notepad from PowerShell, a Sysmon EventCode 1 entry appeared showing the new process along with useful investigation fields such as the command line, parent process, and the user account that executed it.



This type of logging is important because it allows analysts to trace how programs start on a system and identify unusual behavior, such as scripts launching unexpected tools or suspicious parent-child process relationships.



\## Troubleshooting Notes



During the process of setting up this project, I ran into several challenges getting Sysmon logs to appear in Splunk. Even though Sysmon was installed and running, I wasn’t seeing any events at first. After a lot of trial and error, I realized the issue was with the log file path—not being accessible by Splunk. Once I moved the log location to a public user folder and adjusted permissions, events started flowing. I also had to rethink my search logic: initially, I was filtering too narrowly by event code, so I broadened the search, looked at the raw events, and identified the patterns I needed. This taught me a lot about field extraction, indexing, and how to iteratively troubleshoot issues, which are all crucial skills in a SOC environment.



!\[Splunk Event Screenshot](splunk-eventid1.png)



This screenshot displays a Sysmon Event ID 1, which I triggered by launching Notepad. It shows key details like the process name and user, confirming that Sysmon successfully logged the action. By reviewing these fields, I ensured that the process creation events were being captured as expected, validating the integration between Sysmon and Splunk.

