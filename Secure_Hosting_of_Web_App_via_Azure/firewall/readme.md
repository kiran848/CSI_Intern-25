
# 🔥 Azure Firewall Deployment

## 📌 Overview

This module contains the configuration and scripts for deploying **Azure Firewall** in the **Hub VNet** and applying **User Defined Routes (UDRs)** for secure traffic redirection. The firewall ensures all outbound traffic from spoke VNets and simulated on-premises environments is centrally inspected and filtered.

---

## 📂 Folder Contents

| File/Folder              | Description                                                                                                              |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| `screenshots/`           | Contains validation images (portal view, logs, test results) of Firewall deployment and routing.                         |
| `firewallip.json`        | JSON export capturing the **private IP** and details of the deployed Azure Firewall. Used for UDR configuration.         |
| `hub-fw.json`            | JSON template/config used to deploy the **Azure Firewall resource** into the Hub VNet (`vnet-hub`).                      |
| `udr-routes.json`        | Defines User Defined Routes (UDRs) to redirect **spoke VNet traffic → Firewall private IP**.                             |
| `udr-routes-onprem.json` | Defines UDRs for the **on-premises simulation VNet**, ensuring its outbound traffic is also routed through the Firewall. |
| `readme.md`              | Documentation for this folder (you are reading it now).                                                                  |

---

## 🚀 Deployment Steps

1. **Deploy Firewall in Hub**

   ```bash
   az network firewall create \
     --name hub-fw \
     --resource-group <rg-name> \
     --vnet-name vnet-hub \
     --sku AZFW_VNet
   ```

2. **Extract Firewall Private IP**

   * Export details to JSON:

     ```bash
     az network firewall show \
       --name hub-fw \
       --resource-group <rg-name> \
       --query "ipConfigurations[0].privateIpAddress" \
       > firewallip.json
     ```
   * Use this IP when configuring UDRs.

3. **Apply UDR Routes**

   * For spoke VNets:

     ```bash
     az network route-table import \
       --name rt-web \
       --resource-group <rg-name> \
       --file udr-routes.json
     ```
   * For on-prem simulation VNet:

     ```bash
     az network route-table import \
       --name rt-onprem \
       --resource-group <rg-name> \
       --file udr-routes-onprem.json
     ```

4. **Associate Route Tables**

   * Attach each route table to the corresponding subnet in the VNet (Web, Storage, On-prem).

---

## ✅ Validation

* From a VM in spoke or on-prem:

  ```bash
  tracert <external-domain>
  ```

  ➡️ Path should show next hop = **Firewall private IP**.

* Check **Firewall logs** in Azure Monitor for allowed/denied connections.

* Screenshots inside `screenshots/` folder confirm successful routing and firewall inspection.

---

## 🌱 Future Improvements

* Add **Application Rules** to restrict outbound traffic by FQDN.
* Configure **Network Rules** for protocol/port filtering.
* Enable **Threat Intelligence Mode** for real-time malicious IP/domain blocking.
* Integrate with **Azure Log Analytics** for SIEM monitoring.

---

👉 This makes the **firewall folder self-documented**.
