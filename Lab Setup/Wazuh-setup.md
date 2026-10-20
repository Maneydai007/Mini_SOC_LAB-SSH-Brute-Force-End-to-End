# WAZUH
Wazuh is a free, open-source security platform that combines SIEM (Security Information and Event Management) and XDR (Extended Detection and Response). It collects security data from your servers, endpoints, and cloud services, analyzes it, and alerts you to threats, misconfigurations, and compliance problems. It began as a fork of OSSEC and is now a much larger, actively maintained project.

## What it does
* Log analysis: collects and analyzes logs from systems, apps, and network devices using decoders and rules.
* File integrity monitoring (FIM): detects when important files or registry keys are created, changed, or deleted.
* Intrusion and malware detection: spots suspicious behavior, rootkits, and anomalies.
* Vulnerability detection: matches installed software against known CVEs.
* Configuration assessment (SCA): checks systems against benchmarks like CIS.
* Compliance reporting: maps alerts to standards such as PCI DSS, GDPR, HIPAA, and NIST 800-53.
* Active response: automatically takes action, such as blocking an IP, when a rule triggers.
* Cloud and container monitoring: supports AWS, Azure, GCP, Docker, and Kubernetes.

  
# Architecture
Wazuh has three central components plus agents:

| Component | Description |
| :--- | :--- |
| Wazuh agent | Lightweight software installed on monitored machines (Linux, Windows, macOS, etc.). Collects data and sends it to the server. |
| Wazuh server | Analyzes incoming data with rules and decoders, generates alerts, and manages agents. |
| Wazuh indexer | Stores and indexes alerts and events (based on OpenSearch). |
| Wazuh dashboard | Web UI for searching, visualizing, and managing alerts and agents. |   

# Data flow 

Wazuh Data Flow
1. Endpoints (Agents): Monitored systems (Windows, Linux, macOS, cloud) running a lightweight agent that collects logs, tracks file integrity, scans for vulnerabilities, and detects real-time threats.

2. Wazuh Server (Analysis): The central processing engine that receives encrypted agent data, parses it with decoders, and applies rulesets (Sigma/custom) to analyze events and trigger alerts.

3. Wazuh Indexer (Storage): An OpenSearch-based database that indexes and stores analyzed alerts and logs, enabling fast searching, filtering, and long-term retention.

4. Wazuh Dashboard (Visualization): The web user interface that queries the indexer to display security events, compliance reports, and analytics through interactive visual dashboards.

| Flow |
| :--- |
| Endpoints (agents) → Wazuh server (analysis) → Wazuh indexer (storage) → Wazuh dashboard (visualization) |   

# INSTALLING & SETUPING WAZUH
Here is the step-by-step guide to downloading, importing, configuring, and accessing the Wazuh OVA image in VMware Workstation Pro.
### Step 1: Download the Official Wazuh OVA

* Download the pre-built [Wazuh Virtual Machine (OVA)](https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html) file from the official Wazuh Documentation.
* Click on the link to download the Wazuh Virtual Machine (OVA) file.
   - **Note:** The file is approximately 2–3 GB and contains an operating system pre-packaged with the Wazuh Manager, Indexer, and Dashboard.

### Step 2: Import the OVA into VMware Workstation ProOpen VMware Workstation Pro. 
* Click File in the top menu bar, then click Open... (or press Ctrl + O).
* Browse to your Downloads folder, select the downloaded .ova file (e.g., wazuh-4.x.x.ova), and click Open.
* A prompt window titled Import Virtual Machine will appear:
   * Name for the new virtual machine: Set it to Wazuh-Server.
  * Storage path: Choose a location on your SSD with at least 50 GB of free space.
* Click Import. (If VMware shows an OVF specification compliance warning, simply click Retry to force the import process). 

### Step 3: Adjust Hardware Resources
Before booting the machine for the first time, allocate sufficient hardware resources:
* Right-click the newly imported Wazuh-Server VM in the left panel and select Settings (or click Edit virtual machine settings).
* Memory (RAM): Set to at least 4096 MB (4 GB) — 8192 MB (8 GB) is recommended for smoother indexing.
* Processors: Assign 4 vCPUs (or 2 Cores with 2 Processors).
*  Network Adapter: Set it to NAT (or the same custom LAN Segment / Host-Only network where your Attacker and Victim VMs are attached).
*   Click OK to save changes

### Step 4: Boot and Identify the IP Address
* Click Power on this virtual machine.
* Let the system boot up completely until you reach the command-line login prompt.
* Log in to the terminal using the default CLI credentials:
   * Username: wazuh-user
   * Password: wazuh

* Once logged in, run the following command to find the machine's assigned IP address:
  ```bash
  ip a
  ```
* Look under your network interface (usually eth0 or ens33) and note down the IPv4 address (e.g., 192.168.x.x).

### Step 5: Access the Wazuh Dashboard
* Open a web browser on your Host Machine (or inside your Kali Attacker VM if on the same network).
* Navigate to:
  ```bash
  https://<WAZUH_SERVER_IP>
  ```
  (Example: [https://192.168.172.128](https://192.168.172.128))
  * Your browser will show a "Your connection is not private / SSL Warning" because of the default self-signed certificate.
  * Click Advanced and select Proceed / Accept Risk and Continue.
  * Log in to the Web Interface using the default admin credentials:
       * Username: admin
       * Password: admin   
  
 
