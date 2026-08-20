# Chapter 4 -- Firewall Fundamentals & FortiGate Firewall Policies

------------------------------------------------------------------------

## 📌 Chapter Overview

A firewall is one of the most important security controls in a modern
network. It controls traffic between networks and security zones based
on predefined security policies.

In enterprise environments, firewalls are used to control:

-   Internet access
-   Internal network communication
-   Server access
-   Inter-VLAN communication
-   Remote access
-   VPN traffic
-   Application traffic
-   Web access
-   Secure zones such as DMZs
-   East-West and North-South traffic

This chapter introduces the fundamental concepts of firewalls and then
focuses on how **FortiGate Firewall Policies** are designed and
processed.

------------------------------------------------------------------------

# 4.1 What is a Firewall?

A **firewall** is a network security device or software system that
monitors, filters, and controls network traffic according to predefined
security rules.

The primary purpose of a firewall is to determine:

> **Which traffic is allowed and which traffic is denied.**

A firewall can inspect traffic based on several parameters, including:

-   Source IP address
-   Destination IP address
-   Source interface
-   Destination interface
-   Protocol
-   Source port
-   Destination port
-   Application
-   User identity
-   URL category
-   Security profiles
-   Connection state

### Basic Firewall Concept

``` text
              INTERNET
                  |
                  |
            +-----------+
            |  FIREWALL |
            +-----------+
                  |
          -----------------
          |               |
       LAN/Users        DMZ
```

The firewall acts as a security boundary between different networks or
security zones.

------------------------------------------------------------------------

# 4.2 Why Firewalls are Required

Without proper traffic control, systems may communicate with networks
and services that they do not actually require.

A firewall provides:

-   Traffic control
-   Network segmentation
-   Access control
-   Threat prevention
-   Internet security
-   Logging and monitoring
-   Application control
-   User-based access control
-   VPN security
-   Network address translation
-   Security policy enforcement

### Example

``` text
Internet
   |
   v
Firewall
   |
   +------ Corporate LAN
   |
   +------ Server Network
   |
   +------ DMZ
   |
   +------ Guest Network
```

The firewall can enforce different security policies between each zone.

For example:

``` text
LAN → Internet       ALLOW
Guest → Internet     ALLOW
Guest → LAN          DENY
Internet → LAN       DENY
Internet → DMZ       LIMITED
LAN → Servers        ALLOW
```

------------------------------------------------------------------------

# 4.3 Firewall as a Security Boundary

A firewall creates a controlled boundary between networks with different
trust levels.

Typical security zones include:

  Zone                 Typical Trust Level   Example
  -------------------- --------------------- ------------------------------
  Internet             Untrusted             Public Internet
  DMZ                  Semi-trusted          Public Web Servers
  LAN                  Trusted               Corporate Users
  Server Network       Highly Controlled     Application/Database Servers
  Guest Network        Restricted            Guest Wi-Fi
  Management Network   Highly Restricted     Network Management

The firewall determines what communication is permitted between these
zones.

------------------------------------------------------------------------

# 4.4 Types of Firewalls

Firewalls can be classified based on how they inspect and control
traffic.

## 4.4.1 Packet Filtering Firewall

A packet filtering firewall examines basic packet information such as:

-   Source IP
-   Destination IP
-   Protocol
-   Source port
-   Destination port

Example:

``` text
Source:      192.168.10.0/24
Destination: 10.10.10.0/24
Protocol:    TCP
Port:        443
Action:      ALLOW
```

### Advantages

-   Simple
-   Fast
-   Low processing overhead

### Limitations

-   Limited visibility
-   Does not understand applications deeply
-   Limited content inspection

------------------------------------------------------------------------

# 4.5 Stateful Firewall

A stateful firewall maintains information about active connections.

Instead of examining every packet independently, it understands whether
traffic belongs to an existing session.

For example:

``` text
Client → Server
TCP SYN
       ↓
Server → Client
TCP SYN-ACK
       ↓
Client → Server
TCP ACK
```

The firewall tracks this connection as a stateful session.

### Stateful Inspection

The firewall maintains a session table containing information such as:

-   Source IP
-   Destination IP
-   Source port
-   Destination port
-   Protocol
-   Session state
-   NAT information
-   Interface information

This allows return traffic for an established connection to be handled
appropriately.

------------------------------------------------------------------------

# 4.6 Application-Aware Firewall

Modern firewalls can identify applications rather than relying only on
port numbers.

For example:

``` text
TCP/443
```

does not automatically mean that the traffic is simply "HTTPS" from a
security perspective.

A modern firewall may identify applications such as:

-   Microsoft Teams
-   YouTube
-   Facebook
-   BitTorrent
-   WhatsApp
-   SSH
-   RDP
-   DNS
-   SSL
-   HTTP

Application identification allows administrators to create more granular
policies.

------------------------------------------------------------------------

# 4.7 Next-Generation Firewall (NGFW)

A **Next-Generation Firewall (NGFW)** combines traditional firewall
functionality with advanced security capabilities.

Typical NGFW capabilities include:

-   Stateful firewall
-   Application control
-   Intrusion Prevention System (IPS)
-   Antivirus
-   Web filtering
-   DNS filtering
-   SSL/TLS inspection
-   User authentication
-   Malware protection
-   VPN
-   Logging and monitoring
-   SD-WAN
-   High Availability

FortiGate is an example of an NGFW platform.

------------------------------------------------------------------------

# 4.8 FortiGate Firewall

FortiGate is Fortinet's network security platform designed to provide
firewalling and multiple integrated security functions.

FortiGate can provide:

-   Firewall policies
-   NAT
-   Routing
-   VPN
-   Application Control
-   Antivirus
-   IPS
-   Web Filtering
-   DNS Filtering
-   SSL Inspection
-   Authentication
-   Logging
-   SD-WAN
-   High Availability

The firewall policy is one of the most important components of FortiGate
security configuration.

------------------------------------------------------------------------

# 4.9 What is a Firewall Policy?

A **firewall policy** defines how FortiGate should handle traffic
matching specific conditions.

A policy can answer:

> Who can communicate with whom, through which interface, using which
> services, and under what security controls?

A basic policy contains:

``` text
Incoming Interface
        +
Source
        +
Outgoing Interface
        +
Destination
        +
Schedule
        +
Service
        +
Action
```

Additional security controls can then be applied.

------------------------------------------------------------------------

# 4.10 Basic FortiGate Policy Structure

A simplified FortiGate firewall policy can be represented as:

``` text
                FIREWALL POLICY
                      |
        +-------------+-------------+
        |                           |
     Traffic Match              Action
        |                           |
   Source Interface              ACCEPT
   Source Address                 DENY
   Destination Interface
   Destination Address
   Schedule
   Service
```

If traffic matches the conditions of a policy, FortiGate applies the
configured action and associated security controls.

------------------------------------------------------------------------

# 4.11 Main Components of a FortiGate Firewall Policy

A FortiGate policy commonly contains the following elements:

1.  Policy ID
2.  Incoming Interface
3.  Source Address
4.  Outgoing Interface
5.  Destination Address
6.  Schedule
7.  Service
8.  Action
9.  NAT
10. Security Profiles
11. Logging
12. Traffic shaping
13. User or group restrictions
14. Application restrictions

------------------------------------------------------------------------

# 4.12 Incoming Interface

The **Incoming Interface** defines where traffic enters the FortiGate.

Examples:

``` text
LAN
WAN1
WAN2
DMZ
VPN
VLAN10
VLAN20
```

Example:

``` text
Incoming Interface: LAN
Outgoing Interface: WAN1
```

This represents traffic travelling from the LAN toward the Internet
through WAN1.

------------------------------------------------------------------------

# 4.13 Source Address

The source address defines the origin of the traffic.

It can represent:

-   Single IP address
-   Subnet
-   IP range
-   Address object
-   Address group

Example:

``` text
Source:
192.168.10.0/24
```

This means hosts in the `192.168.10.0/24` network can match the policy.

------------------------------------------------------------------------

# 4.14 Outgoing Interface

The outgoing interface defines where the traffic is expected to leave
the FortiGate.

Examples:

``` text
WAN1
WAN2
LAN
DMZ
VPN
```

Example:

``` text
LAN → WAN1
```

means traffic enters through the LAN interface and exits through WAN1.

------------------------------------------------------------------------

# 4.15 Destination Address

The destination address defines where the traffic is going.

Examples:

``` text
all
10.10.10.10
10.10.10.0/24
Web_Server
DNS_Servers
Application_Servers
```

A destination address can be represented using FortiGate address
objects.

------------------------------------------------------------------------

# 4.16 Address Objects

Instead of repeatedly entering IP addresses, FortiGate allows
administrators to create reusable address objects.

Example:

``` text
Name: Internal_Server
IP: 10.10.10.10/32
```

A policy can then reference:

``` text
Source: Internal_Network
Destination: Internal_Server
```

This makes policies easier to understand and manage.

------------------------------------------------------------------------

# 4.17 Address Groups

Multiple address objects can be combined into an address group.

Example:

``` text
WEB_SERVERS
   |
   +--- Web_Server_01
   +--- Web_Server_02
   +--- Web_Server_03
```

A policy can reference the group instead of creating separate policies
for every server.

------------------------------------------------------------------------

# 4.18 Schedule

The **Schedule** determines when the policy is active.

Common options include:

### Always

The policy is active continuously.

``` text
Schedule: Always
```

### Recurring Schedule

The policy can be active during a specific time period.

Example:

``` text
Monday–Friday
09:00–18:00
```

Schedules are useful when access should only be permitted during
business hours.

------------------------------------------------------------------------

# 4.19 Service

A service defines the network protocol and port associated with traffic.

Examples:

  Service   Protocol     Port
  --------- ---------- ------
  HTTP      TCP            80
  HTTPS     TCP           443
  SSH       TCP            22
  DNS       UDP/TCP        53
  FTP       TCP            21
  SMTP      TCP            25
  RDP       TCP          3389
  SNMP      UDP           161

FortiGate provides predefined services and allows administrators to
create custom services.

------------------------------------------------------------------------

# 4.20 Service Objects

Services can be represented as reusable objects.

Example:

``` text
HTTPS
TCP
443
```

A policy can then reference:

``` text
Service: HTTPS
```

This simplifies policy management.

------------------------------------------------------------------------

# 4.21 Action

The firewall policy determines what FortiGate should do when traffic
matches the policy.

Common actions include:

### ACCEPT

Allows the traffic.

``` text
Action: ACCEPT
```

### DENY

Blocks the traffic.

``` text
Action: DENY
```

### IPsec

Used for policies associated with IPsec VPN traffic.

The exact available options depend on the FortiGate configuration and
policy type.

------------------------------------------------------------------------

# 4.22 ACCEPT Policy

An ACCEPT policy allows matching traffic to pass through the firewall.

Example:

``` text
Source:
LAN

Destination:
Internet

Service:
HTTPS

Action:
ACCEPT
```

This allows HTTPS traffic from the LAN toward the Internet, subject to
any additional policy settings.

------------------------------------------------------------------------

# 4.23 DENY Policy

A DENY policy blocks matching traffic.

Example:

``` text
Source:
Guest Network

Destination:
Corporate LAN

Service:
ALL

Action:
DENY
```

This prevents guest users from accessing the corporate LAN.

------------------------------------------------------------------------

# 4.24 NAT in Firewall Policies

**NAT (Network Address Translation)** changes IP addressing information
as traffic passes through the firewall.

A common example is **Source NAT (SNAT)** for Internet access.

Example:

``` text
Internal Host
192.168.10.10
      |
      v
FortiGate
      |
      v
Public IP
203.0.113.10
      |
      v
Internet
```

The private source address can be translated to a public IP address.

For typical Internet access policies, NAT is commonly enabled when the
FortiGate is performing the Internet gateway function.

------------------------------------------------------------------------

# 4.25 Central SNAT vs Policy-Based NAT

FortiGate can handle source NAT through different approaches depending
on configuration.

### Policy-Based NAT

NAT is configured directly within the firewall policy.

### Central SNAT

NAT is controlled using centralized SNAT rules.

Central SNAT can provide more granular separation between:

``` text
Firewall Policy
```

and

``` text
NAT Configuration
```

The appropriate method depends on the network design and FortiGate
configuration.

------------------------------------------------------------------------

# 4.26 Security Profiles

One of the major advantages of FortiGate is the ability to attach
security profiles to firewall policies.

Security profiles can provide deeper inspection of permitted traffic.

Common profiles include:

-   Antivirus
-   Web Filter
-   Application Control
-   IPS
-   DNS Filter
-   File Filter
-   Video Filter
-   Email Filter
-   SSL/SSH Inspection

A simplified policy may therefore look like:

``` text
LAN → Internet
       |
       +--- Firewall Policy
       |
       +--- Antivirus
       |
       +--- Web Filter
       |
       +--- Application Control
       |
       +--- IPS
       |
       +--- SSL Inspection
```

------------------------------------------------------------------------

# 4.27 Firewall Policy with Security Profiles

A typical enterprise Internet policy may contain:

``` text
Incoming Interface:
LAN

Source:
Corporate_Users

Outgoing Interface:
WAN1

Destination:
all

Schedule:
Always

Service:
ALL

Action:
ACCEPT

NAT:
Enabled

Security Profiles:
    Antivirus
    Web Filter
    Application Control
    IPS
    SSL Inspection

Logging:
Enabled
```

This allows traffic while applying multiple security controls.

------------------------------------------------------------------------

# 4.28 Firewall Policy Processing

FortiGate evaluates traffic against its configured firewall policies.

A simplified process is:

``` text
Incoming Packet
       |
       v
Identify Incoming Interface
       |
       v
Determine Destination
       |
       v
Check Firewall Policies
       |
       v
Find Matching Policy
       |
       v
Apply Policy Action
       |
       +------ DENY → Drop Traffic
       |
       +------ ACCEPT
                |
                v
        Apply NAT if configured
                |
                v
        Apply Security Profiles
                |
                v
          Forward Traffic
```

The actual internal processing path can involve additional FortiGate
subsystems and depends on the traffic type and configuration.

------------------------------------------------------------------------

# 4.29 Policy Matching

For a traffic flow to match a policy, relevant policy conditions must
match the traffic.

Important matching criteria include:

``` text
Incoming Interface
Source Address
Outgoing Interface
Destination Address
Schedule
Service
User/Identity
```

Additional features can influence processing depending on the
configuration.

------------------------------------------------------------------------

# 4.30 Policy Order

**Policy order is extremely important.**

FortiGate evaluates applicable firewall policies in sequence.

A simplified example:

``` text
Policy 1
Guest → LAN → DENY

Policy 2
Guest → Internet → ACCEPT

Policy 3
LAN → Internet → ACCEPT
```

If the policy order is incorrect, a broader policy can match traffic
before a more specific policy.

------------------------------------------------------------------------

# 4.31 Specific Policies Before General Policies

A good firewall policy design normally places more specific policies
before broader policies.

Example:

``` text
1. Guest → Server → DENY
2. Guest → Internet → ACCEPT
3. LAN → Internet → ACCEPT
```

A broad policy such as:

``` text
Source: all
Destination: all
Service: all
Action: ACCEPT
```

can create serious security problems if placed before more restrictive
policies.

------------------------------------------------------------------------

# 4.32 Implicit Deny

A critical firewall security concept is the **implicit deny**.

If traffic does not match an applicable policy allowing it, FortiGate
will not simply allow that traffic by default.

Conceptually:

``` text
Traffic
   |
   v
Policy 1 ── No Match
   |
   v
Policy 2 ── No Match
   |
   v
Policy 3 ── No Match
   |
   v
Implicit Deny
   |
   v
Traffic Blocked
```

This follows the security principle:

> **Deny by default and allow only required traffic.**

------------------------------------------------------------------------

# 4.33 Why the Implicit Deny is Important

The implicit deny prevents unknown or unauthorized traffic from
automatically passing through the firewall.

This supports the principle of:

### Least Privilege

Users and systems should receive only the access they actually require.

For example, if an application server only requires HTTPS access to a
specific external service, it should not necessarily receive
unrestricted Internet access.

------------------------------------------------------------------------

# 4.34 Firewall Policy Design Principles

A well-designed firewall policy should follow several principles.

### 1. Least Privilege

Allow only required communication.

### 2. Specificity

Use specific source, destination, and service objects whenever possible.

### 3. Segmentation

Separate networks according to security requirements.

### 4. Default Deny

Traffic should not be permitted unless explicitly required.

### 5. Logging

Enable appropriate logging for security visibility.

### 6. Documentation

Policies should have meaningful names and comments.

### 7. Avoid Excessive `ALL`

Avoid unnecessarily using:

``` text
Source: all
Destination: all
Service: all
```

### 8. Regular Review

Firewall policies should be reviewed periodically.

------------------------------------------------------------------------

# 4.35 Common Firewall Policy Naming Convention

A consistent naming convention makes large firewall environments easier
to manage.

Example:

``` text
LAN_TO_INTERNET_WEB
GUEST_TO_INTERNET
GUEST_TO_LAN_DENY
LAN_TO_DNS
LAN_TO_NTP
USER_TO_SERVER_HTTPS
VPN_TO_INTERNAL_SERVERS
DMZ_TO_DATABASE
```

A useful naming convention can include:

``` text
<SOURCE>_<DESTINATION>_<SERVICE/ACTION>
```

Example:

``` text
LAN_TO_DMZ_HTTPS
```

------------------------------------------------------------------------

# 4.36 Inbound vs Outbound Policies

### Outbound Traffic

Traffic leaving an internal network.

Example:

``` text
LAN → Internet
```

Typical requirements:

-   NAT
-   Web filtering
-   Antivirus
-   Application control
-   IPS
-   SSL inspection

### Inbound Traffic

Traffic entering the organization from an external network.

Example:

``` text
Internet → DMZ
```

Inbound traffic generally requires stricter controls because the source
network is untrusted.

------------------------------------------------------------------------

# 4.37 DMZ Firewall Policies

A **DMZ (Demilitarized Zone)** is a network segment used for systems
that need controlled communication with external networks.

Typical DMZ systems include:

-   Web servers
-   Mail gateways
-   DNS servers
-   Reverse proxies
-   Public application servers

Example:

``` text
                 INTERNET
                     |
                     v
                FORTIGATE
                     |
              +------+------+
              |             |
             DMZ           LAN
              |
        Web Server
```

A typical security model could be:

``` text
Internet → DMZ Web Server → ALLOW HTTPS
Internet → LAN             → DENY
DMZ → LAN                  → LIMITED
DMZ → Database             → SPECIFIC
LAN → DMZ                  → CONTROLLED
```

------------------------------------------------------------------------

# 4.38 Inter-VLAN Firewall Policies

FortiGate can control communication between VLANs when the VLAN
interfaces are configured on the FortiGate.

Example:

``` text
VLAN 10 - Users
VLAN 20 - Servers
VLAN 30 - Guest
VLAN 40 - Management
```

Policies can control communication:

``` text
Users → Servers
Users → Internet
Guest → Internet
Guest → Users
Guest → Servers
Users → Management
```

This provides Layer 3 security segmentation between networks.

------------------------------------------------------------------------

# 4.39 User-Based Firewall Policies

FortiGate can integrate with authentication systems to associate network
traffic with users or groups.

Examples:

-   Active Directory
-   LDAP
-   RADIUS
-   SAML
-   FortiAuthenticator

Instead of creating a policy based only on IP addresses, access can be
based on identity.

Example:

``` text
IT_Administrators
        |
        +----> Management Network
```

while:

``` text
Regular_Users
        |
        +----> Internet
```

This enables identity-aware access control.

------------------------------------------------------------------------

# 4.40 Service-Based Access Control

Firewall policies can restrict access based on required services.

Example:

``` text
Application Server
       |
       +---- HTTPS → Internet
       |
       +---- DNS → DNS Server
       |
       +---- NTP → NTP Server
```

Instead of:

``` text
Application Server → ALL → Internet
```

a more secure design uses only the services required by the application.

------------------------------------------------------------------------

# 4.41 Application Control

Application Control identifies and controls applications running over
the network.

Examples:

``` text
Allow:
Microsoft Teams
Business Applications
Approved Cloud Services

Block:
Torrent
Unauthorized Remote Access
High-Risk Applications
```

Application Control provides visibility beyond traditional TCP/UDP port
filtering.

------------------------------------------------------------------------

# 4.42 Web Filtering

Web filtering controls access to websites based on categories, URLs,
reputation, and other criteria.

Common categories include:

-   Malware
-   Phishing
-   Gambling
-   Adult Content
-   Social Media
-   Streaming
-   Newly Observed Domains
-   Uncategorized Websites

Example:

``` text
User → Internet
          |
          v
      Web Filter
          |
    +-----+-----+
    |           |
 Allowed      Blocked
```

------------------------------------------------------------------------

# 4.43 Antivirus Inspection

FortiGate can inspect network traffic for malicious files and known
threats using antivirus capabilities.

Possible inspection areas include:

-   HTTP
-   HTTPS
-   FTP
-   SMTP
-   POP3
-   IMAP

Encrypted traffic may require SSL/TLS inspection to provide deeper
visibility.

------------------------------------------------------------------------

# 4.44 Intrusion Prevention System (IPS)

IPS detects and can block malicious network activity.

It can help detect:

-   Exploitation attempts
-   Network attacks
-   Suspicious traffic
-   Vulnerability exploitation
-   Protocol anomalies
-   Known attack signatures

A firewall policy can associate an IPS profile with permitted traffic.

------------------------------------------------------------------------

# 4.45 SSL/TLS Inspection

A significant amount of modern Internet traffic is encrypted.

For example:

``` text
Client
   |
 HTTPS
   |
   v
Internet
```

Without appropriate inspection, the firewall may have limited visibility
into encrypted content.

FortiGate supports SSL/TLS inspection methods including:

### Certificate Inspection

Examines certificate-related information without fully decrypting the
traffic content.

### Full SSL Inspection

FortiGate acts as an inspection point and decrypts and re-encrypts
traffic so that security controls can inspect the content.

SSL inspection requires careful planning because it can involve:

-   Certificate deployment
-   Trust configuration
-   Privacy considerations
-   Application compatibility
-   Performance considerations

------------------------------------------------------------------------

# 4.46 Logging in Firewall Policies

Firewall logging provides visibility into network activity.

Logs can help administrators determine:

-   Who generated the traffic
-   Source IP
-   Destination IP
-   Service
-   Application
-   Action
-   Policy ID
-   Bytes transferred
-   Session information
-   Security events

Example:

``` text
Source:      192.168.10.25
Destination: 8.8.8.8
Service:     DNS
Policy:      LAN_TO_INTERNET
Action:      ACCEPT
```

Logging is extremely important for:

-   Troubleshooting
-   Security monitoring
-   Incident investigation
-   Compliance
-   Auditing

------------------------------------------------------------------------

# 4.47 Policy Hit Count

Firewall policies can provide information about how frequently they are
being used.

A policy receiving no traffic for a long period may indicate:

-   Unused configuration
-   Obsolete requirement
-   Incorrect policy
-   Incorrect traffic path

Regular policy review helps maintain a clean firewall configuration.

------------------------------------------------------------------------

# 4.48 Firewall Policy Lifecycle

Firewall policies should not simply be created and forgotten.

A typical lifecycle is:

``` text
Requirement
    |
    v
Design
    |
    v
Policy Creation
    |
    v
Testing
    |
    v
Monitoring
    |
    v
Review
    |
    v
Optimization
    |
    v
Retirement
```

Policies should be reviewed when:

-   Applications change
-   Users change
-   Networks change
-   Security requirements change
-   Business requirements change
-   Policies become obsolete

------------------------------------------------------------------------

# 4.49 Common Firewall Policy Mistakes

## Mistake 1 -- Allowing Any to Any

``` text
Source: all
Destination: all
Service: all
Action: ACCEPT
```

This can create excessive exposure.

------------------------------------------------------------------------

## Mistake 2 -- Incorrect Policy Order

A broad policy placed above a specific policy may capture traffic
unexpectedly.

------------------------------------------------------------------------

## Mistake 3 -- No Logging

Without logs, troubleshooting and security investigations become
difficult.

------------------------------------------------------------------------

## Mistake 4 -- Excessive Services

Allowing `ALL` services when only HTTPS is required increases the attack
surface.

------------------------------------------------------------------------

## Mistake 5 -- Overly Broad Source Networks

Allowing an entire network when only a specific host requires access
violates least privilege.

------------------------------------------------------------------------

## Mistake 6 -- Unused Policies

Old policies increase configuration complexity and can create security
risks.

------------------------------------------------------------------------

## Mistake 7 -- Poor Naming

Names such as:

``` text
Policy1
Test
New Policy
Policy123
```

make large environments difficult to manage.

------------------------------------------------------------------------

# 4.50 Firewall Policy Best Practices

### Use Least Privilege

Allow only required traffic.

### Use Specific Objects

Prefer:

``` text
Web_Server_01
```

over:

``` text
all
```

when appropriate.

### Restrict Services

Use:

``` text
HTTPS
SSH
DNS
```

instead of:

``` text
ALL
```

when possible.

### Use Meaningful Names

Example:

``` text
USER_TO_WEB_HTTPS
```

### Enable Appropriate Logging

Log traffic that is important for security and troubleshooting.

### Review Policies Regularly

Remove unnecessary or obsolete rules.

### Control Administrative Access

Management services such as:

-   SSH
-   HTTPS
-   RDP
-   SNMP

should be restricted to authorized networks or administrators.

### Separate Security Zones

Use appropriate segmentation between:

-   Users
-   Servers
-   Guests
-   Management
-   DMZ
-   Internet

------------------------------------------------------------------------

# 4.51 Firewall Policy Example -- Internet Access

A simplified enterprise Internet policy:

``` text
Policy Name:
LAN_TO_INTERNET

Incoming Interface:
LAN

Source:
Corporate_Users

Outgoing Interface:
WAN1

Destination:
all

Schedule:
Always

Service:
HTTP
HTTPS
DNS
NTP

Action:
ACCEPT

NAT:
Enabled

Security Profiles:
Antivirus
Web Filter
Application Control
IPS
SSL Inspection

Logging:
Enabled
```

This represents a controlled Internet access policy rather than
unrestricted access.

------------------------------------------------------------------------

# 4.52 Firewall Policy Example -- Guest Network

A typical guest network design:

``` text
Policy 1:
Guest → Corporate LAN
Action: DENY

Policy 2:
Guest → Internet
Action: ACCEPT
NAT: Enabled
Web Filter: Enabled
Application Control: Enabled
Logging: Enabled
```

This provides Internet access while protecting the corporate network.

------------------------------------------------------------------------

# 4.53 Firewall Policy Example -- Server Access

Suppose an application server requires HTTPS access to a web server.

A restrictive policy can be designed as:

``` text
Source:
Application_Server

Destination:
Web_Server

Service:
HTTPS

Action:
ACCEPT
```

This is preferable to allowing:

``` text
Application_Server → Web_Server
ALL SERVICES
```

when only HTTPS is required.

------------------------------------------------------------------------

# 4.54 Firewall Policy Example -- Administrative Access

Administrative access should be restricted.

Example:

``` text
Source:
Network_Admins

Destination:
Management_Network

Services:
HTTPS
SSH

Action:
ACCEPT

Logging:
Enabled
```

Other users should not receive unrestricted management access.

------------------------------------------------------------------------

# 4.55 North-South Traffic

**North-South traffic** refers to traffic moving between internal
networks and external networks.

Example:

``` text
Internal Network
       |
       v
   Firewall
       |
       v
   Internet
```

Examples:

``` text
LAN → Internet
Internet → DMZ
VPN → Internal Network
```

Firewalls commonly provide security enforcement for North-South traffic.

------------------------------------------------------------------------

# 4.56 East-West Traffic

**East-West traffic** refers to traffic moving between internal systems
or internal network segments.

Example:

``` text
User VLAN
    |
    v
Firewall
    |
    v
Server VLAN
```

Examples:

``` text
Users → Servers
Servers → Database
Guest → Internal Network
Application → Database
```

Controlling East-West traffic is important for limiting lateral movement
during security incidents.

------------------------------------------------------------------------

# 4.57 Firewall and Network Segmentation

Firewall policies become significantly more effective when combined with
network segmentation.

Example:

``` text
                 FORTIGATE
                     |
       +-------------+-------------+
       |             |             |
      LAN           DMZ         GUEST
       |             |             |
    Users         Servers      Visitors
```

Each zone can have separate policies.

This creates controlled trust boundaries throughout the network.

------------------------------------------------------------------------

# 4.58 Firewall Policy and Zero Trust

Modern security architecture increasingly follows the principles of
**Zero Trust**.

Traditional model:

``` text
Inside = Trusted
Outside = Untrusted
```

Zero Trust model:

``` text
Never Trust Automatically
        +
Verify Explicitly
        +
Least Privilege
        +
Continuous Monitoring
```

Firewall policies can support Zero Trust by enforcing:

-   Identity-based access
-   Application-based access
-   Network segmentation
-   Least privilege
-   Explicit authorization
-   Continuous logging and monitoring

------------------------------------------------------------------------

# 4.59 Firewall Policy Troubleshooting Concepts

When traffic is not working, administrators should determine:

1.  Is the traffic reaching the FortiGate?
2.  Is the correct interface being used?
3.  Is the source address correct?
4.  Is the destination address correct?
5.  Is the correct route available?
6.  Does a firewall policy match?
7.  Is the service allowed?
8.  Is NAT required?
9.  Is a security profile blocking the traffic?
10. Is return traffic correctly routed?
11. Is the session being created?
12. Are logs showing ACCEPT or DENY?

A structured troubleshooting approach is more effective than randomly
changing policies.

------------------------------------------------------------------------

# 4.60 Firewall Policy Troubleshooting Flow

``` text
Client
  |
  v
Packet Reaches FortiGate?
  |
  +---- NO → Check Network/Interface/VLAN/Routing
  |
  YES
  |
  v
Correct Firewall Policy?
  |
  +---- NO → Check Policy Order and Matching Criteria
  |
  YES
  |
  v
Policy Allows Traffic?
  |
  +---- NO → Check Action/Service
  |
  YES
  |
  v
NAT Correct?
  |
  +---- NO → Check NAT Configuration
  |
  YES
  |
  v
Security Profile Blocking?
  |
  +---- YES → Check Security Logs/Profile
  |
  NO
  |
  v
Return Path Correct?
  |
  +---- NO → Check Routing
  |
  YES
  |
  v
Traffic Should Pass
```

------------------------------------------------------------------------

# 4.61 Firewall Policy Security Model

A strong firewall policy architecture can be summarized as:

``` text
                FIREWALL
                   |
        +----------+----------+
        |                     |
   Traffic Control       Security Inspection
        |                     |
   Source/Destination     Antivirus
   Interface              IPS
   Service                Web Filter
   Schedule               Application Control
   User                   SSL Inspection
        |                     |
        +----------+----------+
                   |
                   v
              Final Action
                   |
             +-----+-----+
             |           |
           ALLOW        DENY
```

------------------------------------------------------------------------

# 4.62 Key FortiGate Firewall Objects

A scalable FortiGate configuration commonly uses reusable objects.

### Address Objects

Represent IP addresses or networks.

### Address Groups

Combine multiple address objects.

### Services

Represent protocols and ports.

### Service Groups

Combine multiple services.

### Schedules

Define when policies are active.

### Security Profiles

Define deeper inspection and protection.

### User Groups

Associate access with authenticated users.

Using reusable objects improves:

-   Scalability
-   Readability
-   Consistency
-   Troubleshooting
-   Policy management

------------------------------------------------------------------------

# 4.63 Firewall Policy vs Security Profile

These two concepts should not be confused.

### Firewall Policy

Primarily answers:

> **Should this traffic be allowed to pass?**

### Security Profile

Answers:

> **How should the permitted traffic be inspected and protected?**

Example:

``` text
Firewall Policy
LAN → Internet → ACCEPT
        |
        v
Security Profiles
        |
        +--- Antivirus
        +--- IPS
        +--- Web Filter
        +--- Application Control
        +--- SSL Inspection
```

The firewall policy provides the access decision, while security
profiles add security inspection and enforcement.

------------------------------------------------------------------------

# 4.64 Firewall Policy vs Routing

Routing and firewall policies perform different functions.

### Routing

Determines:

> **Where should the packet go?**

### Firewall Policy

Determines:

> **Is the traffic allowed to go there?**

Example:

``` text
Routing:
LAN → WAN1

Firewall Policy:
LAN → WAN1 → ACCEPT
```

Both routing and policy processing are important for successful
communication.

------------------------------------------------------------------------

# 4.65 Firewall Policy vs NAT

NAT and firewall policies are also separate concepts.

### Firewall Policy

Controls access.

### NAT

Translates addressing information.

Example:

``` text
Private IP
192.168.10.10
      |
      v
Firewall Policy
      |
      v
NAT
      |
      v
Public IP
203.0.113.10
```

A packet may be allowed by the firewall policy but still fail because of
incorrect NAT or routing.

------------------------------------------------------------------------

# 4.66 Important Firewall Terminology

  Term                  Meaning
  --------------------- -------------------------------------------------
  Firewall              Security system that controls network traffic
  Policy                Rule controlling traffic
  Stateful Inspection   Tracking active network sessions
  Source                Origin of traffic
  Destination           Target of traffic
  Service               Protocol/port definition
  Schedule              Time during which policy is active
  NAT                   Network Address Translation
  SNAT                  Source Network Address Translation
  DNAT                  Destination Network Address Translation
  DMZ                   Semi-trusted network zone
  Security Profile      Additional traffic inspection/control
  IPS                   Intrusion Prevention System
  AV                    Antivirus
  Web Filter            Controls web access
  Application Control   Identifies/controls applications
  SSL Inspection        Inspection of encrypted traffic
  Implicit Deny         Default blocking when no policy permits traffic
  Policy Order          Sequence in which policies are evaluated
  Least Privilege       Allow only required access
  Segmentation          Separating networks into security zones



------------------------------------------------------------------------

# 4.67 Chapter Summary

A firewall is a critical security control used to regulate communication
between networks and systems.

FortiGate firewall policies form the foundation of traffic control in
FortiGate environments.

The most important concepts covered in this chapter are:

-   Firewall fundamentals
-   Stateful firewalling
-   Next-Generation Firewalls
-   FortiGate firewall policies
-   Policy components
-   Source and destination matching
-   Interfaces
-   Services
-   Schedules
-   ACCEPT and DENY actions
-   NAT
-   Security profiles
-   Policy order
-   Implicit deny
-   Address objects
-   Address groups
-   Service objects
-   DMZ policies
-   Inter-VLAN policies
-   User-based policies
-   Application Control
-   Web Filtering
-   Antivirus
-   IPS
-   SSL inspection
-   Firewall logging
-   Policy lifecycle
-   Policy best practices
-   North-South traffic
-   East-West traffic
-   Network segmentation
-   Zero Trust principles
-   Firewall troubleshooting concepts

------------------------------------------------------------------------

# 4.68 Key Takeaways

> **A firewall controls network access.**

> **A firewall policy defines who can communicate with whom, through
> which interface, using which service, and under what conditions.**

> **Policy order matters.**

> **Traffic that is not explicitly permitted should be denied.**

> **Least privilege should be the foundation of firewall policy
> design.**

> **Security profiles provide additional inspection and protection
> beyond basic traffic filtering.**

> **NAT translates addressing; it does not replace firewall policy
> control.**

> **Routing determines the path, while firewall policy determines
> whether traffic is permitted.**

> **Logging is essential for visibility, troubleshooting, and security
> monitoring.**

> **A well-designed firewall uses segmentation, specific policies,
> meaningful objects, appropriate logging, and regular policy review.**

------------------------------------------------------------------------

