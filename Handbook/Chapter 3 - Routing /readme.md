# 📘 Chapter 3 — Routing Fundamentals
---

# 📌 Overview

Routing is the process of determining the path that network traffic should take from a source network to a destination network.

Routing is one of the most important foundations of networking and is essential for understanding how routers, Layer 3 switches, and firewalls such as FortiGate forward traffic.

A device must determine:

- Where the destination network is located
- Which path should be used
- Which next-hop device should receive the packet
- Which interface should be used
- Which route is the most appropriate when multiple routes exist

This chapter covers the theoretical foundations of routing, different types of routes, routing tables, route selection, static routing, dynamic routing protocols, OSPF, BGP, route redistribution, and important routing concepts.

---

# 🎯 Learning Objectives

By the end of this chapter, you should be able to:

- The concept of routing
- How routers make forwarding decisions
- Routing tables
- Different types of routes
- Directly connected routes
- Static routes
- Default routes
- Dynamic routes
- Classful and classless routing
- Unicast, multicast, broadcast, and anycast routing concepts
- Interior and exterior routing
- Distance-vector routing
- Link-state routing
- Path-vector routing
- Hybrid routing
- Routing metrics
- Administrative Distance
- Longest-prefix matching
- Route preference
- Convergence
- Routing protocols
- RIP
- OSPF
- EIGRP
- IS-IS
- BGP
- Route redistribution
- Routing loops
- Asymmetric routing
- Policy-based routing
- Recursive routing
- ECMP
- Route summarization
- The difference between routing and forwarding

---

# 🧠 3.1 What is Routing?

**Routing** is the process of selecting a path through one or more networks to reach a destination.

When a device wants to communicate with another network, it needs to determine where the packet should be forwarded.

Example:

```text
Source Network
192.168.1.0/24
       |
       v
    Router
       |
       v
    Router
       |
       v
Destination Network
192.168.10.0/24
```

The routers examine the destination IP address and use their routing information to determine the appropriate path.

---

# 📦 3.2 Routing vs Forwarding

Routing and forwarding are related but different concepts.

### Routing

Routing is the process of:

```text
Learning
     +
Calculating
     +
Selecting
```

the best path to a destination.

### Forwarding

Forwarding is the actual process of sending the packet toward the selected next hop.

Conceptually:

```text
             Packet
                |
                v
        Destination IP
                |
                v
         Routing Decision
                |
                v
          Best Route
                |
                v
           Forwarding
                |
                v
       Outgoing Interface
```

---

# 🌐 3.3 Routing Table

A **routing table** is a database containing information about reachable networks and the paths used to reach them.

A simplified routing table may contain:

| Destination | Next Hop | Interface | Route Type |
|---|---|---|---|
| 192.168.1.0/24 | Directly Connected | LAN | Connected |
| 192.168.10.0/24 | 10.0.0.2 | WAN | Static |
| 10.10.0.0/16 | 10.0.0.3 | WAN | OSPF |
| 0.0.0.0/0 | 10.0.0.1 | WAN | Default |

A routing table helps a device answer:

> **"Where should I send this packet?"**

---

# 🧩 3.4 Components of a Route

A route generally contains information such as:

```text
Destination Network
        |
        +── Prefix / Subnet Mask
        |
        +── Next Hop
        |
        +── Outgoing Interface
        |
        +── Route Source
        |
        +── Metric
        |
        +── Administrative Distance
```

Example:

```text
Destination: 192.168.10.0/24
Next Hop:    10.0.0.2
Interface:   GigabitEthernet0/1
Source:      OSPF
Metric:      20
```

---

# 🔵 3.5 Types of Routes

Routes can be classified in several different ways.

The primary route types are:

```text
1. Connected Routes
2. Static Routes
3. Default Routes
4. Dynamic Routes
```

Dynamic routes can then be learned through different routing protocols.

---

# 🔵 3.6 Connected Routes

A **connected route** represents a network directly attached to an interface.

For example:

```text
Router Interface
192.168.1.1/24
```

The router automatically knows that:

```text
192.168.1.0/24
```

is directly connected.

Conceptually:

```text
192.168.1.0/24
       |
       |
     Router
       |
192.168.1.1
```

No routing protocol or manually configured static route is required for the router to know about its directly connected network.

---

# 🟢 3.7 Static Routes

A **static route** is manually configured by a network administrator.

Example:

```text
Destination:
192.168.10.0/24

Next Hop:
10.0.0.2
```

The administrator is explicitly defining the path toward the destination.

Static routing is commonly associated with:

- Small networks
- Simple topologies
- Specific remote networks
- Backup paths
- Stub networks
- Default routing

---

# ⭐ 3.8 Default Route

A **default route** is used when a more specific route to the destination is not available.

The IPv4 default route is:

```text
0.0.0.0/0
```

The IPv6 default route is:

```text
::/0
```

Conceptually:

```text
Specific Route
      |
      | No match
      v
Default Route
      |
      v
Next Hop
```

A default route is often used to send unknown destinations toward an upstream router or Internet service provider.

---

# 🟣 3.9 Dynamic Routes

Dynamic routes are learned automatically through routing protocols.

Instead of manually configuring every network, routers exchange routing information.

Examples:

```text
RIP
OSPF
EIGRP
IS-IS
BGP
```

Dynamic routing is particularly useful in larger and more complex networks.

---

# 🌍 3.10 Static Routing vs Dynamic Routing

| Feature | Static Routing | Dynamic Routing |
|---|---|---|
| Configuration | Manual | Automatic |
| Route Exchange | No | Yes |
| Scalability | Limited | High |
| Adaptation to Failures | Manual | Automatic |
| Administrative Overhead | Low initially | Higher |
| Resource Usage | Low | Higher |
| Best Use | Small/simple networks | Medium/large networks |

---

# 🧭 3.11 Classful vs Classless Routing

Routing can also be discussed in terms of whether routing information carries subnet mask/prefix information.

### Classful Routing

Classful routing is based on traditional IP address classes:

```text
Class A
Class B
Class C
```

Classful routing does not carry subnet-mask information with every route advertisement.

This can limit flexibility.

---

### Classless Routing

Classless routing includes subnet mask or prefix-length information.

Example:

```text
192.168.10.0/24
```

or:

```text
192.168.10.0/27
```

Classless routing enables:

- VLSM
- CIDR
- Route summarization
- More efficient address utilization

Modern IP networks primarily use classless routing.

---

# 🧮 3.12 CIDR

**CIDR = Classless Inter-Domain Routing**

CIDR represents networks using prefix lengths.

Example:

```text
192.168.1.0/24
```

The `/24` indicates that the first 24 bits represent the network portion.

CIDR allows networks to be represented more flexibly than traditional class-based addressing.

---

# 🧩 3.13 VLSM

**VLSM = Variable Length Subnet Mask**

VLSM allows different subnet sizes to be used within the same overall address space.

Example:

```text
Network:
192.168.0.0/16

Subnet A:
192.168.1.0/24

Subnet B:
192.168.2.0/25

Subnet C:
192.168.2.128/26
```

VLSM improves address utilization by allowing subnet sizes to match requirements.

---

# 🧠 3.14 Route Summarization

**Route summarization** combines multiple smaller routes into a larger summarized route.

Example:

```text
192.168.0.0/24
192.168.1.0/24
192.168.2.0/24
192.168.3.0/24
```

may be summarized as:

```text
192.168.0.0/22
```

Benefits include:

- Smaller routing tables
- Reduced routing updates
- Improved scalability
- Reduced CPU and memory requirements
- Greater routing stability

---

# 🔄 3.15 Routing Protocols

Routing protocols allow routers to exchange information about reachable networks.

They can be categorized based on how they calculate and exchange routing information.

Major categories include:

```text
Distance Vector
Link State
Path Vector
Hybrid
```

---

# 🟠 3.16 Distance Vector Routing

Distance-vector protocols determine routes based on distance and direction.

A router generally learns:

```text
Destination
+
Distance
+
Direction / Next Hop
```

A classic example is:

```text
RIP
```

Distance-vector protocols traditionally use periodic route information exchanges with neighboring routers.

---

# 🔵 3.17 Link-State Routing

Link-state protocols build a more complete view of the network topology.

Routers exchange information about links and use that information to calculate the best paths.

Examples include:

```text
OSPF
IS-IS
```

The general process is:

```text
Discover Neighbors
       ↓
Exchange Link-State Information
       ↓
Build Link-State Database
       ↓
Run SPF Algorithm
       ↓
Calculate Best Paths
       ↓
Install Routes
```

---

# 🔴 3.18 Path-Vector Routing

Path-vector protocols maintain information about the path toward a destination.

The primary example is:

```text
BGP
```

BGP does not simply select a path based on a single numerical metric.

Instead, it uses multiple path attributes and routing policies to determine the preferred path.

---

# 🟣 3.19 Hybrid Routing

Hybrid routing combines characteristics associated with multiple routing approaches.

A common example is:

```text
EIGRP
```

EIGRP uses concepts associated with distance-vector routing while incorporating additional mechanisms for efficient path calculation and convergence.

---

# 📊 3.20 Routing Protocol Classification

| Protocol | Category | Typical Use |
|---|---|---|
| RIP | Distance Vector | Small/legacy networks |
| OSPF | Link State | Enterprise networks |
| IS-IS | Link State | Service providers / large networks |
| EIGRP | Advanced Distance Vector / Hybrid | Enterprise networks |
| BGP | Path Vector | Internet / Inter-domain routing |

---

# 🔵 3.21 RIP

**RIP = Routing Information Protocol**

RIP is a distance-vector routing protocol.

Its primary routing metric is:

```text
Hop Count
```

The maximum usable hop count is:

```text
15
```

A hop count of:

```text
16
```

is considered unreachable.

Because of its limitations, RIP is generally unsuitable for modern large enterprise networks.

---

# 🟢 3.22 OSPF

**OSPF = Open Shortest Path First**

OSPF is a link-state interior gateway protocol.

It is widely used in enterprise networks.

OSPF routers maintain topology information and calculate shortest paths using the SPF algorithm.

---

# 🧠 3.23 OSPF Cost

OSPF uses **cost** as its routing metric.

In general:

```text
Lower Cost
     ↓
Preferred Path
```

Example:

```text
Path A
Cost = 10

Path B
Cost = 30
```

Path A is preferred based on cost when comparing paths within the same OSPF context.

---

# 🗺️ 3.24 OSPF Areas

Large OSPF networks can be divided into areas.

The backbone area is:

```text
Area 0
```

Example:

```text
                 Area 0
          ┌─────────────────┐
          │                 │
        Router            Router
          │                 │
          └────────┬────────┘
                   |
             ┌─────┴─────┐
             |           |
          Area 1       Area 2
```

Areas help improve scalability by reducing the amount of topology information that must be maintained throughout the entire OSPF domain.

---

# 🧩 3.25 OSPF Router Types

Important OSPF router roles include:

### Internal Router

All interfaces belong to the same OSPF area.

### Backbone Router

Has an interface in:

```text
Area 0
```

### Area Border Router

An **ABR** connects different OSPF areas.

```text
Area 1
   |
  ABR
   |
Area 0
```

### Autonomous System Boundary Router

An **ASBR** connects OSPF to another routing domain or routing source.

---

# 🔗 3.26 OSPF Neighbor Relationship

OSPF routers establish neighbor relationships to exchange routing information.

Conceptually:

```text
Router A
   |
   | OSPF
   |
Router B

Neighbor State
     ↓
Established
```

Successful neighbor relationships are essential for exchanging OSPF routing information.

---

# 🔴 3.27 EIGRP

**EIGRP = Enhanced Interior Gateway Routing Protocol**

EIGRP is commonly described as an advanced distance-vector or hybrid routing protocol.

It uses the **DUAL** algorithm to calculate loop-free paths and support rapid convergence.

Important EIGRP concepts include:

- Neighbor relationships
- Feasible successor
- Successor
- Feasible distance
- Reported distance
- Composite metric

EIGRP is primarily associated with Cisco environments.

---

# 🟠 3.28 IS-IS

**IS-IS = Intermediate System to Intermediate System**

IS-IS is a link-state interior gateway protocol.

It is commonly used in:

- Service provider networks
- Large-scale networks
- Internet infrastructure

IS-IS uses a hierarchical design with:

```text
Level 1
Level 2
Level 1-2
```

This provides scalable routing within large network environments.

---

# 🌍 3.29 BGP

**BGP = Border Gateway Protocol**

BGP is a path-vector routing protocol.

It is the primary routing protocol used for exchanging routing information between autonomous systems.

BGP is important for:

- Internet routing
- Service providers
- Multi-homed enterprises
- Large data centers
- Cloud connectivity
- Inter-organizational routing

---

# 🏢 3.30 Autonomous System

An **Autonomous System (AS)** is a collection of networks operating under a common routing administration and routing policy.

Each autonomous system is identified by an:

```text
ASN
```

or:

```text
Autonomous System Number
```

Conceptually:

```text
AS 65001
     |
     | BGP
     |
AS 65002
```

---

# 🔗 3.31 BGP Peering

BGP routers establish neighbor relationships called **BGP peering sessions**.

Example:

```text
Router A
AS 65001
    |
    | BGP
    |
Router B
AS 65002
```

The BGP peers exchange routing information after the session is established.

---

# 🧠 3.32 BGP Path Attributes

BGP uses multiple attributes during path selection.

Important attributes include:

- Weight
- Local Preference
- Locally Originated
- AS Path
- Origin
- MED
- eBGP vs iBGP
- IGP cost to next hop

Different implementations and vendors can have different selection orders and defaults.

The important concept is:

> **BGP uses routing policy and multiple path attributes rather than a simple single metric.**

---

# 🌐 3.33 eBGP vs iBGP

### eBGP

BGP peering between different autonomous systems.

```text
AS 65001
   |
  eBGP
   |
AS 65002
```

### iBGP

BGP peering between routers inside the same autonomous system.

```text
AS 65001

Router A
   |
  iBGP
   |
Router B
```

---

# 🏆 3.34 Administrative Distance

Administrative Distance indicates how trustworthy a routing source is when multiple routing sources provide routes toward the same destination.

Conceptually:

```text
Lower Administrative Distance
            ↓
       More Preferred
```

For example, a device may learn a destination through:

```text
Static Routing
OSPF
BGP
```

Administrative Distance can be used to compare the routing sources.

> **Important:** Administrative-distance values vary between vendors and platforms. Always use the values applicable to the specific platform being studied.

---

# 📏 3.35 Routing Metrics

A metric is a value used by a routing protocol to determine which path is preferable.

Examples:

| Protocol | Metric / Path Selection Concept |
|---|---|
| RIP | Hop Count |
| OSPF | Cost |
| EIGRP | Composite Metric |
| BGP | Path Attributes |

The general concept is:

```text
Same Routing Protocol
        ↓
Compare Metrics / Path Selection
        ↓
Preferred Path
```

---

# 🥇 3.36 Administrative Distance vs Metric

These two concepts should not be confused.

### Administrative Distance

Primarily answers:

> **Which routing source should be preferred?**

### Metric

Primarily answers:

> **Which path should be preferred within that routing protocol?**

Conceptually:

```text
Multiple Routing Sources
          |
          v
Administrative Distance
          |
          v
Preferred Routing Source
          |
          v
Protocol Path Selection
          |
          v
Metric / Attributes
          |
          v
Preferred Path
```

---

# 🎯 3.37 Longest Prefix Match

When multiple routes match a destination, the route with the **longest prefix** is normally preferred.

Example:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
```

Destination:

```text
10.10.10.50
```

The most specific match is:

```text
10.10.10.0/24
```

Therefore, that route is preferred over the broader routes.

---

# 🔄 3.38 Route Convergence

**Convergence** is the process through which routers update their routing information after a topology change and reach a consistent view of the network.

Example:

```text
Normal Network
      |
      v
Link Failure
      |
      v
Routing Protocol Detects Failure
      |
      v
Routing Information Updated
      |
      v
New Path Calculated
      |
      v
Network Converges
```

Fast convergence is important because it reduces the period during which traffic may be disrupted.

---

# 🔁 3.39 Routing Loops

A routing loop occurs when packets are continuously forwarded between routers without reaching their destination.

Example:

```text
Router A
   ↓
Router B
   ↓
Router C
   ↓
Router A
   ↓
Router B
   ↓
...
```

Routing protocols contain mechanisms designed to prevent or reduce routing loops.

Examples include:

- Split horizon
- Route poisoning
- Hold-down mechanisms
- Sequence information
- Link-state topology databases
- BGP AS-path loop prevention

---

# 🔀 3.40 Asymmetric Routing

Asymmetric routing occurs when the forward and return traffic follow different paths.

Example:

```text
Forward Path:

Client
  ↓
Router A
  ↓
Router B
  ↓
Server


Return Path:

Server
  ↓
Router C
  ↓
Router A
  ↓
Client
```

Asymmetric routing can be normal in some network designs but may create problems for stateful firewalls and security devices.

---

# 🔄 3.41 Route Redistribution

**Route redistribution** is the process of taking routes learned from one routing source and making them available through another routing protocol or routing domain.

Example:

```text
             OSPF
              |
              |
          Router
              |
              |
             BGP
```

Routes learned through OSPF may be redistributed into BGP, or routes from another source may be introduced into OSPF.

---

# 🧩 3.42 Sources That Can Be Redistributed

Depending on the platform and configuration, routing information can be redistributed from sources such as:

```text
Connected
Static
OSPF
RIP
EIGRP
BGP
IS-IS
```

Redistribution should be carefully controlled.

---

# ⚠️ 3.43 Redistribution Risks

Poor redistribution design can result in:

- Routing loops
- Route instability
- Unexpected path selection
- Excessive routing information
- Duplicate routes
- Asymmetric routing
- Difficult troubleshooting

Route filtering and careful policy design are therefore important when performing redistribution.

---

# 🎯 3.44 Policy-Based Routing

Traditional routing primarily uses the destination IP address to determine the path.

**Policy-Based Routing (PBR)** allows routing decisions to consider additional criteria.

Depending on the platform, policies may consider factors such as:

- Source IP
- Destination IP
- Protocol
- Port
- Other traffic characteristics

Conceptually:

```text
Traffic
   |
   v
PBR Policy
   |
   +---- Match
   |
   v
Specified Path
```

PBR can be used when destination-based routing alone cannot satisfy the required traffic-engineering behavior.

---

# 🔁 3.45 Recursive Routing

Recursive routing occurs when the next hop of a route must itself be resolved through another route.

Example:

```text
Destination:
192.168.10.0/24

Next Hop:
10.0.0.2
```

The router must determine how to reach:

```text
10.0.0.2
```

It may use another route:

```text
10.0.0.0/24
```

Therefore:

```text
192.168.10.0/24
        ↓
Next Hop 10.0.0.2
        ↓
Resolve 10.0.0.2
        ↓
10.0.0.0/24
        ↓
Outgoing Interface
```

---

# ⚖️ 3.46 Equal-Cost Multi-Path

**ECMP = Equal-Cost Multi-Path**

ECMP allows multiple paths with equal routing preference/cost to be used.

Example:

```text
             Router
            /      \
           /        \
        Path A     Path B
        Cost 10    Cost 10
           \        /
            \      /
           Destination
```

Both paths may be eligible for forwarding.

ECMP can provide:

- Load sharing
- Redundancy
- Better utilization of available links

---

# 🧠 3.47 Unicast Routing

**Unicast** communication is one-to-one communication.

Example:

```text
Host A
   |
   |---- Packet ---->
   |
Host B
```

A single sender communicates with a single receiver.

Unicast is the most common form of IP traffic.

---

# 📡 3.48 Broadcast Routing

Broadcast communication is one-to-all communication within a broadcast domain.

IPv4 broadcast examples include:

```text
255.255.255.255
```

or a subnet-directed broadcast.

Broadcast traffic is generally limited by routers because routers normally do not forward Layer 3 broadcast traffic between interfaces.

---

# 👥 3.49 Multicast Routing

Multicast is one-to-many communication where traffic is sent to a specific multicast group.

Conceptually:

```text
             Sender
                |
                v
           Multicast
             Group
          /    |    \
         /     |     \
       Host   Host   Host
```

Instead of sending separate copies individually to every receiver, multicast routing can efficiently distribute traffic to interested receivers.

Common applications include:

- Video streaming
- IPTV
- Financial market data
- Routing protocols
- Real-time applications

---

# 🌐 3.50 Anycast

**Anycast** allows multiple devices to use the same IP address, with traffic being routed toward the topologically preferred or nearest instance according to the routing system.

Conceptually:

```text
                 Client
                    |
             Routing Decision
              /           \
             /             \
        Server A          Server B
      Same Anycast IP   Same Anycast IP
```

Anycast is commonly used in globally distributed services such as:

- DNS
- Content delivery
- Distributed Internet services

---

# 🧭 3.51 Interior vs Exterior Routing

Routing protocols can also be classified by their scope.

### Interior Gateway Protocol

An **IGP** is used within an autonomous system.

Examples:

```text
RIP
OSPF
EIGRP
IS-IS
```

### Exterior Gateway Protocol

An **EGP** is used to exchange routing information between autonomous systems.

The modern Internet's primary exterior routing protocol is:

```text
BGP
```

---

# 📊 3.52 IGP vs EGP

| Feature | IGP | EGP |
|---|---|---|
| Scope | Inside AS | Between ASes |
| Examples | OSPF, RIP, EIGRP, IS-IS | BGP |
| Main Goal | Internal reachability | Inter-domain routing |
| Policy Complexity | Generally lower | Generally higher |

---

# 🏗️ 3.53 Routing Domain

A routing domain is a network environment in which routing information is exchanged according to a defined routing architecture or administrative policy.

An organization may contain:

```text
Campus Network
      +
Data Center
      +
Branch Network
      +
WAN
```

and use multiple routing technologies depending on the architecture.

---

# 🧠 3.54 Control Plane vs Data Plane

Routing also involves two important forwarding concepts.

### Control Plane

Responsible for learning and calculating routes.

Examples:

```text
OSPF
BGP
RIP
Static Routing
```

### Data Plane

Responsible for forwarding packets using the selected forwarding information.

Conceptually:

```text
             Control Plane
                   |
            Routing Decision
                   |
                   v
             Forwarding Table
                   |
                   v
               Data Plane
                   |
                   v
               Packet
```

---

# 🔥 3.55 Routing and Firewalls

For a firewall such as FortiGate, routing is one component of packet processing.

A simplified conceptual model is:

```text
Incoming Packet
       |
       v
Routing Decision
       |
       v
Security Policy
       |
       v
NAT / Security Processing
       |
       v
Outgoing Interface
       |
       v
Destination
```

Routing determines the path.

Security policies determine whether traffic is permitted.

These are separate functions.

---

# 🧠 3.56 Important Routing Terminology

| Term | Meaning |
|---|---|
| Route | Information describing a path to a destination |
| Routing Table | Database containing routes |
| Next Hop | Next Layer 3 device in the path |
| Metric | Value used to compare paths |
| Administrative Distance | Preference between routing sources |
| Prefix | Network portion represented by a length |
| Convergence | Process of reaching a consistent routing state |
| Redistribution | Sharing routes between routing sources |
| Summarization | Combining multiple routes into one larger route |
| PBR | Routing based on defined traffic policies |
| ECMP | Multiple equal-cost paths |
| IGP | Routing protocol used inside an AS |
| EGP | Routing protocol used between ASes |
| ASN | Autonomous System Number |

---

# 📊 3.57 Routing Protocol Comparison

| Protocol | Type | Metric / Selection | Scope | Scalability |
|---|---|---|---|---|
| RIP | Distance Vector | Hop Count | IGP | Low |
| OSPF | Link State | Cost | IGP | High |
| EIGRP | Advanced Distance Vector / Hybrid | Composite Metric | IGP | High |
| IS-IS | Link State | Cost | IGP | Very High |
| BGP | Path Vector | Path Attributes | EGP | Very High |

---

# 🧠 3.58 Complete Routing Classification

Routing can be viewed from several different perspectives.

### By Configuration

```text
Static
Dynamic
```

### By Destination Preference

```text
Specific Route
Default Route
```

### By Route Source

```text
Connected
Static
RIP
OSPF
EIGRP
IS-IS
BGP
```

### By Routing Algorithm

```text
Distance Vector
Link State
Path Vector
Hybrid
```

### By Routing Scope

```text
IGP
EGP
```

### By Traffic Type

```text
Unicast
Broadcast
Multicast
Anycast
```

---

# 🏆 3.59 Route Selection — Complete Concept

When a router has multiple possible paths, the route-selection process can be summarized conceptually as:

```text
Destination IP
      |
      v
Find Matching Routes
      |
      v
Longest Prefix Match
      |
      v
Compare Routing Sources
      |
      v
Administrative Distance
      |
      v
Protocol-Specific
Path Selection
      |
      v
Metric / Attributes
      |
      v
Best Route
      |
      v
Next Hop
      |
      v
Outgoing Interface
```

The exact implementation and order can vary by vendor and platform, so platform-specific documentation should always be consulted for precise behavior.

---

# 🔥 3.60 Routing Design Principles

A good routing design should consider:

- Scalability
- Redundancy
- Convergence
- Route summarization
- Failure recovery
- Traffic engineering
- Security
- Simplicity
- Operational visibility
- Consistent addressing
- Avoidance of routing loops
- Proper route filtering

The objective is not simply to make routes work.

The objective is to create a routing architecture that remains:

```text
Stable
Scalable
Predictable
Redundant
Manageable
```

---

# 📌 3.61 Chapter Summary

Routing is the process of determining how packets reach their destination.

The major route types include:

```text
Connected
Static
Default
Dynamic
```

Dynamic routing protocols can be categorized as:

```text
Distance Vector
Link State
Path Vector
Hybrid
```

Important protocols include:

```text
RIP
OSPF
EIGRP
IS-IS
BGP
```

Important route-selection concepts include:

```text
Longest Prefix Match
Administrative Distance
Metric
Route Preference
```

Advanced routing concepts include:

```text
Route Summarization
Route Redistribution
Policy-Based Routing
Recursive Routing
ECMP
Convergence
Asymmetric Routing
Routing Loops
```

Routing can also be classified by traffic type:

```text
Unicast
Broadcast
Multicast
Anycast
```

And by routing scope:

```text
IGP
EGP
```

---

# 🧠 Key Takeaways

- Routing determines the path traffic takes between networks.
- A routing table contains information about reachable destinations.
- Connected routes represent directly attached networks.
- Static routes are manually configured.
- Dynamic routes are learned through routing protocols.
- A default route is used when no more specific route exists.
- CIDR provides classless network representation.
- VLSM allows different subnet sizes within an address space.
- Route summarization reduces routing-table size.
- Distance-vector protocols use distance and direction concepts.
- Link-state protocols build a topology view of the network.
- Path-vector routing uses path information and policy.
- RIP uses hop count.
- OSPF uses cost.
- EIGRP uses a composite metric.
- IS-IS is a link-state IGP.
- BGP is a path-vector protocol used for inter-domain routing.
- Administrative Distance compares routing sources.
- Metrics generally compare paths within a routing protocol.
- Longest-prefix matching selects the most specific matching route.
- Convergence is required after topology changes.
- Redistribution allows routes to move between routing sources.
- PBR allows routing decisions based on defined traffic characteristics.
- ECMP allows multiple equal-cost paths.
- Asymmetric routing means forward and return traffic use different paths.
- Routing and firewall policy are separate functions.

---

# 📚 Quick Revision

```text
                         ROUTING
                            |
        +-------------------+-------------------+
        |                   |                   |
   Route Types         Protocol Types      Traffic Types
        |                   |                   |
   +----+----+        +-----+------+       +----+----+----+
   |    |    |        |     |      |       |    |    |    |
Connected Static Default Dynamic  RIP   OSPF   BGP  Unicast Broadcast
                                                   |
                                              Multicast / Anycast


                 ROUTE SELECTION
                       |
          +------------+------------+
          |            |            |
     Prefix Match     AD         Metric
          |
     Most Specific
          |
      Best Route
```

