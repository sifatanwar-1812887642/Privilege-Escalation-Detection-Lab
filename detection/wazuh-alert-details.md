# Wazuh Alert Details

## Unauthorized Sudo / Privilege Escalation

The test account `poc_user` attempted to execute:

```bash
sudo su
```

Wazuh detected the unauthorized attempt with the following observed fields:

| Field | Observed Value |
|---|---|
| Rule ID | `5405` |
| Rule Description | `Unauthorized user attempted to use sudo` |
| Source User | `poc_user` |
| Destination User | `root` |
| Command | `/usr/bin/su` |
| TTY | `pts/1` |
| PWD | `/home/poc_user` |
| Agent / Host | `ubuntu-target-10.0.2.7` |
| Agent IP | `10.0.2.7` |
| MITRE Technique | `T1548.003 – Sudo and Sudo Caching` |
| Tactics | Privilege Escalation, Defense Evasion |

The underlying sudo/journald log indicated that `poc_user` was not in sudoers.

## Validation Commands Used

```bash
groups poc_user
```

Observed group membership:

```text
poc_user : poc_user users
```

The account did not belong to the `sudo` group.

Active sessions were later checked with:

```bash
who
```

Only authorized `sifat` sessions were present at the time of verification.

The account status was checked with:

```bash
sudo passwd -S poc_user
```

The status contained `L`, confirming that the test account was locked.
