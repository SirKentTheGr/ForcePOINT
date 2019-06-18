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
- FW/VPN, L2 FW, and IPS Roles
  - Firewall Inspection Template, Layer 2 Firewall Inspections Template, IPS Template, are Templates for Access

- Hierarchical Policies: Policy Templates
 - Firewall TEMPLATE
   - Custom TEMPLATE
      - HELSINKI POLICY
      - Paris POLICY
      - Atlanta POLICY

            *Use the custom Template to apply to all of your firewall policies*

 - Hierarchical Policies: Alias elements
  - Share your Policies Using Alias elements
  - Allow the same policy to be used on several engines
  - The Alias represents one or several IP addresses
    - the alias allow you to write a single rule, and apply it to multiple clusters, and will give it a unique translation value automatically
  - Most Useful in Template Policies and Sub-Policies

          *A bit of advice is avoid using 'any' - for the most secure/tighter firewall rules*
          *'ANY' actually represent 0.0.0.0 which includes all private and public ip addresses*
- to 'Continue' action can be used for:
  - Logging Options
  - Deep inspections
  - Protocol agents
  - QoS classes
- Most commonly used in Templates

      *Continue rules are under 'ACTION', and simply direct traffic to continue down the policies*

      *the best use case for the "continue rule" is logging*

 - Sub-POLICIES (not required)
  - Easier Policy management with shorter main POLICIES
  - Faster rule traversal
  - Option to limit administrator rights to Sub-Policies only
        *Use the Jump rule to access sub-policies*
        *This enforces boundaries on policies and conserves resources*
        *If the sub policy is accessed, the traffic will continue through additional policies unless a Discard Action is included in the Sub-Policy*


 - Ethernet Access rules
  - Layer 2 Interface Policy
  - IPS POLICY
  - Layer 2 Firewall Policy
  - With in the service Column the functionality i.e. arp, rarp, stp pertain to MAC addresses
  - the Hits counters shows the number of connections that have matched the rule
  - The Rule name is a unique identifier that is generated - this number ties logs to policies and policies to Logs

- Access Rules (IPv4/IPv6)
  - Firewall VPN POLICY
  - IPS POLICY
  - Layer 2 Firewall POLICY

        Everything in the source and destination is resolved to an IP address
          Basic Network elements
          Zones
          Users and User groups (Must integrate with LDAP or Active direct - with the ability to drag and drop users and user groups)(Required FUID - ForcePOINT User Identification)
          Domain names (When the Domain name is used in a policy it populates a list of IP addresses associated with that Domain )
          Countries
          Endpoint applications (you can drag and drop executables to block them. Like Internet Explorer)
          Endpoint Settings (This can ensure that any endpoint generating traffic, has baseline requirements met)
          Services (essential a port Number)
          Proxy
          URL categories (requires Threatseeker license)
          applications
          IP lists

        Additional Matching Criteria
          Users and User groups (if users are put here - they will be forced to authenticate)
          Authentication services
          Source VPN
          Time
#### Access Rules Actions List
1. Allow
2. Discard
3. Refuse (never use for outside facing traffic)
4. VPN
5. BLACKLIST
6. Continue
7. Jump

#### Logging - Options for logging:
1. NONE (It doesn't log anything)
2. STORED (Entries are sent to the log Server and stored) (for syslog or Splunk use stored)
3. TRANSIENT (From the engine to the log server, appears in log browser, but is not stored AKA written to disk) (best used for trouble shoot or testing)
4. ESSENTIAL  (works like stored, but if the engine losses contact to the server, Essential will temporary store logs, over writing older logs) (this is better than STORED because it will preserve essential logs )
5. ALERT (ALERT's appear in the home view, and anything that is set to ALERT can be configured to send notifications like emails to Administrator's)
  - Can be configure to kick off a chain of events called an ALERT CHAIN

- under Logging Options
  - you can log user information
  - log network applications (offers great visibility to the applications used within network)
  - log URL categories (requires threatseeker license )
  - Log Endpoint Information

- Logs Stored using UTC (GMT), Timestamps

#### ACTIONs - RC Actions _ CLICK ALLOW
- (This is where Deep inspection can be setup)
  - Deep Inspection File Filtering, Anti-SPAM
  - Decryption
  - Connection Tracking
  - Ide Timeout
  - Enforce TCP MSS
  - DoS Protection

- under connection tracking mode set to Inherited from Continue Rules

- Discard has options as well - User Response for blocked connections
 - When FW detects a pattern to be blocked in HTTP(s) connections, users can be notified
    - http redirect or HTML based responses for HTTP(s)
    - Connection reset
    - Silent Connection Discard

# Policies Knowledge Check
1.
2.
3.
4.
5.





# SMC MASTER SET UP FOR DUMMIES

                  1. Create master engines
                  2. Add physical Interfaces and
                     - set one up to be Management. (Add IP)
                     - Routing
                  3. Once you've added the physical interfaces, click on virtual resources and create the virtual resources next.
                     - Add to domain if applicable
                  4. Select 'Interfaces' and assign Virtual Resources to physical Interfaces
                  5. Create Master Engine Policy
               Note:  *The Master Engine needs a policy for the communication to the FW's*
                  6. Configure Virtual Firewall





# Assign Multiple Firewalls with IP's within the same Sub-Net - to go out of one physical interface  

                    Right click -> Edit interfaces --> Multiple Virtual Resources
