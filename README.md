# Digital-Twin-IDS
Master's Degree Thesis : Digital Twin-based Intrusion Detection System for Industry 4.0 Connected Sensor.

# 🛡️ Digital Twin-based Intrusion Detection System for Industry 4.0 Connected Sensor

**Author:** Ruchi Giradkar  
**Institution:** Deggendorf Institute of Technology  
**Supervisor:** Prof. Dr. Ing. Frank Denk  
**Date:** February 2021  

---

## 📘 Overview
This repository showcases my Master Thesis project: a **Digital Twin-based Intrusion Detection System (IDS)** that detects **ransomware/malware** activity in **Industry 4.0 connected sensor** environments.  
The core idea is simple and scalable: run an **actual system** and its **digital twin** as identical Docker containers, continuously **compare their process behavior and network traffic**, and **alert** on deviations that indicate compromise.

---

## 🎯 Goal
Detect ransomware or other malicious processes on Industry 4.0 connected sensors by **comparing runtime behavior** (process lists and network activity) of an **actual system** versus its **digital twin** in near real time.

---

## 🧩 Technical Stack
- **Languages:** Python, Shell scripting  
- **Technologies:** Docker, Linux (Ubuntu), SQLite, tcpdump  
- **Security Concepts:** Digital Twin, IDS (HIDS+NIDS hybrid idea), Industry 4.0 security  
- **Networking:** TCP/IP, C&C (Command & Control) simulation

---

## ⚙️ System Architecture

| Component | Description |
|---|---|
| 🖥️ **Actual System Container** | Mimics an Industry 4.0 sensor (Ubuntu + SSH + Apache). Process list + outgoing traffic logged to SQLite. |
| 🧭 **Digital Twin Container** | Identical replica used as the “ground truth” baseline. Process list logged to SQLite. |
| 💀 **C&C Server Container** | Simulates a ransomware Command & Control (malicious peer) to create a realistic intrusion signal. |
| 🧠 **IDS Module (Host)** | Python + shell scripts that read both databases every few seconds and raise an alert if the actual system deviates from the twin. |

**Working Principle (high-level):**
1. Run **two identical containers** (Actual & Twin) so their normal background process lists match.
2. Continuously gather **process lists** (via `top`) and store them in **SQLite**.
3. Capture **outgoing traffic** from the Actual container using **`tcpdump`** for forensics.
4. The **IDS** compares the two sets of data periodically.  
5. A **deviation** (e.g., an **extra process** on Actual) triggers an **alert** → likely intrusion.

---

## 🧪 Implementation Steps

1. **Create identical Docker containers**
   ```bash
   # Build a base image that includes Ubuntu, Python, procps (for top), tcpdump, etc.
   docker build -t sensor_image .

   # Start Actual System and Digital Twin from the same image
   docker run -itd --name actual_system sensor_image
   docker run -itd --name digital_twin  sensor_image

   # (Optional) Start a C&C server container to simulate ransomware traffic later
   docker run -itd --name cnc_server sensor_image
Monitor processes (Actual & Twin) and store into SQLite

Inside each container, run top periodically (via a small script/cron) and export the PIDs.

On the host (IDS), ingest the PID lists into:

example_data/Actual_System.db

example_data/Digital_Twin.db

Example idea (conceptual, your scripts may differ):

bash
Copy code
# Inside each container (pseudo):
top -b -n 1 | awk '{print $1}' > /shared/top_pids.txt
# A host-side Python or shell task reads /shared/top_pids.txt -> inserts into SQLite
Capture outgoing traffic from the Actual System using tcpdump

bash
Copy code
# On the host, attach to the Docker bridge/network used by actual_system
# and write packets to a pcap; parse & store summary rows in SQLite for forensics.
sudo tcpdump -i any host <ACTUAL_SYSTEM_IP> -w example_data/actual_system_traffic.pcap
# A small parser can populate example_data/Tcpdump_log.db with timestamp, dst IP/port, etc.
Compare databases every few seconds using a Python script (compare.py)

The IDS reads the PID sets from Actual_System.db and Digital_Twin.db.

If Actual - Twin is non-empty, log and print an ALERT with the suspicious PIDs.

(Optionally) correlate with the tcpdump DB to show suspicious outbound connections.

Example run:

bash
Copy code
# From repo root
python3 compare.py
Alerting behavior

No deviation:

csharp
Copy code
[OK] Systems are monitored. No deviation detected.
Deviation detected:

yaml
Copy code
[ALERT] Suspicious PIDs detected in Actual System: [4444, 5555]
(Optional) Also print the most recent outbound connections around the detection time.

🧠 Experimentation
To simulate a ransomware-like event:

Run a C&C server container and establish a socket connection to the Actual System.

This connection causes the Actual System to spawn extra background processes (simulated “malware”).

The Digital Twin remains clean (no connection), so its process list does not include those PIDs.

The IDS identifies the deviation between Actual and Twin → alerts.

📊 Results
✅ Successfully detected a ransomware-like intrusion by identifying extra processes created after C&C connection.

✅ Raised a real-time alert before encryption or further compromise.

✅ Maintained a forensic trail of suspicious outbound connections via tcpdump.

🚀 Outcome
Built a generic, scalable intrusion detection approach using digital twins and containers.

Lower false positives by comparing two synchronized systems instead of relying only on static signatures.

Naturally fits CI/CD—when software updates, rebuild both containers so the twin remains an accurate baseline.

🔭 Future Enhancements
Dashboard & cloud-based monitoring (Grafana/ELK).

Support for industrial protocols (MQTT, OPC UA).

Docker Compose/Kubernetes automation.

ML-based anomaly scoring on processes + traffic metadata.

🖼️ Architecture Diagram
Add an image at:

bash
Copy code
/images/architecture_diagram.png
(Export a diagram from your thesis PPT and place it there. The README will render it automatically if linked below.)

markdown
Copy code
![Architecture Diagram](images/architecture_diagram.png)
📁 Repository Structure
graphql
Copy code
📦 DigitalTwin-IDS
├── compare.py                  # IDS logic for process comparison (reads SQLite DBs and alerts)
├── monitor_system.sh           # Starter script for periodic comparisons / background monitoring
├── Dockerfile                  # Base image for Actual/Twin/C&C containers
├── example_data/
│   ├── Actual_System.db        # SQLite DB with Actual System process logs (sample/demo or generated)
│   ├── Digital_Twin.db         # SQLite DB with Digital Twin process logs (sample/demo or generated)
│   └── Tcpdump_log.db          # SQLite DB (optional) with parsed tcpdump metadata
├── images/
│   └── architecture_diagram.png
└── README.md
🛠️ Quickstart (Demo Flow)
This demo assumes you’ll add simple placeholder scripts that populate the SQLite DBs with sample PIDs.

bash
Copy code
# 1) Build and run containers
docker build -t sensor_image .
docker run -itd --name actual_system sensor_image
docker run -itd --name digital_twin  sensor_image
# (Optional) docker run -itd --name cnc_server sensor_image

# 2) Ensure example_data/ contains demo or generated DB files (Actual_System.db, Digital_Twin.db).
#    You can start with demo data and then replace with real logs later.

# 3) Run the comparator (alerts on deviations)
python3 compare.py

# 4) (Optional) Start background monitoring
chmod +x monitor_system.sh
./monitor_system.sh
📚 Reference
Giradkar, Ruchi. Digital Twin-based Intrusion Detection System for Industry 4.0 Connected Sensor.
Master Thesis, Deggendorf Institute of Technology, 2021.

💡 Summary (Ready for recruiters)
Developed an IDS that leverages digital twin technology and Docker to detect ransomware by comparing process and network behavior between a live system and its replica.
Validated the approach using a simulated C&C attack, achieving real-time anomaly detection and improved resilience for Industry 4.0 environments.

⭐ If you find this project interesting, I’d love your feedback!
