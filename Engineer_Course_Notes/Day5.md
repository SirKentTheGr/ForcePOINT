# Day 5 - Friday June 21th, 2019
=============================================================================

### Deep Inspection:

- Full inspection functionality
  - Protocol validation
  - System fingerprints for known exploits
- Dynamic updates
    Constantly updated!
- Custom fingerprints
  - Regular expression-based fingerprints
- NGFW Licenses provides Full deep inspection


#### Deep Packet Inspection Configuration
- Access control rules controls what network traffic goes to deep inspection  in the Inspection Policy
- Inspection-specific configuration is done in the Inspection Policy

1. FW TEMPLATE, TURN ON DEEP PACKET INSPECTION Rule by Rule (This is the surgical approach)
2. FW INSPECTION TEMPLATE


#### NGFW Policy Templates
- Inspection, File Filtering, and Layer 2 interface

#### Verifying and Tuning Inspection Passive Termination
- Passive termination. When passive termination is used (Advanced tab
   in the NGFW element properties), the NGFW creates a special log
    entry that notes that a certain connection was selected for
     termination, but the engine does not actually terminate the
      connection. This allows you to check the logs and adjust your
       policy without the risk of cutting important business
        communications.

- The Exceptions are matched before the main rules, which is reflected
 in location of the Exceptions panel before the main Rules tree. The
  most frequent use of Exceptions is to eliminate false positives,
   which typically require permitting a pattern for some part of the
   traffic while it still triggers a reaction when encountered in any
    other traffic.

- Logical interfaces, are extremely portable and can be used in all three firewall roles.


### Inspection Exceptions Rules Configuration
- Allow more heuristic actions to behaviors




### Malware Detection Process

1. New file comes in
2. Anti-Malware can creates a hash and verifies it against MacAfee GTI Global Threat Intelligence,
3. Known File hash is also sent to Forcepoint Advanced malware Detection which is supported both on cloud or Local sandbox

##### Malware Detection:
  - In the access rules:
        - If you want to send connections from HTTP for file filtering, simple elect that in the Action Column.

  - In the File Filtering Policy:
      - You can elect Allow After rules
        - File Reputation Scan
        - Anti-malware Scan
        - Advanced Malware Sandbox Scan (AMD)
            - You can specify the buffering level: Medium
            - Log Level When File Is Discarded: Alert
            - Action When No Scanners Are Available: Allow
        - keep Decompress Archives and Rematch Content

    - Anti-Malware Scan
        - Based on McAfee's Engine. Mirrors - update.nai.com/Products/CommonUpdater
        - Unfortunately - it is not intended to be used all the time.
        - Supported Protocols (IPv4)
            - HTTP, HTTPs
            - FTP
            -SMTP,POP3, IMAP
        - File filtering Policy determines which traffic is inspected for malware
    - McAfee GTI File Reputation Service
        - Cloud File Reputation Service
