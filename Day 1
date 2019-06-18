Day 1 - Monday June 6th, 2019
=============================================================================
"there's a time a place for dynamic routing, and that's never and in 	the trash"

The only time Dynamic routing in required in the commnad line is with RIP, the rest can be done in the GUI"

AGENDA:

1. SMC OVERVIEW

Licensing covers everything, except for cloud services which would have to be purchased seperate

NGFW is just software and can be installed in three different roles
  Firewall/vpn		(access control)
  Layer 2 Firewall	(access control)
  IPS
The deep inspections is identical with all three roles

The capabilities are almost identical with all three Operting roles
  high availability/clustering (L3) 	(not true clustering for L2FW - more like master slave)
            (For IPS clustering - just connect to switch or vpn with heart beat enable)
            (additionally - Serial clustering is baked into the liscense)
  static and dynamic routing (L3)
  network address translation (L3)
  multilink (L3)
  site to site vpn (L3)
  client to gateway vpn (L3)
  ssl vpn portal (L3)
  authentication (L3)
  server load balancing (L3)
  IDS monitoring (L2)
  Fail-open interfaces (L2) 		(L2FW does not fail open)
  Anti-Malware
add on services that require separate licensing
  Web Filtering
  Advanced Malware Detection* (Cloud Sandbox)


NGFW Engines:
  Runs on an integrated, secure Linux-based operating system
  Security patches are included in engine upgrades
  Not Hot-swappable "We don't do Hot Fixes - it is integrated into upgrades)

Appliances:
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

Testing at NSS Labs:???
  NGFW 3301 throughput
  Data Sheet = ???
  Enterprise Perimeter Protocol Mix???
  Third variable ??? = ???
  (I wasn't able to grab this data in time)

**	Vacabulary:
NGFW VIRTUAL APPLIANCE = 	NGFW engine installed on a virtualization platform = 	AWS, AZURE, VMWARE, Microsoft Hyper-V, KVM
NGFW VIRTUAL CONTEXT = 		Split physical appliance into logical NGFWs =		physical machine with multiple interfaces

NGFW Requirements:
  Interfaces:
    1 or More certified network interfaces for the Firewal/VPN role
    2 or more certified network interfaces for IPS and IDS configuration
    3 or more ertified network interfaces for Inline IPS or Layer 2 Firewall

  Master Engine:
    Each Master Engine must run on a seperate physical NGFW device
    All the Virtual Security Engines hosted by a Master Engine or Master Engine Cluser must hae the same role and the same Failure Mode

  Compliance Limitations:
    Standby Clustering only for FW/VPN Role
    Clustering is not supported in IPS or L2FW Role

**	Check out liturature _ HANDLEY & PAXON IPS Journal

Installation of NGFW
  USB drive installation with automatic policy push
    Management Client -> SMC -> Save Inintial Configuration File -> USB -> Machine

  Manual Configuration via the configuration wizard
    Client ....Serial Connection...Machine
  You will need:
    IP ADDR. FOR Engine (Subnet MASK, GW)
    IP ADDR of SMC
    ONE-TIME Pasword (set within the initial configuration file) (Painfully long AES 256)
      (If wizard says "perhaps Passord is wrong" it is 100% wrong)
      'sg-contact-mgmt <OTP>' - USE IN CASE OF WRONG OTP
      'sg-reconfigure' - NEVER RUN IN PRODUCTION -WILL PROMP RECONFIGURATION OF FW
      'sg-reconfigure --no-shutdown'

Remote Engine Upgrades:
  - One-Step for engine upgrades
    - Upgrade through Management Client
      -No Local Physical Action needed
    - Fail-safe remote engine upgrades
      - version roll-back in case of problem
      - Old engine version operative until the new one is ready
** in the case that the FW should refer back to old FM version
      'sg-toggle-active'
    - Automatic scheduling available
    - Also upgradeable from USB drive

Security Management Center (DIAGRAM of SMC connecting to multiple locations geographically)
  Headquarter - ForcePOINT NGFW Single Physical Appliance managing multiple NGIPS physical/virtual appliances
  Regional Office
  Branch Location
  Home OFF
  Public Cloud
  Datacenter - private cloud

NGFW Architecture
  Security Management Center Consists of
    Web portal server  (optional)
      accessed by Web Portal (Customer or Helpdesk)
      connects to management server
    management server (required) (uses postgreSQL database)
      access by Management Client (Administrator)
      connects to web portal server, log server, NGFW Engines, and Mangement Client
    and log server (required)
      access by mangement client
      connects to 3rd party device, NGFW engines, Management Server, and Management Client
        3rd party devices require .2 of an SMC license

NGFW System Architecture
  Centralized management and High Availability Options
    Layer 3 NGFW
    Layer 2 NGFW
    Layer 2 IPS
    Layer 3 Virtual NGFW
    Thrid Party Device
  ** In a headquarters basic setup - You must allow conectivitey from oulying firewalls to connect back to SMC. Which takes two rules on 			Firewall template (this will not work with OOB management grid topology or VPN)

Capacity
  One management server can manage up to 2,000 NGFW engines
  A log Server can process more than 500,000 records per second
  Additional Log Servers can be added to increase scalability
  High Availability option for
    Management Server (requires separate license for MS HA)
    Log Server

Management Server High Availability
  Only one server is active at a time
  Data is synchronized from the active Management Server to the standby servers
  Data Replication is incremental
  Switch from standby mode to Active mode is manual
  Control Management Servers through the Management Client
  Requires special licensing

**NOTE: TCP PORT 3020 is required for LOG SERVER communication, which is responsible for all monitoring in the SMC

Configuring Additional Management Servers
  Install a new license that lists the IP address of all Management Servers
  Allow Communication to the new Management Server through firewalls as necessary
    SG control (8902-8913)
    SG Status Monitoring (3023)
  Install the additional Management Servers on target servers.
    New Management Server Element is autmatically created in SMC

  Simply click Install as an Additional Management Server for High Availability

Administrate management Servers (menu - system tools - control management servers)
  The control Management Servers dialog box allows to:
    Change the active Management Server
    Enable, Disable, & Force Database Synchronization
    Run diagnostic

Log Server High Availability
  Ensures that system monitoring continues if one Log Server fails


Setting log Servers as Backup
  Create the additional Log Server elemente
  Setting Log servers as Backup
    Open Properties of the primary Log Server
    Set up all the secondary log servers associated to primary
    Install Policy on the engine
    (View under properties - Under High Availability - add or remove)

Upgrading the System
  One SMC installer for all SMC components
  All SMC components must use the same SMC software version
  Upgrade of the Management Client is automatic when Java WebStart is used
  SMC can manage NGFW engines with lower versions
  Upgrade steps:
    Update Licenses in SMC
    Upgrade all SMC components
    Upgrade Engines

Upgrading Management Server HA System
  Backup Active Management Server (optional)
  Exclude Standby Management Server
  Upgrade Active Management Server
  Start Active Management Server
    OK
    KO

Management System Licensing
  Subscription Liceneses (12 Months)
    Per Seat Licensing (# of nodes)
    Bundle of Management and log server Licenses
    HA alternative
  Annual Support fee (2 support levels + account level MCS)
    Premium (includes software upgrades)
    Premium Priority (includes software upgrades
  Additional Feature pack licenses (subscription 12 Months)
    Web filtering
    cloud sandbox
  Additional Licenses (12 month subscription + support)
    Additional Log Server $
    Domains (200 Domains)$
    Web Portal Server (users included in base license) $

Software Firewall Licensing
  NGFW Subscription Licenses (12 months)
    7 Price bands depending on the number of CPU's

  Annual Support fee

  Sidewinder Virtual Firewall

  Annual Support fee

  Perpetual Licenses

Node Counting in SMC
  Node Counting for SMC license
    Physical appliance
    Virtual Appliance
    Virtual contexts = Only master is counted
    AWS PAYGO Virtual Appliance = not counted
    Third party device = .2
    Management Center components = NOT COUNTED
  Note counting for EOS products
    Legacy virtual appliances licenses = not counted
    FW-310L/FW315L appliance (also cluster) = .5
    FW-105 appliance = .2
  Appliances are still operating after maintenance contract, but license cannot be upgraded to new version. Software licenses can expire and 			policy refresh is not possible if that happens

Generate Liceneses
  IP address binding: The license is statically bound to the IP address of the licensed component
    Management Server
    log Server
    Web Portal Server
  management server proof of license code binding (POL): Liceneses are dynamically bound to the Managment Server's POL code
    Log Server
    Web portal Serverl
    Vitual appliances
    Engines install on your own hardware
  Appliance proof-of-serial (POS) code binding: The license is bound to the unique POS code of a preinstalled equipement
    Preinstalled equipment




ACCESSING LAB ENVIRONMENT: (I.E. not recommended)
https://traininglab.forcepoint.com/portal
USERN:	ngfw30@fpcert.com
PASSWD:	o6EH1D7BwnOa

 - with in the langing_machine (esx host)
 - Select 'Open Console'
  no password - please allow post logon script to complete and to sleep for two minutes
- avoid using Vsphere client
- use the menu that pops up to launch VM's
