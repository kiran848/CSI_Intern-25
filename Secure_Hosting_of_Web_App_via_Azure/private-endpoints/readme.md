
# 🔒 Private Endpoints

## 📌 Overview

This folder contains deployment templates and configuration files for setting up **Azure Private Endpoints** for both the Web App and Storage Account. These private endpoints allow secure access to PaaS resources using **private IP addresses** from VNets, while also integrating with **Private DNS Zones** for name resolution.

The goal is to ensure that:

* The Web App (`*.azurewebsites.net`) and Storage (`*.blob.core.windows.net`) resolve to **10.x.x.x private IPs**.
* All communication to these services stays inside the **Hub-and-Spoke network** and flows through the **Azure Firewall**.

---

## 📂 Folder Contents

| File Name                                                 | Description                                                                                      |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| `screenshot/`                                             | Contains validation screenshots (DNS resolution, portal views, test results).                    |
| `wepapp-pe.json`                                          | Deploys the **Private Endpoint** for the Web App, binding it to the Web VNet subnet.             |
| `wepapp-pe-nic.json`                                      | Configures the **NIC resource** automatically created for the Web App Private Endpoint.          |
| `storage-pe.json`                                         | Deploys the **Private Endpoint** for the Storage Account, binding it to the Storage VNet subnet. |
| `storage-pe-nic.json`                                     | Configures the **NIC resource** automatically created for the Storage Private Endpoint.          |
| `privatelink.azurewebsites.net Private DNS zone.json`     | Creates a **Private DNS Zone** for Web App private endpoints.                                    |
| `privatelink.blob.core.windows.net Private DNS zone.json` | Creates a **Private DNS Zone** for Storage Account private endpoints.                            |
| `readme.md`                                               | Documentation for this folder (you are reading it now).                                          |

---

## 🚀 Deployment Steps

1. **Deploy Web App Private Endpoint**

   ```bash
   az deployment group create \
     --resource-group <rg-name> \
     --template-file wepapp-pe.json
   ```

2. **Deploy Storage Private Endpoint**

   ```bash
   az deployment group create \
     --resource-group <rg-name> \
     --template-file storage-pe.json
   ```

3. **Create Private DNS Zones**

   * For Web App:

     ```bash
     az deployment group create \
       --resource-group <rg-name> \
       --template-file "privatelink.azurewebsites.net Private DNS zone.json"
     ```
   * For Storage Account:

     ```bash
     az deployment group create \
       --resource-group <rg-name> \
       --template-file "privatelink.blob.core.windows.net Private DNS zone.json"
     ```

4. **Link DNS Zones to VNets**

   * Associate the Private DNS Zones with `vnet-web`, `vnet-storage`, and `onpremisevnet` so name resolution works across all environments.

---

## ✅ Validation Checklist

* From On-Premises VM or Spoke VM, run:

  ```bash
  nslookup <yourwebapp>.azurewebsites.net
  ```

  ➡️ Returns **10.x.x.x private IP**.

* Run:

  ```bash
  nslookup <yourstorage>.blob.core.windows.net
  ```

  ➡️ Returns **10.x.x.x private IP**.

* Test internal connectivity:

  ```bash
  curl https://<yourwebapp>.azurewebsites.net
  ```

  ➡️ Should respond internally (SSL warning expected).

* Public internet should **fail** to access these resources.

---

## 🌱 Future Improvements

* Automate zone linking across VNets using scripts.
* Add Private Endpoints for additional services like **Azure SQL Database, Key Vault, App Config**.
* Apply **DNS Forwarding Rulesets** (Azure DNS Private Resolver) instead of DNS VM for managed forwarding.
* Monitor private endpoint traffic using **NSG Flow Logs** or **Firewall Diagnostic Logs**.

---
