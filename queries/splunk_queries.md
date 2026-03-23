# 🔍 Splunk Queries — Brute Force Investigation

---

## 1. Detect Failed Login Attempts

### Purpose
Identify multiple failed login attempts which may indicate brute-force activity.

### Query
index="linux-alert" sourcetype="linux_secure" 10.10.242.248 
| search "Accepted password for" OR "Failed password for" OR "Invalid user"
| sort + _time

---

## 2. Identify Top users attacked

### Purpose
This query retrieves failed login events and groups them by user and count to identify repeated login failures on each user

### Query
index="linux-alert" sourcetype="linux_secure" 10.10.242.248 "Failed password for"
| rex field=_raw "Failed password for(?: invalid user)? (?<username>\S+)"
| stats earliest(_time) as start latest(_time) as end count by username
| eval duration_minutes=round((end-start)/60,0)
| table username count duration_minutes
| sort - count
