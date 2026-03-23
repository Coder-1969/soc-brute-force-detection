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
```
### 🧠 Explanation

- This query filters Linux authentication logs to display:

- Successful logins (Accepted password)

- Failed login attempts (Failed password)

- Invalid username attempts (Invalid user)

- Sorting events chronologically allows reconstruction of attacker behavior.

### 📊 Outcome

- Multiple failed login attempts observed

- Invalid user attempts indicate username enumeration

- Continuous sequence of authentication activity detected

- Suggests automated brute-force behavior

## 2️⃣ Identify Source IP and Targeted Accounts

### 🎯 Purpose
To identify which source IPs and user accounts are involved in failed authentication attempts.

### 🔎 Query
```spl
index="linux-alert" "Failed password" OR "Authentication failure"
| stats count by src_ip user
```

### 🧠 Explanation

This query:

- Aggregates failed authentication events

- Groups results by source IP and username

- Highlights which IP is generating the most login failures

### 📊 Outcome

- Identified source IP responsible for majority of failed attempts

- Revealed targeted usernames

- Confirmed concentration of attack activity from a single source

## 3️⃣ Identify Most Targeted User Accounts

### 🎯 Purpose
To extract usernames from failed login events and determine which accounts were most frequently targeted, along with attack duration.

### 🔎 Query
```spl
index="linux-alert" sourcetype="linux_secure" 10.10.242.248 "Failed password for"
| rex field=_raw "Failed password for(?: invalid user)? (?<username>\S+)"
| stats earliest(_time) as start latest(_time) as end count by username
| eval duration_minutes=round((end-start)/60,0)
| table username count duration_minutes
| sort - count
```
### 🧠 Explanation

This query:

- Extracts usernames using regex (rex)

- Counts failed login attempts per user

- Calculates the duration of attack activity

- Displays results in a structured format

### 📊 Outcome

- Multiple user accounts targeted

- Certain usernames experienced significantly higher attack frequency

- Attack duration varied across accounts

- Indicates automated brute-force attempts with possible username enumeration
