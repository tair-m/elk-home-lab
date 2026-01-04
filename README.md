# :microscope: ELK Stack v9.2.2 & Suricata IDS Home Lab

## Project Overview
This repository contains ELK (Docker) configuration files and methodological materials for deploying a personal Home SIEM Lab. The project demonstrates the integration of a network IDS sensor and endpoint monitoring into a unified log analysis system.

> During the development of this project, I relied on the architectural principles described in the elkninja / elastic-stack-docker-part-two guide, adapting them for the current v9.2.2.

### What's Included in This Repository

I have shared only the parts of the code that were customized for the laboratory environment:

* docker-compose.yml, kibana.yml, .env — pre-configured stack for v9.2.2.
* Methodology: A detailed description of the component integration process.

### Architectural Solution
* **System Core (Docker):** Elasticsearch, Kibana, and Fleet Server are deployed in containers to ensure modularity and easy updates.
* **IDS Sensor (Native Windows):** Suricata is installed directly on the host OS, allowing for direct interaction with the network interface via Npcap. This solution guarantees packet capture without loss or conflicts with Docker virtual networks.
* **Hybrid VirtualBox Environment:**
    * **Kali Linux:** The primary tool for launching attacks and testing signatures.
    * **Ubuntu Server:** An additional monitoring node with Suricata and Elastic Agent installed.
    * **VulnHub VMs:** Vulnerable machines used as targets to practice detecting real-world exploits.

### Technology Stack

**1. SIEM Core (Containerized)**

> Central data processing and visualization hub deployed in an isolated environment:

* Elastic Stack v9.2.2 (Docker): Elasticsearch for index storage and Kibana for analytics.
* Fleet Server: Management of agent lifecycles and data collection policies.
* Docker Desktop: Container orchestration environment for the SIEM core.

**2. Detection & Monitoring Layer (Host & Agents)**

> Components responsible for threat detection at the network traffic and endpoint levels:

* Suricata IDS:

    * Windows Native: Installed on the host system for direct network interface access (via Npcap) and VirtualBox traffic analysis.
    * Ubuntu Server: An additional sensor within the virtual network.

* ET Open Ruleset: A signature-based analysis rule set from Emerging Threats.
* Elastic Agent (Fleet-managed): Agents for collecting system logs and Suricata eve.json events.

**3. Attack & Lab Environment (Virtualization)**
> Isolated environment for traffic generation and exploit testing:

* VirtualBox: Hypervisor for running lab environments.
* Kali Linux: Toolset for conducting penetration tests.
* VulnHub Target Machines: Target vulnerable systems.
* Ubuntu 22.04 LTS: Auxiliary server node.

---

### Step-by-Step Guide

1. Подготовка SIEM (Docker)

    * Deploy the stack: docker-compose up -d.
    * Activate Fleet Server in the Kibana interface and create data collection policies (refer to elkninja and Evermight Systems YouTube channels for detailed instructions).
  
<p align="center">
  <img src="img/1-Docker.png" width="1200" title="Docker">
</p>

<p align="center">
  <img src="img/2-Fleet-server.png" width="1200" title="Fleet-server">
</p>

2. Sensor Configuration (Windows Host)

    * Install Suricata and Npcap on the host system (refer to Hacker Sploit YouTube channel and the official Suricata documentation for details).
    * Enable the ET Open ruleset for up-to-date threat signatures.
    * Verify that logs are being generated in eve.json format (default path: C:\Program Files\Suricata\log).
    * Install Elastic Agent on Windows and enroll it into the Fleet Server.
  
<p align="center">
  <img src="img/3-Fleet-Agents.png" width="1200" title="Fleet-Agents">
</p>

> I have successfully configured both Windows and Suricata integrations.

<p align="center">
  <img src="img/4-Fleet-Windows.png" width="1200" title="Fleet-Windows">
</p>

4. VirtualBox Infrastructure

    * Configure a Host-only Network in VirtualBox and enable the DHCP Server.
    * Install Kali Linux and Ubuntu Server. Configure network adapters for these VMs (Kali, Ubuntu) in Bridged mode and Host-only Network with Promiscuous Mode set to "Allow All" so that Suricata can intercept their traffic.
    * Install Elastic Agent on Ubuntu and enroll it into the Fleet Server.
    * Deploy vulnerable virtual machines from Vulnhub. Set their network adapters to the Host-only Network with Promiscuous Mode enabled.

<p align="center">
  <img src="img/5-Fleet-Ubuntu.png" width="1200" title="Fleet-Ubuntu">
</p>

<p align="center">
  <img src="img/6-VirtualBox.png" width="1200" title="VirtualBox">
</p>

<p align="center">
  <img src="img/7-NetworkSettings.png" width="1200" title="NetworkSettings">
</p>

<p align="center">
  <img src="img/8-DHCP-Settings.png" width="1200" title="DHCP-Settings">
</p>

6. Analytics and Testing

    * Perform a network scan (e.g., using nmap) from Kali Linux against a target vulnerable machine.
    * Verify that alerts are being triggered and displayed in Kibana.
    * Configure Dashboards for effective incident visualization and monitoring.

<p align="center">
  <img src="img/9-Analytics.png" width="1200" title="Analytics">
</p>

<p align="center">
  <img src="img/10-Analytics.png" width="1200" title="Analytics">
</p>
