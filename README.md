# Digital-Twin-IDS
Master's Degree Thesis : Digital Twin-based Intrusion Detection System for Industry 4.0 Connected Sensor.

# 🛡️ Digital Twin-based Intrusion Detection System for Industry 4.0 Connected Sensor

**Author:** Ruchi Giradkar  
**Institution:** Deggendorf Institute of Technology  
**Supervisor:** Prof. Dr. Ing. Frank Denk  
**Date:** February 2021  

---

## 📘 Overview
This repository showcases my Master Thesis project, where I developed a **Digital Twin-based Intrusion Detection System (IDS)** to detect **ransomware and malware** activity in **Industry 4.0 connected sensor environments**.

The concept uses **Docker containerization** and **Digital Twin technology** to create a real-time comparison between an actual system and its identical digital replica. Any deviation between the two systems indicates possible malicious activity.

---

## 🎯 Goal
The main goal of this project was to **detect ransomware or malicious processes** running on Industry 4.0 connected sensors by comparing the runtime behavior of an **actual system** and its **digital twin**.

---

## 🧩 Technical Stack
- **Languages:** Python, Shell scripting  
- **Technologies:** Docker, Linux (Ubuntu), SQLite, Tcpdump  
- **Concepts:** Digital Twin, Intrusion Detection System (IDS), Industry 4.0 Security  
- **Networking:** TCP/IP, Command & Control (C&C) simulation  

---

## ⚙️ System Architecture
The project architecture includes three main containers and one IDS host system.

| Component | Description |
|------------|-------------|
| **Actual System Container** | Represents an Industry 4.0 sensor setup that runs real processes and sends live data. |
| **Digital Twin Container** | A virtual replica of the actual system, used as a baseline for comparison. |
| **C&C Server Container** | Simulates a ransomware command-and-control attack scenario. |
| **IDS Module (Host)** | Continuously compares data from both containers and raises alerts when deviations occur. |

**Working Principle:**  
The actual system and the digital twin run identical workloads. The IDS continuously monitors their process behavior and network activity. If any new or suspicious process appears in the actual system that does not exist in the digital twin, it is flagged as a potential intrusion.

---

## 🧠 Implementation Steps
1. Created identical Docker containers for the actual system and its digital twin.  
2. Monitored the running processes of both containers and stored the information in a database.  
3. Captured outgoing network traffic from the actual system for analysis.  
4. Compared both process and traffic data periodically to detect deviations.  
5. Generated alerts whenever suspicious behavior or additional processes were identified.  

---

## 🧪 Experimentation
To simulate a ransomware attack, a **C&C (Command & Control) server** was created to connect with the actual system.  
When this connection occurred, it introduced additional background processes, similar to how malware operates.  
The **IDS successfully identified** these deviations by detecting the extra processes on the actual system that were not present in the digital twin, thereby confirming an intrusion.

---

## 📊 Results
- Successfully detected ransomware-like intrusions in the simulated Industry 4.0 setup.  
- Raised real-time alerts before system encryption or compromise occurred.  
- Maintained forensic evidence of suspicious outbound connections and process deviations.  

---

## 🚀 Outcome
- Demonstrated a **generic and scalable IDS** based on Digital Twin technology.  
- Reduced false positives by comparing two synchronized systems instead of relying on fixed signatures.  
- Presented a solution that can easily integrate into **CI/CD pipelines** for industrial cybersecurity monitoring.  

---

## 🔭 Future Enhancements
- Integration with cloud-based monitoring dashboards (e.g., Grafana or ELK).  
- Support for additional IoT and industrial protocols (e.g., MQTT, OPC UA).  
- Automated deployment using Docker Compose or Kubernetes.  
- Application of Machine Learning for predictive anomaly detection.  

---

## 🖼️ Architecture Diagram
The architecture diagram illustrating the system setup and data flow is available at:  
**`/images/architecture_diagram.png`**  
*(You can add your exported diagram image from your thesis presentation here.)*

---

## 📁 Repository Structure
- **compare.py** – Main logic for comparing process data between actual system and digital twin.  
- **monitor_system.sh** – Script to automate periodic monitoring and comparison.  
- **Dockerfile** – Container setup for actual system, digital twin, and C&C server.  
- **example_data/** – Contains sample databases or logs used for testing.  
- **images/** – Contains architecture diagrams or result visuals.  
- **README.md** – Project documentation (this file).

## 📁 Repository Structure

| File/Folder | Description |
|--------------|-------------|
| **compare.py** | IDS logic for process comparison (reads SQLite databases and raises alerts). |
| **monitor_system.sh** | Script that automates periodic monitoring and comparison between the actual system and its digital twin. |
| **Dockerfile** | Base configuration for creating the Docker containers used for the actual system, digital twin, and C&C server. |
| **example_data/** | Folder containing example or generated databases for testing the IDS. |
| ├── *Actual_System.db* | SQLite database storing process logs from the actual system. |
| ├── *Digital_Twin.db* | SQLite database storing process logs from the digital twin system. |
| └── *Tcpdump_log.db* | Optional SQLite database with parsed network traffic captured for forensic analysis. |
| **images/** | Folder for storing visual materials (e.g., architecture diagrams or results). |
| └── *architecture_diagram.png* | Diagram representing the system setup and data flow (optional). |
| **README.md** | Project documentation containing overview, setup details, and reference information. |


---

## 🛠️ Quickstart (Concept)
1. Build and run identical containers for the actual system and its digital twin.  
2. Ensure both containers log their process and network data in the database.  
3. Run the IDS module to compare both datasets and detect deviations.  
4. Review alerts and network activity for confirmation of intrusion.  

---

## 📚 Reference
Giradkar, Ruchi. *Digital Twin-based Intrusion Detection System for Industry 4.0 Connected Sensor.*  
Master Thesis, Deggendorf Institute of Technology, 2021.  

---

## 💡 Summary
Developed a proof-of-concept Intrusion Detection System using **Digital Twin and Docker technologies** to detect ransomware in real time.  
The IDS compared live process and network data between an actual and virtual system to identify malicious activity, achieving **scalable, accurate, and real-time intrusion detection** for Industry 4.0 environments.

---

⭐ *If you found this project interesting or useful, feel free to connect or share feedback!*
