
CASE INFORMATION

    CASE ID: Brute 002
    EVENT TYPE :  Brute Force Attack 
    SEVERITY: HIGH 
    DATE : 29-07-2026
    ANALYST : Edom Chinedu Williams 
    PLATFORM : LetsDefend SIEM
    STATUS : Resolved




| Investigation Outcome | Result                     |
| --------------------- | ----------------------    |
| Incident Type         | Brute Force Attack         |
| Alert Status          | True Positive              |
| Endpoint Compromised  | Yes                        |
| Initial Access        | Successful Brute Force     |
| Privilege Escalation  | Not conclusively confirmed |
| Persistence           | Confirmed                  |
| Lateral Movement      | Not Observed               |
| Data Exfiltration     | Not Observed               |
| Overall Severity      | High                       |



Incident Details 
    
    ALERT NAME :  Sudoers File Modification Detected
    EVENT ID : 302
    Alert Time :  06:35 AM
    Hostname : KRISTINE
    Operating System : LINUX OS 
    Source ip : 149.88.25.133
    Destination ip: 172.16.17.129



Alert  Analysis 

    | Field            | Value                                 |
    | ---------------- | ------------------------------------- |
    | Alert Name       | Sudoers File Modification Detected    |
    | Detection Source | Endpoint security (LetsDefend)        |
    | Severity         | High                                  |
    | Detection Time   | 06:35 AM                              |
    | Affected Host    | KRISTINE                              |




Authentication Analysis 

    Analysis of the available authentication logs identified multiple failed login attempts against the KRISTINE endpoint (172.16.17.129). The attempts targeted multiple user accounts, including analyst, admin, and united, within approximately one minute at 04:32 hrs, indicating an automated credential attack.

    Approximately one minute later, the following SSH authentication event was recorded:

    Accepted password for analyst from 149.88.25.133 port 55080 ssh2

    This confirms that the attacker successfully authenticated to the endpoint using the analyst account from the identified source IP.

    Subsequent analysis of the available audit logs identified privileged command execution, including creation of a new local account and modification and enumeration of sudo privileges. These activities confirm that the successful authentication resulted in unauthorized post-authentication activity and compromise of the endpoint.


Source Ip Analysis 

    Overall Reputation Assessment :(malicious)  
    
        The source IP address (149.88.25.133) was assessed using multiple threat intelligence platforms, including VirusTotal, ipvoid, liveipmap and AbuseIPDB.
        VirusTotal returned limited intelligence, with 2 of 91 security vendors identifying the IP as malicious.
        While this alone is insufficient to classify the IP as malicious, AbuseIPDB had a confidence score of 23% and contained 80 abuse reports associated with the address.

        Review of the AbuseIPDB reports indicated recurring malicious activity, including brute-force authentication attempts, web application attacks,
        and botnet-related activity associated with distributed denial-of-service (DDoS) campaigns. These historical reports are consistent with the brute-force 
        activity observed during this investigation. Based on the correlation between external threat intelligence and the authentication logs,
        the source IP is assessed as malicious and likely associated with ongoing malicious scanning and credential attack activity.


Whois Information 
  
    OrgName:        Cogent Communications, LLC
    OrgId:          COGC
    Address:        2450 N Street NW
    City:           Washington
    StateProv:      DC
    PostalCode:     20037
    Country:        US
    RegDate:        2000-05-30
    Updated:        2025-09-23


Asssesment

    The source IP address 149.88.25.133 is assessed as a malicious IOC. The assessment is supported by observed brute-force activity against the affected endpoint and corroborating AbuseIPDB reports of previous malicious activity. Limited VirusTotal detections were observed but were not relied upon as the sole basis for classification.


Endpoint Acivity Analysis

    User Logon Activity: 
        Analysis of the available endpoint authentication logs identified multiple failed SSH authentication attempts against the KRISTINE endpoint (172.16.17.129). The attempts targeted multiple accounts, including analyst, admin, and united, within a short time frame at approximately 04:32 hrs, indicating automated brute-force activity. Approximately one minute later, a successful SSH authentication was recorded for the analyst account from the external IP address 149.40.58.149. This confirms that the authentication attack resulted in successful access to the endpoint. Subsequent audit logs recorded privileged activity associated with the compromised session, confirming unauthorized post-authentication activity.


    Proccess Activity: 
        Analysis of endpoint process telemetry identified approximately 918 process creation events during the investigated timeframe. The high volume of process activity is consistent with routine Linux operating-system and service activity and, by itself, does not indicate malicious behaviour.

        Further examination of the available command-line telemetry identified 9 command executions within the relevant timeframe. These commands were reviewed and correlated with the authentication activity to determine whether they were associated with the suspected unauthorized session.

        The distinction between the large volume of routine process activity and the limited number of command-line executions was considered during the investigation to avoid incorrectly attributing normal system processes to malicious activity.

        Several of the identified commands were subsequently correlated with the compromised analyst session and were determined to be associated with post-authentication activity, including account creation and sudo privilege enumeration.


    Command line Activity : 
       Review of the command-line telemetry following the successful authentication identified a series of commands consistent with post-compromise activity rather than routine system operations.

        The observed activity included the discovery command whoami, followed by creation of a new local account, letsdefend1, using sudo useradd -m letsdefend1. The attacker subsequently enumerated the system's sudo configuration using sudo cat /etc/sudoers to identify users and groups with administrative privileges. The attacker then executed sudo visudo, indicating an attempt to access or modify the sudoers configuration.

        Subsequent activity included further sudo-related enumeration and execution of groups letsdefend1 to determine the group memberships assigned to the newly created account. The sequence of commands demonstrates post-compromise activity involving account discovery, privilege enumeration, account creation, and an apparent attempt to establish or maintain privileged access.

        The available logs do not independently confirm the specific changes made through visudo; therefore, the investigation does not conclusively establish that letsdefend1 was granted administrative privileges unless supported by additional audit evidence.

    Scheduled Tasks : 
        No scheduled tasks were created or modified during the investigated timeframe based on the available logs.

    
    Security Control Modification:
        No confirmed security-control modification was identified. Although sudo visudo was executed, the available logs do not confirm what, if any, changes were made to the sudoers configuration.

    
    Network Activity: 
      Analysis of the available network telemetry identified no evidence of outbound traffic associated with unauthorized data transmission or C2 communication during the investigated timeframe.


    Endpoint Assesment : 
        Analysis of the available logs and telemetry, including command-line and process activity, indicates that the brute-force attack against the KRISTINE endpoint was successful and resulted in unauthorized access to the system. Following authentication, the attacker created a new local account, letsdefend1, and performed account and sudo privilege enumeration. The observed activity is consistent with an attempt to establish persistence and maintain privileged access.

        Based on the observed post-authentication activity, the incident is assessed as a confirmed endpoint compromise. Immediate containment of the affected host is recommended to prevent further attacker activity, persistence, or potential lateral movement. Although no evidence of lateral movement or data exfiltration was identified in the available logs, the confirmed unauthorized access and account manipulation warrant further forensic analysis and continued monitoring before the endpoint is returned to normal operation.


Threat Intelligence : 

    Mitre mapping:

        | Tactic                                 | Technique                     | ID            | Evidence                                                                               |
        | -------------------------------------- | ----------------------------- | ------------- | -------------------------------------------------------------------------------------- |
        | **Credential Access**                  | Brute Force                   | **T1110**     | Multiple failed authentication attempts against several accounts within a short period |
        | **Initial Access**                     | Valid Accounts                | **T1078**     | Successful SSH authentication as `analyst`                                             |
        | **Discovery**                          | Account Discovery             | **T1087**     | `getent passwd` and examination of account information                                 |
        | **Discovery**                          | Permission Groups Discovery   | **T1069**     | `groups letsdefend1` used to identify the account's group memberships                  |
        | **Persistence**                        | Create Account: Local Account | **T1136.001** | `sudo useradd -m letsdefend1` created a new local account                              |
        | **Persistence / Privilege Escalation** | Account Manipulation          | **T1098**     | `sudo passwd letsdefend1` and attempted modification of sudo configuration             |



    Indicators of Compromise :
        | IOC Type                             | Indicator                                          |Description                                                                            |
        | ------------------------------------ | -------------------------------------------------- | -------------------------------------------------------------------------------------- |
        | **Source IP**                        | `149.88.25.133`                                    | External IP associated with the successful SSH authentication and brute-force activity |
        | **Compromised Host**                 | `KRISTINE`                                         | Endpoint targeted and subsequently accessed                                            |
        | **Destination IP**                   | `172.16.17.129`                                    | IP address of the affected endpoint                                                    |
        | **Compromised Account**              | `analyst`                                          | Account successfully authenticated by the attacker                                     |
        | **Unauthorized Account**             | `letsdefend1`                                      | Local account created following successful compromise                                  |
        | **SSH Activity**                     | `Accepted password for analyst from 149.88.25.133` | Successful authentication event confirming unauthorized access                         |
        | **Account Creation Command**         | `sudo useradd -m letsdefend1`                      | Command used to create the unauthorized local account                                  |
        | **Password Modification Command**    | `sudo passwd letsdefend1`                          | Command used to assign a password to the new account                                   |
        | **Privilege Configuration Activity** | `sudo visudo`                                      | Command indicating access to the sudoers configuration                                 |
        | **Account Enumeration**              | `getent passwd`                                    | Command used to enumerate system accounts                                              |
        | **Group Enumeration**                | `groups letsdefend1`                               | Command used to identify the new account's group memberships                           |






Incident Classification: True Positive 

    The incident is classified as a High-Severity True Positive based on the available evidence confirming that the brute-force attack against the KRISTINE endpoint was successful. The attacker gained unauthorized access through the analyst account and subsequently executed multiple privileged commands, including the creation of a new local account, letsdefend1, indicating an attempt to establish persistence and maintain access.

    The attacker also accessed and interacted with the system's sudo configuration. Although the available logs do not conclusively confirm that letsdefend1 was assigned administrative privileges, the observed activity indicates an attempt to obtain or maintain elevated access.

    Based on the confirmed post-authentication activity, the affected endpoint should be immediately contained from the organizational network to prevent further unauthorized activity or potential lateral movement. Further forensic investigation should be conducted to determine the full extent of the compromise and identify any additional affected accounts or assets.


RESPONSE: 
    Containment:
        The compromised endpoint KRISTINE should be isolated from the enterprise network to prevent further unauthorized activity and potential lateral movement.

        The compromised analyst account should be disabled and its credentials reset. The unauthorized letsdefend1 account should be disabled or removed pending further investigation. All administrative accounts and sudo privileges on the affected endpoint should be reviewed and validated to identify any unauthorized privilege modifications.
    
    Eradicaation: 
        The unauthorized letsdefend1 account should be removed from the compromised endpoint after forensic evidence has been preserved. The compromised analyst account credentials should be reset, and the sudo configuration should be reviewed and restored to its known-good state if unauthorized modifications are identified. All unauthorized accounts, privilege changes, and persistence mechanisms should be identified and removed. The endpoint should also be examined for any additional artifacts or modifications associated with the compromise.
            
    Recovery: 
        Following eradication, the integrity of the KRISTINE endpoint should be verified before reconnecting it to the enterprise network. Legitimate user accounts and required privileges should be validated, and the system should be monitored for renewed authentication attempts or other suspicious activity. The endpoint should only be returned to normal operation after confirming that unauthorized access and persistence mechanisms have been removed.



Lessons Learned
    Enable MFA across user accounts to reduce the risk of successful credential-based attacks.
    Restrict administrative privileges and ensure that only authorized administrators can modify user accounts and system security configurations.
    Monitor for unusual authentication patterns, particularly multiple failed login attempts against different accounts within a short period.
    Monitor unauthorized account creation and privilege changes to detect persistence following account compromise.
