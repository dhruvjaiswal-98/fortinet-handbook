# 🧪 Lab 04 – Inter-VLAN Routing using FortiGate (802.1Q Trunk)

---

# 📌 Overview

This lab demonstrates how to configure **Inter-VLAN Routing using FortiGate** with an **802.1Q VLAN trunk** between a Cisco switch and FortiGate.

FortiGate is configured as the **Layer 3 gateway** for multiple VLANs. A single physical interface on the FortiGate carries traffic for multiple VLANs using 802.1Q tagging.

In this lab:

- VLAN 10 is created for the SALES network.
- VLAN 20 is created for the HR network.
- VLAN 999 is configured as the Native VLAN.
- A trunk link is established between the Cisco switch and FortiGate.
- VLAN subinterfaces are created on FortiGate.
- FortiGate provides DHCP services to both VLANs.
- Firewall policies are created to provide Internet access.
- Additional firewall policies are created to allow communication between VLAN 10 and VLAN 20.
- NAT is disabled for internal Inter-VLAN communication.
- FortiView is used to monitor traffic and policy hits.

This lab demonstrates how a FortiGate can perform both **routing and security enforcement** between internal VLANs.

---

# 🎯 Objectives

At the end of this lab you will be able to:

- Understand VLAN segmentation
- Configure VLAN 10
- Configure VLAN 20
- Configure a Native VLAN
- Configure an 802.1Q trunk
- Configure Cisco switch access ports
- Create VLAN interfaces on FortiGate
- Configure FortiGate as a Layer 3 gateway
- Configure DHCP for multiple VLANs
- Configure VLAN-to-WAN firewall policies
- Configure Inter-VLAN firewall policies
- Understand why NAT is not required between internal VLANs
- Verify Inter-VLAN connectivity
- Verify Internet connectivity
- Monitor traffic using FortiView
- Troubleshoot VLAN and routing issues

---

# 🛠 Technologies Used

- FortiGate VM
- FortiOS 7.0.5
- EVE-NG
- Cisco IOS Switch
- VPC
- VLAN
- IEEE 802.1Q
- Trunking
- Inter-VLAN Routing
- DHCP
- Firewall Policies
- NAT
- FortiView

---

# 📚 Skills Gained

- VLAN Configuration
- 802.1Q Trunk Configuration
- Cisco Switch Configuration
- FortiGate VLAN Interface Configuration
- Layer 3 Routing
- DHCP Configuration
- Firewall Policy Configuration
- Inter-VLAN Routing
- Source NAT Configuration
- Traffic Monitoring
- FortiView Analysis
- Network Troubleshooting

---

# 🌐 Lab Topology

<img width="590" height="327" alt="image" src="https://github.com/user-attachments/assets/3a3b229d-ecb7-41b3-92ec-2286dc0c9404" />

---

# 🖥 Lab Addressing

## WAN Network

| Device | Interface | IP Address |
|--------|-----------|------------|
| FortiGate | Port1 | DHCP |
| Upstream Gateway | - | DHCP |

---

## VLAN 10 – SALES

| Device | Interface | IP Address |
|--------|-----------|------------|
| FortiGate | Port2.10 | 192.168.1.1/24 |
| VPC4 | NIC | DHCP |
| Network | - | 192.168.1.0/24 |

---

## VLAN 20 – HR

| Device | Interface | IP Address |
|--------|-----------|------------|
| FortiGate | Port2.20 | 192.168.2.1/24 |
| VPC3 | NIC | DHCP |
| Network | - | 192.168.2.0/24 |

---

# 🏷 VLAN Plan

| VLAN | Name | Network | Purpose |
|------|------|---------|---------|
| 10 | SALES | 192.168.1.0/24 | Sales Users |
| 20 | HR | 192.168.2.0/24 | HR Users |
| 999 | NATIVE | N/A | Native VLAN |

---

# 💡 Why Use VLANs?

VLANs logically divide a Layer 2 network into separate broadcast domains.

For example:

```text
             Cisco Switch
                  |
        +---------+---------+
        |                   |
     VLAN 10              VLAN 20
      SALES                 HR
        |                   |
   192.168.1.0/24     192.168.2.0/24
```

This provides:

- Network segmentation
- Smaller broadcast domains
- Improved security
- Better network organization
- Easier policy enforcement

---

# 📝 Step 1 — Deploy FortiGate VM

For this lab, FortiGate VM was deployed in EVE-NG.

The FortiGate image used for this lab was:

```text
FortiOS 7.0.5
```

The image provides the interfaces required for this lab.

Start the FortiGate and allow the system to boot completely.

### Screenshot

<img width="558" height="322" alt="image" src="https://github.com/user-attachments/assets/87be6e5a-d0c8-47db-bebc-5f1476d44d63" />

---

# 📝 Step 2 — Configure Hostname

Configure a meaningful hostname for easier identification.

### CLI

```bash
config system global
set hostname FW
end
```

### Screenshot

<img width="505" height="146" alt="image" src="https://github.com/user-attachments/assets/e5b6b093-736f-49d6-a7e6-d287ff0259df" />

---

# 📝 Step 3 — Configure Port1

Port1 is used as the WAN/Management interface.

For this lab, Port1 receives its IP address through DHCP.

### CLI

```bash
config system interface
edit port1
set mode dhcp
set allowaccess ping https ssh
next
end
```

Verify the interface:

```bash
get system interface physical
```

### Screenshot

<img width="607" height="202" alt="image" src="https://github.com/user-attachments/assets/e063ac09-cc8f-4cb9-b77e-d8446d6ab44e" />

---

# 📝 Step 4 — Verify WAN Connectivity

Test connectivity between the FortiGate and the laptop/upstream network.

```bash
execute ping <Laptop-IP>
```

A successful response confirms that the FortiGate can reach the upstream network.

### Screenshot

<img width="612" height="183" alt="image" src="https://github.com/user-attachments/assets/487d6483-6f06-4be6-8dd6-4a0c2c51bf93" />

---

# 📝 Step 5 — Verify FortiGate Interfaces

At this stage Port1 should have an IP address while Port2 does not require a Layer 3 IP address.

Port2 will be used as the **802.1Q trunk interface**.

The VLAN subinterfaces will later provide the Layer 3 gateways.

Conceptually:

```text
FortiGate Port2
      |
      +---- VLAN 10 → 192.168.1.1/24
      |
      +---- VLAN 20 → 192.168.2.1/24
```

---

# 📝 Step 6 — Configure the Cisco Switch

The Cisco switch is configured with:

- VLAN 10
- VLAN 20
- VLAN 999
- Trunk port
- Access port for VLAN 10
- Access port for VLAN 20

---

## Create VLAN 10

```bash
Switch(config)# vlan 10
Switch(config-vlan)# name SALES
Switch(config-vlan)# exit
```

---

## Create VLAN 20

```bash
Switch(config)# vlan 20
Switch(config-vlan)# name HR
Switch(config-vlan)# exit
```

---

## Create Native VLAN 999

```bash
Switch(config)# vlan 999
Switch(config-vlan)# name NATIVE
Switch(config-vlan)# exit
```

---

# 📝 Step 7 — Configure Trunk Port

Configure the switch interface connected to FortiGate Port2 as an 802.1Q trunk.

Example:

```bash
Switch(config)# interface gi0/0
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk native vlan 999
Switch(config-if)# switchport trunk allowed vlan 10,20,999
Switch(config-if)# no shutdown
Switch(config-if)# exit
```

The trunk carries:

```text
VLAN 10
VLAN 20
VLAN 999
```

between the Cisco switch and FortiGate.

---

# 📝 Step 8 — Configure Access Port for VLAN 10

Configure Gi0/1 as an access port for SALES.

```bash
Switch(config)# interface gi0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# spanning-tree portfast
Switch(config-if)# no shutdown
Switch(config-if)# exit
```

---

# 📝 Step 9 — Configure Access Port for VLAN 20

Configure Gi0/2 as an access port for HR.

```bash
Switch(config)# interface gi0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20
Switch(config-if)# spanning-tree portfast
Switch(config-if)# no shutdown
Switch(config-if)# exit
```

---

# 📝 Step 10 — Verify Switch VLAN Configuration

Verify the VLANs:

```bash
show vlan brief
```

Verify the trunk:

```bash
show interfaces trunk
```

The output should show VLAN 10 and VLAN 20 allowed on the trunk.

---

# 📝 Step 11 — Save Switch Configuration

The switch configuration must be saved so that it survives a reboot.

Two commonly used commands are:

```bash
write memory
```

or:

```bash
copy running-config startup-config
```
<img width="506" height="357" alt="image" src="https://github.com/user-attachments/assets/709cfc79-12bd-4701-8245-ee653f62014b" />

<img width="770" height="226" alt="image" src="https://github.com/user-attachments/assets/4ed68eef-80d2-4caa-9c55-b493adedaaca" />

---

# 📝 Step 12 — Create VLAN 10 Interface on FortiGate

Navigate to:

```text
Network
   ↓
Interfaces
   ↓
Create New
   ↓
Interface
```

Create a VLAN interface with the following configuration:

| Setting | Value |
|---------|-------|
| Name | VLAN10-SALES |
| Type | VLAN |
| Interface | Port2 |
| VLAN ID | 10 |
| IP Address | 192.168.1.1/24 |
| Role | LAN |

This interface becomes the default gateway for VLAN 10.

---

# 📝 Step 13 — Enable DHCP for VLAN 10

Enable DHCP Server on the VLAN 10 interface.

Example DHCP configuration:

| Parameter | Value |
|-----------|-------|
| DHCP Status | Enabled |
| Network | 192.168.1.0/24 |
| Gateway | 192.168.1.1 |
| DHCP Range | 192.168.1.2 – 192.168.1.254 |
| Netmask | 255.255.255.0 |

FortiGate will now provide IP addresses automatically to SALES clients.


---

# 📝 Step 14 — Create VLAN 20 Interface on FortiGate

Create another VLAN interface.

| Setting | Value |
|---------|-------|
| Name | VLAN20-HR |
| Type | VLAN |
| Interface | Port2 |
| VLAN ID | 20 |
| IP Address | 192.168.2.1/24 |
| Role | LAN |

This interface becomes the default gateway for VLAN 20.

---

# 📝 Step 15 — Enable DHCP for VLAN 20

Enable DHCP Server on VLAN 20.

| Parameter | Value |
|-----------|-------|
| DHCP Status | Enabled |
| Network | 192.168.2.0/24 |
| Gateway | 192.168.2.1 |
| DHCP Range | 192.168.2.2 – 192.168.2.254 |
| Netmask | 255.255.255.0 |

### Screenshot

<img width="720" height="575" alt="image" src="https://github.com/user-attachments/assets/ce106a25-ee37-4d3f-947e-52b57761f6fa" />

<img width="685" height="527" alt="image" src="https://github.com/user-attachments/assets/5ba13e14-8060-49b1-b7aa-d799ad16eaef" />


---

# 📝 Step 16 — Verify VLAN Interfaces

After creating both VLAN interfaces, the FortiGate interface list should contain:

```text
Port1
Port2
VLAN10-SALES
VLAN20-HR

```

Conceptually:

```text
                 FortiGate
                     |
                   Port2
                     |
             802.1Q Trunk
              /          \
             /            \
        VLAN 10          VLAN 20
       192.168.1.1     192.168.2.1
            |                 |
          SALES               HR
```

### Screenshot

![FortiGate VLAN Interfaces](images/07-vlan-interfaces.png)

---

# 📝 Step 17 — Configure VPC for VLAN 10

Connect VPC4 to the switch access port configured for VLAN 10.

Configure the VPC as a DHCP client:

```text
dhcp
```

The VPC should receive an IP address from the FortiGate DHCP server.

Example:

```text
VPC4
IP Address : 192.168.1.x
Gateway    : 192.168.1.1
```

Verify:

```text
show ip
```

---

# 📝 Step 18 — Test VLAN 10 Gateway

From VPC4:

```text
ping 192.168.1.1
```

The FortiGate VLAN 10 interface should respond.

> 💡 **Observation**
>
> The first ping may sometimes result in an RTO while ARP resolution is taking place. Subsequent pings should succeed.

### Screenshot

<img width="1280" height="352" alt="image" src="https://github.com/user-attachments/assets/72cecd1b-5a71-4006-abf2-4d90c786897c" />

---

# 📝 Step 19 — Configure VPC for VLAN 20

Connect VPC3 to the switch access port configured for VLAN 20.

Configure the VPC as a DHCP client:

```text
dhcp
```

The VPC should receive an IP address from the FortiGate DHCP server.

Example:

```text
VPC3
IP Address : 192.168.20.x
Gateway    : 192.168.20.1
```

Verify:

```text
show ip
```

---

# 📝 Step 20 — Test VLAN 20 Gateway

From VPC3:

```text
ping 192.168.20.1
```

The FortiGate VLAN 20 interface should respond.

### Screenshot

<img width="1287" height="456" alt="image" src="https://github.com/user-attachments/assets/e86aa2b6-17ce-4884-9824-f798af91cd4a" />

---

# 📝 Step 21 — Test Internet Connectivity

At this stage both VLANs have connectivity to their respective FortiGate gateways.

However, Internet access will not work yet because the required firewall policies have not been created.

```text
VLAN 10
   |
   v
FortiGate
   |
   X
Internet

VLAN 20
   |
   v
FortiGate
   |
   X
Internet
```

---

# 📝 Step 22 — Create VLAN 10 to WAN Policy

Create a firewall policy allowing VLAN 10 users to access the Internet.

| Setting | Value |
|---------|-------|
| Policy Name | VLAN10-to-WAN |
| Incoming Interface | VLAN10-SALES |
| Outgoing Interface | Port1 |
| Source | all |
| Destination | all |
| Service | ALL |
| Action | ACCEPT |
| NAT | Enabled |
| Log Allowed Traffic | All Sessions |

Enable logging for all sessions to make troubleshooting and traffic analysis easier.

### Screenshot

<img width="828" height="600" alt="image" src="https://github.com/user-attachments/assets/b9c6776e-4749-4d3b-a9a7-6c1b0a6a1dc5" />

<img width="873" height="408" alt="image" src="https://github.com/user-attachments/assets/bcb22941-124d-4a7f-b42f-4ac839a818bf" />

---

# 📝 Step 23 — Create VLAN 20 to WAN Policy

Create a similar firewall policy for VLAN 20.

| Setting | Value |
|---------|-------|
| Policy Name | VLAN20-to-WAN |
| Incoming Interface | VLAN20-HR |
| Outgoing Interface | Port1 |
| Source | all |
| Destination | all |
| Service | ALL |
| Action | ACCEPT |
| NAT | Enabled |
| Log Allowed Traffic | All Sessions |

---

# 📝 Step 24 — Verify Internet Connectivity

After creating the VLAN-to-WAN policies:

### From VLAN 10

```text
ping 8.8.8.8
```

### From VLAN 20

```text
ping 8.8.8.8
```

Both VLANs should now be able to access the Internet through FortiGate.

The traffic flow is:

```text
VLAN 10
192.168.10.0/24
       |
       v
FortiGate
       |
     NAT
       |
       v
   Internet
```

and:

```text
VLAN 20
192.168.20.0/24
       |
       v
FortiGate
       |
     NAT
       |
       v
   Internet
```

---

# 📝 Step 25 — Understanding Inter-VLAN Routing

Now we configure communication between VLAN 10 and VLAN 20.

This process is known as:

# 🔀 Inter-VLAN Routing

Inter-VLAN routing allows devices located in different VLANs and different IP networks to communicate with each other through a Layer 3 device.

In this lab, **FortiGate performs the Layer 3 routing**.

---

# 📝 Step 26 — Test Inter-VLAN Connectivity Before Policy

From the VLAN 20 client, attempt to ping the VLAN 10 client.

Example:

```text
VPC3 → VPC4
```

```text
ping 192.168.10.x
```

The traffic will fail because a FortiGate firewall policy has not yet permitted communication between the two VLANs.

This demonstrates an important FortiGate behavior:

> Routing information alone does not automatically permit traffic through the firewall.

---

# 📝 Step 27 — Create VLAN 10 to VLAN 20 Policy

Create a firewall policy allowing VLAN 10 users to communicate with VLAN 20.

| Setting | Value |
|---------|-------|
| Policy Name | VLAN10-to-VLAN20 |
| Incoming Interface | VLAN10-SALES |
| Outgoing Interface | VLAN20-HR |
| Source | all |
| Destination | all |
| Service | ALL |
| Action | ACCEPT |
| NAT | Disabled |
| Log Allowed Traffic | All Sessions |

### Important

**NAT must remain disabled.**

Both networks are internal private networks and FortiGate is routing between them directly.

---

# 📝 Step 28 — Create VLAN 20 to VLAN 10 Policy

Create the reverse policy.

| Setting | Value |
|---------|-------|
| Policy Name | VLAN20-to-VLAN10 |
| Incoming Interface | VLAN20-HR |
| Outgoing Interface | VLAN10-SALES |
| Source | all |
| Destination | all |
| Service | ALL |
| Action | ACCEPT |
| NAT | Disabled |
| Log Allowed Traffic | All Sessions |

### Screenshot

<img width="925" height="656" alt="image" src="https://github.com/user-attachments/assets/c8676486-c94f-4fec-9e44-dc5a99b976bc" />

---

# 📝 Step 29 — Verify Inter-VLAN Connectivity

Now test communication between the VLANs.

### VLAN 10 → VLAN 20

```text
ping 192.168.20.x
```

### VLAN 20 → VLAN 10

```text
ping 192.168.10.x
```

The traffic should now succeed.

The traffic flow is:

```text
VLAN 10
192.168.10.0/24
       |
       v
FortiGate
       |
       v
VLAN 20
192.168.20.0/24
```

FortiGate routes the traffic and applies the configured firewall policies.

---

# 📝 Step 30 — Final Firewall Policy Structure

After completing the lab, the FortiGate policy table should contain policies similar to:

| Policy | Source | Destination | NAT |
|--------|--------|-------------|-----|
| VLAN10-to-WAN | VLAN10 | WAN | ✅ |
| VLAN20-to-WAN | VLAN20 | WAN | ✅ |
| VLAN10-to-VLAN20 | VLAN10 | VLAN20 | ❌ |
| VLAN20-to-VLAN10 | VLAN20 | VLAN10 | ❌ |

The exact policy order may vary depending on the final configuration.

---

# 📝 Step 31 — Monitor Traffic using FortiView

FortiView can be used to analyze traffic generated by the VLAN clients.

Navigate to:

```text
FortiView
```

---

## FortiView Sources

The **Sources** view can be used to identify:

- Source IP addresses
- Traffic volume
- Sessions
- Applications
- Network activity

This helps identify which devices are generating traffic.

### Screenshot

<img width="1343" height="395" alt="image" src="https://github.com/user-attachments/assets/44ba1753-bfe6-4920-8db9-71a2f91fbc52" />

---

## FortiView Policies

The **Policies** view can be used to identify:

- Which firewall policies are being hit
- Traffic volume
- Source devices
- Destination traffic
- Policy activity

This is extremely useful during troubleshooting.

### Screenshot

<img width="1376" height="452" alt="image" src="https://github.com/user-attachments/assets/b88ad756-ecec-4740-bbf5-bb3fddc53a26" />

---

# 🔍 Troubleshooting

If VLAN clients cannot communicate or obtain IP addresses, verify the following.

### Switch Verification

```bash
show vlan brief
```

```bash
show interfaces trunk
```

Verify that:

- VLAN 10 exists
- VLAN 20 exists
- VLAN 999 exists
- Gi0/0 is configured as trunk
- VLAN 10 and VLAN 20 are allowed
- Access ports are assigned to the correct VLAN

---

### FortiGate Verification

Check interfaces:

```bash
show system interface
```

Check routing:

```bash
get router info routing-table all
```

Check DHCP:

```bash
show system dhcp server
```

Check firewall policies:

```bash
show firewall policy
```

---

# 🚨 Common Problems

## Problem 1 — Client Does Not Receive DHCP Address

Check:

- Access VLAN configuration
- Trunk configuration
- VLAN ID
- FortiGate VLAN interface
- DHCP configuration
- DHCP address pool

---

## Problem 2 — Client Can Reach Gateway but Not Internet

Check:

- Default route
- VLAN-to-WAN firewall policy
- NAT
- WAN connectivity
- DNS
- Policy order

---

## Problem 3 — VLAN 10 Cannot Reach VLAN 20

Check:

- VLAN10-to-VLAN20 firewall policy
- VLAN20-to-VLAN10 firewall policy
- NAT is disabled
- Correct source interface
- Correct destination interface
- Client IP addresses
- FortiGate routing table

---

## Problem 4 — Trunk Not Working

Check:

```bash
show interfaces trunk
```

Verify that:

```text
VLAN 10
VLAN 20
VLAN 999
```

are allowed on the trunk.

Also verify that the native VLAN matches on both sides.

---

# 📊 Verification Matrix

| Test | Expected Result | Status |
|------|------------------|--------|
| FortiGate Port1 receives IP | Successful | ✅ |
| Port1 reaches upstream | Successful | ✅ |
| VLAN 10 created | Successful | ✅ |
| VLAN 20 created | Successful | ✅ |
| VLAN 999 created | Successful | ✅ |
| 802.1Q trunk | Successful | ✅ |
| VLAN 10 DHCP | Successful | ✅ |
| VLAN 20 DHCP | Successful | ✅ |
| VLAN 10 → Gateway | Successful | ✅ |
| VLAN 20 → Gateway | Successful | ✅ |
| VLAN 10 → Internet | Successful | ✅ |
| VLAN 20 → Internet | Successful | ✅ |
| VLAN 10 → VLAN 20 | Successful | ✅ |
| VLAN 20 → VLAN 10 | Successful | ✅ |
| FortiView Traffic | Visible | ✅ |

---

# 🎓 Key Takeaways

- VLANs divide a Layer 2 network into separate broadcast domains.
- Access ports carry traffic for a single VLAN.
- Trunk ports carry multiple VLANs using 802.1Q tagging.
- FortiGate can create VLAN subinterfaces on a physical interface.
- Each VLAN interface can act as the default gateway for its VLAN.
- FortiGate can provide DHCP services for multiple VLANs.
- FortiGate performs Layer 3 routing between VLANs.
- Routing between VLANs requires appropriate FortiGate firewall policies.
- NAT is required when private networks access the Internet.
- NAT is normally not required for internal Inter-VLAN communication.
- The native VLAN can be changed from the default VLAN 1 to an unused VLAN such as VLAN 999.
- FortiView provides useful visibility into sources, policies, sessions, and traffic.
- Firewall logging is extremely useful for troubleshooting and traffic analysis.

---

# 🧠 Important Concepts

## VLAN

A VLAN logically separates a physical Layer 2 network into multiple broadcast domains.

---

## Access Port

An access port carries traffic for a single VLAN and normally sends and receives untagged Ethernet frames.

Example:

```text
Gi0/1
   |
   +---- VLAN 10
```

---

## Trunk Port

A trunk port carries multiple VLANs over a single physical connection using 802.1Q tagging.

Example:

```text
FortiGate Port2
      |
      | 802.1Q
      |
Cisco Gi0/0
      |
  +---+---+
  |       |
VLAN10  VLAN20
```

---

## IEEE 802.1Q

802.1Q is the VLAN tagging standard used to identify VLAN traffic across trunk links.

---

## Native VLAN

The native VLAN is the VLAN whose frames are transmitted without an 802.1Q tag on a trunk.

In this lab:

```text
Native VLAN = 999
```

---

## Why Change VLAN 1?

VLAN 1 is the default VLAN on Cisco switches and is commonly used by various control protocols.

Using an unused VLAN such as VLAN 999 as the native VLAN is a common security hardening practice.

However, changing the native VLAN alone does **not** provide complete switch security. Other switch hardening controls should also be implemented.

---

## Inter-VLAN Routing

Inter-VLAN routing allows devices in different VLANs to communicate through a Layer 3 device.

In this lab:

```text
VLAN 10
192.168.10.0/24
       |
       v
FortiGate
       |
       v
VLAN 20
192.168.20.0/24
```

FortiGate performs the routing and security enforcement.


# 📊 Final Lab Result

The lab successfully demonstrates:

```text
                 INTERNET
                     |
                  Port1
                     |
               +-----------+
               | FortiGate |
               +-----+-----+
                     |
                  Port2
                     |
                802.1Q Trunk
                     |
               Cisco Switch
                /         \
               /           \
          VLAN 10          VLAN 20
           SALES             HR
             |                |
            VPC              VPC
             |                |
             +-------+--------+
                     |
              Inter-VLAN Routing
```

Both VLANs can:

```text
✅ Obtain IP addresses using DHCP
✅ Reach their FortiGate gateway
✅ Access the Internet
✅ Communicate with each other
✅ Be monitored through FortiView
```

---

---

# 📝 Lab Status

```text
LAB COMPLETED ✅

FortiGate VLAN Configuration       ✅
Cisco 802.1Q Trunk                 ✅
VLAN 10 – SALES                    ✅
VLAN 20 – HR                       ✅
Native VLAN 999                    ✅
DHCP                                ✅
Internet Access                    ✅
Inter-VLAN Routing                 ✅
Firewall Policies                  ✅
FortiView Monitoring               ✅
```

