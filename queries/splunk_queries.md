# 🔍 Splunk Queries — Brute Force SSH Investigation (Linux)

This document contains the Splunk SPL queries used to investigate a suspected brute-force SSH attack on a Linux system.

---

## 🧠 Investigation Context

- Target System: Linux Host  
- Log Source: `linux_secure`  
- Index: `linux-alert`  
- Suspicious IP: `10.10.242.248`  

The investigation focused on detecting repeated SSH authentication attempts and identifying attack patterns.

---

## 1️⃣ Detect SSH Authentication Activity (Timeline Analysis)

### 🎯 Purpose
To retrieve all authentication-related events (successful, failed, and invalid user attempts) from the suspicious IP address and build a timeline of attacker activity.

### 🔎 Query
```spl
index="linux-alert" sourcetype="linux_secure" 10.10.242.248
| search "Accepted password for" OR "Failed password for" OR "Invalid user"
| sort + _time
