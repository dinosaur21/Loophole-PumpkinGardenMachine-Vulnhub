# MISSION-PUMPKIN V1.0 - Loophole in PumpkinGarden

### Link to challenge
https://www.vulnhub.com/entry/mission-pumpkin-v10-pumpkingarden,321/

### Description
I discovered a loophole in the 1st level-PumpkinGarden of the Mission Pumpkin series ont he site *Vulnhub* that wasn't mentioned in the rules. This loophole allows for unintended shortcuts(gaining root access) that can undermine the intended challenge.

### System Details
1. Host- Parrot Security OS (Debian 64-bit)
2. Target Machine- Linux OS- https://www.dropbox.com/s/xu9mwtn49rijipc/PumpkinGarden.ova?dl=0
   
### Steps 

| **Step** | **Description**                                                                                             | **Command**                                       |
|----------|-------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| Step 1   | Run Nmap all port scan at highest speed (5).                                                                | `sudo nmap -p- <ip addr> -T 5`                    |
| Step 2   | Perform version detection scan to identify running services. Apache and SSH are on ports 1515 and 3535.     | `nmap -p- <ip addr> -sV`                          |
| Step 3   | Remotely log in to the FTP service using Name: `ftp` and Password: `username`.                              | `ftp <ip addr>`                                   |
| Step 4   | List files within FTP to find something interesting, download `note.txt` onto the system, and exit FTP.     | `ls`<br>`get note.txt`<br>`exit`                  |
| Step 5   | Display the contents of `note.txt` for a possible hint.                                                     | `cat note.txt`                                    |
| Step 6   | Use the hint "jack" from `note.txt` to log in via SSH on port 3535.                                         | `ssh jack@<ip addr> -p 3535`                      |
| Step 7   | Start Apache server on port 1515 in web browser by typing the URL given to find a hint for password.        | (URL in web browser, find "PumpkinGarden" hint)   |
| Step 8   | Log in as "jack" with the password hint from the Apache service.                                            | (Use password "PumpkinGarden" for SSH login)      |
| Step 9   | Check Jack's permissions to see he can run all commands as root except `/bin/su`.                           | `sudo -l`                                         |
| Step 10  | Try to use `sudo /bin/bash` or `sudo /bin/sh` to start a root shell directly; it works (the loophole).      | `sudo /bin/bash`<br>`sudo /bin/sh`                |
| Step 11  | After gaining root access, navigate to any directory to find the `PumpkinGarden_key` file which has the base-64 encoded format for the phrase "Congratulations!" Gaining access to this file was the primary goal of the CTF. | (Navigate to find `PumpkinGarden_key` file)       |

### Impact
- This loophole allows gaining direct root shell access in early stages of the machine(login of first user itself).There are supposed to be more hints to gain access to 2 more users and there passwords which can be skipped in this case).
- This root shell that we can start can allow us to move into a directory of our choice and gain the key without going through the other steps.
- It helps us skip almost 2/3rd of the machine.

### Suggested Fix
- Disable the command *sudo /bin/bash* or *sudo /bin/sh* for logged in user accounts(privilege handling).

### Screenshots



