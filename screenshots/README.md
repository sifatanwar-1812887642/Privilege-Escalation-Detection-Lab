# Screenshots / Evidence

The completed lab report contains the following evidence screenshots. Upload the extracted images to this folder using these names so they can be referenced directly from the main README.

| Filename | Evidence |
|---|---|
| `01-create-poc-user.png` | `sudo adduser poc_user` and creation of the low-privileged POC account |
| `02-unauthorized-sudo-attempt.png` | `poc_user` executes `sudo su` and receives `poc_user is not in the sudoers file.` |
| `03-auth-log-evidence.png` | `/var/log/auth.log` showing `poc_user : user NOT in sudoers`, `TTY=pts/1`, `PWD=/home/poc_user`, `USER=root`, and `COMMAND=/usr/bin/su` |
| `04-wazuh-rule-5405.png` | Wazuh dashboard filtered on `rule.id:5405`, showing unauthorized sudo alerts from `ubuntu-target-10.0.2.7` |
| `05-wazuh-alert-details.png` | Wazuh document details showing Agent ID `003`, IP `10.0.2.7`, source user `poc_user`, destination `root`, and command `/usr/bin/su` |
| `06-wazuh-full-log.png` | Event/full-log details for the unauthorized sudo attempt |
| `07-account-lock-response.png` | `sudo usermod -L poc_user`, `sudo passwd -S poc_user`, locked state `L`, and failed login verification |

## Suggested README Placement

- Put screenshots 01–02 under **Attack Simulation**.
- Put screenshot 03 under **Verify the Linux Authentication Log**.
- Put screenshots 04–06 under **Wazuh Detection**.
- Put screenshot 07 under **Containment Response**.

These screenshots are evidence from the completed controlled lab and should not be replaced with unrelated sample images.