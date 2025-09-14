
---

# 📸 Screenshots – Deployment Validation

## 📌 Overview

This folder contains **deployment-related screenshots and resources** that validate the secure hosting of a Web App in Azure using **Hub-and-Spoke architecture, Private Endpoints, and Firewall rules**.
Each screenshot and resource demonstrates a critical part of the configuration or testing process.

---

## 📂 Folder Contents

| File/Folder            | Description                                                                                                                                                       |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `onpremise vm/`        | Contains screenshots and logs related to the **on-premises VM setup**, validating DNS resolution, VNet peering, and private access.                               |
| `resource to dns vm/`  | Shows the creation and configuration of a **DNS forwarder VM**, which ensures that private endpoints resolve correctly in the on-premises and spoke networks.     |
| `storage account/`     | Screenshots of the **Azure Storage Account deployment**, including network restrictions, private endpoint configuration, and firewall rules applied.              |
| `404-error-webapp.png` | Screenshot showing a **404 error from the Web App**, verifying that requests are routed to the private endpoint rather than the public one.                       |
| `curl-ssl-error.png`   | Captures a **cURL SSL error**, demonstrating that access attempts from unauthorized environments fail, confirming the **security enforcement**.                   |
| `ssl-warning.png`      | Browser screenshot showing an **SSL certificate warning**, which occurs when accessing the Web App through a test/private endpoint without a trusted certificate. |
| `readme.md`            | Documentation for this folder (you are reading it now).                                                                                                           |

---

## 🔎 Key Validations

1. **On-Premises VM Testing**

   * Verified that DNS queries resolve to **private IPs** only.
   * Ensured connectivity flows through the firewall and private endpoints.

2. **DNS Forwarder VM**

   * Deployed for **hybrid name resolution**.
   * Forwarded DNS requests to Azure-provided DNS, enabling resolution of private zones.

3. **Storage Account Restrictions**

   * Storage Account accessible **only from VNets with Private Endpoint**.
   * Public endpoint access disabled, tested via curl and browser.

4. **Web App Access**

   * 404 page returned from private endpoint → proves private access path.
   * SSL warnings/errors validate isolation from the public internet.

---

## ✅ Validation Checklist

* [x] Web App accessible internally via private endpoint.
* [x] Storage Account locked to VNet/private access.
* [x] Public access blocked with SSL errors.
* [x] DNS forwarder working for on-premises resolution.
* [x] End-to-end deployment screenshots documented.

---

## 🌱 Future Enhancements

* Add more automated deployment captures (ARM template logs).
* Include **Azure Firewall logs** screenshots for traffic verification.
* Add **step-by-step annotated screenshots** for better report clarity.

---
