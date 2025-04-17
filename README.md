# TryHackMe Write-up

## Nmap Scan

I began by running an Nmap scan to identify open ports and services on the target machine.

![Nmap Scan](https://raw.githubusercontent.com/Anjai7/Tryhackme_CTF/main/nmap.png)

The scan revealed that FTP, HTTP, and SSH services were running.

## Initial Web Exploration

I visited the website, but initially, nothing significant was found.

### Gobuster Directory Bruteforce

Next, I performed a Gobuster directory brute force attack:

![Gobuster Output](https://raw.githubusercontent.com/Anjai7/Tryhackme_CTF/main/gobuster.png)

This led me to the path [http://10.10.2.131/simple/](http://10.10.2.131/simple/), where I discovered that the website was running CMS Made Simple version 2.2.8. This information was hinted at by the "hints" button on the TryHackMe page.

![CMS Made Simple Version](https://raw.githubusercontent.com/Anjai7/Tryhackme_CTF/main/website.png)

## Exploit Search

I used SearchSploit to find a relevant exploit and found a SQL injection vulnerability.

![SearchSploit Results](https://raw.githubusercontent.com/Anjai7/Tryhackme_CTF/main/searchsploit.png)
## Exploit Execution

The initial exploit was written in Python2. I searched for a Python3 version and found it on GitHub:

- [Python3 Exploit Script](https://github.com/Jason-Siu/CVE-2019-9053-Exploit-in-Python-3/blob/main/46635.py)

I integrated this script with the original exploit and executed it. The process took about 10-15 minutes.

![Exploit Execution](https://raw.githubusercontent.com/Anjai7/Tryhackme_CTF/main/hashcracking.png)

## Cracking the Password Hash

The exploit provided a salt and password hash, which I used with Hashcat to crack the password.

![Hashcat Output](https://raw.githubusercontent.com/Anjai7/Tryhackme_CTF/main/hashcracking.png)

## SSH Access

Using the cracked password, I connected via SSH:

```bash
ssh -v mitch@10.10.2.131 -p 2222
