# Digital-Twin-IDS
Master's Degree Thesis : Digital Twin-based Intrusion Detection System for Industry 4.0 Connected Sensor.

# 🛡️ Digital Twin-based Intrusion Detection System for Industry 4.0 Connected Sensor

**Author:** Ruchi Giradkar  
**Institution:** Deggendorf Institute of Technology  
**Supervisor:** Prof. Dr. Ing. Frank Denk  
**Date:** February 2021  

---

## 📘 Overview
This project is part of my Master Thesis, where I developed a **Digital Twin-based Intrusion Detection System (IDS)** for detecting **ransomware and malware** activity in **Industry 4.0 connected sensor environments**.  
It uses **Docker containerization** and **Digital Twin concepts** to identify anomalies in real-time through process and network monitoring.

---

## 🎯 Goal
To **detect ransomware or malicious processes** on Industry 4.0 connected sensors by comparing the runtime behavior of an **actual system** with its **digital twin**.

---

## 🧩 Technical Stack
- **Programming Languages:** Python, Shell scripting  
- **Technologies:** Docker, Linux, SQLite, Tcpdump  
- **Concepts:** Digital Twin, Intrusion Detection System (IDS), Industry 4.0, Network Security  
- **Networking:** TCP/IP, Command & Control (C&C) simulation  

---

## ⚙️ System Architecture
The architecture consists of three main containers and an IDS host:

| Component | Description |
|------------|-------------|
| 🖥️ **Actual System Container** | Mimics an Industry 4.0 sensor (Ubuntu + Apache + SSH) |
| 🧭 **Digital Twin Container** | Identical replica used for comparison |
| 💀 **C&C Server Container** | Simulates a ransomware Command & Control attack |
| 🧠 **IDS Module** | Python script that compares process and traffic logs every few seconds |

**Working Principle:**
1. The **actual system** and its **digital twin** run identical workloads.  
2. The IDS continuously monitors both containers’ **process lists** (via `top`) and **network traffic** (via `tcpdump`).  
3. If a ransomware or malware attack triggers new background processes or suspicious outbound traffic,  
   the IDS identifies the deviation and triggers an **intrusion alert**.

---

## 🧪 Implementation Steps
1. **Create identical Docker containers**
   ```bash
   docker build -t sensor_image .
   docker run -itd --name actual_system sensor_image
   docker run -itd --name digital_twin sensor_image
