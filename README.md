# Lab 09: Running a SOC and Hunting for Threats

## Aim
To perform fast, offline threat hunting on Windows Event Logs (EVTX) using Sigma rules to uncover advanced attackers and behavioral anomalies that evade standard automated SOC alerts.

---

## Architecture Diagram

```mermaid
graph TD
    subgraph Log Sources
        SEC[Security.evtx]
        SYS[Sysmon.evtx]
        PS[PowerShell.evtx]
    end

    subgraph Threat Hunting Engine
        SIGMA[Sigma Rules Library]
        HAYA[Hayabusa CLI Analyzer]
    end

    subgraph SOC Analysis
        CSV[threat_timeline.csv]
        ANALYST[SOC Analyst]
    end

    SEC -->|Load Logs| HAYA
    SYS -->|Load Logs| HAYA
    PS -->|Load Logs| HAYA
    
    SIGMA -->|Apply Detection Logic| HAYA
    HAYA -->|Process 100k+ Events| CSV
    CSV -->|Identify Anomalies & Credential Dumping| ANALYST
```

---

## Tools Required
* **Hayabusa (CLI):** A timeline generator and threat hunting tool designed for rapid forensic analysis of Windows event logs.
* **Sigma Rules:** A generic, open signature format allowing defenders to describe relevant log events in a standardized way.
* **Sample EVTX Logs:** Raw Windows Event Logs containing mock malicious activity for analysis.

---

## Execution Steps

### 1. Log Preparation
1. Download mock malicious Windows Event Logs (e.g., `Security.evtx`, `Sysmon.evtx`, `PowerShell Operational`) into a local directory named `evtx_samples/`.

### 2. Execute Hayabusa Hunt
1. Open a command prompt and execute Hayabusa against the log directory to generate a consolidated CSV timeline:
   ```cmd
   hayabusa-windows-amd64.exe csv-timeline -d ./evtx_samples/ -o threat_timeline.csv
   ```
2. Note the console output detailing the number of events processed, rules applied, and severity of the matches found.

### 3. Timeline Analysis
1. Open the resulting `threat_timeline.csv` in a spreadsheet editor.
2. Filter by `high` and `crit` levels to locate specific indicators of compromise.
3. Identify the timestamps and command-line details for the Suspicious PowerShell EncodedCommand and the LSASS Credential Dumping activities.

---

## Screenshots

### 1. Hayabusa Execution and Summary
<img width="1438" height="619" alt="image" src="https://github.com/user-attachments/assets/28eaffd9-1331-40ce-bef4-4bd5e1949d39" />
<img width="1417" height="540" alt="image" src="https://github.com/user-attachments/assets/f9c1d607-3a7f-4e3d-b572-8de611420c29" />
<img width="1464" height="544" alt="image" src="https://github.com/user-attachments/assets/b7385ace-988e-4985-afa3-e3ec68aaea16" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8b8db048-1d45-4432-89bd-2a31d12d017b" />
<img width="1400" height="620" alt="image" src="https://github.com/user-attachments/assets/ebfcfca9-067b-4c4c-bceb-42f778689b42" />
<img width="1375" height="784" alt="image" src="https://github.com/user-attachments/assets/7393b5dc-aacf-4b92-802f-19be9ab94a9d" />


### 2. CSV Threat Timeline Analysis
*(Student: Insert a screenshot here of your spreadsheet application showing the parsed timeline and highlighting the detected malicious events)*
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c3d32ff1-fdf1-4add-9814-e1fc5fb49dc2" />


---

## Result
* Successfully deployed Hayabusa to conduct rapid, offline threat hunting across raw Windows Event Logs.
* Applied standardized Sigma rules to successfully identify behavioral anomalies, specifically uncovering PowerShell evasion tactics and LSASS memory access used for credential dumping.
