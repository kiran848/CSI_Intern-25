
---

# 🧪 Testing & Validation

## 📌 Overview

This folder contains test results, screenshots, and validation outputs that confirm the **secure hosting environment** works as intended.
The tests include **DNS resolution, private endpoint access, firewall restrictions, and application reachability** from different environments (on-premises, web VNet, storage VNet).

---

## 📂 Folder Contents

| File Name                     | Description                                                                                                                                        |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `404-error-webapp.png`        | Shows the Web App returning a **404 error**, validating that the request reached the **private endpoint** instead of the public endpoint.          |
| `curl-outputs.txt`            | Output of `curl` commands testing access to the Web App and Storage endpoints, showing expected behavior (success internally, blocked externally). |
| `dns-nslookup-web.png`        | Screenshot of `nslookup` for Web App domain (`*.azurewebsites.net`), resolving to a **private IP (10.x.x.x)**.                                     |
| `dns-nslookup-storage.png`    | Screenshot of `nslookup` for Storage domain (`*.blob.core.windows.net`), resolving to a **private IP (10.x.x.x)**.                                 |
| `nslookup.png`                | Consolidated screenshot of DNS resolution tests for multiple resources.                                                                            |
| `onprem-nslookup-results.txt` | Raw output of `nslookup` from **on-premises VM**, proving hybrid DNS resolution is working correctly.                                              |
| `readme.md`                   | Documentation for this folder (you are reading it now).                                                                                            |

---

## 🔎 Validation Tests

### 1. **DNS Resolution**

* From On-Premises VM or Spoke VM:

  ```bash
  nslookup <yourwebapp>.azurewebsites.net
  nslookup <yourstorage>.blob.core.windows.net
  ```

  ✅ Expected: **10.x.x.x private IP** returned.

### 2. **Application Access (cURL)**

* From VM in VNet:

  ```bash
  curl https://<yourwebapp>.azurewebsites.net
  curl https://<yourstorage>.blob.core.windows.net/<container>/<file>
  ```

  ✅ Expected: Response comes back internally.
  ❌ External access fails (no internet route).

### 3. **Firewall & Isolation**

* Attempt access from public internet:

  * Should fail (web app/storage are not reachable over public endpoint).
* Attempt access from On-Premises VM:

  * Should succeed (traffic flows via VPN/ExpressRoute → Firewall → Private Endpoint).

---

## ✅ Validation Checklist

* [x] Web App resolves to private IP.
* [x] Storage Account resolves to private IP.
* [x] cURL outputs confirm internal-only access.
* [x] External access is blocked.
* [x] Firewall logs show traffic inspection.

---

## 🌱 Future Enhancements

* Automate test cases via **Azure CLI scripts**.
* Add **PowerShell test suite** for repeated validation.
* Integrate **Azure Monitor + Log Analytics** for real-time validation dashboards.

---

