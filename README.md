# 🔐 SOC Investigation: Brute Force Login Detection

## 📌 Overview
This project demonstrates a Security Operations Center (SOC) Level 1 investigation into a brute-force login attack using log analysis and SIEM (Splunk). The objective was to detect, analyze, and respond to suspicious authentication activity.

---

## 🚨 Scenario
Multiple failed login attempts were detected targeting a user account within a short period of time, indicating a potential brute-force attack.

---

## 🔍 Investigation Process
The following steps were performed:

- Identified abnormal authentication activity in logs
- Analyzed failed login events (Windows Event ID 4625)
- Correlated repeated login attempts from a single source IP
- Checked for successful logins (Event ID 4624)
- Assessed attack pattern and behavior

---

## 🧪 Tools & Technologies
- Splunk (SIEM)
- Windows Event Logs
- MITRE ATT&CK Framework

---

## 📊 Key Findings
- High volume of failed login attempts detected
- Repeated authentication attempts from a single IP address
- Targeted user account identified
- No successful login detected (or modify if applicable)

---

## 🧠 MITRE ATT&CK Mapping
- **T1110 — Brute Force**

---

## ⚠️ Conclusion
The activity was identified as a brute-force attack attempt targeting user authentication mechanisms.

---

## 🛡️ Recommendations
- Block the malicious IP address at firewall level
- Enable account lockout policies
- Implement Multi-Factor Authentication (MFA)
- Monitor authentication logs for similar patterns

---

## 📎 Evidence
Refer to:
- `/screenshots/` for log evidence
- `/queries/` for Splunk search queries
- `/report/` for full investigation report

---

## 📁 Project Structure



---

## 💡 Skills Demonstrated
- SIEM (Splunk) log analysis  
- Alert triage and investigation  
- Authentication log analysis  
- Threat detection  
- MITRE ATT&CK mapping  
- Incident reporting  

---

## 🔗 Author
[Sahil Pathak]  
Aspiring SOC Analyst | Cyber Security Graduate
