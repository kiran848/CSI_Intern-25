
# 🚀 Secure Hosting of Web App via Azure Hub-and-Spoke Architecture

This project demonstrates secure hosting of a Web App and a Storage Account on Azure using a **Hub-and-Spoke architecture**. It implements custom DNS resolution, Private Endpoints, Azure Firewall for traffic control, and simulates on-premises connectivity via a separate VNet. The solution ensures that no public access is allowed to the web app or storage, and all traffic is routed securely through Azure Firewall.

---

## 📘 Project Overview

- **Hub VNet**: Centralized components like Azure Firewall, DNS Forwarder VM, Azure Bastion.
- **Spoke VNet 1**: Contains Web App (with Private Endpoint).
- **Spoke VNet 2**: Contains Storage Account (with Private Endpoint).
- **On-Premises VNet**: Simulated environment with a Windows VM for DNS and HTTPS testing.
- **All traffic** to/from Web App and Storage is routed through **Azure Firewall**.
- **Custom DNS** implemented using a Linux-based DNS Forwarder (dnsmasq).

---

## 🧩 Components Summary

| Component          | Description                                |
|--------------------|--------------------------------------------|
| `rg-project`       | Resource Group for entire deployment       |
| `vnet-hub`         | Hub network with Firewall, DNS, Bastion    |
| `vnet-web`         | Spoke VNet hosting the Web App             |
| `vnet-storage`     | Spoke VNet hosting the Storage account     |
| `vnet-onprem`      | Simulated on-prem VNet with Windows VM     |
| `dns-vm`           | Linux VM with `dnsmasq` DNS forwarder      |
| `onprem-vm`        | Windows VM for validation and testing      |
| `hub-fw`           | Azure Firewall for routing & security      |
| `rt-web`, `rt-storage`, `rt-onprem` | UDRs for routing traffic via Firewall |

---

## ✅ Task Completion Table

| # | Task                                                                                 | Status   |
|---|--------------------------------------------------------------------------------------|----------|
| 1 | Create VNets (Hub, Web, Storage, On-Prem)                                           | ✅ Done   |
| 2 | Peer Hub to Spokes and On-Prem                                                      | ✅ Done   |
| 3 | Deploy Linux DNS Forwarder in Hub                                                   | ✅ Done   |
| 4 | Configure custom DNS across all VNets                                               | ✅ Done   |
| 5 | Deploy Web App and Storage Account with Private Endpoints                           | ✅ Done   |
| 6 | Test DNS Resolution from On-Prem and Spokes                                         | ✅ Done   |
| 7 | Deploy Azure Firewall and configure route tables                                    | ✅ Done   |
| 8 | Test access to Web App and Storage from On-Prem with SSL Warning (expected)         | ✅ Done   |

---

## 📁 Folder Structure for GitHub Repository

```
azure-secure-webapp/
│
├── 📁 architecture/
│   └── hub-spoke-diagram.png
│
├── 📁 dns-forwarder/
│   ├── dnsmasq.conf
│   └── DNS-test-output.txt
│
├── 📁 firewall/
│   ├── firewall-rules.json
│   └── udr-config-screenshots/
│
├── 📁 private-endpoints/
│   ├── webapp-private-endpoint.png
│   └── storage-private-endpoint.png
│
├── 📁 test-outputs/
│   ├── nslookup-results.txt
│   ├── curl-outputs.txt
│   └── tracert-results.txt
│
└── 📄 README.md (this file)
```

---

## 🔧 DNS Forwarding Configuration

The DNS Forwarding VM is a Linux VM with `dnsmasq` configured.

### Sample `/etc/dnsmasq.conf`

```conf
listen-address=127.0.0.1,10.0.3.4
bind-interfaces
server=168.63.129.16
```

### Verification from DNS-VM

```bash
nslookup webapp00-xxxxx.privatelink.azurewebsites.net 127.0.0.1
```

Expected: Resolves to private IP `10.x.x.x`.

---

## 🔐 Private Endpoints Setup

- **Web App**: Created with Private Endpoint in `vnet-web`. Public access disabled.
- **Storage Account**: Created with Private Endpoint in `vnet-storage`. Public access disabled.

```bash
nslookup storageprivatedemo.blob.core.windows.net
```

Expected: Resolves to `10.x.x.x`, not `20.x.x.x`.

---

## 🔥 Azure Firewall Setup

- **Deployed** in `vnet-hub` in a subnet named `AzureFirewallSubnet`.
- **UDRs created** for:
  - `vnet-web` subnet
  - `vnet-storage` subnet
  - `vnet-onprem` subnet

```text
Route: 0.0.0.0/0
Next hop: Virtual Appliance
Next hop IP: [Private IP of Azure Firewall]
```

### Firewall Verification

From on-prem VM, run:

```powershell
tracert 8.8.8.8
```

Expected: First hop is Azure Firewall IP.

---

## 🧪 Testing and Validation

### DNS Resolution

```powershell
nslookup webapp00-xxxxx.privatelink.azurewebsites.net
nslookup storageprivatedemo.blob.core.windows.net
```

### Curl from On-Prem VM

```powershell
curl https://10.1.1.4
```

Expected: SSL warning, but connection success (verifies private access).

---

## ❗ Issues & Observations

- SSL Certificate warning is expected for Private Web App.
- Public IP limit (3) used for Bastion, Firewall, and DNS-VM.
- Application Gateway and Power BI Dashboard not implemented due to IP constraints.

---

## 📝 Deployment Notes

- All deployment done via Azure Portal (manual).
- IPs are static for DNS, Firewall, Bastion to ensure consistent routing.
- Resources created in **Central US** region.

---

## 🎯 Learning Outcomes

- Implemented Hub-and-Spoke architecture in Azure.
- Configured Private Endpoints, Azure Firewall & DNS Forwarding.
- Ensured secure traffic routing with forced tunneling via UDRs.
- DNS traffic resolved privately from On-Prem to Azure resources.
- Validated deployment via `nslookup`, `curl`, and `tracert`.

---

## 👨‍💻 Author

**Kiran**  
Final Year BTech CSE | Azure Cloud Intern | Network Security Enthusiast  

---

## 🏷️ Tags

`#Azure` `#Networking` `#PrivateLink` `#Firewall` `#HubSpoke` `#DNSForwarder` `#WebAppSecurity`
