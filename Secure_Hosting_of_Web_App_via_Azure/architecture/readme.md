

## 🏗️ Architecture Overview

The project uses a **Hub-and-Spoke network topology** in Azure to ensure **secure, private, and controlled access** to Web App and Storage resources.

![Hub-and-Spoke Architecture](architecture/Architecture model.png)

### 🔹 Key Components

1. **On-Premises VNet** 🖥️

   * Simulated on-premises environment.
   * Connects to Azure Hub VNet via **VPN / ExpressRoute** for secure communication.

2. **Hub VNet** 🏢

   * **Azure Firewall (10.0.1.4)** 🔥: Centralized security and traffic control.
   * **DNS VM (10.0.3.4)** 🟦: Handles private DNS resolution across VNets using `dnsmasq`.

3. **Spoke VNets**

   * **Web VNet** 🌐: Contains `web-subnet` with **Private Endpoint** for the Web App.
   * **Storage VNet** 💾: Contains `storage-subnet` with **Private Endpoint** for the Storage Account.

### 🔹 Traffic Flow

* All traffic between Spoke VNets and On-Premises VNet flows **through the Hub VNet**, enforcing security policies via Azure Firewall.
* **Private Endpoints** ensure that the Web App and Storage Account are **accessible only over private IPs** within Azure.
* DNS queries are resolved **privately** via the DNS VM in the Hub.

### 🔹 Highlights

* ✅ **No public IP exposure** for Web App or Storage.
* ✅ **Centralized security** with Azure Firewall.
* ✅ **Private DNS resolution** for seamless connectivity across VNets.
* ✅ **On-Premises integration** for hybrid testing.

> This architecture ensures a **secure, enterprise-grade network** for hosting Azure resources with controlled and private access.

---

