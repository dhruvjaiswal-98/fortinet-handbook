# 🧪 Lab 01 – FortiGate Initial Setup & Internet Access Configuration

![Fortinet](https://img.shields.io/badge/Fortinet-FortiGate-red)
![Firewall](https://img.shields.io/badge/Firewall-Lab-blue)
![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-success)

---

# 📌 Overview

This lab demonstrates the complete initial deployment of a FortiGate Firewall in an EVE-NG environment.

The objective is to configure the FortiGate firewall from scratch, establish WAN connectivity, activate the evaluation license, configure the internal LAN interface, create the required firewall policy with Source NAT (PAT), and verify end-to-end Internet connectivity.

This lab is ideal for beginners learning FortiGate administration and forms the foundation for all future firewall labs.

---

# 🎯 Objectives

At the end of this lab you will be able to:

- Deploy a FortiGate VM
- Configure the FortiGate hostname
- Configure WAN Interface
- Configure LAN Interface
- Configure Static Routing
- Verify Internet Connectivity
- Activate Evaluation License
- Configure Firewall Policies
- Configure Source NAT
- Verify End-to-End Connectivity
- Perform basic troubleshooting

---

# 🛠 Technologies Used

- FortiGate VM
- EVE-NG
- FortiOS GUI
- FortiOS CLI
- Windows Client / VPCS
- IPv4
- Static Routing
- Firewall Policies
- NAT (PAT)

---

# 📚 Skills Gained

- GUI Navigation
- CLI Configuration
- Interface Configuration
- Static Route Configuration
- Firewall Policy Creation
- Source NAT Configuration
- Connectivity Verification
- Troubleshooting

---

# 🌐 Lab Topology

<img width="668" height="357" alt="image" src="https://github.com/user-attachments/assets/276109c0-fe9b-44e9-ab9a-a29249291fd3" />

```

---

# 🖥 Lab Addressing

## Management Network

| Device | IP Address |
|----------|------------|
| EVE-NG | 10.236.58.182 |
| Laptop | 10.236.58.252 |
| Gateway | 10.236.58.10 |

---

## Internal LAN

| Device | Address |
|----------|------------|
| FortiGate Port2 | 192.168.100.1/24 |
| Windows Client | 192.168.100.10/24 |

---

# 📝 Step 1 — Configure Hostname

Configure a meaningful hostname for easier device identification.

### CLI

```bash
config system global
set hostname FW1
end
```
<img width="552" height="276" alt="image" src="https://github.com/user-attachments/assets/fe55479f-9b64-4a7b-ba38-a86f13b8fa68" />

```

---

# 📝 Step 2 — Configure WAN Interface

Assign an IP address to Port1 and enable management access.

Example:

```bash
config system interface
edit port1
set mode static
set ip <WAN-IP>/<Mask>
set allowaccess ping https ssh
end
```
<img width="695" height="428" alt="image" src="https://github.com/user-attachments/assets/0eb9470d-bda5-4254-a5fb-ebfe19a3772f" />

<img width="720" height="427" alt="image" src="https://github.com/user-attachments/assets/731284e0-22ac-4159-b445-5d2b35c50c68" />

---

# 📝 Step 3 — Access the GUI

Browse to:

```
https://<FortiGate-IP>
```

Login using the administrator account.

<img width="630" height="333" alt="image" src="https://github.com/user-attachments/assets/9bfd5fdd-6d42-44ef-8772-a61d936e1f66" />

---

# 📝 Step 4 — Verify Internet Connectivity

Before activating the license, verify Internet connectivity.

```bash
execute ping 8.8.8.8
```

If unreachable, configure the default route.

---

# 📝 Step 5 — Configure Static Route

```bash
config router static
edit 1
set gateway 10.236.58.10
set device port1
next
end
```

Verify:

```bash
execute ping 8.8.8.8
```

<img width="652" height="277" alt="image" src="https://github.com/user-attachments/assets/1a9130be-9ea4-4ff9-a4c8-67cc75698ee8" />

```
---

# 📝 Step 6 — Activate Evaluation License

Once Internet connectivity is confirmed:

- Open the FortiGate GUI
- Navigate to License
- Sign in using FortiCloud
- Activate Evaluation License

```
<img width="690" height="332" alt="image" src="https://github.com/user-attachments/assets/af956eae-35de-4b23-8a9a-883e4d2fbc44" />

---

# 📝 Step 7 — Configure Port2 (Internal LAN)

Configure Port2 as the LAN gateway.

| Setting | Value |
|---------|-------|
| Interface | Port2 |
| Address | 192.168.100.1/24 |

<img width="675" height="457" alt="image" src="https://github.com/user-attachments/assets/11dc7d92-324e-4417-b95a-0f7be78cccc3" />

```

---

# 📝 Step 8 — Configure the Client

Assign:

| Parameter | Value |
|-----------|-------|
| IP | 192.168.100.10 |
| Mask | 255.255.255.0 |
| Gateway | 192.168.100.1 |

Verify:

```text
ping 192.168.100.1
```

The gateway should respond.

However:

```text
ping 8.8.8.8
```

will fail because no firewall policy exists yet.

<img width="343" height="152" alt="image" src="https://github.com/user-attachments/assets/2b94a94f-7e3a-47b0-838c-7cb6defb83be" />

```

---

# 📝 Step 9 — Create Firewall Policy

Create a LAN-to-WAN policy.

| Setting | Value |
|----------|------|
| Incoming | Port2 |
| Outgoing | Port1 |
| Source | all |
| Destination | all |
| Service | ALL |
| Action | ACCEPT |
| NAT | Enabled |

```
<img width="676" height="491" alt="image" src="https://github.com/user-attachments/assets/b2ae7868-0258-4055-b1a0-8bd6d48444cb" />

```

---

# 📝 Step 10 — Enable NAT

Enable Source NAT using the outgoing interface address.

```
NAT → Enabled
Use Outgoing Interface Address
```

```
<img width="720" height="638" alt="image" src="https://github.com/user-attachments/assets/e09c8e4a-4414-4c1e-98cd-371f4266ee3d" />

```
---

# 📝 Step 11 — Verify Connectivity

Test from the client.

```text
ping 192.168.100.1
```

✅ Success

```text
ping 8.8.8.8
```

✅ Success

The internal client should now have Internet access through the FortiGate firewall.

<img width="677" height="412" alt="image" src="https://github.com/user-attachments/assets/fa7ba0c8-58c2-40db-83f0-15f724816161" />

```

---

# 🔍 Troubleshooting

If Internet access does not work, verify the following:

- Interface IP configuration
- Default Route
- DNS Configuration
- Firewall Policy
- NAT Configuration
- Client Gateway
- Internet Reachability
- Routing Table

Useful Commands

```bash
execute ping 8.8.8.8
```

```bash
get router info routing-table all
```

```bash
show firewall policy
```

```bash
show system interface
```

---

# 📊 Result

| Test | Status |
|-------|--------|
| FortiGate Internet Access | ✅ |
| Evaluation License | ✅ |
| LAN Connectivity | ✅ |
| Firewall Policy | ✅ |
| NAT | ✅ |
| Client Internet Access | ✅ |

---

# 🎓 Key Takeaways

- Configured a FortiGate firewall from scratch.
- Configured WAN and LAN interfaces.
- Verified Internet connectivity.
- Activated the evaluation license.
- Created a LAN-to-WAN firewall policy.
- Configured Source NAT (PAT).
- Successfully provided Internet access to an internal client.
- Learned the importance of routing, firewall policies, and NAT in packet forwarding.
```

---

## 🚀 Next Lab

➡️ **Lab 02 – FortiGate Firewall Policies & NAT Deep Dive**
