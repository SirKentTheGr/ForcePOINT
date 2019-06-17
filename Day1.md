# Day 1 - Monday June 6th, 2019
=============================================================================
"there's a time a place for dynamic routing, and that's never and in 	the trash"

"The only time Dynamic routing in required in the command line is with RIP, the rest can be done in the GUI"

## AGENDA:
1. SMC OVERIVEW

2. SINGLE FW

3. ROUTING/ANTI-SPOOFING

4. POLICIES

## SMC OVERVIEW

- Licensing covers everything, except for cloud services which would have to be purchased separate

#### NGFW is just software and can be installed in three different roles
  - Firewall/VPN		(access control)
  - Layer 2 Firewall	(access control)
  - IPS

** The deep inspections is identical with all three roles **

#### The capabilities are almost identical with all three Operating roles
  - high availability/clustering (L3) 	
     - (not true clustering for L2FW - more like master slave)
     - (For IPS clustering - just connect to switch or vpn with heart beat enable)
     - (additionally - Serial clustering is baked into the license)
  - static and dynamic routing (L3)
  - network address translation (L3)
  - multilink (L3)
  - site to site vpn (L3)
  - client to gateway vpn (L3)
  - ssl vpn portal (L3)
  - authentication (L3)
  - server load balancing (L3)
  - IDS monitoring (L2)
  - Fail-open interfaces (L2) 		(L2FW does not fail open)
  - Anti-Malware

add on services that require separate licensing
  - Web Filtering
  - Advanced Malware Detection (Cloud Sandbox)



#### NGFW Engines:
  - Runs on an integrated, secure Linux-based operating system
  - Security patches are included in engine upgrades
  - Not Hot-swappable "We don't do Hot Fixes - it is integrated into upgrades)

#### Appliances:
  NGFW - 51 FW : 	1.9 Gbps: NGFW 	200 MBPS: VPN 	1 GBPS	(Small Business)	(Multilink is possible but clustering isn't too practicle)
  NGFW - 110	1.5		150		.600	(Small Business)	(Multilink is possible but clustering isn't too practicle)
    330	4		350		1.4	(Small Business)
    331	5		700		2	(Small Business)
    335	7		1		3	(Small Business)
    1101	50		1.5		4.5	(Business)
    1105	60		3		8.5	(Business)
    2101	60		5		13	(Corporate)
    2105	80		7.5		16	(Corporate)
    3301	80		9		18	(Enterprise)
    3305	160		15		20	(Enterprise)
    6205	240		22		22	(Enterprise)


#### **	Vocabulary:
- NGFW VIRTUAL APPLIANCE = 	NGFW engine installed on a virtualization platform = 	AWS, AZURE, VMWARE, Microsoft Hyper-V, KVM
- NGFW VIRTUAL CONTEXT = 		Split physical appliance into logical NGFWs =		physical machine with multiple interfaces

#### NGFW Requirements:
  - Interfaces:
    - 1 or More certified network interfaces for the Firewall/VPN role
    - 2 or more certified network interfaces for IPS and IDS configuration
    - 3 or more certified network interfaces for Inline IPS or Layer 2 Firewall

  - Master Engine:
    - Each Master Engine must run on a seperate physical NGFW device
    - All the Virtual Security Engines hosted by a Master Engine or Master Engine Cluser must hae the same role and the same Failure Mode

  - Compliance Limitations:
    - Standby Clustering only for FW/VPN Role
    - Clustering is not supported in IPS or L2FW Role

'''
Check out literature _ HANDLEY & PAXON IPS Journal
'''

#### Installation of NGFW
  - USB drive installation with automatic policy push
    Management Client -> SMC -> Save Initial Configuration File -> USB -> Machine

  - Manual Configuration via the configuration wizard
    Client ....Serial Connection...Machine
    - You will need:
      - IP ADDR. FOR Engine (Subnet MASK, GW)
      - IP ADDR of SMC
      - ONE-TIME Password (set within the initial configuration file) (Painfully long AES 256)
      (If wizard says "perhaps Password is wrong" it is 100% wrong)
      - 'sg-contact-mgmt <OTP>' - USE IN CASE OF WRONG OTP
      - 'sg-reconfigure' - NEVER RUN IN PRODUCTION -WILL PROMP RECONFIGURATION OF FW
      - 'sg-reconfigure --no-shutdown'

#### Remote Engine Upgrades:
- One-Step for engine upgrades
    - Upgrade through Management Client
      - No Local Physical Action needed
    - Fail-safe remote engine upgrades
      - version roll-back in case of problem
      - Old engine version operative until the new one is ready
      - in the case that the FW should refer back to old FM version
       - 'sg-toggle-active'
    - Automatic scheduling available
    - Also upgradeable from USB drive

#### Security Management Center (DIAGRAM of SMC connecting to multiple locations geographically)
  - Headquarter - ForcePOINT NGFW Single Physical Appliance managing -  multiple NGIPS physical/virtual appliances
  - Regional Office
  - Branch Location
  - Home OFF
  - Public Cloud
  - Datacenter - private cloud

#### NGFW Architecture
  - Security Management Center Consists of
    - Web portal server  (optional)
      - accessed by Web Portal (Customer or Helpdesk)
      - connects to management server
    - management server (required) (uses postgreSQL database)
      - access by Management Client (Administrator)
      - connects to web portal server, log server, NGFW Engines, and Management Client
    - and log server (required)
      - access by management client
      - connects to 3rd party device, NGFW engines, Management Server, and Management Client
         - 3rd party devices require .2 of an SMC license

#### NGFW System Architecture
  - Centralized management and High Availability Options
    - Layer 3 NGFW
    - Layer 2 NGFW
    - Layer 2 IPS
    - Layer 3 Virtual NGFW
    - Third Party Device

  In a headquarters basic setup - You must allow connectivity from outlying firewalls to connect back to SMC. Which takes two rules on  Firewall template (this will not work with OOB management grid topology or VPN)

#### Capacity
  - One management server can manage up to 2,000 NGFW engines
  A log Server can process more than 500,000 records per second
  Additional Log Servers can be added to increase scalability
  High Availability option for
    - Management Server (requires separate license for MS HA)
    - Log Server

#### Management Server High Availability
-  Only one server is active at a time
-  Data is synchronized from the active Management Server to the standby servers
-  Data Replication is incremental
-  Switch from standby mode to Active mode is manual
-  Control Management Servers through the Management Client
-  Requires special licensing

NOTE: TCP PORT 3020 is required for LOG SERVER communication, which is responsible for all monitoring in the SMC

#### Configuring Additional Management Servers
-  Install a new license that lists the IP address of all Management Servers
-  Allow Communication to the new Management Server through firewalls as necessary
  -  SG control (8902-8913)
  -  SG Status Monitoring (3023)
-  Install the additional Management Servers on target servers.
  -  New Management Server Element is automatically created in SMC

-  Simply click Install as an Additional Management Server for High Availability

#### Administrate management Servers (menu - system tools - control management servers)
-  The control Management Servers dialog box allows to:
  -  Change the active Management Server
  -  Enable, Disable, & Force Database Synchronization
  -  Run diagnostic

#### Log Server High Availability
-  Ensures that system monitoring continues if one Log Server fails


#### Setting log Servers as Backup
-  Create the additional Log Server element
-  Setting Log servers as Backup
  -  Open Properties of the primary Log Server
  -  Set up all the secondary log servers associated to primary
  -  Install Policy on the engine
    (View under properties - Under High Availability - add or remove)

#### Upgrading the System
-  One SMC installer for all SMC components
-  All SMC components must use the same SMC software version
-  Upgrade of the Management Client is automatic when Java WebStart is used
-  SMC can manage NGFW engines with lower versions
-  Upgrade steps:
  -  Update Licenses in SMC
  -  Upgrade all SMC components
  -  Upgrade Engines

#### Upgrading Management Server HA System
-  Backup Active Management Server (optional)
-  Exclude Standby Management Server
-  Upgrade Active Management Server
-  Start Active Management Server

#### Management System Licensing
-  Subscription Licenses (12 Months)
  -  Per Seat Licensing (# of nodes)
  -  Bundle of Management and log server Licenses
  -  HA alternative
-  Annual Support fee (2 support levels + account level MCS)
  -  Premium (includes software upgrades)
  -  Premium Priority (includes software upgrades
-  Additional Feature pack licenses (subscription 12 Months)
  -  Web filtering
  -  cloud sandbox
-  Additional Licenses (12 month subscription + support)
  -  Additional Log Server $
  -  Domains (200 Domains)$
  -  Web Portal Server (users included in base license) $

#### Software Firewall Licensing
-  NGFW Subscription Licenses (12 months)
  -  7 Price bands depending on the number of CPU's

-  Annual Support fee

-  Sidewinder Virtual Firewall

-  Annual Support fee

-  Perpetual Licenses

#### Node Counting in SMC
-  Node Counting for SMC license
  -  Physical appliance
  -  Virtual Appliance
  -  Virtual contexts = Only master is counted
  -  AWS PAYGO Virtual Appliance = not counted
  -  Third party device = .2
  -  Management Center components = NOT COUNTED
-  Note counting for EOS products
  -  Legacy virtual appliances licenses = not counted
  -  FW-310L/FW315L appliance (also cluster) = .5
  -  FW-105 appliance = .2
-  Appliances are still operating after maintenance contract, but license cannot be upgraded to new version. Software licenses can expire and policy refresh is not possible if that happens

#### Generate Licenses
-  IP address binding: The license is statically bound to the IP address of the licensed component
  -  Management Server
  -  log Server
  -  Web Portal Server
-  management server proof of license code binding (POL): Licenses are dynamically bound to the Management Server's POL code
  -  Log Server
  -  Web portal Server
  -  Virtual appliances
  -  Engines install on your own hardware
  -  Appliance proof-of-serial (POS) code binding: The license is bound to the unique POS code of a preinstalled equipment
  -  Preinstalled equipment






**ACCESSING LAB ENVIRONMENT: (I.E. not recommended)**

https://traininglab.forcepoint.com/portal

~~USERNane:	ngfw30@fpcert.com~~

~~PASSWD:	o6EH1D7BwnOa~~

- avoid using Vsphere client
- use the menu that pops up to launch VM's

**Complete Lab1:**

Notes: trouble shooting

Create new Super User

  - systemctl stop sgMgtServer
  - systemctl stop sgLogServer
  - /usr/local/forcepoint/smc/bin/sgCreateAdmin.sh
  - input username: and Password:


## SINGLE FW

#### Key Benefits & Differentiators
- One Software
  - Various Platform (Virtual, Software, Appliances, Azure, AWS)
  - 3 different Roles (NGFW/VPN,IPS,L2FW) under 1 NGFW Licenses
  - multi-layer deployment for NGFW/VPN Roles
  - Evolving Software

- Multi-Layer inspections
  - Application Control
  - TLS inspections
  - User Management

- IPS and AET Prevention
  - Full IPS capability
  - Full-STACK normalization
  - Detect advanced evasion

- Centralized management
  - Scalable and user-friendly
  - Hierarchical POLICIES
  - Log Analysis and Reporting
  - Detect advanced evasion

- Forcepoint Integration
  - Web Filtering with Threatseeker Intelligence
  - Web Gateway (Cloud Service)
  - Malware Detection
    - Forcepoint Advanced Malware Detection (Sandbox)
    - McAfee GTI

- multi-link and VPN capabilities
  - Load Balancing Links (ISP/MPLS)
  - Run VPN over Multi-link (HA)
  - QoS on Multi-links

#### NGFW Multi-Layer inspections

- Layer 2 access control configure in Ethernet Rules filter MAC frames based on MAC addresses and ether type values
- Layer 3 access control configure in Iv4 and IPv6 access rules filters traffic based on IP addresses, TCP/UDP ports or IP protocol numbers
- Layer 3 access control can match more advanced elements like applications, URL categories or lists, users or groups and ECA provided database
- Full stack, multilayer traffic normalization deconstructs and decodes packets
- dynamic stream-based deep inspections
- File filtering capability which can be extended with file reputation checks, anti-malware and sandbox investigation by external service

#### Single NGFW Configuration
1. Define a single firewall
2. Define interfaces/VLANs
 - Interface ID starts at zero (0 = eth0)
 - with virtual installation type 'sg-reconfigure --no-shutdown'
 - you can apply a type, zone, QoS
 - Type can be Capture Interface or In-line pair
  - In-line will act as if the firewall in between traffic
  - IF later on you want to apply specific pairs for in-line interfaces you can define a pair of interfaces as a Logical interface
  - Modem Interface and Tunnel Interface are only used for Single Firewall
  - some firewall models allow for wireless interface to wireless routers
3. Set IP addresses
  - Static or dynamic IP configurations
4. Define Interface Options
  - With a single firewall or with a cluster you must set the interface in which the firewall will be managed
    - This is important if the firewall is being managed remotely
  - A backup interface can also be set
  - Default IP address for outgoing traffic is typically useless
  - Identity for Authentication Requests: when the firewall is doing any authentication the ip that should be used is the IP that AD will recognize
    - these are the setting a firewall is using when it is routing traffic to an authentication server
  - the Source for Authentication Requests: According to Routing unless traversing a VPN
5. Save Initial configuration

##### New features include
 - DNS Relay
  - NDFW acting as a local DNS resolver
  - Specific domain names can be associated to:
    - Specific DNS Servers
    - Fixed DNS results

- under Configuration: Advanced Setting
 - never uncheck Policy Handshake
 - Keep Automated Node Certificate Renewal

**Lab 2 Single firewall installation**


 **Factory reset of NGFW and ssh password**

This gives the Administrator the ability to ssh to the firewall directly
ssh root@172.31.200.1

 - ' sg-clear-all '
(this boots into recovery kernel)

  - select option 2
  - select yes for recovery wizard
  - type in arbitrary hostname
  - type in Root password - this is the console Password
      - Password
  - do not forget to select enable SSH Daemon
  - click next
  - select proper interface
  - the last thing is that you need to enter a nope IP address manually
    - 172.31.200.1
    - Netmask/Prefix Length: * 24
  - Ensure that Do not Contact is enabled

- 'sg-reconfigure'
 - select Contact and
 - paste copied one time password into step 3 password


## ROUTING/ANTI-SPOOFING

#### Static ROUTING
 - Defines next hop router for packets that NDFW is sending outgoing  
  - Traffic reaches NGFW
  - Routing Dictates which next hop
 - Static routing is created by using the Routing Tools Pane in the routing section of the Engine Editor
 - Route to directly connected networks are configure automatically
 - static netlinks will not show up in the routers routing table (ip r)
 - use the QueryRoute tab to trouble shoot routing
 - there's more than one way to add a router
  - Click on the add route tab and you can add route forever

#### Static Routing with Route Metrics
- Route metrics can be used to create active and standby route to the same destination network using static routing
- Route monitoring is used to monitor health of routes and trigger NGFW to use standby route when active route has failed
- Does not require applying dynamic source NAT to traffic like multilink
- Secondary route can easily be observed based off of Metric Number in the GUI

#### Static Routing with ECMP ROUTING
- ECMP (equal-cost multi-path) routing enables packets destined to same destination network to take multiple routes via different connections
- Benefit is increased bandwidth because multiple connections are used at same time
- Route Monitoring is used to monitor ECMP route heath.
- Does not require applying dynamic source NAT to traffic like multilink

#### Special Routing Condition
- Dynamic IP address (supports BGP and OSPF as well)
 - Routing when a firewall has a dynamic IP addresses
 - Management connections are initiated from note with dynamic control interfaces
 - Routing can be applied to interface based off of DHCP automatically


- Policy Routing (recommended that this is not used by instructor due to lack of port filtering)
  - Packets from specific source IP addresses to specific destination addresses are routed through a selected gateway
#### Multicast Routing
  - The NDFW supports
    - Static IP Multicast
    - IGMP-based mulitcast forwarding (IGMP Proxying)
    - Protocol-independent multicast (PIM)
    - PIM Dense Mode
    - PIM Sparse Mode
    - PIM Source-Specific Multicast Mode

**Multicast traffic can be forwarded over Route-based VPN but not over IPsec VPN---**
**GRE is the workaround**

      The order in which Forcepoint Process connections is
        1. ACCESS Rules
        2. INSPECTION
        3. NAT
        4. ROUTING

#### Dynamic ROUTING
- Configurable via SMC
 - OSPF v2
 - BGP v 4
 - PIM (RCF 4601)
- Configurable via Commandline
 - RIP v1 & 2
- SIngle node or clustering
- Route monitoring and configuration backup / restore through Management Client is available even when configuring dynamic


#### Anti Spoofing


## POLICIES
