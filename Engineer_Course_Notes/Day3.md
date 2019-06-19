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
3. Management Rules, Other common setting for services and tasks, discard all,
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
