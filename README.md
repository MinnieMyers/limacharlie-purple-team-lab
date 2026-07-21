# limacharlie purple team lab

## Overview

A hands on Purple Team exercise simulating real world attacker behavior against a Windows endpoint while validating detection and response capabilities using LimaCharlie EDR/SIEM.

This lab demonstrates how security teams can identify and stop early stage attacker activity before ransomware encryption impacts operations.

[![Watch the Demo](https://img.shields.io/badge/YouTube-Watch%20the%20Demo-purple?logo=youtube&logoColor=white)](https://youtu.be/hisZTYzv_nQ?si=Hh-grzVMIn8RlRQy)

## Responsible Use

This repository documents offensive techniques (credential access, C2 deployment, shadow copy deletion) performed in an isolated home lab for educational and detection engineering purposes only. All activity was executed against systems owned and controlled by the author. Nothing in this repository should be used against systems you do not own or have explicit written authorization to test.

## Objectives

* Simulate attacker techniques using LOLBINs and Sliver C2
* Validate endpoint visibility and detection capabilities
* Develop custom detection and response rules in LimaCharlie
* Convert passive detections into active prevention controls
* Map adversary behavior to MITRE ATT&CK techniques

## Lab Environment

* Windows VM as the target endpoint, running the LimaCharlie sensor
* Linux VM as the adversary machine, running Sliver C2
* LimaCharlie used as the EDR and SIEM platform for detection and response

## Attack Simulation and Detection Engineering

### 1. LOLBIN Abuse and Credential Access (Red Team)

* Used the trusted and signed `rundll32.exe` process to execute a malicious command
* Gained unauthorized access to LSASS and dumped hashed credentials
* Simulated a credential access technique commonly used prior to lateral movement

### 2. Command and Control Deployment (Red Team)

* Delivered a Sliver C2 implant to the target network through an Edge browser download
* Established an active session between the Linux adversary VM and the target Windows VM

### 3. C2 Detection Rule (Blue Team)

* Developed a custom detection and response rule in LimaCharlie EDR/SIEM to detect Sliver activity
* Configured the rule to generate an escalated detection alert for future C2 activity on the target network

### 4. Volume Shadow Copy Deletion (Red Team)

* Used the command line to gain elevated privileges
* Launched a shell to interact directly with the target operating system
* Executed the following command to remove Volume Shadow Copies in preparation for a ransomware attack:

```text
vssadmin delete shadows /all
```

### 5. Shadow Copy Block Rule (Blue Team)

* Analyzed LimaCharlie's default detection rule for the Delete Shadow Copy event
* Converted the detection into an active blocking rule applied to the parent process tree
* Modified the response to add the `deny_tree` command, blocking future processes attempting to delete backups

## MITRE ATT&CK Mapping

| Technique | ID | Tactic |
|---|---|---|
| OS Credential Dumping: LSASS Memory | T1003.001 | Credential Access |
| System Binary Proxy Execution: Rundll32 | T1218.011 | Defense Evasion |
| Application Layer Protocol (C2) | T1071 | Command and Control |
| Inhibit System Recovery | T1490 | Impact |

🎓 This lab is based on Eric Capuano's So You Want To Be A SOC Analyst course. It has been completed as a personal educational project to reinforce detection engineering and SOC analyst skills.

[![Learn More](https://img.shields.io/badge/Digital%20Defense%20Institute-Learn%20More-purple)](https://digitaldefenseinstitute.com/)

## Key Takeaway

Early stage attacker behavior, such as LSASS access and shadow copy deletion, can be detected and actively blocked before ransomware encryption occurs, when EDR detection logic is converted into enforced prevention rules.

## License

This project is licensed under the MIT License. See the LICENSE file for details.
