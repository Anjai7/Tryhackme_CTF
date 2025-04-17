# TryHackMe Write-up

## Nmap Scan
I started my attack by running an Nmap scan to discover the open ports and the services that were running on the target machine.

![Nmap Scan](https://hackmd.io/_uploads/rkGHNORRkg.png)

From the scan, I saw that FTP, HTTP, and SSH services were all running on the target.

## Initial Web Exploration
At first, I visited the website, but nothing significant was found on the main page. 

### Gobuster Directory Bruteforce
Next, I ran a Gobuster directory brute force attack to uncover hidden directories and files on the web server:

![Gobuster Output](https://hackmd.io/_uploads/rJvZBOACyg.png)

This gave me the path [http://10.10.2.131/simple/](http://10.10.2.131/simple/), where I found that the website was running CMS Made Simple version 2.2.8. I got this information after checking the "hints" button on the TryHackMe page, which was a helpful clue.

![CMS Made Simple Version](https://hackmd.io/_uploads/HJcwSu0AJe.png)

## Exploit Search
With the information I gathered, I turned to SearchSploit to search for potential vulnerabilities. It suggested an SQL injection exploit for CMS Made Simple.

![SearchSploit Results](https://hackmd.io/_uploads/ryFU8_AAyg.png)

## Exploit Execution
The exploit I initially found was written in Python2, but I didn't have enough time to modify the code to work with Python3. Instead, I searched for a Python3 version and found one on GitHub:

- [Python3 Exploit Script](https://github.com/Jason-Siu/CVE-2019-9053-Exploit-in-Python-3/blob/main/46635.py)

I copied the code from the GitHub repository and merged it with the original exploit. After executing it, the script ran for about 10-15 minutes, performing the exploitation.

![Exploit Execution](https://hackmd.io/_uploads/rJ_30dCRke.png)

## Cracking the Password Hash
After the exploit completed, I received a salt and password hash. I used Hashcat to crack the password from the hash.

![Hashcat Output](https://hackmd.io/_uploads/S1lGxFR0yl.png)

## SSH Access
Once I had the cracked password, I attempted to SSH into the target machine. Initially, I was stuck because I hadn’t specified the correct port. After revisiting the Nmap scan, I realized the correct port was 2222, so I ran:

```bash
ssh -v mitch@10.10.2.131 -p 2222
