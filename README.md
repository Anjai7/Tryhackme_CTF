# 🧠 TryHackMe – CMS Made Simple CTF Write-up

**Machine IP:** `10.10.2.131`  
**Goal:** Capture User & Root Flags  
**Platform:** TryHackMe  
**Tags:** Enumeration, SQL Injection, Hash Cracking, Privilege Escalation

---

## 🔍 Step 1: Nmap Scan

Started with an `nmap` scan to identify open ports and running services.

```bash
nmap  -sV  10.10.2.131
![Nmap Scan](https://github.com/Anjai7/Tryhackme_CTF/blob/f51f367164480e655acb46cb9d7c14fbbcf033b3/nmap.png?raw=true)
