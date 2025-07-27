# ☁️ Week 7 Assignment – Azure Internal & External Load Balancer Setup

This project demonstrates how to create and configure **Azure Load Balancers (Standard SKU)** — both **External** (with Public IP) and **Internal** (with Private IP) — to distribute HTTP traffic across multiple virtual machines running Apache Web Server.

---

## 🧩 Project Structure

```
📁 week7/
├── ext-lb (External Load Balancer)
├── int-lb (Internal Load Balancer)
├── Virtual machine
├── Vnet , subnets, NSG
└── README.md
```

---

## 🛠️ Step-by-Step Configuration Guide

### 🔹 1. Create Resource Group

```bash
az group create --name rg-week7 --location "East US"
```

---

### 🔹 2. Create Virtual Network and Subnets

- **VNet Name**: `vnet`
- **Address Space**: `10.0.0.0/16`
- **Subnets**:
  - `front-subnet`: `10.0.2.0/24` – for `vm3`, Bastion
  - `back-subnet`: `10.0.3.0/24` – for `vm1`, `vm2`

- **Network Security Groups (NSGs)**:
  - `nsg-front`: allow inbound SSH (22) for Bastion
  - `nsg-back`: allow inbound HTTP (80) for load balancer probes

---

### 🔹 3. Create NSG Rules

#### NSG: `nsg-front`
- **Inbound Rule**: Allow port `22` (SSH)
- **Priority**: 100
- **Protocol**: TCP

#### NSG: `nsg-back`
- **Inbound Rule**: Allow port `80` (HTTP)
- **Priority**: 100
- **Protocol**: TCP

---

### 🔹 4. Create VMs

| VM     | Subnet         | Public IP | Purpose                              |
|--------|----------------|-----------|--------------------------------------|
| `vm1`  | `back-subnet`  | ❌ No     | Apache Web Server                    |
| `vm2`  | `back-subnet`  | ❌ No     | Apache Web Server                    |
| `vm3`  | `front-subnet` | ❌ No     | Internal Load Balancer testing only  |
| Bastion| `front-subnet` | ✅ Yes    | SSH access to all VMs (via portal)   |

---

### 🔹 5. Install Apache on `vm1` and `vm2`

SSH into each via **Bastion**, then run:

```bash
sudo apt update
sudo apt install apache2 -y
echo "Hello from vm1" | sudo tee /var/www/html/index.html   # on vm1
echo "Hello from vm2" | sudo tee /var/www/html/index.html   # on vm2
```

---

### 🔹 6. Create External Load Balancer (`ext-lb`)

- **Frontend IP Config**: Public IP (static, standard)
- **Backend Pool**: Add `vm1`, `vm2`
- **Health Probe**:
  - Protocol: HTTP, Port: 80, Path: `/`
- **Load Balancing Rule**:
  - Name: `http-rule`
  - Frontend/Backend Port: 80
  - Session Persistence: None
- **Outbound Rule**:
  - Name: `httpoutbound`
  - Backend pool: `vm1`, `vm2`
  - Protocol: All
  - TCP Reset: Enabled

---

### 🔹 7. Test External Load Balancer

Copy the **public IP** of `ext-lb` and open in your browser:

```
http://<ext-lb-ip>
```

✅ You should see:
- "Hello from vm1"
- or "Hello from vm2"

---

### 🔹 8. Create Internal Load Balancer (`int-lb`)

- **Type**: Internal
- **Frontend IP**: Private IP `10.0.2.100`
- **Subnet**: `back-subnet`
- **Backend Pool**: Add `vm1`, `vm2`
- **Health Probe**: HTTP on port 80
- **Load Balancing Rule**:
  - Frontend Port: 80
  - Backend Port: 80
  - Session Persistence: None

---

### 🔹 9. Test Internal Load Balancer from `vm3`

SSH into `vm3` via Bastion and run:

```bash
curl http://10.0.2.100
```

✅ You should get:
- "Hello from vm1" or
- "Hello from vm2"

---

## 📈 Future Real-World Use Cases

### ✅ External Load Balancer

- **Web hosting**: Balances incoming traffic across multiple web servers
- **Failover & redundancy**: Keeps services running even if one VM fails
- **Auto-scaling backends**: Works with VMSS (Virtual Machine Scale Sets)

### ✅ Internal Load Balancer

- **Private microservices**: Services that must remain within a secure VNet
- **Database load balancing**: Across replicas or read-only nodes
- **App tiers communication**: Internal apps (e.g., API gateways ↔ backend servers)

---

## 📘 Notes

- Free trial allows max **3 public IPs**
- Use **Azure Bastion** to avoid extra public IPs
- All VMs must be in the **same VNet and region** for backend pool association

---

## 🙌 Author

**Kittu K.**  
B.Tech CSE (AI & DL)  
Azure Cloud Intern | CSI-Intern 2025
