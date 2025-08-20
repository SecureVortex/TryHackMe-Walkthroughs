**Introduction**

Aurora EDR is an advanced endpoint detection and response solution that leverages Event Tracing for Windows (ETW) to provide comprehensive threat detection capabilities. This room focuses on understanding how Aurora EDR works, its configuration options, and practical implementation for threat hunting and incident response.

Event Tracing for Windows (ETW) is a Windows OS logging feature that provides a mechanism to trace and log events raised by user-mode applications and kernel-mode drivers. ETW provides the capability for applications and drivers to write events. For cybersecurity defenders, this becomes a vital source of detection information.

**ETW Components**

ETW is made up of three distinct parts:

- **Controllers:** These applications are used to configure event tracing sessions. They also initiate the providers. An example of a Controller is logman.exe.
- **Providers:** These are the applications that produce event logs.
- **Consumers:** These applications subscribe and listen to events in real-time or from a file.

**Event Information Categories**

The event information is categorised under these types of levels:

- **Information:** Describes the successful operation of a driver, application or service. Basically, a service is calling home.
- **Warning:** Describes an event that may not be a present issue but can cause problems in the future.
- **Error:** Describes a significant problem with a service or application.
- **Success Audit:** Outlines that an audited security access operation was successful. For example, a user's successful login to the system.
- **Failure Audit:** Outlines that an audited security access operation failed. For example, a failed access to a network drive by a user.

**Windows Log Categories**

Windows logs are placed under different categories, with three major ones used for system troubleshooting and investigations:

- **Application:** Records log events associated with system components such as drivers and interface components that run an app.
- **System:** Records events related to programs installed and running on the system.
- **Security:** Records events associated with security, such as logon attempts and resource access.

**Q1. What does ETW stand for?**

ETW stands for Event Tracing for Windows, which is a Windows OS logging feature that provides a mechanism to trace and log events raised by user-mode applications and kernel-mode drivers.

Answer: _Event Tracing for Windows_


**Q2. Which ETW component is responsible for writing events?**

Providers are the applications that produce event logs. They are responsible for writing events to the ETW subsystem.

Answer: _Providers_


**Q3. What is the purpose of ETW Controllers?**

Controllers are applications used to configure event tracing sessions and initiate the providers. An example of a Controller is logman.exe.

Answer: _Configure event tracing sessions_


**Q4. Which Windows log category records events related to programs installed and running on the system?**

The System log category records events related to programs installed and running on the system.

Answer: _System_


**Aurora EDR Overview**

Aurora is a Sigma-based EDR solution that uses ETW data for event matching. It comes in two versions:

**Aurora vs Aurora Lite Comparison:**

|Aurora|Aurora Lite|
|---|---|
|Sigma-based event matching on ETW data|Sigma-based event matching on ETW data|
|An open-source rule set (1300+ rules)|An open-source rule set (1300+ rules)|
|Nextron's Sigma rule set|No Nextron's Sigma rule set|
|Open-source IOC set|Open-source IOC set|
|Nextron's IOC set|No Nextron's IOC set|
|Alert output channels: Eventlog, File, UDP/TCP|Alert output channels: Eventlog, File, UDP/TCP|
|Comfortable management with ASGARD|-|
|Additional detection modules|-|
|Unlimited number of response actions|-|
|Rule encryption|-|

**Aurora vs Sysmon Comparison:**

|Aurora|Sysmon|
|---|---|---|
|Event Source|Event Tracing for Windows (ETW)|Sysmon Kernel Driver.|
|Sigma Rule Event Coverage|95%|100%|
|Relative Log Volume|Low|High|
|Sigma & IOC Matching|Yes|No|
|Response Actions|Yes|No|
|Resource Control (CPU Load, Output Throttling)|Yes|No|
|Output: Eventlog|Yes|Yes|
|Output: File|Yes|No|
|Output: TCP/UDP Target|Yes|No|
|Risk: Incomplete Data due to Filters|No|Yes|
|Risk: Blue Screen|No|Yes|
|Risk: High System Load|No|Yes|

**Q5. What is the main advantage of Aurora over Sysmon in terms of log volume?**

Aurora generates a relatively low log volume compared to Sysmon's high log volume, making it more efficient for processing and storage.

Answer: _Low log volume_


**Q6. Which version of Aurora includes Nextron's Sigma rule set?**

Only the full version of Aurora includes Nextron's Sigma rule set, while Aurora Lite does not include it.

Answer: _Aurora (full version)_


**Q7. What percentage of Sigma rule event coverage does Aurora provide?**

Aurora provides 95% Sigma rule event coverage compared to Sysmon's 100%.

Answer: _95%_


**Aurora Configuration Presets**

Aurora can be configured to use four different configuration formats that dictate how the solution would fetch events and raise alerts:

- **Standard:** This configuration covers events at a medium level of severity.
- **Reduced:** This configuration looks at events considered to be at a high minimum reporting level.
- **Minimal:** This configuration looks at events considered to be at a high minimum reporting level.
- **Intense:** This configuration looks at events considered to be at a low minimum reporting level.

**Preset Configuration Details:**

|Affected Setting|Standard|Reduced|Minimal|Intense|
|---|---|---|---|---|
|Deactivated sources|Registry, Raw Disk Access, Kernel Handles, Create Remote Thread|Registry, Raw Disk Access, Process Access|Registry, Raw Disk Access, Kernel Handles, Create Remote Thread, Process Access, Image Loads|-|
|CPU Limit|35%|30%|20%|100%|
|Process Priority|Normal|Normal|Low|Normal|
|Minimum Reporting Level|Medium|High|High|Low|
|Deactivated Modules|-|LSASS Dump Detector|LSASS Dump Detector, BeaconHunter|-|

**Q8. Which Aurora preset configuration has the lowest CPU limit?**

The Minimal preset configuration has the lowest CPU limit at 20%.

Answer: _Minimal_


**Q9. Which preset configuration covers events at a low minimum reporting level?**

The Intense preset configuration looks at events considered to be at a low minimum reporting level.

Answer: _Intense_


**Q10. What is the CPU limit for the Standard configuration?**

The Standard configuration has a CPU limit of 35%.

Answer: _35%_


**Aurora Response Actions**

Aurora responses extend the Sigma services and can be used to set specific actions to be performed when an event matches. These actions can help contain a threat actor or limit their damage to the system host.

**Predefined Responses:**

- **Suspend:** Used to stop a specified process.
- **Kill:** Used to kill a specified process.
- **Dump:** Used to create a dump file found in the `dump-path` folder.

**Custom Responses:**

Custom responses are meant to call an internal program to execute a function based on a raised alert. The program has to be available from `PATH` and the response would be a command-line query.

**Response Flags:**

|Flag|Definition|
|---|---|
|Simulate|Used to test out rules, and responses that won't be triggered. A log will be created to indicate the type of response that would be triggered.|
|Recursive|It is used to specify that the response will affect descendent processes. It is usually `true` by default.|
|Low privilege only|Marked by the flag `lowprivonly.` The flag specifies that the response will be triggered if the target process does not run as `LOCAL SYSTEM` or at an elevated role.|
|Ancestor|The `ancestors` flag specifies that the response will affect a process's ancestor, not itself. The key: value pair is indicated by integers to show the level of ancestors, e.g. one (1) is for parent process, 2 for grand-parent, and so on.|
|Process ID field|The `processidfield` flag specifies the field contains the process ID that shall be affected by the response.|

**Q11. What are the three predefined response actions available in Aurora?**

The three predefined response actions are Suspend, Kill, and Dump.

Answer: _Suspend, Kill, Dump_


**Q12. Which flag is used to test rules without actually triggering responses?**

The Simulate flag is used to test out rules and responses that won't be triggered. A log will be created to indicate the type of response that would be triggered.

Answer: _Simulate_


**Q13. What does the 'ancestors' flag specify in Aurora responses?**

The ancestors flag specifies that the response will affect a process's ancestor, not itself. The key: value pair is indicated by integers to show the level of ancestors (1 for parent process, 2 for grand-parent, etc.).

Answer: _Affects process ancestors_


**Aurora Event IDs**

Aurora uses specific event IDs to log to the Windows Eventlog. Here are some important ones:

**Sigma Rule Event IDs:**
- **1:** A process creation Sigma rule matched
- **3:** A network connection sigma rule matched
- **5:** A process termination Sigma rule matched
- **7:** An image-loaded Sigma rule matched
- **11:** A file creation Sigma rule matched
- **22:** A DNS query Sigma rule matched

**System Event IDs:**
- **102:** Aurora Agent started
- **103:** Aurora Agent is terminating
- **200:** BeaconHunter
- **300:** Lsass Dump Detector
- **6000:** A response for a sigma match was executed

**Q14. Which Event ID indicates that a process creation Sigma rule matched?**

Event ID 1 indicates that a process creation Sigma rule matched.

Answer: _1_


**Q15. What Event ID is used when Aurora Agent starts?**

Event ID 102 is used when Aurora Agent starts.

Answer: _102_


**Q16. Which Event ID corresponds to the BeaconHunter module?**

Event ID 200 corresponds to the BeaconHunter module.

Answer: _200_


**Detection Gaps and Solutions**

**Named Pipes**

Named pipes are a one-way communication channel between processes that are subject to security checks in the memory. ETW has no provider to gather information about the creation or connection to named pipes.

**Solution:**
- Using Aurora under "Intense" configurations
- Complement Aurora configuration with Sysmon to capture the events

**Registry Events**

Registry events are generated on the ETW by creating keys or writing values, primarily via the Microsoft-Windows-Kernel-Registry. However, this information may not be directly usable as all registry handles must be tracked individually.

**Solution:**
- Using Aurora under "Intense" configurations
- Complement Aurora configuration with Sysmon to capture the events

**ETW Disabled**

Attackers have the ability to disable ETW events by patching the system calls that Windows would use to create the events from user space.

**Solution:**
- Aurora's full version uses the ETW Canary module to detect any manipulations of ETW
- Using the flag `--report-stats` allows for reporting the agent's status to your SIEM

**Q17. What is one limitation of ETW regarding named pipes?**

ETW has no provider to gather information about the creation or connection to named pipes.

Answer: _No provider for named pipe events_


**Q18. Which module in Aurora's full version detects ETW manipulations?**

The ETW Canary module detects any manipulations of ETW in Aurora's full version.

Answer: _ETW Canary_


**Q19. What flag can be used to report Aurora's status to your SIEM?**

The `--report-stats` flag allows for reporting the agent's status to your SIEM and will include stats of the observed, processed and dropped events.

Answer: _--report-stats_


**Running Aurora**

Aurora can be run with different configuration files:

```powershell
C:\Program Files\Aurora-Agent>aurora-agent.exe -c agent-config-minimal.yml
```

**Output Options:**

- **Windows EventLog:** Source Application Source AuroraAgent
- **Log File:** Using the `--logfile` flag
- **UDP/TCP Targets:** Using `--udp-target` and `--tcp-target` flags

**Q20. What command-line argument is used to specify a configuration file when running Aurora?**

The `-c` argument is used to specify a configuration file when running Aurora.

Answer: _-c_


**Conclusion**

What we've learned:

1. Understanding Event Tracing for Windows (ETW) fundamentals
2. Aurora EDR features and capabilities compared to Sysmon
3. Configuration presets and their use cases
4. Response actions and automation capabilities
5. Event IDs and their significance
6. Detection gaps and mitigation strategies

