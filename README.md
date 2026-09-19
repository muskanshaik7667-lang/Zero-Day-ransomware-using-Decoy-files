# 🛡️ Behavior-Based EDR Engine for Zero-Day Ransomware Prevention via Decoy File Traps

## 📌 Project Overview

**Behavior-Based EDR Engine** is an endpoint security system designed to detect and respond to suspicious ransomware-like behavior using **decoy files (trap files)** and **process behavior monitoring**.

Traditional signature-based antivirus solutions primarily depend on known malware signatures. This project focuses on a **behavior-based approach**, where suspicious activity can be detected based on how a process interacts with protected files, even when the exact ransomware variant is unknown.

The system places specially created **decoy files** in monitored directories. When a suspicious process accesses, modifies, renames, or attempts to rapidly manipulate these files, the EDR engine collects information about the responsible process and analyzes its behavior.

If the activity crosses defined detection conditions, the system generates a security alert and can initiate a response such as terminating or isolating the suspicious process.

The detected events and investigation information are presented through a web-based dashboard.

---

## 🎯 Problem Statement

Ransomware can encrypt or modify large numbers of files within a short period of time. Traditional security mechanisms that depend heavily on known signatures may have difficulty identifying new or previously unseen ransomware variants.

There is therefore a need for a lightweight endpoint detection and response mechanism that can identify **suspicious file-system behavior** and respond to potential ransomware activity at an early stage.

---

## 💡 Proposed Solution

The proposed system uses **decoy file traps combined with behavior-based monitoring**.

The system:

1. Creates decoy files in selected directories.
2. Continuously monitors these files for suspicious activity.
3. Detects access, modification, deletion, renaming, or other abnormal operations.
4. Identifies the process responsible for the activity.
5. Collects process and system information.
6. Analyzes the observed behavior using predefined detection rules.
7. Generates a security event when suspicious behavior is detected.
8. Initiates an appropriate response.
9. Displays the investigation and response information on the dashboard.

---

## 🔄 System Workflow

```text
                    ┌───────────────────┐
                    │   Endpoint System │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │   Decoy Files     │
                    │ .xlsx / .pdf etc. │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │ File System       │
                    │ Monitoring        │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │ Suspicious File   │
                    │ Activity Detected │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │ Process           │
                    │ Identification    │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │ Behavior          │
                    │ Analysis          │
                    └─────────┬─────────┘
                              │
                     ┌────────┴────────┐
                     │                 │
                  Benign            Suspicious
                     │                 │
                     ▼                 ▼
                  Continue        ┌─────────────┐
                                  │   Response  │
                                  └──────┬──────┘
                                         │
                                         ▼
                                  ┌─────────────┐
                                  │ Event Log & │
                                  │ Investigation│
                                  └──────┬──────┘
                                         │
                                         ▼
                                  ┌─────────────┐
                                  │ Web Dashboard│
                                  └─────────────┘
```

---

## ✨ Key Features

### 1. 🪤 Decoy File Traps

The system creates realistic-looking decoy files such as:

* `.xlsx`
* `.pdf`
* `.docx`
* Other selected file types

These files act as early-warning indicators.

A legitimate application should normally have no reason to unexpectedly interact with specially placed decoy files.

---

### 2. 👁️ File-System Monitoring

The EDR engine monitors selected directories for suspicious operations involving decoy files.

Possible events include:

* File access
* File modification
* File creation
* File deletion
* File renaming
* Rapid repeated file operations

---

### 3. 🔍 Process Identification

When suspicious activity occurs, the system attempts to identify the process responsible for the event.

Relevant information may include:

* Process ID (PID)
* Process name
* Executable path
* Parent process
* Process creation information
* User associated with the process

---

### 4. 🧠 Behavior-Based Detection

Instead of relying only on malware signatures, the system evaluates observed behavior.

Potential indicators include:

* Interaction with decoy files
* Repeated file modifications
* Rapid file operations
* Multiple suspicious operations within a short period
* Unusual process activity

Multiple indicators can be combined to determine whether an event should be treated as suspicious.

---

### 5. 🚨 Alert Generation

When suspicious behavior satisfies the configured detection conditions, the system generates a security alert containing relevant investigation information.

Example:

```text
ALERT: Suspicious ransomware-like activity detected

Process: suspicious_process.exe
PID: 4820
Target: Documents/decoy.xlsx
Operation: Modified
Risk Level: High
Action: Process terminated
```

---

### 6. 🛑 Automated Response

Depending on the detection result, the system can initiate a response mechanism such as:

* Terminating the suspicious process
* Recording the incident
* Isolating or restricting the process where supported
* Preserving investigation information

The response mechanism is intended to reduce the potential impact of suspicious activity.

---

### 7. 📊 Security Dashboard

A web-based dashboard provides visibility into endpoint events.

The dashboard can display:

* Current security status
* Detection alerts
* Suspicious processes
* Decoy file events
* Risk/severity
* Event timestamps
* Response actions
* Investigation details

---

## 🏗️ System Architecture

```text
                  ┌──────────────────────────┐
                  │        Frontend          │
                  │      Web Dashboard       │
                  └────────────┬─────────────┘
                               │
                               │ REST API
                               ▼
                  ┌──────────────────────────┐
                  │        Backend           │
                  │      EDR Engine          │
                  └────────────┬─────────────┘
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                    │
          ▼                    ▼                    ▼
   Decoy Manager        File Monitor         Process Monitor
          │                    │                    │
          └────────────────────┼────────────────────┘
                               │
                               ▼
                     Behavior Analyzer
                               │
                               ▼
                     Detection Engine
                               │
                               ▼
                     Response Engine
                               │
                               ▼
                     Event / Incident DB
```

---

## 🧩 Main Components

### Decoy File Manager

Responsible for:

* Creating decoy files
* Maintaining decoy file locations
* Identifying monitored decoys
* Managing decoy file metadata

### File-System Monitor

Responsible for:

* Monitoring selected directories
* Detecting file operations
* Capturing suspicious events
* Passing events to the detection engine

### Process Monitor

Responsible for:

* Identifying processes
* Collecting process information
* Tracking suspicious processes
* Providing process information for investigation

### Behavior Analyzer

Responsible for:

* Evaluating observed activity
* Correlating multiple events
* Applying detection rules
* Calculating a suspiciousness/risk level

### Response Engine

Responsible for:

* Triggering response actions
* Terminating suspicious processes where appropriate
* Recording response actions
* Updating incident status

### Backend API

Provides communication between the EDR engine and frontend dashboard.

### Frontend Dashboard

Provides a visual interface for:

* Alerts
* Investigations
* Events
* Process information
* Response actions
* System status

---

## 🛠️ Technology Stack

### Backend

* **Python**
* **FastAPI**
* **psutil**
* **Watchdog**

### Frontend

* **React**
* **JavaScript**
* **HTML**
* **CSS**

### Database

* **MySQL / SQLite**

The final database choice may depend on the project deployment requirements.

### Development Tools

* VS Code
* Git
* GitHub
* Postman
* Windows/Linux test environment

---

## 📁 Project Structure

The project is organized into separate frontend and backend components.

```text
Behavior-Based-EDR/
│
├── frontend/
│   ├── src/
│   ├── public/
│   ├── package.json
│   └── ...
│
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── api/
│   │   ├── core/
│   │   ├── monitors/
│   │   ├── detection/
│   │   ├── response/
│   │   ├── decoys/
│   │   └── database/
│   │
│   ├── requirements.txt
│   └── ...
│
├── README.md
├── .gitignore
└── ...
```

> The structure may evolve as development progresses.

---

## 🔐 Detection Concept

The core idea is that ransomware generally performs file operations at a scale and speed that differ from normal user activity.

Instead of attempting to identify every possible ransomware sample, this project focuses on detecting **suspicious behavior**.

For example:

```text
Process starts
      ↓
Accesses decoy.xlsx
      ↓
Modifies decoy.xlsx
      ↓
Accesses another decoy
      ↓
Performs repeated file operations
      ↓
Detection conditions satisfied
      ↓
Generate alert
      ↓
Initiate response
```

This allows the system to focus on **observable behavior rather than only known malware signatures**.

---

## 🧪 Testing Approach

The system will be tested using controlled and safe simulations.

Testing scenarios may include:

### Normal Activity

A legitimate application accesses ordinary files without interacting with decoy traps.

Expected result:

```text
No ransomware alert
```

### Decoy File Access

A controlled test process accesses a decoy file.

Expected result:

```text
Decoy event recorded
Process identified
```

### Suspicious Repeated Activity

A controlled test generates repeated operations involving decoy files.

Expected result:

```text
Suspicious behavior detected
        ↓
Alert generated
        ↓
Investigation information collected
        ↓
Response triggered
```

Testing will be performed only in controlled environments.

---

## 📈 Expected Outcome

The completed system is expected to provide:

* Early detection of suspicious ransomware-like activity
* Behavior-based monitoring
* Decoy-based detection
* Process-level investigation
* Automated response capability
* Security event logging
* Real-time dashboard visibility

---

## 🎯 Project Objectives

1. Develop a lightweight endpoint detection and response mechanism.
2. Implement decoy files as early-warning indicators.
3. Monitor file-system activity for suspicious behavior.
4. Identify processes responsible for suspicious events.
5. Analyze behavior using configurable detection rules.
6. Implement an appropriate automated response mechanism.
7. Provide investigation details through a web dashboard.
8. Demonstrate the complete detection-to-response workflow.

---

## 🚀 Future Enhancements

Possible future improvements include:

* Machine-learning-based behavior classification
* Advanced anomaly detection
* Process tree visualization
* Network activity monitoring
* File entropy analysis
* Memory analysis
* Threat intelligence integration
* Endpoint isolation
* Multi-endpoint management
* Centralized security monitoring
* Advanced forensic investigation
* Improved ransomware family classification

---

## ⚠️ Limitations

This project is an academic prototype and is not intended to replace a production-grade EDR platform.

Possible limitations include:

* Limited operating-system support
* Dependence on monitoring permissions
* Rule-based detection limitations
* Possibility of false positives
* Limited visibility into kernel-level activity
* Controlled testing environment

---

## 👥 Team

**Project:** Behavior-Based EDR Engine for Zero-Day Ransomware Prevention via Decoy File Traps

**Project Type:** Academic Semester Project

### Team Members

* **Shaik Muskan** 
* **Pudi Pavani Priya** 

---

## 📜 Disclaimer

This project is developed for **educational and defensive cybersecurity purposes**.

Testing should only be performed on systems and files for which the user has explicit authorization.

The ransomware-related testing used during development should be performed using controlled simulations rather than real malware.

---

## ⭐ Project Vision

The goal of this project is to demonstrate how **behavior-based endpoint detection, decoy file traps, process monitoring, and automated response** can work together to identify suspicious ransomware-like activity at the endpoint level.

```text
        Detect → Investigate → Respond → Protect
```
