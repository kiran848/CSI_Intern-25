
# DNS Forwarder VM Setup (Linux)

This folder documents the setup and configuration of the **DNS Forwarding VM** used in the **Hub VNet** to resolve private DNS zones for Azure services like Web App and Storage.

---

## ✅ Objective

To forward DNS queries from on-premises and spoke VNets to Azure-provided DNS and enable name resolution for private endpoints (e.g., webapp and storage).

---

## 🔧 Installation of `dnsmasq`

1. **Login** to your Ubuntu/Linux VM (DNS-VM) in `vnet-hub`.

2. **Update package list and install dnsmasq**:

   ```bash
   sudo apt update
   sudo apt install dnsmasq -y
   ```

3. **Enable systemd-resolved compatibility** (if needed):

   ```bash
   sudo systemctl disable systemd-resolved
   sudo systemctl stop systemd-resolved
   sudo rm /etc/resolv.conf
   echo "nameserver 127.0.0.1" | sudo tee /etc/resolv.conf
   ```

---

## ⚙️ Configuration

1. Edit the dnsmasq configuration file:

   ```bash
   sudo nano /etc/dnsmasq.conf
   ```

2. Add/modify the following lines:

   ```ini
   listen-address=127.0.0.1,10.0.3.4
   bind-interfaces
   server=168.63.129.16
   ```

   - `10.0.3.4`: The private IP of this DNS-VM (get using `ip a`).
   - `168.63.129.16`: Azure's internal DNS resolver for private zones.

3. Restart the dnsmasq service:

   ```bash
   sudo systemctl restart dnsmasq
   ```

4. Verify it's listening on port 53:

   ```bash
   sudo ss -ulnp | grep 53
   ```

   Expected:
   ```
   UNCONN 0      0            0.0.0.0:53        0.0.0.0:*    users:(("dnsmasq",...))
   UNCONN 0      0               [::]:53           [::]:*    users:(("dnsmasq",...))
   ```

---

## 🧪 Testing the DNS Forwarder

### 🔹 1. From DNS-VM itself:

```bash
nslookup webapp00-xyz.centralus-01.privatelink.azurewebsites.net 127.0.0.1
```

**Expected Output** (example):

```
Non-authoritative answer:
webapp00-xyz...privatelink.azurewebsites.net
  canonical name = ...
  Address: 10.1.1.4
```

### 🔹 2. From On-Prem VM or Spoke VM

Ensure their DNS setting is updated to point to the DNS-VM IP (`10.0.3.4`):

```powershell
Get-DnsClientServerAddress
```

If not set, update it:

```powershell
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.3.4
```

Then test:

```powershell
nslookup webapp00-xyz.centralus-01.privatelink.azurewebsites.net
```

✅ If the response is a **10.x.x.x private IP**, the DNS forwarding is working.

---

## 📝 Notes

- Ensure NSG allows **DNS (UDP port 53)** to DNS-VM.
- This setup is crucial for **private link resolution** of Web Apps and Storage.
- DNS-VM must be accessible from spoke VMs for resolution to work.

---
