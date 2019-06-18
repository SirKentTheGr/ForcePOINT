# Day 2 - Monday June 7th, 2019
=============================================================================
## Day 1 review
1. What are the servers that make up the SMC?
2. what are the 3 platforms on which the SMC will run?
3. What interface types are supported by FW/VPN Role?
4. On which platforms will the NGFW Run?
5. What Routes are added Automatically?
6. Can Multicast be Tunneled over a VPN?
7. What Multicast protocols can be statically routed?
8. What is the Policy Handshake?
9. Name two Feature supported by Single FW and not clusters?
10. What information is required for initial contact?
11. What is the Preferred way to upgrade an Engine?
12. What are 3 ways to Deploy Mgt. Client?
13. How any standby servers are supported in SMC HA? (Mgt)
15. LogServer does support Replication
16. SMC supports replication (incremental)


## Answer Key:

1. Log Server, management server, log servers
2. Windows, Linux, SMC appliance
3. Layer 2 interfaces, Layer 3 interfaces, VLAN interfaces,
Tunnel Interfaces
4. Microsoft Hyperv, KVM, Azure, VMWARE, AWS, You're own hardware (must be intel), Appliance
5. Directly connected
6. Yes, but it's important to keep in mind that only route based vpns, due to added unicast header added to policy based VPNS, operating in tunnel mode, with different packet sizes.
7. IGMP, PIM (three PIM modes)
8. Prevents lockout of SMC; the SMC compiles the policy in a format, applies configuration to engine, the SMC and engine communicate, but if the communication is broken the NGFW will dump the changes and roll back the configuration (after 60 seconds)
9.  Wireless interfaces, and one ForcePOINT model has a switch built in, ADSL Interfaces
10. SMC IP address, OTP, Specified mgmt interface, & IP address for engine

          If the OTP is incorrect use 'sg-contact-mgmt <OPT>'
11. Via the SMC
12. WEBswing, JavaClient, WEBstart
13. 4 standby (1 active for a total of 5 but only 4 standby)


## Module 4. POLICIES

#### NGFW Policy types
 - One Policy for each NDFW roles
    - FIEWALL/VPN role
      - FIREWALL policy
        - Layer 2 interface
 - IPS Policies for IPS roles
 - Layer 2 Firewall Policies for Layer2 Firewall Roles (DIAGRAM BELOW)
          NAT rules
          IPv4/IPv6 Access rules
                                                    ====== ROUTING
          Layer 2 interface policy - Ethernet ACCESS rules
          INSPECTION policy
          Files FIltering Policy

- Firewall Policy for FW/VPN Roles
- Additionally Layer 2 Interface Policy if Layer 2 Interfaces are used: Diagram Below

      IPv4/IPv6 Access rules
                  ================== Routing
      Ethernet ACCESS rules
      INSPECTION policy
      Files Filtering Policy
      ("mixed mode" is Layer 2 and Layer 3 firewall at the same time on the same box - intermixed L2 & L3 interfaces)
- The NGFW Policy gathers together all the rules from the different policy elements
  - template Policy
  - Inspection policy
  - Sub policy  
  - SSL VPN Policy
  - File Filtering Policy
        Firewall template (NO INSPECTION)
        ||
        ====> Custom template (Inspection)
                ||
                ==========> _____________________________
                            |SUB Policy                   |
                            |Access Control - NAT Rules   |
                            |INSPECTION Policy            |
                            |FILE Filtering policy        |
                            =============================
                                      NGFW POLICY
                                          ||
                                          ||
                                          \/
                                          FW
        Processes in order from the top Firewall Template to Sub Policies
        The Firewall and policy contain an arbitrary number of rules &
        The first rule to match the firewall stop the Process
        If no match is found - the firewall has a default deny rule

- NGFW Policy Templates
  - Each NGFW Policy must always be based on a Template POLICY
  - Template Policies reduce the need for creating the same rule in several POLICIES
  - Template Policies can enforce administrative boundaries

        Firewall template (NO INSPECTION)
          ||
            ====> Custom template (Inspection)
                        ||
                        ||                          _____________________________
                        ==========>   NGFW POLICY  |Firewall Template Rules
                                                   | Firewall Template Rules  <==== FIREWALL TEMPLATE INSERT POINT
                                                   |Custom TEMPLATE rules    <====  Custom TEMPLATE INSERT POINT
                                                   | NGFW POLICY Rules
                                                   =============================
 - FW/VPN and L2 FW Roles
 - Firewall Template and Layer 2 Firewall Template are Templates for Access Control only
