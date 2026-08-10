# 🧪 Lab 02 – DHCP Server Configuration on FortiGate

---

# 📌 Overview

This lab demonstrates how to configure **FortiGate as a DHCP Server** and automatically assign IP addresses and network parameters to internal clients.

The objective is to configure the FortiGate internal interface with a static IP address, enable the DHCP Server, define a DHCP address pool, and verify that internal VPC clients receive their IP configuration automatically.

This lab was performed in an **EVE-NG virtual environment** using FortiOS version **7.2.8**.

---

# 🎯 Objectives

At the end of this lab you will be able to:

- Configure a FortiGate hostname
- Configure the WAN/Management interface
- Verify interface status
- Configure an internal LAN interface
- Configure FortiGate as a DHCP Server
- Configure a DHCP address range
- Configure the default gateway
- Configure DNS settings
- Configure VPC clients as DHCP clients
- Verify DHCP address assignment
- Verify DHCP leases
- Perform basic DHCP troubleshooting

---

# 🛠 Technologies Used

- FortiGate VM
- FortiOS 7.2.8
- EVE-NG
- FortiGate GUI
- FortiGate CLI
- VPC
- IPv4
- DHCP
- Virtual Switching

---

# 📚 Skills Gained

- FortiGate GUI Navigation
- FortiGate CLI Configuration
- Interface Configuration
- DHCP Server Configuration
- IP Address Management
- DHCP Verification
- DHCP Troubleshooting
- Basic Network Connectivity Testing

---

# 🌐 Lab Topology

<img width="686" height="402" alt="image" src="https://github.com/user-attachments/assets/b5f624fb-3476-4368-ba17-cb0fe0826b1f" />

---

# 🖥 Lab Addressing

## Management Network

| Device | IP Address |
|----------|------------|
| EVE-NG | 10.130.193.182 |
| Laptop | 10.130.193.79 |
| Gateway | 10.130.193.157 |

---

## FortiGate Interfaces

| Interface | IP Address |
|----------|------------|
| Port1 / Management | 10.130.193.81 |
| Port2 / LAN | 192.168.1.1/24 |

---

## Internal DHCP Network

| Parameter | Value |
|----------|-------|
| Network | 192.168.1.0/24 |
| DHCP Server | FortiGate |
| DHCP Range | 192.168.1.2 – 192.168.1.254 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.1.1 |
| Client | VPC |

---

# 📝 Step 1 — Deploy FortiGate VM

For this lab, the FortiGate VM was deployed in **EVE-NG** using:

```text
FortiOS 7.2.8
```

The firewall was allowed to boot completely before beginning the configuration.

### Screenshot

<img width="627" height="365" alt="image" src="https://github.com/user-attachments/assets/0311faca-e78c-4cb6-8e81-f35d3412f0e1" />

---

# 📝 Step 2 — Configure Hostname

Configure a meaningful hostname for easier device identification.

### CLI

```bash
config system global
set hostname FW
end
```
<img width="557" height="161" alt="image" src="https://github.com/user-attachments/assets/346df692-e356-4f37-99e8-9c8b7db4db29" />

---

# 📝 Step 3 — Configure Port1

Port1 was configured as the WAN/Management interface.

For this lab, the interface receives its IP address using DHCP.

### CLI

```bash
config system interface
edit port1
set mode dhcp
set allowaccess https http ssh ping telnet
next
end
```

Port1 received:

```text
10.130.193.81/24
```
---

# 📝 Step 4 — Verify Port1

Use the following command to verify the physical interface:

```bash
get system interface physical
```

The interface should show:

```text
Mode   : DHCP
IP     : 10.130.193.81
Status : UP
```

###

<img width="638" height="252" alt="image" src="https://github.com/user-attachments/assets/24ab613a-b08c-42d1-bffa-d8e1bf281a5a" />

---

# 📝 Step 5 — Test Connectivity

Test connectivity between the FortiGate and the laptop.

```bash
execute ping 10.130.193.79
```

A successful ping confirms that the FortiGate can reach the laptop through the management network.

### Screenshot

<img width="672" height="207" alt="image" src="https://github.com/user-attachments/assets/7e3b63d6-dc85-431b-890a-d38d193844bd" />

---

# 📝 Step 6 — Configure Port2

Port2 is configured as the internal LAN interface.

Configure:

| Setting | Value |
|----------|-------|
| Interface | Port2 |
| Address | 192.168.1.1/24 |
| Role | Internal LAN |

Port2 will act as the gateway for internal clients.

### Screenshot

<img width="701" height="471" alt="image" src="https://github.com/user-attachments/assets/08597e31-d2b5-4f0d-82ff-f2ca4b61c262" />

---

# 📝 Step 7 — Enable DHCP Server

Enable the **DHCP Server** on Port2.

### DHCP Configuration

| Setting | Value |
|----------|-------|
| DHCP Status | Enabled |
| Address Range | 192.168.1.2 – 192.168.1.254 |
| Netmask | 255.255.255.0 |
| Default Gateway | Same as Interface IP |
| DNS Server | Same as System DNS |
| Lease Time | 604800 seconds |

FortiGate will now automatically provide IP configuration to clients connected to Port2.

### Screenshot

<img width="767" height="640" alt="image" src="https://github.com/user-attachments/assets/b949475b-7143-4c33-83e7-506fc6293b0a" />

---

# 📝 Step 8 — Configure VPC as DHCP Client

The VPC clients connected to the internal network are configured to obtain their IP address automatically.

From the VPC console:

```text
dhcp
```

The client sends a DHCP request to FortiGate.

Example:

```text
VPCS> dhcp

DDORA IP 192.168.1.2/24 GW 192.168.1.1
```

The exact IP address may vary depending on the available DHCP lease.

### Screenshot

<img width="676" height="335" alt="image" src="https://github.com/user-attachments/assets/f263e170-1389-4afc-b61d-f105f9f80dc0" />

---

# 📝 Step 9 — Verify DHCP Address Assignment

After the DHCP process completes, the VPC should receive:

- IP Address
- Subnet Mask
- Default Gateway
- DNS Information

Example:

```text
IP Address      : 192.168.1.2
Subnet Mask     : 255.255.255.0
Default Gateway : 192.168.1.1
```

The client is now successfully receiving its network configuration from FortiGate.

---

# 📝 Step 10 — Verify DHCP Configuration

The DHCP configuration can be verified from the FortiGate CLI.

```bash
show system dhcp server
```

This allows you to verify the configured DHCP server and address pool.

---

# 📝 Step 11 — Verify DHCP Lease

Use the following command to view active DHCP leases:

```bash
execute dhcp lease-list
```

This can be used to verify whether the connected VPC has received a DHCP lease.

---

# 🔍 Troubleshooting

If the VPC does not receive an IP address, verify the following:

- Port2 interface status
- Port2 IP address
- DHCP Server configuration
- DHCP address range
- VPC network connection
- DHCP lease table
- Client configuration

---

## Useful Commands

### Check Physical Interfaces

```bash
get system interface physical
```

### Check DHCP Configuration

```bash
show system dhcp server
```

### Check DHCP Leases

```bash
execute dhcp lease-list
```

### Test Gateway Connectivity

```text
ping 192.168.1.1
```

---

# 📊 DHCP Troubleshooting Flow

```text
VPC
 |
 | DHCP Request
 v
FortiGate Port2
 |
 | DHCP Server
 |
 +---- Check Interface
 |
 +---- Check DHCP Configuration
 |
 +---- Check DHCP Pool
 |
 +---- Check DHCP Lease
 |
 v
IP Address Assigned
```

---
# 📊 Result

| Test | Status |
|------|--------|
| FortiGate VM Boot | ✅ |
| Hostname Configuration | ✅ |
| Port1 Configuration | ✅ |
| Port1 Connectivity | ✅ |
| Port2 Configuration | ✅ |
| DHCP Server | ✅ |
| DHCP Pool | ✅ |
| VPC DHCP Request | ✅ |
| IP Address Assignment | ✅ |
| DHCP Lease Verification | ✅ |

---

# 🎓 Key Takeaways

- FortiGate can operate as a **DHCP Server**.
- Port2 was configured as the internal LAN interface.
- `192.168.1.1/24` was configured as the LAN gateway.
- FortiGate was configured with a DHCP pool from `192.168.1.2` to `192.168.1.254`.
- VPC clients successfully obtained their IP configuration automatically.
- DHCP configuration can be verified using the FortiGate CLI.
- DHCP leases can be checked using `execute dhcp lease-list`.
- Proper interface configuration and DHCP scope configuration are essential for successful IP assignment.

---
---

# 📝 Lab Result

> **FortiGate was successfully configured as a DHCP Server and the internal VPC clients successfully received IP addressing information automatically.**

```text
                    FortiGate
                   Port2
                192.168.1.1
                     |
                     |
                DHCP Server
                     |
          +----------+----------+
          |                     |
          v                     v
       VPC 01                VPC 02
       DHCP                  DHCP
          |                     |
          v                     v
    DHCP Assigned        DHCP Assigned
       IP Address           IP Address
```
