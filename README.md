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
![Nmap Scan](https://github.com/Anjai7/Tryhackme_CTF/blob/main/nmap.png?raw=true)
