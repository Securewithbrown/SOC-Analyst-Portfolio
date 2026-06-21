# SOC Login Monitoring Dashboard (Splunk)

### Overview
These SOC login monitoring dashboards were built in splunk to provide real-time windows authentication login activities.

---

### 1. Failed vs Successful Login Panel
This panel helps identify abnormal spikes in failed login which could indicate Brute-force attempts.

![Chart: Failed vs Successful Logins (Time-Based) line graph displaying an authentication attempt baseline of around 300 successful login events stretching over time with a lower trajectory trend line marking failure events.](/Screenshots/SIEM-Work/Failed-vs-Successful-login-attempt.png)

#### SPL Queries:
```splunk
index=main sourcetype=csv "Event ID" IN (4624, 4625)
|eval status=if('Event ID'="4624","Sucesss","Failure")
| timechart count span=5m by status
```

---

### 2. Top Source IP Panel
This panel helps highlight external hosts IP address generating authentication failures, allowing analysts to identify and block suspicious sources.

![Bar Chart: Top Source IP of Failed Logon Attempts highlighting the external host IP 113.161.192.227 generating a high frequency of authentication failures (over 3,000 counts).](/Screenshots/SIEM-Work/Top-source-IP.png)

#### SPL Queries:
```splunk
index=main sourcetype="csv" "Event ID"=4625 SourceIp=\(Ip_token\)
|stats count by SourceIp
|sort - count
|head 10
```

---

### 3. Most Targeted Accounts panel
This panel reveals the top target accounts generating logon audit failures, allowing analysts to take proper measures to protect and monitor the account for suspicious activities.

![Data Table: Most Targeted Account overview illustrating a breakdown table where the username 'administrator' generated 3,103 failed login audit counts.](/Screenshots/SIEM-Work/Most-targeted-account.png)

#### SPL Queries:
```splunk
index=main sourcetype="csv" "Event ID"=4625 TargetUserName=\(username_token\)
|stats count by TargetUserName
|sort - count
```

---

### 4. Full SOC Login Monitoring Dashboard

![Dashboard Interface Screenshot: Full SOC Login Monitoring Dashboard layout incorporating search tokens for Time Range, Source IP, and User Name, with the unified panels for failed vs successful login chart tracking, most targeted account data, and top source IP distributions grouped into a single layout pane.](/Screenshots/SIEM-Work/Full-SOC-Monitoring-dashboard.png)

### Summary
This Dashboard supports Tier 1 SOC analyst by enabling rapid detection, investigation and escalation of authentication based threats.
