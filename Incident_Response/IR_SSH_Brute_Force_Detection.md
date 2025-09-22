# Incident Response Report – SSH Brute Force Detection  

**Incident ID:** IR-SSH-20250919-01  
**Date Logged:** 2025-09-19  
**Analyst:** Sanele Thusi  
**System:** Linux SSH Authentication Logs  
**Source Data:** `Auth_log_labeled.csv`  

---

## 1. Summary  
On 2025-09-19, Splunk detected repeated **failed SSH login attempts** targeting multiple user accounts. The activity matches a brute-force attack pattern, where an attacker systematically attempts different usernames and passwords to gain unauthorized access.  

---

## 2. Detection Details  
- **Detection Method:** Splunk query using regex on `message` field:  
  ```spl
  index="brute-force-logs" sourcetype="csv"
  | rex field=message "(?i)(?<result>Failed password|Accepted password)"
  | stats count by src_ip, user, result
  | where result="Failed password" AND count > 5

- **Detection Time:** 14:20 UTC  
- **Trigger Rule:** More than **5 failed attempts** from the same source IP in a 5-minute window  

---

## 3. Findings  

### Accounts Targeted  
- **testuser** - 57 accumulation of failed attempts from multiple IPs  
- **root** - 51 accumulation of failed attempts from multiple IPs 
- **admin** - 47 failed attempts from multiple IPs  

### Top Source IPs  

| Source IP       | Failed Attempts | Notes                                  |
|-----------------|-----------------|----------------------------------------|
| 10.0.0.50       | 26              | External attacker, high volume         |
| 10.0.0.51       | 23              | Brute-force against `root`             |
| 10.0.0.52       | 20              | Internal system, suspicious `admin`    |
| 10.0.0.25       | 9               | Internal, unusual activity             |

---

## 4. Impact Assessment  

- **Impact Level:** Critical - multiple user accounts (including privileged accounts such as `root` and `svc_backup`) were successfully compromised.  
- **Confirmed Breach:** Several IPs successfully authenticated after repeated failed attempts, confirming brute-force success.  
- **Potential Risk:**  
  - Attackers may already have **interactive shell access** to the affected systems.  
  - Compromise of `root` allows **full administrative control**.  
  - Compromise of service accounts (e.g., `svc_backup`) may enable **persistence, privilege escalation, and lateral movement**.  
- **Scope:**  
  - Affected accounts: `root`, `alice`, `carol`, `testuser`, `svc_backup`.  
  - Multiple internal and external IPs involved in the brute-force campaign.  
  - Logs confirm activity across at least **10 unique IPs** with confirmed successes.  
- **Business Impact:**  
  - Potential data exfiltration, service disruption, or system takeover.  
  - Elevated risk of ransomware deployment or backdoor installation if attackers maintain access. 

---

## 5. Response Actions Taken  

1. **Blocked offending IPs** at the firewall and intrusion prevention systems.  
2. **Force-reset passwords** for all affected accounts (`root`, `alice`, `carol`, `testuser`, `svc_backup`) and disabled logins for non-essential accounts.  
3. **Collected forensic evidence** (log exports, Splunk searches, and authentication timelines) for incident review.  
4. **Isolated compromised hosts** (10.0.0.7, 10.0.0.4, 10.0.0.8, 10.0.0.9, 10.0.0.2, 10.0.0.3, 10.0.0.6) from the production network pending investigation.  
5. **Escalated incident severity** to Critical and notified management and the incident response (IR) team.  
6. Configured Splunk to **alert in real-time** when multiple failed attempts followed by success occur within a 5-minute window.  

---

## 6. Recommendations  

- **Credential Security**  
  - Immediately rotate credentials for all impacted accounts.  
  - Enforce **strong password complexity** requirements.  
  - Implement **Multi-Factor Authentication (MFA)** for all privileged accounts.  

- **System Hardening**  
  - Disable direct root SSH access.  
  - Restrict SSH logins to known IP ranges (VPN or bastion host recommended).  
  - Deploy **fail2ban** or similar intrusion prevention to auto-block repeated login failures.  

- **Monitoring & Detection**  
  - Enhance Splunk correlation rules to detect **failed → success patterns** in brute-force attempts.  
  - Continuously monitor accounts like `svc_backup` for abnormal behavior.  
  - Add detection rules for **lateral movement** and privilege escalation attempts.  

- **Forensic & Recovery**  
  - Perform **disk and memory analysis** on compromised hosts to check for persistence mechanisms or malware.  
  - Review system logs for potential **data exfiltration** or **command execution**.  
  - If persistence is found, consider **full rebuild of affected systems**.  

- **Long-Term Strategy**  
  - Conduct **red team simulation** to test resilience against brute-force and credential-based attacks.  
  - Provide **security awareness training** for admins and users on SSH hygiene.  
  - Incorporate this incident into the organization’s **threat model** for continuous improvement.  

---
