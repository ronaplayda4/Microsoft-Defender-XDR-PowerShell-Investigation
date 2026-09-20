# Microsoft Defender XDR PowerShell Investigation

## Project Overview

This project documents a hands-on SOC investigation performed in a controlled Microsoft Defender XDR lab environment.

The purpose of the investigation was to generate PowerShell activity on a Windows 11 Azure endpoint, locate the activity using Advanced Hunting, identify the user involved, reconstruct the parent and child process relationships, and determine whether the activity showed signs of malicious behavior.

This project demonstrates my investigation workflow rather than simply identifying PowerShell execution.

---

## Lab Resources

- Windows 11 Azure Virtual Machine
- Endpoint: `ronap-lab-vm`
- Microsoft Defender XDR
- Microsoft Defender for Endpoint
- Advanced Hunting
- Kusto Query Language (KQL)
- DeviceProcessEvents
- DeviceNetworkEvents

---

## Tools and Integrations

### Microsoft Defender XDR
Used as the primary security platform for investigating endpoint telemetry.

### Microsoft Defender for Endpoint
Provided endpoint process and network telemetry from the Windows 11 lab machine.

### Advanced Hunting
Used to query Defender telemetry and investigate PowerShell activity using KQL.

### Azure Virtual Machine
Used as the Windows 11 endpoint where the controlled PowerShell activity was generated.

---

## Investigation Scenario

A controlled PowerShell session was generated on the Windows 11 Azure lab endpoint.

The investigation started by hunting for PowerShell activity in Microsoft Defender XDR. After locating the event, I identified the associated user and examined the parent and child processes.

The investigation established the following process chain:

`explorer.exe → powershell.exe → whoami.exe`

The activity was associated with the lab account:

`ronapadmin`

---

## Investigation Workflow

1. Hunt for PowerShell activity using Advanced Hunting
2. Identify the user associated with the event
3. Examine the initiating parent process
4. Identify child processes created by PowerShell
5. Check for related network activity
6. Correlate the evidence
7. Determine the final verdict

---

## Key Findings

- PowerShell was launched interactively by `ronapadmin`.
- The activity occurred on `ronap-lab-vm`.
- `explorer.exe` was identified as the parent process.
- `powershell.exe` was launched from the interactive Windows session.
- `whoami.exe` was executed as a child process of PowerShell.
- No related outbound PowerShell network activity was identified.
- The observed activity matched the controlled lab test.

### Process Chain

`ronapadmin → explorer.exe → powershell.exe → whoami.exe`

---

## Network Investigation

After identifying the PowerShell process, I pivoted from process telemetry to network telemetry to determine whether PowerShell established an external connection.

No related PowerShell network activity was identified.

A query returning zero results was still useful because it documented that network activity had been checked as part of the investigation.

---

## Verdict

**Expected / Benign Controlled Lab Activity**

The account, endpoint, process chain, and command execution were consistent with the activity intentionally generated in the lab.

PowerShell was not classified as suspicious simply because it executed. The verdict was reached after correlating the user, parent and child processes, command context, and available network telemetry.

---

## Investigation Evidence

The investigation included screenshots documenting:

1. Initial PowerShell hunt
2. User and parent process identification
3. Child process pivot
4. Network activity check

Screenshots will be stored in the `/screenshots` directory.

---

## Skills Practiced

- Microsoft Defender XDR
- Microsoft Defender for Endpoint
- Advanced Hunting
- KQL querying
- Endpoint investigation
- Process-tree analysis
- Parent/child process correlation
- Network telemetry analysis
- Evidence collection
- Incident investigation
- Determining an investigation verdict

---

## Reflection

This investigation helped me understand that finding PowerShell activity is only the beginning of an investigation.

Instead of assuming that PowerShell was malicious, I learned to start with the event, identify the user and endpoint, examine parent and child process relationships, review the command being executed, and pivot to network telemetry.

One of my biggest takeaways was that context matters. A legitimate administrative tool such as PowerShell can also be used by attackers, so an analyst needs to correlate multiple pieces of evidence before determining whether an activity is malicious or benign.

I also learned that a query returning zero results can still provide useful investigative evidence because it documents what was checked.

---

## Key Takeaways

- Start with the event tied to the investigation.
- Do not assume PowerShell activity is automatically malicious.
- Use parent and child processes to reconstruct activity.
- Pivot between process and network telemetry when investigating.
- Document both positive and negative findings.
- Use context and correlated evidence before reaching a verdict.

---

## Next Steps

I plan to continue improving my skills in:

- KQL threat hunting
- Microsoft Sentinel
- Microsoft Defender XDR
- Alert triage
- Process analysis
- Incident investigation
- Detection engineering
- SOC documentation

---

## Disclaimer

This investigation was performed in a controlled lab environment for cybersecurity training and educational purposes.
