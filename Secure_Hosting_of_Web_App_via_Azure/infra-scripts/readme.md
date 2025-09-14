
# 🏗️ Infrastructure Scripts (infra-scripts)

## 📌 Overview

This folder contains deployment scripts and ARM/JSON templates for provisioning the **core infrastructure** required for the secure hosting project. These resources form the baseline **Hub-and-Spoke architecture**, simulated **On-Premises environment**, and PaaS service foundations (Web App + Storage).

---

## 📂 Folder Contents

| File Name                                 | Description                                                                                                          |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `ASP-rgproject-b321 Linux plan.json`      | Creates the **App Service Plan** (Linux) that hosts the Web App inside the designated resource group.                |
| `webapp00.json`                           | Deploys the **Azure Web App** configured to run under the above App Service Plan.                                    |
| `storageprivatedemo Storage account.json` | Creates the **Storage Account** (with private endpoint capability enabled).                                          |
| `vnet-hub.json`                           | Deploys the **Hub VNet** with required subnets, including `AzureFirewallSubnet` and `AzureFirewallManagementSubnet`. |
| `vnet-web.json`                           | Deploys the **Spoke VNet for Web App workloads**.                                                                    |
| `vnet-storage.json`                       | Deploys the **Spoke VNet for Storage Account workloads**.                                                            |
| `onpremisevnet.json`                      | Creates the **On-Premises Simulation VNet** to test cross-site connectivity.                                         |
| `onpremise-vm.json`                       | Provisions the **VM inside On-Prem VNet** for testing DNS, private endpoint, and routing scenarios.                  |
| `onpremise-vm-nic.json`                   | Configures the **Network Interface Card (NIC)** for the On-Prem VM, attaching it to the correct subnet.              |
| `onpremise-nsg.json`                      | Creates a **Network Security Group (NSG)** applied to the On-Prem VM subnet for traffic control.                     |
| `readme.md`                               | Documentation for this folder (you are reading it now).                                                              |

---

## 🚀 Deployment Steps

1. **Provision Hub VNet**

   ```bash
   az deployment group create \
     --resource-group <rg-name> \
     --template-file vnet-hub.json
   ```

2. **Deploy Spoke VNets**

   ```bash
   az deployment group create --resource-group <rg-name> --template-file vnet-web.json
   az deployment group create --resource-group <rg-name> --template-file vnet-storage.json
   ```

3. **Deploy On-Premises Simulation**

   ```bash
   az deployment group create --resource-group <rg-name> --template-file onpremisevnet.json
   az deployment group create --resource-group <rg-name> --template-file onpremise-vm.json
   az deployment group create --resource-group <rg-name> --template-file onpremise-vm-nic.json
   az deployment group create --resource-group <rg-name> --template-file onpremise-nsg.json
   ```

4. **Deploy PaaS Resources**

   * App Service Plan:

     ```bash
     az deployment group create --resource-group <rg-name> --template-file "ASP-rgproject-b321 Linux plan.json"
     ```
   * Web App:

     ```bash
     az deployment group create --resource-group <rg-name> --template-file webapp00.json
     ```
   * Storage Account:

     ```bash
     az deployment group create --resource-group <rg-name> --template-file "storageprivatedemo Storage account.json"
     ```

---

## ✅ Validation Checklist

* Hub and Spoke VNets are created and visible in Azure Portal.
* On-Prem simulation VNet and VM are up and reachable.
* Web App is deployed (public initially, later linked with private endpoint).
* Storage Account is deployed with private endpoint capability.
* NSG rules allow internal communication for testing.

---

## 🌱 Future Improvements

* Convert JSON templates into **Bicep** or **Terraform** for modular automation.
* Parameterize templates for **reusable deployments** across environments.
* Add **diagnostic settings** for VNets, NSGs, and VMs to send logs to Log Analytics.
* Integrate with **Azure Bastion** instead of exposing VM public IPs.

---

👉 This makes `infra-scripts` folder self-explanatory and usable.

