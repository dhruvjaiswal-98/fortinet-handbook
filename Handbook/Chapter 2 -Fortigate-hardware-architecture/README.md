# Chapter 2 – FortiGate Hardware Architecture

> **Difficulty:** 🟢 Beginner → 🟡 Intermediate  
> **Prerequisites:** Basic Networking Knowledge and Chapter 1 – Fortinet Company & Security Fabric

---

# 📖 Overview

This chapter introduces **FortiGate**, Fortinet's flagship Next-Generation Firewall (NGFW), and explains the hardware architecture that allows FortiGate appliances to process and inspect network traffic efficiently.

FortiGate combines traditional firewall capabilities with advanced security technologies such as **Intrusion Prevention System (IPS), Antivirus, Web Filtering, Application Control, VPN, SD-WAN, SSL Inspection, and Zero Trust Network Access (ZTNA)**.

One of the major architectural differences between FortiGate and many traditional firewall platforms is Fortinet's use of purpose-built **Security Processing Units (SPUs)** and **FortiASIC technology** to accelerate networking and security workloads.

Understanding FortiGate hardware architecture is important because concepts such as **CPU processing, ASIC acceleration, session handling, packet flow, throughput, and operating modes** directly affect firewall performance and troubleshooting.

---

# 🎯 Learning Objectives

After completing this chapter, you should be able to:

- Explain what FortiGate is
- Understand the role of FortiGate within the Security Fabric
- Identify different FortiGate product families
- Identify major FortiGate hardware components
- Explain FortiASIC technology
- Understand the purpose of Network Processors (NP)
- Understand the purpose of Content Processors (CP)
- Explain the basic FortiGate packet processing flow
- Understand how FortiGate maintains sessions
- Explain the FortiGate session lifecycle
- Differentiate NAT Mode and Transparent Mode
- Understand important FortiGate throughput metrics
- Differentiate FortiCare and FortiGuard services
- Understand common FortiGate deployment models

---

# 📑 Table of Contents

- 2.1 What is FortiGate?
- 2.2 Why FortiGate is Different
- 2.3 FortiGate Product Families
- 2.4 FortiGate Hardware Components
- 2.5 FortiASIC Technology
- 2.6 Network Processor (NP)
- 2.7 Content Processor (CP)
- 2.8 FortiGate Packet Processing Flow
- 2.9 Session Table
- 2.10 Session Lifecycle
- 2.11 FortiGate Operating Modes
- 2.12 Throughput Metrics
- 2.13 FortiGate Licensing and Security Services
- 2.14 FortiGate Deployment Models
- 2.15 Important Hardware Terms
- Key Takeaways

---

# 2.1 What is FortiGate?

**FortiGate** is Fortinet's flagship **Next-Generation Firewall (NGFW)** platform.

<img width="450" height="265" alt="image" src="https://github.com/user-attachments/assets/e5fe224f-4ecc-4c90-98a9-07d57db61ed7" />

Traditional firewalls primarily control traffic based on information such as:

- Source IP address
- Destination IP address
- Protocol
- Source port
- Destination port

Modern networks require much deeper security inspection.


FortiGate therefore combines networking and security functions within a single platform.

## Core FortiGate Capabilities

| Capability | Purpose |
|------------|---------|
| Firewall | Controls traffic between networks |
| IPS | Detects and blocks network attacks |
| Antivirus | Detects malicious files and malware |
| Web Filtering | Controls access to websites |
| Application Control | Identifies and controls applications |
| IPsec VPN | Provides encrypted site-to-site and remote connectivity |
| SSL VPN | Provides secure remote access where supported |
| SD-WAN | Intelligently manages multiple WAN connections |
| SSL Inspection | Inspects encrypted traffic according to policy |
| ZTNA | Provides identity- and context-aware application access |

FortiGate therefore performs much more than basic packet filtering.

Within many Fortinet environments, FortiGate also acts as a central enforcement point within the **Fortinet Security Fabric**.

---

> 💡 **Key Concept**
>
> Think of FortiGate as both a **network gateway** and a **security enforcement platform**. It can route traffic while simultaneously applying firewall policies, security inspection, VPN services, and other controls.

---

# 2.2 Why FortiGate is Different

Network security devices must perform several operations on traffic.

For example, a firewall may need to:

1. Receive a packet
2. Identify an existing session
3. Determine where the packet should go
4. Match a firewall policy
5. Apply NAT
6. Inspect the traffic
7. Encrypt or decrypt traffic when required
8. Forward the packet

Performing all of these operations using only a general-purpose CPU can consume significant processing resources.

Fortinet addresses this challenge using purpose-built hardware acceleration technologies known collectively as **FortiASICs** and **Security Processing Units (SPUs)**.

---

## General-Purpose Processing

A simplified software-oriented architecture may look like:

```text
Network Traffic
      │
      ▼
     CPU
      │
      ├── Routing
      ├── Session Processing
      ├── NAT
      ├── VPN
      ├── IPS
      ├── Application Inspection
      └── Other Processing
      │
      ▼
Forward Traffic
```

As traffic and security inspection increase, CPU utilization can also increase.

---

## FortiGate Hardware-Accelerated Processing

On supported FortiGate platforms, eligible workloads can be accelerated by specialized hardware.

```text
                   ┌─────────────────────┐
                   │      FortiGate      │
                   └──────────┬──────────┘
                              │
                ┌─────────────┼─────────────┐
                │             │             │
                ▼             ▼             ▼
              CPU            NP            CP
                │             │             │
         System Control    Network      Content /
         & Management    Acceleration   Security
```

This architecture can reduce the amount of work performed directly by the CPU.

---

## Benefits of Hardware Acceleration

FortiGate hardware acceleration can provide:

- ✅ High firewall throughput
- ✅ Low latency
- ✅ Efficient packet forwarding
- ✅ Improved VPN performance
- ✅ Improved security inspection performance
- ✅ Reduced CPU utilization
- ✅ Better scalability

---

> 💡 **Important**
>
> Not every packet or security function is automatically processed by an ASIC. Hardware acceleration depends on the FortiGate model, FortiOS version, traffic type, configuration, enabled features, and whether the traffic is eligible for offloading.

---

# 2.3 FortiGate Product Families

<img width="1079" height="614" alt="image" src="https://github.com/user-attachments/assets/3cc5cbfb-fb84-46f8-a934-48ae5648ec4f" />

FortiGate is not a single firewall model.

Fortinet provides different appliance families designed for environments ranging from small offices to large enterprises, service providers, and data centers.

A simplified classification is shown below.

| Category | Typical Environment | Example Models |
|----------|--------------------|----------------|
| Desktop / Entry | Small office, retail, branch, lab | 40F, 50G, 60F, 70F, 80F |
| Branch / Mid-Range | Medium business, larger branch | 90G, 100F, 120G, 200F |
| Enterprise | Large organizations and campuses | 400F, 600F, 900G, 1000F |
| High-End / Data Center | High-throughput environments | 1800F, 2600F, 4200F |
| Chassis | Telecom, ISP, very large data centers | FortiGate 6000 and 7000 families |

> **Note:** Fortinet's active model portfolio changes over time. Always verify current models and specifications using the official Fortinet product matrix and datasheets.

---

## 🖥 Desktop / Entry-Level FortiGate

These appliances are commonly used in:

- Small businesses
- Retail stores
- Small branch offices
- Remote offices
- Training environments
- Home labs

Examples include:

```text
FortiGate 40F
FortiGate 50G
FortiGate 60F
FortiGate 70F
FortiGate 80F
```

They provide enterprise security capabilities in a smaller physical form factor.

---

## 🏢 Branch and Mid-Range FortiGate

These models are designed for environments with larger user counts and higher traffic requirements.

Typical deployments include:

- Regional offices
- Medium-sized organizations
- Larger branch locations
- Campus environments

Examples include:

```text
FortiGate 90G
FortiGate 100F
FortiGate 120G
FortiGate 200F
```

---

## 🏦 Enterprise FortiGate

Enterprise platforms are designed for organizations requiring greater:

- Throughput
- Port density
- Concurrent sessions
- VPN capacity
- Redundancy
- Security inspection performance

They may be deployed in:

- Large enterprises
- Financial institutions
- Headquarters
- Large campuses
- Data center environments

Examples include:

```text
FortiGate 400F
FortiGate 600F
FortiGate 900G
FortiGate 1000F
```

---

## 🏭 High-End / Data Center FortiGate

High-end appliances are designed for environments where extremely high traffic volumes must be inspected.

Common environments include:

- Data centers
- Large enterprises
- Service providers
- High-speed network edges

Examples include:

```text
FortiGate 1800F
FortiGate 2600F
FortiGate 4200F
```

---

## 🏗 Chassis Platforms

Fortinet also provides modular chassis-based platforms for very large environments.

Examples include:

```text
FortiGate 6000 Series
FortiGate 7000 Series
```

These platforms are commonly associated with:

- Telecom networks
- Internet service providers
- Large data centers
- Carrier-scale environments

---

# 2.4 FortiGate Hardware Components

<img width="1226" height="449" alt="image" src="https://github.com/user-attachments/assets/4a2755af-faa4-4149-a5f8-aa2de6f7a124" />

Although hardware varies significantly between FortiGate models, a physical FortiGate appliance generally contains several major components.

```text
┌─────────────────────────────────────────┐
│                FORTIGATE                │
├─────────────────────────────────────────┤
│ CPU                                     │
│ RAM                                     │
│ Storage / Flash                         │
│ Network Interfaces                      │
│ Security Processing Hardware            │
│ Power System                            │
└─────────────────────────────────────────┘
```

Depending on the model, specialized FortiASIC components may include dedicated or integrated networking and content-processing capabilities.

---

## 🧠 CPU

The CPU performs general system processing and handles tasks that cannot or should not be hardware-offloaded.

Typical responsibilities include:

- FortiOS system operations
- Management processes
- Control-plane functions
- Routing protocol processing
- Configuration changes
- Logging processes
- Administrative access
- Traffic that is not eligible for hardware acceleration

The CPU remains a critical component even on hardware-accelerated FortiGate platforms.

---

## 💾 RAM

RAM provides temporary working memory for FortiOS.

It is used for information such as:

- Active sessions
- Routing information
- Runtime processes
- Policy-related runtime data
- Security processes
- System operations

One particularly important structure maintained in memory is the **session table**.

---

## 💽 Storage

Depending on the FortiGate model, storage may include flash memory or SSD-based storage.

Storage can be used for:

- FortiOS
- Configuration data
- Firmware
- Local logs
- System files

The exact storage capability depends on the appliance model.

---

## 🔌 Network Interfaces

FortiGate appliances may contain different interface types depending on their performance class.

Examples include:

| Interface | Typical Use |
|-----------|-------------|
| RJ45 GE | Standard Ethernet connectivity |
| SFP | Fiber or copper transceiver connectivity |
| SFP+ | Higher-speed connectivity, commonly 10 GbE |
| SFP28 | Commonly used for 25 GbE |
| QSFP+/QSFP28 | High-speed uplinks such as 40/100 GbE |

Higher-end FortiGate platforms generally provide greater port density and higher-speed interfaces.

---

## ⚡ Security Processing Hardware

Supported FortiGate appliances may include specialized processing hardware designed to accelerate networking and security operations.

Two important concepts are:

### Network Processor (NP)

Optimized for network and packet-processing workloads.

### Content Processor (CP)

Optimized for content and security inspection workloads.

We will examine both in detail later in this chapter.

---

## 🔋 Power Supply

Smaller FortiGate appliances may use a single power source.

Enterprise and data center models may support:

- Redundant power supplies
- Hot-swappable power supplies
- Higher availability designs

Redundant power is particularly important in environments where firewall downtime can affect the entire organization.

---

# 2.5 FortiASIC Technology

**ASIC** stands for:

> **Application-Specific Integrated Circuit**

An ASIC is a processor designed to perform specific tasks efficiently.

A general-purpose CPU must support many different workloads.

An ASIC, by comparison, can be optimized for a narrower set of operations.

Fortinet develops purpose-built **FortiASIC** technology to accelerate networking and security workloads.

---

## Why Use ASICs?

Consider a firewall processing large volumes of traffic.

Without specialized acceleration:

```text
Packets
   │
   ▼
General CPU
   │
   ├── Forwarding
   ├── NAT
   ├── VPN
   ├── Security Processing
   └── Other Tasks
```

The CPU must handle many workloads.

With hardware acceleration, eligible processing can be distributed:

```text
                    FORTIGATE
                        │
          ┌─────────────┼─────────────┐
          │             │             │
          ▼             ▼             ▼
         CPU            NP            CP
          │             │             │
     Control /       Network       Security
     Management    Acceleration   Acceleration
```

This allows the system to process traffic more efficiently.

---

## Security Processing Unit (SPU)

Fortinet commonly uses the term **Security Processing Unit (SPU)** for its purpose-built security processing technology.

Depending on the FortiGate platform and generation, these capabilities may be implemented using different ASIC architectures.

For learning purposes, two important processor concepts are:

```text
SPU / FortiASIC Architecture
           │
      ┌────┴────┐
      │         │
      ▼         ▼
     NP         CP
 Network      Content
Processing   Processing
```

---

> 💡 **Remember**
>
> **CPU = General-purpose processing**
>
> **NP = Network acceleration**
>
> **CP = Content/security acceleration**

---

# 2.6 Network Processor (NP)

**NP** stands for **Network Processor**.

The Network Processor is designed to accelerate eligible network traffic processing.

Examples of Fortinet Network Processor generations include:

- NP6
- NP7

---

## What Does the NP Do?

Depending on the hardware platform, configuration, and traffic eligibility, NP technology can accelerate functions associated with:

- Packet forwarding
- Session processing
- NAT
- Network traffic handling
- Certain VPN operations
- Other supported network-processing functions

---

## Without Network Acceleration

A simplified representation is:

```text
Packet
   │
   ▼
  CPU
   │
   ├── Session Processing
   ├── Forwarding
   ├── NAT
   └── Other Operations
   │
   ▼
Forward
```

As traffic increases, this can place additional load on the CPU.

---

## With NP Acceleration

Eligible traffic can take advantage of specialized processing:

```text
Packet
   │
   ▼
FortiGate
   │
   ▼
NP Acceleration
   │
   ▼
Forward
```

The exact processing path depends on the traffic and configuration.

---

> ⚠️ **Important**
>
> NP offloading is not simply "all packets bypass the CPU." A session normally requires software processing before eligible traffic can benefit from hardware acceleration, and some features or traffic conditions can prevent offloading.

---

# 2.7 Content Processor (CP)

**CP** stands for **Content Processor**.

While the NP focuses primarily on network-processing acceleration, CP technology is designed to accelerate certain security and content-inspection operations.

Examples of Content Processor generations include:

- CP8
- CP9

---

## CP Responsibilities

Depending on the FortiGate platform and inspection mode, Content Processor technology may assist with workloads associated with:

- Intrusion Prevention System (IPS)
- Antivirus
- Application identification
- Pattern matching
- Security inspection
- Cryptographic/security-related processing on supported architectures

SSL/TLS inspection involves several processing stages, so it should not be understood as simply "all SSL inspection happens on the CP."

---

## Conceptual Architecture

```text
                    Incoming Traffic
                           │
                           ▼
                      FortiGate
                           │
               ┌───────────┴───────────┐
               │                       │
               ▼                       ▼
        Network Processing      Security Inspection
               │                       │
               ▼                       ▼
              NP                      CP
               │                       │
               └───────────┬───────────┘
                           │
                           ▼
                     Forward Traffic
```

This is a conceptual representation rather than an exact packet path for every FortiGate model.

---

# 2.8 FortiGate Packet Processing Flow

<img width="800" height="908" alt="image" src="https://github.com/user-attachments/assets/4eb47f35-d3dd-4124-b03c-8cfe38090c44" />

Understanding packet flow is one of the most important FortiGate concepts.

FortiGate is a **stateful firewall**.

This means it does not treat every packet as completely independent.

Instead, FortiGate tracks network connections using **sessions**.

---

## Simplified First-Packet Flow

A useful conceptual flow for new traffic is:

```text
┌─────────────────────┐
│   Packet Received   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Interface Processing│
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│   Session Lookup    │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Routing / Forwarding│
│      Decision       │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Firewall Policy     │
│      Match          │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Security Inspection │
│   When Configured   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ NAT / Translation   │
│   When Required     │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Traffic Forwarding  │
└─────────────────────┘
```

This is intentionally simplified.

Actual FortiOS packet flow contains additional checks and the precise order can depend on features such as:

- DNAT/VIP
- Central NAT
- Policy routes
- Local-in traffic
- IPsec
- Security profiles
- Proxy-based inspection
- Flow-based inspection
- Hardware offloading

These topics will be covered in later chapters.

---

## Existing Session Traffic

Once a valid session exists, FortiGate can process subsequent packets using information already stored in the session.

Conceptually:

```text
Packet Arrives
      │
      ▼
Session Lookup
      │
      ├── No Session ──► New Session Processing
      │
      └── Existing Session
                    │
                    ▼
             Process According
              to Session State
                    │
                    ▼
                  Forward
```

This is one reason the session table is extremely important when troubleshooting FortiGate traffic.

---

# 2.9 Session Table

The **session table** maintains information about active network connections passing through FortiGate.

For example, suppose a client accesses a website:

```text
Client
192.168.1.10

      │
      │ HTTPS
      ▼

FortiGate

      │
      ▼

Web Server
142.250.183.46
```

FortiGate creates a session containing information associated with that connection.

---

## Session Information

A session can contain information such as:

- Source IP address
- Destination IP address
- Source port
- Destination port
- Protocol
- Session state
- Firewall policy information
- NAT information
- Interfaces
- Timeout information
- Hardware offload state where applicable

Example:

```text
Source
192.168.1.10:52344

Destination
142.250.183.46:443

Protocol
TCP

State
ESTABLISHED
```

---

## Why Sessions Matter

Without state tracking, the firewall would need to perform much more processing for every individual packet.

Session tracking allows FortiGate to understand:

> "This packet belongs to a connection that I already know about."

That makes stateful inspection and efficient forwarding possible.

---

> 💡 **Troubleshooting Concept**
>
> When traffic is not behaving as expected, checking whether FortiGate created a session can help determine how far the traffic progressed through firewall processing.

---

# 2.10 Session Lifecycle

A FortiGate session goes through a lifecycle.

```text
New Traffic
     │
     ▼
Session Creation
     │
     ▼
Session Active
     │
     ▼
Idle / Closing
     │
     ▼
Session Timeout or Termination
     │
     ▼
Session Removed
```

---

## Stage 1 – Session Creation

A new connection reaches FortiGate.

FortiGate evaluates the traffic and, if permitted, creates session state for the connection.

---

## Stage 2 – Active Session

Traffic flows between the communicating devices.

FortiGate tracks the connection using the session table.

---

## Stage 3 – Session Closing or Inactivity

A session may begin closing normally, or traffic may stop.

FortiGate maintains timeout mechanisms for different protocols and session states.

---

## Stage 4 – Session Removal

When the connection terminates or the relevant timeout expires, the session entry is removed and its resources are released.

---

# 2.11 FortiGate Operating Modes

FortiGate commonly operates in one of two major deployment modes:

1. **NAT/Route Mode**
2. **Transparent Mode**

---

## 🌐 NAT/Route Mode

NAT/Route mode is the most common FortiGate deployment mode.

In this mode, FortiGate operates as a Layer 3 device.

Interfaces normally have IP addresses and FortiGate can participate in routing.

Example:

```text
LAN
 │
 ▼
FortiGate
 │
 ▼
Internet
```

### Capabilities

- Layer 3 routing
- Static routing
- Dynamic routing
- NAT
- Firewall policies
- VPN
- Security inspection
- SD-WAN

---

## 🔍 Transparent Mode

In Transparent Mode, FortiGate is inserted into the network primarily as a Layer 2 security device.

A simplified topology is:

```text
LAN / Switch
      │
      ▼
  FortiGate
 Transparent
      │
      ▼
    Router
      │
      ▼
   Internet
```

FortiGate can inspect and enforce security policies without acting as the primary Layer 3 router for the transit traffic.

A management IP is still required for administration.

---

## NAT/Route vs Transparent Mode

| Feature | NAT/Route Mode | Transparent Mode |
|---------|----------------|------------------|
| Primary Forwarding Role | Layer 3 | Layer 2 |
| Routing | Yes | Not the primary transit function |
| NAT | Supported | Generally not used for transparent transit |
| Security Inspection | Yes | Yes |
| Firewall Policies | Yes | Yes |
| Common Deployment | Gateway Firewall | Inline Firewall |
| Network Redesign | May be required | Often easier to insert inline |

---

> 💡 **Key Concept**
>
> Use **NAT/Route Mode** when FortiGate should act as a Layer 3 gateway.
>
> Use **Transparent Mode** when FortiGate needs to be inserted inline with minimal changes to the existing Layer 3 addressing design.

---

# 2.12 Throughput Metrics

Choosing a FortiGate based only on its model number is not enough.

Different security features require different amounts of processing.

Fortinet datasheets therefore provide several performance metrics.

---

## Firewall Throughput

**Firewall Throughput** represents packet-forwarding performance under the test conditions specified by the vendor.

It is useful for understanding raw firewall forwarding capacity.

---

## IPS Throughput

**IPS Throughput** measures performance while Intrusion Prevention functionality is enabled under the specified test methodology.

IPS requires deeper traffic inspection than basic firewall forwarding.

Therefore:

```text
Firewall Throughput
        │
        ▼
Usually Higher

IPS Throughput
        │
        ▼
Usually Lower
```

---

## NGFW Throughput

**NGFW Throughput** represents performance under Fortinet's defined NGFW test profile.

It reflects a more realistic security workload than basic firewall throughput.

Always check the exact test conditions listed in the datasheet.

---

## Threat Protection Throughput

**Threat Protection Throughput** measures performance under a broader security inspection workload defined by Fortinet.

This is often an important metric when sizing a firewall for environments where multiple security services will be enabled simultaneously.

---

## IPsec VPN Throughput

**IPsec VPN Throughput** represents the firewall's ability to process encrypted IPsec traffic under the specified test conditions.

This is especially important for:

- Site-to-site VPNs
- Branch connectivity
- Remote networks
- Data center connectivity

---

## Why Throughput Metrics Matter

Consider two organizations with the same 1 Gbps internet connection.

### Organization A

Uses:

- Firewall policies
- Basic NAT

### Organization B

Uses:

- IPS
- Antivirus
- Application Control
- SSL Inspection
- Web Filtering

Organization B requires significantly more security processing.

Therefore, selecting a FortiGate based only on **Firewall Throughput** can lead to incorrect sizing.

---

> ⚠️ **Sizing Principle**
>
> Size the firewall according to the security services you actually intend to enable, expected traffic patterns, user count, session requirements, interface speeds, VPN demand, and future growth — not only the internet bandwidth.

---

# 2.13 FortiGate Licensing and Security Services

FortiGate combines the FortiOS platform with support and subscription services.

Two terms you will frequently encounter are:

```text
FortiCare
and
FortiGuard
```

They are related to the Fortinet ecosystem but serve different purposes.

---

## 🛠 FortiCare

**FortiCare** is Fortinet's support and service offering.

Depending on the purchased support level and product entitlement, FortiCare can provide services such as:

- Technical support
- Firmware access
- Hardware replacement / RMA services
- Support portal access

---

## 🛡 FortiGuard

**FortiGuard security services** provide threat intelligence and security content used by Fortinet security technologies.

Examples include services associated with:

- IPS signatures
- Antivirus
- Web Filtering
- DNS security
- Application-related security intelligence
- Other subscribed security services

---

## FortiCare vs FortiGuard

| FortiCare | FortiGuard |
|-----------|------------|
| Product support | Security intelligence/services |
| Technical assistance | IPS content |
| Firmware entitlement according to support contract | Antivirus updates |
| Hardware support/RMA according to contract | Web/DNS security services |
| Operational support | Threat protection intelligence |

---

# 2.14 FortiGate Deployment Models

<img width="2899" height="1766" alt="image" src="https://github.com/user-attachments/assets/770c12c9-d44e-4c59-a576-da7c4896c9ed" />

FortiGate can be deployed in many different network architectures.

Four common examples are:

- Branch
- Headquarters
- Data Center
- Cloud

---

## 🏢 Branch Deployment

A small branch may use FortiGate as the main network security gateway.

```text
                 Internet
                    │
                    ▼
              ┌───────────┐
              │ FortiGate │
              └─────┬─────┘
                    │
                    ▼
              Branch Network
                    │
             ┌──────┴──────┐
             │             │
           Users        Devices
```

FortiGate may provide:

- Internet firewall
- NAT
- VPN to headquarters
- SD-WAN
- Web Filtering
- IPS
- Application Control

---

## 🏢 Headquarters Deployment

Larger environments may deploy two FortiGate appliances in a High Availability configuration.

```text
                    Internet
                       │
                       ▼
               ┌──────────────┐
               │ FortiGate HA │
               │   Cluster    │
               └──────┬───────┘
                      │
                      ▼
                 Core Switch
                      │
          ┌───────────┼───────────┐
          │           │           │
          ▼           ▼           ▼
        Users       Servers     Wireless
```

The goal is to avoid relying on a single firewall appliance.

---

## 🏭 Data Center Deployment

FortiGate can protect data center networks and server environments.

```text
                     Internet
                        │
                        ▼
                FortiGate Cluster
                        │
                        ▼
                  Core Network
                        │
               ┌────────┴────────┐
               │                 │
               ▼                 ▼
          Application         Database
            Servers            Servers
```

Depending on the architecture, FortiGate may provide:

- Network segmentation
- East-west or north-south security controls
- IPS
- Application Control
- VPN
- High availability
- Threat protection

---

## ☁ Cloud Deployment

FortiGate is also available as a virtual appliance for cloud and virtualized environments.

Common platforms include:

- AWS
- Microsoft Azure
- Google Cloud
- VMware

Conceptually:

```text
                 Cloud Environment
                        │
                        ▼
                 ┌────────────┐
                 │ FortiGate  │
                 │     VM     │
                 └─────┬──────┘
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
       Workloads    Servers    Cloud Networks
```

This allows organizations to apply Fortinet security controls to workloads outside traditional physical data centers.

---

# 2.15 Important Hardware Terms

The following terms should be familiar before moving into deeper FortiGate topics.

| Term | Meaning |
|------|---------|
| ASIC | Application-Specific Integrated Circuit |
| FortiASIC | Fortinet purpose-built processing technology |
| SPU | Security Processing Unit |
| NP | Network Processor |
| CP | Content Processor |
| CPU | General-purpose processor |
| RAM | Temporary system memory |
| Session | Stateful representation of a network connection |
| Session Table | Table containing active connection state |
| Throughput | Amount of traffic processed over a period of time |
| Offloading | Moving eligible processing from general CPU execution to specialized hardware |
| NGFW | Next-Generation Firewall |

---

# 📌 Key Takeaways

- **FortiGate** is Fortinet's flagship Next-Generation Firewall platform.
- FortiGate integrates networking and security capabilities such as firewalling, IPS, VPN, Application Control, Web Filtering, SD-WAN, and SSL Inspection.
- Fortinet uses purpose-built **FortiASIC/SPU technology** to accelerate eligible networking and security workloads.
- The **Network Processor (NP)** focuses on network-processing acceleration.
- The **Content Processor (CP)** assists with supported security and content-inspection workloads.
- The **CPU remains essential** for FortiOS, management, control-plane functions, and traffic that cannot be hardware-offloaded.
- FortiGate is a **stateful firewall** and maintains information about active connections in the **session table**.
- New traffic requires more processing than traffic belonging to an established session.
- **NAT/Route Mode** is the most common Layer 3 deployment mode.
- **Transparent Mode** allows FortiGate to operate primarily as an inline Layer 2 security device.
- Firewall throughput alone should **not** be used when sizing a FortiGate.
- IPS, NGFW, Threat Protection, VPN, session capacity, interface speed, and expected security services must also be considered.
- **FortiCare** primarily provides support services, while **FortiGuard** provides security intelligence and subscription-based security services.
- FortiGate can be deployed in branch offices, headquarters, data centers, virtual environments, and public clouds.

---

# 📚 References

For specifications, current product models, FortiOS behavior, and hardware acceleration details, refer to:

- Fortinet Product Documentation
- FortiGate / FortiOS Administration Guide
- FortiGate Hardware Acceleration Documentation
- Fortinet Product Matrix
- FortiGate Product Datasheets
- FortiGuard Labs
- Fortinet Training Institute

---

## ⬅ Previous Chapter

**Chapter 1 – Fortinet Company & Security Fabric**

## ➡ Next Chapter

**Chapter 3 – FortiGate Initial Configuration**
