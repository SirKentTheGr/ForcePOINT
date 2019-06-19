# Day 3 - Wednesday June 19th, 2019
=============================================================================

## Day 2 review:
1. What are the rules that will ALWAYS be present in a Firewall POLICY?

2. What Optional Policies can be Associated W/ the FW POLICY?
3. What Rules are in the FW template?
4. What is the difference between the FW template & FW inspection template?
5. What does an insert point do?
6. In what order are rules processed?
7. What is an Alias and what is the Benefit of using it?
8. What are the log levels?
9. What are the supported rule actions?
10. What is a continue Rule & what is the benefit of using it?
11. List the Objects used in source and destination columns?
12. What is a contact Address?
13. What is a Location?
14. If there are remote firewalls and the SMC is NAT-ted behind an "HQ" Firewall, For example, what must you configure?
15. Where would you enable Deep inspection or enable network application logging?


## Answer Key
1. Access Rules & NAT rules
2. Inspection, file filtering, L2 interface policy
3. Management Rules, Other common setting for services and tasks, discard all
4. the FW inspection template has A series of continue rules that mark traffic for deep inspection.
5. It is a place holder in a template that allows the administrator to control the order of the rules.
6. - "Firewall rules" = "top down"/"in order"
   - "Policy guides processed" = Access, inspection, NAT, Routing, QoS

7. (Page 142 - 143) It is an object that is defined by its translation value for every Firewall you intend to use it on. Its a way of making sure that it translates to the correct value. User defined aliases always begin with a $ system defined begins with $$.

- Where are Aliases used most often? - Templates
  - Templates are created to apply similar rules across multiple firewalls; preventing individual management

##### Create an Aliases
- Alias Properties under POLICY
  - Add Translation values i.e. Network Subnet to Cluster values i.e. FW's under a single cluster

8. None, stored, essential, TRANSIENT, ALERT
  1. NONE (It doesn't log anything)
  2. STORED (Entries are sent to the log Server and stored) (for syslog or Splunk use stored)
  3. TRANSIENT (From the engine to the log server, appears in log browser, but is not stored AKA written to disk) (best used for trouble shoot or testing)
  4. ESSENTIAL  (works like stored, but if the engine losses contact to the server, Essential will temporary store logs, over writing older logs) (this is better than STORED because it will preserve essential logs )
  5. ALERT (ALERT's appear in the home view, and anything that is set to ALERT can be configured to send notifications like emails to Administrator's)
  - Can be configure to kick off a chain of events called an ALERT CHAIN

9. Allow, Continue, discard, refuse, VPN, Jump, Blacklist
  1. Allow
  2. Discard (silently drops the connection)
  3. Refuse (never use for outside facing traffic)(will send ICMP error code)
  4. VPN
  5. BLACKLIST
  6. Continue
  7. Jump

10. continue values set will take effect if permitted further down in the policy.
 - Provides Improved administration
 - the best use case for the "continue rule" is logging'
 - allow you to set deep inspection, logging levels, QoS values, Idle timeouts, Protocol Agents

11. Domain Names, end-point info, network elements, users, & zones

  - to use users Object you must configure End Point Context Agent (ECA), AD, and FUID (ForcePOINT User Identification)

12. An address used to reach comments behind a NAT

13. Determines whether a component uses a Contact Address or the Real IP Address

14. Contact Address, and Location, NAT rules, Access Rules

15. The Action options
  - (Right Click) edit action options, enable Deep Inspection
  - Never configure deep inspection on everything!

===================================================================================================================
#### Load Balancing

- CVI - KNOW FOR TEST

- NDI - KNOW FOR TEST
  - VPN does not terminate to the NDI, it terminates to the CVI
    - The IKE SA is known by all nodes in a cluster - SO there is almost zero delay in VPN connectivity and tunnel status in case of node failure.

- Packet Dispatcher - The default and recommended clustering mode is called the Packet Dispatch mode
  - selected by load and availability(i.e. State Table)
  - To avoid a problem where the firewall is deciding the path for every connection, configure the individual nodes to handle the entire connection (sticky connection)
  - If the primary heartbeat node goes down (Maintenance or anything else), the gratuitous ARP is what populates the switch table with the primary Heartbeat. If gratuitous ARP is turned off, this will not work.

#### PCAP
 - Two ways to take PCAP
    1. Through the SMC, you can configure a PCAP on multiple nodes simultaneously
    - Right Click Firewall - Select Tools - Capture Traffic - Select Interfaces - Configure PCAP Size
    - This will temporarily store itself on the SMC and then SMC itself to the workstation.
    2. You can also open simultaneous SSH connections and generate a TCPdump argument

#### Firewall Cluster Configuration
    1. Define a Firewall Cluster
      - Right Click new Firewall Clustering
    2. Set Clustering Properties
      - This screen is optional
      - Under Clustering - there will be two dummy nodes
      - At the bottom: Clustering Mode: is by default to "Balancing"
      - Heartbeat message Period (1000ms) and Heartbeat Failover Time (5000ms) are set by default
    3. Define Physical / VLAN Interfaces
      - Define L3 int first
      - if it is a set of clustered interfaces - choose clustering as PACKET DISPATCH - where you fill in Fake MAC address.
      - Apply QoS if necessary
    4. Set IP Addresses
      - If it is a clustered interfaces put the cluster IP (.254) is the IPv4 Address and the individual CVI addresses for Nodes.
    5. Define Options
      - Choose Primary control interface (Should be external if it is remotely being managed)
      - Set Primary and Backup Heartbeat Interface (typically not through internal network)
      - Set Identity Authentication Request IP
      - Set Source for Authentication Request (Set to routing unless Authentication is over a VPN)
      - Set Default IP address for Outgoing traffic
    6. Save Initial Configuration
      - Once the firewall cluster is configured, the license binding and the initial contact for each firewall node is done the same way as for a single firewall.
      - Under 'View Details' - You will also have a OTP for each node in the firewall
      - If the OTP is wrong - reinitiate the contact using 'sg-contact-mgmt <OTP>'

      (for a two node cluster you will need 3 IP's per node)


#### Tester Settings
- Periodical self-tests to ensures proper functioning
  - The swap-Space is a linux term and is used when that the node is out of space
      - do not perform a reboot on the firewall!
      - perform 'sginfo -f' for support reasons

#### Outbound Multi-Link
- Easy and Simple ISP multi-homing
  - Dynamic Load-Balancing across  multiple WAN connections
  - Bandwidth Aggregation - If you do multilink within a VPN
  - Fault-tolerant VPN tunnels using multiple WAN connections

- Limitations
  - Multi-Link applies only in the Firewall/VPN role
  - Currently supports IPv4 links only
  - Not transparent failover,

- Methods for Link Selections
  - Round Trip Time (RTT)
    - Measure time to setup connection on each NetLink and use to determine fastest link.
  - 10:100
    - Ratio Method
      - Link capacity is used to create a ratio for distributing Traffic
  - High Priority
    - QoS Method
      - QoS class is used to select outgoing NetLink

- A few terms that need to be committed to memory

  - Netlink
    - It is an object that represents a link to the internet (isp)
    - a netlink looks suspiciously like a router - however a Router can be used by the firewalls kernel (itself), the Netlink is not available by the Firewalls kernel
    - A Netlink is based upon a Router
  - Outboud MultiLink Objects
    - Is a collection of all of your NetLinks
  - To active you Multilink Dynamic Load Balancing : create a NAT rule connecting them. This can be done inside a VPN





























===================================================================================================================
