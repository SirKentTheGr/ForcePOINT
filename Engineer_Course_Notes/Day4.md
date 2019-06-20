# Day 4 - Wednesday June 20th, 2019
=============================================================================

Commands overview - found in product guide - Engine Commands

| Item | Value | Qty |
| :------- | ----: | :---: |
|  | | |

# VPN Continued

- Full Mesh VPN
  -
- For a Star Topology simply drag and drop all of the Satellite/Spoke to the Satellite column.


#### Multi-Link VPN
- Link Mode The Link Mode of a VPN link determines how the link is used for VPN traffic:   
 - Active
 - Standby
 - Aggregate (Sub Tunnel aggregation is an all or nothing thing. The latency has to be nearly the same or packets will arrive out of order)
 - QoS-Based link selection

#### VPN Configuration
 1. Select End-Points
 2. Define Site content
    - Most of every one will uncheck "add and update IP addresses based on routing" and assign them manually under sites.
 3. Create Policy-Based VPN
    - The IPSec is based off of the "VPN consortium"
    - Creating a Policy-based vpn is as easy as RC-ing SD-WAN and create VPN
    - VPN Profile: IKE/IPSEC settings for VPN (Phase 1 and Phase 2)
    - Gateway Profile: Defines Capabilities
    - Note: It is not required that you configure custom VPN profile or Gateway Profile when creating a VPN from site to site Forcepoint nodes. But would be required for a third party node.
    - This is where NAT is applied to the VPN if necessary
 4. Define VPN Topology
    - Either Star(Hub and Spoke) or full mesh based on column selection. Keep in mind converging from Star to Full Mesh would require reconfiguration of Access Rules.
 5. Create Access Rule

#### Tunnel Mode Configuration
- The VPN links can be in three different modes:
    - Active: The link is used at all times. If there are multiple links in Active mode between the Gateways, the VPN traffic is load-balanced between the links.

    - Aggregate: The link is used at all times and each VPN connection is load-balanced in round robin fashion between all the links that are in the Aggregate mode. For example, if there are two links in Aggregate mode, a new VPN connection is directed to both links.

    - Standby: The link is used only when all Active or Aggregate mode links are unusable.

- QoS-Based VPN Multi-Link Selection You can classify VPN traffic with QoS classes and select the VPN Multi-Link tunnel based on this QoS classification.

#### VPN Tools
- Automatic VPN Validation
    - The Management Client has automatic IPsec VPN validation that checks that the settings you have selected are compatible with each other. If issues are displayed, read the descriptions and fix the problems that are described.
- Link Summary
- VPN SA Monitoring
    -The VPN SA Monitoring view lists all VPN Security Associations (SAs) that have currently been negotiated in the firewall
- SD-WAN dashboard
  - The SD-WAN dashboard allows you to monitor SD-WAN features, such as Multi-Link and VPNs, and to view statistics and reports related to SD-WAN features.


####  Hub VPN Overview

-  A Full mesh VPN between a large amount of gateways can create a very high number of tunnels. The Hub VPN allows to have the same VPN connectivity with much lower number of tunnels, since all gateways are able to connect to each other through the gateway that forwards the traffic.

-  In a VPN hub topology, a gateway is configured to forward VPN traffic between different VPN tunnels. The gateway that does this forwarding is called a hub gateway. The other gateways are spoke gateways.

-  The Hub gateway can route traffic from a Mobile VPN to a Site-to-Site VPN, or from one Site-to-Site VPN to another Site-to-Site VPN.

-  The Hub VPN allows to have the same VPN connectivity with much lower number of tunnels for the spoke gateways which allows using smaller appliances at spokes. For the Hub Gateway, the number of tunnel is the same as in a full mesh configuration. There is also an increased resource and bandwidth consumption on the Hub gateway, as each packet is encrypted on the spoke, decrypted and encrypted on the hub, and then decrypted again on another spoke. Bandwidth usage increases as well.

- The hub gateway must be set up specifically as a hub. The hub configuration is reflected in the topology, the Site definitions, and the VPN rules. The spoke gateways do not require any hub-specific configuration. In this example configuration, VPN tunnels are established from all spoke gateways to the hub gateway. All networks of all gateways are configured as reachable through the hub. Connections are allowed only as defined in the Firewall Access rules


#### Route-Based VPN

- Route-Based VPN can protect and route multicast communications between remote sites
    - Dynamic routing communications used by RIP, OSPF
    - Multicast stream
- Traffic is forwarded to Tunnel Interfaces based on routing.
- Tunneled traffic is typically encrypted using Ipsec.
- It is possible use not encrypted tunnel with GRE, IP-IP or SIT.

#### VPN Configuration
1. Select Tunnel End-Points
2. Define Tunnel Interface
  - You have to add a tunnel interface to the firewall
3. Configure Routing
  - RC and add your partners network to the tunnel interface
4. Create Route-Based VPN
5. Create Access Rule
    - In a route based VPN under action, do not select enforce, simply just allow the traffic.

#### Site-to-Site VPN and NAT Rules
    - Disable NAT for a specific traffic.
    - Create NAT rule to make an exception to other NAT rules
    - Match traffic based on Source, Destination, and Service
    - Leave the NAT cell empty












 ==========================================================================================
