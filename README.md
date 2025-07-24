
# ☁️ Celebal Technologies – Cloud Infra & Security Internship `2025`
> 📁 **Weekly Assignment Repository**  
> 🧑‍💻 Domain: Cloud Infrastructure & Security | 🔐 Platform: Microsoft Azure  
> 🏢 Organization: **Celebal Technologies**  
> 📆 Duration: [June–Aug 2025]

---

## ✨ Overview

This repository documents all **8 weekly assignments** completed during my internship, covering in-depth **R&D**, **Azure-based hands-on work**, and foundational **networking concepts**.

Each week explores essential components of building and securing **cloud-native infrastructure** using Azure's core services. 📡

---

## 📅 Weekly Assignments Summary

### 📘 Week 1: OSI & Network Protocols
🔹 OSI Model – 7 Layers  
🔹 TCP/IP Stack – Functionality & Layers  
🔹 Protocols Explored:
- 🧵 TCP vs UDP
- 🌐 HTTP & HTTPS
- 📶 ICMP and its role in diagnostics

---

### 📗 Week 2: IP Addressing, Subnetting, and MAC
🔹 IPv4 & IPv6 structures  
🔹 Subnetting (Classful, CIDR, VLSM)  
🔹 Calculated usable hosts, subnets  
🔹 MAC Address 🆚 IP Address  
🔹 Protocols: ARP & RARP

---

### 📙 Week 3: Azure Global Infrastructure 🌍
🔹 Azure Geographies, Regions & Availability Zones  
🔹 Azure Data Centers & their global distribution  
🔹 Fault tolerance & replication across regions

---

### 📕 Week 4: Azure VNet, Subnets & Peering 🔗
🔹 CIDR planning for address space  
🔹 Created a VNet with 2 subnets  
🔹 Deployed Windows & Linux VMs in subnets  
🔹 Enabled ping (ICMP) between them  
🔹 Established VNet Peering between two VNets  
📸 Step-by-step configuration with screenshots

---

### 📒 Week 5: NSGs, ASGs, Public IPs & NICs 🔐
🔹 Created & managed Network Security Groups (NSG)  
🔹 Configured ASGs to group VM rules  
🔹 Static vs Dynamic Public IPs  
🔹 Used Service Tags in NSG Rules  
🔹 Created & associated NICs with VMs  

---

### 📓 Week 6: 3-Tier Architecture Deployment 🏗️
🔹 Subnets Created:
- 🌐 Web Tier
- 🖥️ App Tier
- 🗄️ DB Tier

🔹 Access Rules:
- DB tier ❌ Cannot access others
- App tier ✅ Access to DB + Web
- Web tier 🌍 Internet access only

🔹 Deployed:
- 🐧 Linux VMs (Apache Web Server)
- 🪟 Windows VMs (IIS Server)

---

### 📔 Week 7: Load Balancing in Azure ⚖️
🔹 Created:
- 🌐 External Load Balancer
- 🛡️ Internal Load Balancer

🔹 Setup:
- Backend Pools
- Health Probes 🩺
- Load Balancing Rules

✅ Verified Load Distribution across multiple VMs

---

### 📚 Week 8: VPN Connectivity 🌐🔐
🔹 R&D on **Point-to-Site VPN**:
- Certificate-based authentication
- Secure remote access setup

🔹 R&D on **Site-to-Site VPN**:
- Simulated On-Prem setup using Hyper-V
- Routing across on-prem and Azure VNets

---

## 🧰 Tools & Technologies Used

| Tool | Description |
|------|-------------|
| ☁️ Microsoft Azure | Main cloud platform |
| 🧱 Virtual Network (VNet), Subnet | Network segmentation |
| 🔐 NSG, ASG, Public IP | Access control |
| ⚖️ Load Balancer | Traffic distribution |
| 🧑‍💻 Linux & Windows VMs | Deployment targets |
| 🛜 Hyper-V | Site-to-site VPN simulation |

---

## 🗂️ Repository Folder Structure

```bash
.
├── Week1_OSI_TCP_IP/
├── Week2_IP_Subnetting_ARP/
├── Week3_Azure_Global_Infra/
├── Week4_VNet_Subnets_Peering/
├── Week5_NSG_PubIP_NIC_ASG/
├── Week6_3Tier_Network_VMs/
├── Week7_LoadBalancers/
├── Week8_P2S_S2S_VPN/
└── README.md
