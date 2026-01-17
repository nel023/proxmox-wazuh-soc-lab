# File Integrity Monitoring (FIM) Implementation

## Objective
Implement and validate File Integrity Monitoring (FIM) on both Windows and Linux endpoints using Wazuh. This enables detection of unauthorized file creation, modification, and deletion on endpoint systems.

---

## Lab Setup
- **SIEM**: Wazuh (Proxmox-based deployment)  
- **Endpoints**:
  - Windows 10 VM (Wazuh agent installed)  
  - Ubuntu Server VM (Wazuh agent installed)  
- **Purpose**: Simulate realistic user-level and system-level file changes and observe corresponding alerts in Wazuh Dashboard.

---

## Windows 10 FIM Configuration

**Monitored Path (example):**
`C:\Users\Public\test-fim`

**Setup Guide:**
1. Open Wazuh agent configuration file:
`C:\Program Files (x86)\ossec-agent\ossec.conf`

2. Add or update the `<syscheck>` block:
```xml
<syscheck>
    <disabled>no</disabled>
    <directories check_all="yes" report_changes="yes" realtime="yes"C:\Users\Public\test-fim</directories>
    <realtime>yes</realtime>
</syscheck>
```

3. Save changes and restart the Wazuh agent:

```xml
Restart-Service Wazuh
```

4. Test FIM by creating or modifying files in `C:\Users\Public\test-fim`, for example:
```xml
echo "Confidential file" > "C:\Users\Public\test-fim\fim_test.txt"
```

**Expected Result:**
File creation/modification events appear in Wazuh Dashboard under Endpoint Security → File Integrity Monitoring. Due to Windows agent behavior, alerts may appear with a short delay.

> *Image above is an example of syscheck event in Wazuh File Integrity Monitoring Dashboard. Folder and file names may be different depending on your setup*

## Ubuntu FIM Configuration

**Monitored Path:** `/home/ubuntu/confidential`

**Setup Guide:**

1. Open Wazuh agent configuration:
```xml
sudo nano /var/ossec/etc/ossec.conf
```

2. Add or update the <syscheck> block:
```xml
<syscheck>
    <disabled>no</disabled>
    <directories check_all="yes" report_changes="yes" realtime="yes">/home/ubuntu/confidential</directories>
    <interval>5m</interval>
</syscheck>
```

3. Save changes and restart the Wazuh agent:
```xml
sudo systemctl restart wazuh-agent
```

4. Optionally, rebuild baseline to include new paths:
```xml
sudo rm -f /var/ossec/queue/syscheck.db
sudo systemctl restart wazuh-agent
```

5. Test FIM by creating or modifying files:
```xml
mkdir -p /home/ubuntu/confidential
echo "Sensitive Data" > /home/ubuntu/confidential/fim_test.txt
```

**Expected Result:**
Alerts with syscheck metadata appear in Wazuh Dashboard, showing the full path and event type (added or modified). Linux FIM events are typically detected near real-time.

> *Image above is an example of syscheck event in Wazuh File Integrity Monitoring Dashboard. Folder and file names may be different depending on your setup*

## Validation

FIM functionality was validated by:
- Creating new files in monitored directories
- Modifying existing files
- Observing alerts in Wazuh Dashboard under **Endpoint Security → File Integrity Monitoring**

**Sample Events Detected:**
- File added: /home/ubuntu/confidential/fim_test.txt
- File modified: C:\Users\Public\fim_test.txt

## Observations
- Linux FIM events are detected almost immediately due to `inotify` real-time monitoring.
- Windows FIM events may exhibit a short delay due to agent-side batching and scheduled processing.
- Both endpoints reliably report file changes, demonstrating cross-platform FIM coverage.

## Conclusion

File Integrity Monitoring was successfully implemented and validated on both Windows and Linux endpoints. The setup provides:
- Visibility into unauthorized file activity
- Detection of potential data tampering or post-compromise actions
- A foundation for SOC monitoring and alert correlation
