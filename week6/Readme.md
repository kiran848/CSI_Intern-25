
# 🌐 Week 6 – Azure 3-Tier Architecture Deployment (CSI Internship)

---

## 🔰 Introduction for Beginners

In modern cloud-based applications, separating services into **tiers** (web, app, and database) is a foundational architecture pattern. This enhances **security, scalability**, and **manageability**.

This project implements a **3-tier network architecture in Microsoft Azure** using both **Linux** and **Windows** virtual machines.

Even if you're new to Azure, this walkthrough is beginner-friendly — showing step-by-step how to use the Azure Portal to create networks, VMs, and security groups.

---

## 🎯 Objective

Deploy and secure a **3-tier cloud environment** in Azure:

- 🧩 **Web Tier** – Accessible from the internet (HTTP/HTTPS)
- 🔧 **App Tier** – Internal processing layer
- 🔒 **DB Tier** – Backend tier not directly accessible from outside

Each tier contains:
- 1 **Linux VM** running Apache
- 1 **Windows VM** running IIS

---

## 🧱 Architecture Overview

```plaintext
             Internet
                 ↓
          ┌────────────┐
          │  Web Tier  │  ← Apache + IIS (Linux + Win)
          └────────────┘
                 ↓
          ┌────────────┐
          │  App Tier  │  ← Internal tier for logic
          └────────────┘
                 ↓
          ┌────────────┐
          │  DB Tier   │  ← No internet, only accessible from App
          └────────────┘
```

---

## 📍 Subnet & VM Design

| Tier     | Subnet       | Address Range  | VMs Created        |
|----------|--------------|----------------|--------------------|
| Web      | web-subnet   | 10.0.2.0/24     | `web-lnx`, `web-win` |
| App      | app-subnet   | 10.0.3.0/24     | `app-lnx`, `app-win` |
| DB       | db-subnet    | 10.0.4.0/24     | `db-lnx`, `db-win`   |

---

## 🧠 Step-by-Step Guide (for Beginners)

---

### ✅ Stage 1: Create VNet + Subnets

1. Go to **Azure Portal > Virtual Networks > + Create**
2. Set:
   - Name: `vnet-week6`
   - Resource Group: `celebal-rg` (or create new)
   - Region: `East US`
3. Go to **IP Addresses > Subnets**
   - Add:
     - `web-subnet` → `10.0.2.0/24`
     - `app-subnet` → `10.0.3.0/24`
     - `db-subnet`  → `10.0.4.0/24`
4. Click **Review + Create**

---

### ✅ Stage 2: Create NSGs

1. Azure Portal > **Network Security Groups > + Create**
2. Create:
   - `nsg-web`
   - `nsg-app`
   - `nsg-db`
3. Go to:
   - Virtual Network > Subnets > Assign NSGs:
     - `web-subnet` → `nsg-web`
     - `app-subnet` → `nsg-app`
     - `db-subnet`  → `nsg-db`

---

### ✅ Stage 3: Add NSG Rules

| NSG     | Direction | Source        | Port        | Action |
|---------|-----------|---------------|-------------|--------|
| `nsg-web` | Inbound   | Internet       | 80,443,22,3389 | Allow  |
|         | Outbound  | 10.0.3.0/24   | *           | Allow  |
| `nsg-app` | Inbound   | 10.0.2.0/24   | *           | Allow  |
|         | Outbound  | 10.0.4.0/24   | *           | Allow  |
| `nsg-db`  | Inbound   | 10.0.3.0/24   | 22,80       | Allow  |
|         | Inbound   | 10.0.2.0/24   | *           | Deny   |

---

### ✅ Stage 4: Create VMs

1. Go to **Virtual Machines > + Create**
2. Create 6 VMs:
   - 1 Linux + 1 Windows for each tier
3. In **Networking tab**:
   - Choose appropriate subnet
   - Enable Public IP only for Web Tier
   - Do **not** assign NSG here (already assigned to subnet)

---

### ✅ Stage 5: Install Web Servers

#### 🔹 On Linux (Apache)

```bash
sudo apt update
sudo apt install apache2 -y
echo "<h1>This is Apache on $(hostname)</h1>" | sudo tee /var/www/html/index.html
```

> On `db-lnx`, no internet: use `python3 -m http.server`

#### 🔹 On Windows (IIS via PowerShell)

```powershell
Install-WindowsFeature -Name Web-Server -IncludeManagementTools
Set-Content -Path "C:\inetpub\wwwroot\index.html" -Value "<h1>This is IIS on $(hostname)</h1>"
```

---

## 🧪 Validation Tests

- ✅ `web-lnx` / `web-win` → Accessible from browser
- ✅ `app-lnx` → curl to `db-lnx`
- ❌ `web-lnx` → cannot access DB (as expected)
- ✅ `db-win` IIS working via localhost
- ✅ All NSG rules tested

---

## ⚠️ Errors Faced & Fixes

| Error                             | Cause                                     | Solution                              |
|----------------------------------|-------------------------------------------|----------------------------------------|
| `QuotaExceeded` (vCPUs = 4 max)  | Free tier limitation                      | Created 4 VMs only, reused for setup   |
| Apache failed (no internet)      | DB subnet has no outbound internet        | Used `python3 -m http.server` instead  |
| SCP failed from app → db         | NSG blocked port 22                       | Allowed SSH in `nsg-db` from App CIDR  |
| IIS 401 Unauthorized             | Windows auth misconfigured                | Manually opened HTML or used `curl`    |

---

## 📁 Folder Structure

```
week6/
├── README.md
├── scripts/
│   ├── install-apache.sh
│   └── install-iis.ps1
├── screenshots of each /
```

---

## 📜 Scripts

### `install-apache.sh`
```bash
#!/bin/bash
sudo apt update
sudo apt install apache2 -y
echo "<h1>This is Apache on $(hostname)</h1>" | sudo tee /var/www/html/index.html
```

### `install-iis.ps1`
```powershell
Install-WindowsFeature -Name Web-Server -IncludeManagementTools
Set-Content -Path "C:\inetpub\wwwroot\index.html" -Value "<h1>This is IIS on $(hostname)</h1>"
```

---

## 🎓 Learning Outcomes & Real-World Relevance

- 📡 **VNet/Subnet segmentation** improves security & isolation
- 🔐 **NSG rules** mirror firewall policies used in enterprise networks
- 🐧 Learned how to deploy and secure **Linux + Windows workloads**
- 🌍 No Internet DB = production-like backend setup
- 🔄 This architecture is directly used in **web applications**, **e-commerce**, **ERP**, **banking apps**

> 💼 **Real-world value**: You can now design cloud infrastructure like a DevOps/Cloud Engineer!


---

## 🏁 Conclusion

This assignment strengthened my understanding of:
- Azure networking (VNet, Subnets, NSGs)
- Cross-tier communication
- Public vs. private access control
- Installing and testing web servers on real cloud VMs

> ✅ **Successfully completed and tested Week 6 — ready for review.**
