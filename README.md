# Privilege Escalation Detection Lab

## Overview

This lab demonstrates the detection of an unauthorized privilege escalation attempt on an Ubuntu endpoint using Wazuh SIEM.

A low-privileged account named `poc_user` attempted to execute a privileged shell using `sudo su`. The account was not a member of the sudo group and was not authorized in the sudoers configuration. Wazuh detected the attempt and generated an alert mapped to MITRE ATT&CK technique **T1548.003 – Sudo and Sudo Caching**.

## Lab Environment

| Component | Details |
|---|---|
| Target OS | Ubuntu Linux |
| Target Host | `ubuntu-target-10.0.2.7` |
| Target IP | `10.0.2.7` |
| Test User | `poc_user` |
| SIEM | Wazuh |
| Log Source | Linux journald / sudo authentication logs |

## Step 1 – Verify the Low-Privileged User

The group membership of the test account was checked:

```bash
groups poc_user
```

Observed output:

```text
poc_user : poc_user users
```

The account was not a member of the `sudo` group.

## Step 2 – Unauthorized Privilege Escalation Attempt

The following command was executed from the low-privileged account:

```bash
sudo su
```

The attempt failed because `poc_user` was not authorized to use sudo.

The sudo/journald event recorded that the user was not in the sudoers configuration.

## Step 3 – Wazuh Detection

Wazuh generated an alert for the unauthorized sudo attempt.

### Observed Alert Details

| Field | Value |
|---|---|
| Wazuh Rule ID | `5405` |
| Description | `Unauthorized user attempted to use sudo` |
| Source User | `poc_user` |
| Destination User | `root` |
| Command | `/usr/bin/su` |
| TTY | `pts/1` |
| Working Directory | `/home/poc_user` |
| Host | `ubuntu-target-10.0.2.7` |
| Agent IP | `10.0.2.7` |

The event confirmed that the unauthorized privilege escalation attempt was successfully detected by Wazuh.

## Step 4 – MITRE ATT&CK Mapping

The alert was mapped to:

- **Technique:** T1548.003 – Sudo and Sudo Caching
- **Tactic:** Privilege Escalation
- **Additional Tactic:** Defense Evasion

## Step 5 – Verify Active Sessions

Active sessions were checked using:

```bash
who
```

Only the authorized `sifat` sessions were observed. No active `poc_user` session was present during the response verification.

## Step 6 – Verify Account Status

The status of the test account was checked using:

```bash
sudo passwd -S poc_user
```

The output showed the account state as:

```text
poc_user L ...
```

`L` indicates that the account was locked.

## Detection Flow

```text
poc_user
   |
   v
sudo su
   |
   v
Unauthorized sudo attempt
   |
   v
Linux journald / sudo logs
   |
   v
Wazuh Agent
   |
   v
Wazuh Rule 5405
   |
   v
Privilege Escalation Alert
   |
   v
MITRE ATT&CK T1548.003
```

## Result

The lab successfully demonstrated that Wazuh can detect an unauthorized sudo-based privilege escalation attempt from a low-privileged Linux account. The generated alert identified the source user, target user, attempted command, terminal, working directory, and MITRE ATT&CK technique.

## Repository Structure

```text
Privilege-Escalation-Detection-Lab/
├── README.md
├── detection/
│   └── wazuh-alert-details.md
└── screenshots/
    └── README.md
```

## Disclaimer

This lab was performed in a controlled environment for cybersecurity training and detection engineering purposes only.
