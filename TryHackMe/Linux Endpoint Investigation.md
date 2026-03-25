### Linux Incident Surface

>  **Linux *Attack* Surface** refers to all the points of interaction in a Linux system where an adversary might attempt to exploit vulnerabilities to gain unauthorized access or carry out malicious activities *(pre-compromise).* 
>  GOAL: *Minimize it as prevention before an incident.*
>  eg. open ports, running services, network comms. etc. 
>  
>   **Linux *Incident* Surface**, on the other hand, refers to all the system areas involved in the detection, management, and response to an actual security incident *(post-compromise)*.
>   GOAL: *Identify it to detect, respond to, and recover after an incident*
>   eg. system logs/syslog, auth.log, running processes etc.

###### How to investigate footprints of processes:
- `ps aux | grep <sus_process_name>` - displays processes for (a)ll users in a detailed (u)ser-oriented format (includes user/pid, %/cpu/%mem and start time),  and e(x)tend output to include processes not attached to a terminal (useful for finding background/daemon processes). BSD style syntax. Alternative (SysV/POSIX style syntax): `ps -ef`
- `lsof -p <pid>` - list open files/resources connected with this (p)rocess  

###### How to investigate footprints of network communications:
- `ps aux | grep <sus_process_name>`
- `lsof -i -P -n` - list open files with (-i)nternetwork socket connections, show (-P)orts and IP addressess (-n)umerically (no converting to port names or hostnames)
- `osquery` tool: explore processes and its network connection with SQL syntax/output
	- `osqueryi` - start interactive osquery session
		- `osquery> SELECT pid, fd, socket, local_address, remote_address FROM process_open_sockets WHERE pid = 267490;`

###### How to investigate the footprints of persistence:
- examine log files in `/var/log/` directory
	- eg. `cat auth.log | grep useradd` - finds any user account creation activities
- examine `/etc/passwd`, `/etc/shadow`, `/etc/group`, `/etc/sudoers`
- examine the *Cron* job scheduler for malicious entries: 
	- `crontab -e` (current user)
	- view the actual cronjob files for a user in `/var/spool/cron/crontabs/<user>/` 
- examine all services installed under `/etc/systemd/system` for any suspicious-looking services
	- examine any found further via logs: 
		- `cat /var/log/syslog | grep suspicious`
		- `sudo journalctl -u suspicious`
- examine suspicious installed packages :
	- DEB-based: `dpkg -l | grep malicious`
	- RPM-based: `rpm -qa | grep malicious` or `dnf list --installed | grep ..`
	- examine package install logs:
		- `grep "malicious" /var/log/dpkg.log`
		- `grep " install " /var/log/dpkg.log` for DEB-based systems 
		- `grep "Installed" /var/log/dnf.rpm.log` for RPM-based systems

###### Important log files to examine:
- `/varlog/syslog` - for identifying system-wide events, errors, and warnings
- `/var/log/messages` - for diagnosing system issues and tracking system activity
- `/var/log/auth.log` - for detecting unauthorized access attempts and brute-force attacks
	- *Note*: check for older auth.log.n archives as well as the currently active one:
		- `/var/log/auth.log.1` - most recently rotated auth.log archive
		- `zless /var/log/auth.log.2.gz` - after `auth.1`, older archives (`.2` etc.) are `gzip` compressed to save space; `zless`/`zmore` tools allow viewing without having to decompress first
