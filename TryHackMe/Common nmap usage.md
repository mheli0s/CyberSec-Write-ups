---
category: TryHackMe
---

### host discovery:

|Scan Type|Example Command|
|---|---|
|ARP Scan|`sudo nmap -PR -sn MACHINE_IP/24`|
|ICMP Echo Scan|`sudo nmap -PE -sn MACHINE_IP/24`|
|ICMP Timestamp Scan|`sudo nmap -PP -sn MACHINE_IP/24`|
|ICMP Address Mask Scan|`sudo nmap -PM -sn MACHINE_IP/24`|
|TCP SYN Ping Scan|`sudo nmap -PS22,80,443 -sn MACHINE_IP/30`|
|TCP ACK Ping Scan|`sudo nmap -PA22,80,443 -sn MACHINE_IP/30`|
|UDP Ping Scan|`sudo nmap -PU53,161,162 -sn MACHINE_IP/30`|

Remember to add `-sn` if you are only interested in host discovery without port-scanning. Omitting `-sn` will let Nmap default to scanning live hosts for ports.

|Option|Purpose|
|---|---|
|`-n`|no DNS lookup|
|`-R`|reverse-DNS lookup for all hosts|
|`-sn`|host discovery only|-

### port scan types on discovered hosts:

| Port Scan Type   | Example Command               |
| ---------------- | ----------------------------- |
| TCP Connect Scan | `nmap -sT 10.145.146.57`      |
| TCP SYN Scan     | `sudo nmap -sS 10.145.146.57` |
| UDP Scan         | `sudo nmap -sU 10.145.146.57` |

| Option                  | Purpose                                  |
| ----------------------- | ---------------------------------------- |
| `-p-`                   | all ports                                |
| `-p1-1023`              | scan ports 1 to 1023                     |
| `-F`                    | 100 most common ports                    |
| `-r`                    | scan ports in consecutive order          |
| `-T<0-5>`               | -T0 being the slowest and T5 the fastest |
| `--max-rate 50`         | rate <= 50 packets/sec                   |
| `--min-rate 15`         | rate >= 15 packets/sec                   |
| `--min-parallelism 100` | at least 100 probes in parallel          |
### advanced/less common usage:
|Port Scan Type|Example Command|
|---|---|
|TCP Null Scan|`sudo nmap -sN 10.146.189.81`|
|TCP FIN Scan|`sudo nmap -sF 10.146.189.81`|
|TCP Xmas Scan|`sudo nmap -sX 10.146.189.81`|
|TCP Maimon Scan|`sudo nmap -sM 10.146.189.81`|
|TCP ACK Scan|`sudo nmap -sA 10.146.189.81`|
|TCP Window Scan|`sudo nmap -sW 10.146.189.81`|
|Custom TCP Scan|`sudo nmap --scanflags URGACKPSHRSTSYNFIN 10.146.189.81`|
|Spoofed Source IP|`sudo nmap -S SPOOFED_IP 10.146.189.81`|
|Spoofed MAC Address|`--spoof-mac SPOOFED_MAC`|
|Decoy Scan|`nmap -D DECOY_IP,ME 10.146.189.81`|
|Idle (Zombie) Scan|`sudo nmap -sI ZOMBIE_IP 10.146.189.81`|
|Fragment IP data into 8 bytes|`-f`|
|Fragment IP data into 16 bytes|`-ff`|

|Option|Purpose|
|---|---|
|`--source-port PORT_NUM`|specify source port number|
|`--data-length NUM`|append random data to reach given length|

These scan types rely on setting TCP flags in unexpected ways to prompt ports for a reply. Null, FIN, and Xmas scan provoke a response from closed ports, while Maimon, ACK, and Window scans provoke a response from open and closed ports.

|Option|Purpose|
|---|---|
|`--reason`|explains how Nmap made its conclusion|
|`-v`|verbose|
|`-vv`|very verbose|
|`-d`|debugging|
|`-dd`|more details for debugging|

### post port scan usage:

eg: `nmap -sC -sV -oN nmap-initial.txt 10.0.0.1`

| Option                      | Meaning                                         |
| --------------------------- | ----------------------------------------------- |
| `-sV`                       | determine service/version info on open ports    |
| `-sV --version-light`       | try the most likely probes (2)                  |
| `-sV --version-all`         | try all available probes (9)                    |
| `-O`                        | detect OS                                       |
| `--traceroute`              | run traceroute to target                        |
| `--script=SCRIPTS`          | Nmap scripts to run                             |
| `-sC` or `--script=default` | run default NSE scripts                         |
| `-A`                        | equivalent to `-sV -O -sC --traceroute`         |
| `-oN`                       | save output in normal format                    |
| `-oG`                       | save output in grepable format                  |
| `-oX`                       | save output in XML format                       |
| `-oA`                       | save output in normal, XML and Grepable formats |

> *Credit:* Tryhackme -  Nmap rooms in Network Security module 
