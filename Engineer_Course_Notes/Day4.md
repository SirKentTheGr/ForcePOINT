# Day 4 - Thursday June 20th, 2019
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

#### Active Directory Integration
- Directory Server provides access to information about user accounts in a user database.
  - Internal Directory Server: The Management Server’s internal user database
  - External directory servers: Active Directory or other LDAP

#### Authentication method defines how the user can authenticate.

- Internal authentication methods
  - User Password
  - Certificates
  - Pre-shared keys (for use with some third-party VPN clients) (not recommended because it puts IKE into quick mode)

- External authentication methods
  - Radius
  - Network Policy Server (Active Directory radius server) "Radius on Steroids"
  - TACACS+
  - LDAP

#### Authentication Process with radius

- Authentication Process with Active Directory
    -This example illustrates how authentication is done when Active Directory is used both as the directory server and the Network Policy Server as Radius Server.

1. A user requests authentication either when establishing a Client-to-Gateway VPN or to access a resource that requires authentication.

2. The Firewall checks whether the user exists in the LDAP domain (according to the LDAP domain information for the default LDAP domain). By default, the default Authentication Method defined for the default LDAP domain is used.

3. The firewall sends the authentication request to the Network Policy Server to verify the user-supplied credentials. The Network Policy Server server replies to the request with the result of the authentication (whether authentication is successful or not).

4. The result of the authentication is sent to the user. If authentication succeeds, the firewall lists the user as an authenticated user, taking note of both user name and authentication method. When the user opens new connections, Access rules that contain an authentication requirement may now match


#### User Authentication with Radius

        1. Create an Active Directory Server Element
        Task 1: Create an Active Directory Server Element Create Active Directory Server
         element to represent the external Active Directory server. This allows you to
          use the Active Directory user database as an external user database. In
           addition, NPS/IAS authentication can be configured in the same element.
           However, this is not mandatory and other authentication methods can be used as
            well. The Active Directory Server element includes both the LDAP user
             database (AD) and the RADIUS authentication service (NPS/IAS) configuration,
             so only one element is needed for user authentication purposes. For other
              external servers, you must create separate server elements for each of the
               two features.

        Define the LDAP settings. The settings include user account information (Bind
           User ID) and other settings (like Base DN, the LDAP tree under which the
              authenticating users’ accounts are stored) that the Firewalls and the
               Management Server uses to connect to the Active Directory server. You can
                also select LDAPS or Start TLS to enable Transport Layer Security (TLS)
                 for connections to the server.


        2. Define Authentication Server settings
             Define the allowed authentication methods for the users stored on the
              Active Directory server. If you use NPS/IAS  to authenticate the users,
               authentication settings must be configured. The RADIUS protocol is
                used.


        3. Add an LDAP Domain
          Each LDAP server has its own LDAP domain in the SMC. One LDAP domain can be
           selected as the default LDAP domain. Users who belong to the default LDAP
            domain do not need to specify the domain (for example, “username@domain”)
             when they are authenticating. Default domain can also be defined per NGFW in
              engine properties.

              Select the default Authentication Methods for all accounts in this LDAP
               Domain

        4. Connect to the LDAP Domain
            When the LDAP domain is defined, the Management Server connects to the
             external domain, and users and groups can be browsed from the Management
             Client.

        5. Define User Authentication in Access Rules
          Define user authentication in the access rules. In Firewall Access Rules, the
            users/groups and authentication methods defined in the Authentication cell are
            used as matching criteria for accessing a particular service. Connections from
              users that have not successfully authenticated or whose authentication has
              expired do not match rules with an authentication requirement (the
                connections are matched to rules further down in the policy).

          To configure the authentication matching parameters, you can Drag and Drop
           users and the associated authentication methods.

           Alternatively you can Double-click the Authentication cell to open the
            Authentication Parameters dialog.
            • You should enable require authentication
             and select one of the authorization types: by Connection or Client IP.
             • In the Users tab of the Authentication Parameters dialog, you select the
              Users or User Groups that this rule applies to.
             • In the Authentication tab of the Authentication Parameters dialog, you
              select the Authentication Method(s) or click Set to ANY to allow any
               authentication method.

#### Enhanced User Identification

- Access Control by User and User Group without requiring Firewall authentication
    - View User related statistics
    - User names are mapped to IP addresses from user directories using FUID agents
- FUID services monitors:
    - Logon events to associate users with IP addresses
    - Information about Group
#### Endpoint Context Agent (ECA)
- Endpoint Context Agent (ECA) collects the Windows endpoint metadata and sends it to the
NGFW
- Provides access control and/or logging based on:
  - Executables initiating network connections
  - Logged-in Windows user
  - Platform attributes like OS version, antimalware.

        Endpoint Context Agent (ECA) is a Windows client application that provides endpoint
        information to the Forcepoint NGFW. The endpoint information can be used to identify
        users, log their actions, and control access. Forcepoint NGFW can enforce security
        policies based on the information that is sent by the endpoints. The information can
        also be viewed in log data and used in reports. The main elements to use in Access
        Rules are the new Endpoint Application elements that are delivered through dynamic
          update packages. Applications can be identified by hash, certificate, or signer,
          for example. You can also create custom elements.

          ECA is a replacement for the McAfee Endpoint Intelligence Agent (EIA).


#### Custom EndPoint Application
- System EndPoint Application are provided by Dynamic Update packages
- Custom Endpoint Application Identified By
  - Signer Information
  - List of Executables
- Executable List Tool available to extract executable information from endpoints


#### User Session Monitoring
- Firewalls track active users identified by various components: FUID, ECA service and
 also the users directly authenticated against the Firewall.

- The User ID Service status is monitored in the Firewall info view

#### User Dashboard
Monitor user activities

- In the Home view of the Management Client, there are user dashboards where you can see
 an overview of user activity.

#### Mobile VPN
- Client based : IPsec & SSL VPN client
  - Client software installation
  - Enable secure access to any service and protocol in the corporate
 network

- Clientless: SSL VPN Portal
   - No need to install new software to enable secure connection to Web
 services
   - Log-in via SSL VPN portal

- The NAT Pool method is the quickest methods
  - No DCHP or Virtual Adapter needed
  - all users share the same NAT-ed internal IP addresses (this is bad)
- The second method is using the Virtual Adapter Methods
  - Virtual IP address is attached to a Virtual Adapter on the VPN
   client
  - Virtual IP address is assigned by DHCP server

### Mobile VPN VPN Client High Availability
- If the Forcepoint NGFW which terminates the client VPN has multiple
 endpoints these provide high availability for client VPN connections
  too. VPN clients have knowledge of the other end-points and the
   Mobile VPN tunnel is established with the other end-point if the
    active end-point fails.

####  Gateway Configuration
    1. Define the VPN client address management
    2. Define the Mobile VPN
    3. Define authentication settings (IPsec Client)
    4. Define users and authentication
    5. Create Access Rules
      - The difference between Apply and Enforce - Typically have the same effect but if the action is set to enforce and a packet that arrive with a vpn rule not on the VPN tunnel a packet with enforce applied will drop the packet, Apply will allow the connection to traverse further down the Access rules in case there was a positive match.


#### Troubleshooting Tools: VPN Client Diagnostics
  - Under Diagnostic Information
      - Search for 'FAIL BALOON' to find diagnostic Information
  -Traffic Capture - (PCAP?)

### Connection control

  - Traffic Inspection
