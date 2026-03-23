# 🔍 Splunk Queries — Brute Force Investigation

---

## 1. Detect Failed Login Attempts

### Purpose
Identify multiple failed login attempts which may indicate brute-force activity.

### Query
index="linux-alert" sourcetype="linux_secure" 10.10.242.248 
| search "Accepted password for" OR "Failed password for" OR "Invalid user"
| sort + _time

