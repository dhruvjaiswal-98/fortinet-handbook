# 🧪 Lab 03 – FortiGate Firewall Policy & NAT (LAN to Internet)

---

# 📌 Overview

This lab demonstrates how to configure a **FortiGate Firewall Policy and Source NAT (PAT)** to provide Internet access to internal LAN users.

The FortiGate is deployed in an EVE-NG environment with:

- Port1 configured as the WAN interface
- Port2 configured as the internal LAN interface
- DHCP configured for internal clients
- A default route configured toward the Internet
- A LAN-to-WAN firewall policy created
- Source NAT (PAT) enabled
- Traffic logging enabled
- End-to-end Internet connectivity verified

This lab demonstrates one of the most fundamental FortiGate traffic flows:

```text
LAN → Firewall Policy → NAT → WAN → Internet
```

---

# 🎯 Objectives

At the end of this lab you will be able to:

- Configure FortiGate WAN connectivity
- Configure a FortiGate LAN interface
- Configure DHCP for internal clients
- Configure a default route
- Create a LAN-to-WAN firewall policy
- Configure Source NAT (SNAT)
- Understand Port Address Translation (PAT)
- Enable traffic logging
- Verify Internet connectivity
- Monitor active sessions
- Perform basic FortiGate troubleshooting

---

# 🛠 Technologies Used

- FortiGate VM
- EVE-NG
- FortiOS GUI
- FortiOS CLI
- VPC
- Virtual Switch
- IPv4
- DHCP
- Static Routing
- Firewall Policy
- Source NAT (SNAT)
- Port Address Translation (PAT)
- FortiView Sessions

---

# 📚 Skills Gained

- FortiGate CLI Configuration
- FortiGate GUI Navigation
- Interface Configuration
- DHCP Configuration
- Static Route Configuration
- Firewall Policy Configuration
- Source NAT Configuration
- PAT Configuration
- Traffic Logging
- Session Monitoring
- Connectivity Testing
- Basic Troubleshooting

---

# 🌐 Lab Topology

```text
                         INTERNET
                            |
                            |
                     FortiGate Port1
                       DHCP WAN
                     10.136.208.81
                            |
                         FortiGate
                            |
                     FortiGate Port2
                     192.168.1.1/24
                            |
                          Switch
                         /      \
                        /        \
                     PC1          PC2
                    DHCP         DHCP
```

---

# 🖥 Lab Addressing

## WAN / Management Network

| Device | IP Address |
|--------|------------|
| EVE-NG | 10.136.208.81 |
| Laptop | 10.136.208.244 |
| Gateway | 10.136.208.215 |

---

## FortiGate Interfaces

| Interface | IP Address | Purpose |
|-----------|------------|---------|
| Port1 | 10.136.208.81 | WAN |
| Port2 | 192.168.1.1/24 | LAN |

---

## Internal LAN

| Device | Configuration |
|--------|---------------|
| PC1 | DHCP |
| PC2 | DHCP |
| Default Gateway | 192.168.1.1 |

---

# 📝 Step 1 — Boot the FortiGate

Start the FortiGate VM from EVE-NG and open the FortiGate console.

Allow the FortiGate to complete the boot process.

```text
FortiGate-VM64-KVM login:
```

The FortiGate should be fully initialized before continuing with the configuration.

<img width="637" height="370" alt="image" src="https://github.com/user-attachments/assets/7270c7e8-a1f1-4573-9996-2f6c50b9513a" />

---

# 📝 Step 2 — Configure Hostname

Configure a meaningful hostname for easier identification.

### CLI

```bash
config system global
set hostname FW
end
```

The hostname helps identify the firewall when managing multiple FortiGate devices.

<img width="575" height="191" alt="image" src="https://github.com/user-attachments/assets/9675ec80-856c-422c-8419-5dc81443fcc5" />

---

# 📝 Step 3 — Configure Port1 Using DHCP

In this lab, Port1 is connected directly to the WAN network and receives its IP address using DHCP.

### CLI

```bash
config system interface
edit port1
set mode dhcp
set allowaccess https http ssh ping telnet
next
end
```

Port1 should receive an IP address from the WAN network.

Expected IP:

```text
10.136.208.81
```
<img width="692" height="230" alt="image" src="https://github.com/user-attachments/assets/ccf5a1de-5f3a-4b12-8b6c-e42172d4d14a" />

<img width="647" height="256" alt="image" src="https://github.com/user-attachments/assets/9151d003-2fb9-4b24-904c-2aa550127806" />

---

# 📝 Step 4 — Verify Physical Interface Status

Use the following command to verify the interface status:

```bash
get system interface physical
```

Verify:

- Port1 is UP
- DHCP mode is enabled
- IP address is assigned
- Interface is operational

---

# 📝 Step 5 — Test WAN Connectivity

Test connectivity from the FortiGate to the laptop/WAN network.

```bash
execute ping 10.136.208.244
```

Successful replies confirm that the FortiGate has reachability to the upstream network.

<img width="676" height="215" alt="image" src="https://github.com/user-attachments/assets/9e0078b3-ae87-4106-976d-ea2a29047c83" />

---

# 📝 Step 6 — Check FortiGate System Status

Use:

```bash
get system status
```

This command can be used to verify important system information such as:

- FortiOS version
- Firmware information
- System information
- License information
- Device information

---

# 📝 Step 7 — Access the FortiGate GUI

Open a browser and access the FortiGate using its WAN IP:

```text
https://10.136.208.81
```

Login using the configured administrator account.

<img width="672" height="358" alt="image" src="https://github.com/user-attachments/assets/a528ef94-db4e-465b-8d34-8692c6d6ffb0" />

---

# 📝 Step 8 — Configure Port2

Configure Port2 as the internal LAN interface.

| Setting | Value |
|---------|-------|
| Interface | Port2 |
| IP Address | 192.168.1.1/24 |
| Role | LAN |

Port2 will act as the default gateway for the internal clients.

<img width="722" height="482" alt="image" src="https://github.com/user-attachments/assets/e0f87991-76bf-439a-9b75-047d41ceb9b5" />

---

# 📝 Step 9 — Configure DHCP Server

Configure Port2 as a DHCP server for the internal LAN.

The internal network is:

```text
192.168.1.0/24
```

The FortiGate LAN interface acts as the default gateway:

```text
192.168.1.1
```

The internal clients will obtain their IP addresses automatically through DHCP.

---

# 📝 Step 10 — Verify DHCP on the VPC

The VPC receives its IP address from the FortiGate DHCP server.

Example:

```text
VPC> dhcp
```

Expected result:

```text
IP Address : 192.168.1.x/24
Gateway    : 192.168.1.1
```
<img width="755" height="387" alt="image" src="https://github.com/user-attachments/assets/d1bc7a83-2ce3-4195-ba41-df20ac2635ff" />

---

# 📝 Step 11 — Test Internal LAN Connectivity

First, verify that the VPC can reach the FortiGate LAN gateway.

```text
ping 192.168.1.1
```

Expected:

```text
SUCCESS
```

At this stage, Internet connectivity will not work yet because the required routing and firewall policy have not been configured.

Test:

```text
ping 8.8.8.8
```

Expected:

```text
Request timeout
```

<img width="678" height="387" alt="image" src="https://github.com/user-attachments/assets/26c675d3-1b53-4a32-bb65-84dec26e963c" />

---

# 📝 Step 12 — Configure Default Route

Create a default route for Internet-bound traffic.

Navigate to:

```text
Network
   ↓
Static Routes
```

Create a default route:

| Setting | Value |
|---------|-------|
| Destination | 0.0.0.0/0 |
| Gateway | 10.136.208.215 |
| Interface | WAN (Port1) |
| Administrative Distance | 10 |

The default route tells FortiGate where to send traffic for destinations that are not present in the routing table.

```text
LAN
 |
 ↓
FortiGate
 |
 ↓
0.0.0.0/0
 |
 ↓
10.136.208.215
 |
 ↓
Port1
 |
 ↓
Internet
```
<img width="1282" height="642" alt="image" src="https://github.com/user-attachments/assets/bf9d008d-7a30-40f4-af60-d4dad94f06ff" />

<img width="1047" height="738" alt="image" src="https://github.com/user-attachments/assets/e78d9b3d-ef02-402d-9c51-51ae3592108d" />

---

# 📝 Step 13 — Verify the Default Route

Use:

```bash
get router info routing-table all
```

Verify that the default route exists:

```text
0.0.0.0/0
```

The route should point toward:

```text
10.136.208.215
```

---

# 📝 Step 14 — Create Firewall Policy

Create a firewall policy allowing traffic from the internal LAN to the WAN.

Navigate to:

```text
Policy & Objects
      ↓
Firewall Policy
      ↓
Create New
```

Configure the policy as follows:

| Setting | Value |
|---------|-------|
| Name | LAN_TO_INTERNET |
| Incoming Interface | LAN (Port2) |
| Outgoing Interface | WAN (Port1) |
| Source | all |
| Destination | all |
| Service | ALL |
| Action | ACCEPT |

Traffic flow:

```text
LAN (Port2)
     |
     ↓
LAN_TO_INTERNET
     |
     ↓
WAN (Port1)
```

<img width="876" height="675" alt="image" src="https://github.com/user-attachments/assets/2b4a51b4-cc58-4baa-a65d-2b16b45a99d1" />

<img width="788" height="697" alt="image" src="https://github.com/user-attachments/assets/15893281-064e-4963-8fe3-58510f3e0b69" />


---

# 📝 Step 15 — Configure Source NAT

Source NAT must be enabled so that private LAN addresses can access the Internet.

Under the firewall policy, enable:

```text
NAT → Enabled
```

Select:

```text
Use Outgoing Interface Address
```

This allows the FortiGate WAN interface address to be used for Source NAT/PAT.

<img width="817" height="602" alt="image" src="https://github.com/user-attachments/assets/65c0fadd-258e-4e4e-8feb-e37f53eb3211" />

---

# 🔄 Source NAT / PAT Example

Before NAT:

```text
192.168.1.10
        ↓
8.8.8.8
```

After NAT:

```text
192.168.1.10
        ↓
10.136.208.81
        ↓
8.8.8.8
```

Multiple internal clients can share the same WAN IP.

```text
192.168.1.10 ─┐
192.168.1.20 ─┼──→ FortiGate ──→ 10.136.208.81 ──→ Internet
192.168.1.30 ─┘
```

---

# 📝 Step 16 — Enable Traffic Logging

Enable:

```text
Log Allowed Traffic
```

Select:

```text
All Sessions
```

instead of logging only security events.

This is useful during troubleshooting because it allows normal permitted traffic sessions to be viewed in the FortiGate logs/session monitoring.

<img width="673" height="382" alt="image" src="https://github.com/user-attachments/assets/9a66ad04-e5b9-47f6-8910-85ae31332593" />

---

# 📝 Step 17 — Verify Internet Connectivity

After configuring:

- LAN interface
- DHCP
- Default route
- Firewall policy
- Source NAT

test Internet connectivity from the VPC.

### Test Gateway

```text
ping 192.168.1.1
```

Expected:

```text
SUCCESS
```

### Test Internet

```text
ping 8.8.8.8
```

Expected:

```text
SUCCESS
```

The VPC should now be able to access the Internet through the FortiGate.

<img width="777" height="477" alt="image" src="https://github.com/user-attachments/assets/33d9bf80-29de-42e5-ba16-f68891df9789" />

---

# 📝 Step 18 — Verify Active Sessions

The traffic can also be observed from the FortiGate dashboard.

Navigate to:

```text
Dashboard
   ↓
FortiView
   ↓
Sessions
```

The active sessions generated by the internal VPC should be visible.

This confirms that the traffic is passing through the FortiGate firewall.

<img width="792" height="311" alt="image" src="https://github.com/user-attachments/assets/3796b888-a05a-48a0-980b-0a83ea3eea9e" />

---

# 🔄 Complete LAN-to-Internet Packet Flow

The complete traffic flow in this lab is:

```text
                 PC
                  |
                  |
             192.168.1.x
                  |
                  ↓
             Port2 (LAN)
                  |
                  ↓
          Routing Table Lookup
                  |
                  ↓
           Default Route
                  |
                  ↓
        Firewall Policy Match
                  |
                  ↓
            Source NAT
                  |
                  ↓
             Port1 (WAN)
                  |
                  ↓
          10.136.208.81
                  |
                  ↓
              INTERNET
```

---

# 🔍 Troubleshooting

If the internal client cannot access the Internet, verify each layer systematically.

---

## 1. Check the Client IP Address

Verify that the VPC received an IP address from the FortiGate DHCP server.

```text
dhcp
```

The client should receive an address from:

```text
192.168.1.0/24
```

---

## 2. Check the Default Gateway

Test:

```text
ping 192.168.1.1
```

If this fails, check:

- Port2 configuration
- DHCP configuration
- VPC configuration
- Switch connectivity

---

## 3. Check WAN Connectivity

From the FortiGate:

```bash
execute ping 10.136.208.244
```

Verify that the FortiGate can reach the upstream network.

---

## 4. Check Default Route

Run:

```bash
get router info routing-table all
```

Verify that:

```text
0.0.0.0/0
```

exists and points toward:

```text
10.136.208.215
```

---

## 5. Check Firewall Policy

Verify:

```text
Incoming Interface : Port2
Outgoing Interface : Port1
Source              : all
Destination         : all
Service             : ALL
Action              : ACCEPT
```

---

## 6. Check NAT

Verify:

```text
NAT : Enabled
```

and:

```text
Use Outgoing Interface Address
```

is selected.

---

## 7. Check Active Sessions

Navigate to:

```text
Dashboard
   ↓
FortiView
   ↓
Sessions
```

Look for traffic generated by the internal client.

---

# 🧠 Important Concepts

## Firewall Policy

A firewall policy determines whether traffic is allowed or denied.

```text
LAN
 ↓
Firewall Policy
 ↓
ACCEPT / DENY
```

---

## Source NAT

Source NAT changes the source IP address of outbound traffic.

```text
192.168.1.10
      ↓
10.136.208.81
```

---

## PAT

PAT allows multiple private IP addresses to share one public/WAN IP by translating source port numbers.

```text
192.168.1.10:52001 ─┐
192.168.1.20:52005 ─┼──→ 10.136.208.81 ──→ Internet
192.168.1.30:52010 ─┘
```

---

## Default Route

The default route is used when no more specific route exists for the destination.

```text
0.0.0.0/0
     ↓
10.136.208.215
```

---

# 🆚 Routing vs Firewall Policy

| Routing | Firewall Policy |
|---------|-----------------|
| Determines where traffic should go | Determines whether traffic is allowed |
| Uses the routing table | Uses firewall policies |
| Determines next-hop/interface | Determines Allow/Deny |
| Focuses on packet forwarding | Focuses on security control |

A simple way to remember:

```text
Routing
   ↓
Where should the packet go?

Firewall Policy
   ↓
Is the packet allowed?
```


In this lab:

```text
PC
 ↓
Port2
 ↓
Default Route
 ↓
LAN_TO_INTERNET Policy
 ↓
NAT
 ↓
Port1
 ↓
Internet
```

---

# 📊 Final Verification

| Test | Status |
|------|--------|
| FortiGate Boot | ✅ |
| Hostname Configuration | ✅ |
| Port1 WAN Connectivity | ✅ |
| Port2 LAN Configuration | ✅ |
| DHCP Configuration | ✅ |
| Client → Gateway | ✅ |
| Default Route | ✅ |
| Firewall Policy | ✅ |
| Source NAT | ✅ |
| Traffic Logging | ✅ |
| Client → Internet | ✅ |
| FortiView Session Monitoring | ✅ |

---

# 🎓 Key Takeaways

- A **Firewall Policy** controls whether traffic is allowed or denied.
- **Routing** determines where traffic should be forwarded.
- A **default route** is required for Internet-bound traffic when no more specific route exists.
- **Source NAT** changes the source IP address of outbound traffic.
- **PAT** allows multiple private clients to share a single WAN IP.
- **Use Outgoing Interface Address** can be used for PAT.
- FortiView can be used to monitor active sessions.
- Traffic logging is useful for troubleshooting.
- Routing, firewall policy, and NAT must work together for successful LAN-to-Internet connectivity.


---

# ➡️ Next Lab

**Lab 04 – FortiGate Security Profiles**
