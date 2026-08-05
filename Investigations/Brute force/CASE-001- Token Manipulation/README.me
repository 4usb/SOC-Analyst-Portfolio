
CASE INFORMATION

    CASE ID: Brute 001
    EVENT TYPE :  Brute Force Attempt 
    SEVERITY: HIGH 
    DATE : 26-07-2026
    ANALYST : Edom Chinedu Williams 
    PLATFORM : LetsDefend SIEM
    STATUS : Resolved


Executive Summary 

    This report documents the investigation of a suspected Windows endpoint compromise following a brute-force authentication
    attack against the host **EC2AMAZ-ILGVOIN**. The investigation identified repeated failed login attempts from a foreign IP address,
    followed by PowerShell execution and a sequence of activities consistent with post-compromise attacker behaviour.

    Endpoint telemetry revealed evidence of system and privilege discovery, attempts to evade host security controls by modifying
    Microsoft Defender and Windows Firewall settings,and the creation of a scheduled task to establish persistence. An EDR alert 
    titled **"Possible Token Manipulation Detected"** further indicated behaviour consistent with an attempted privilege escalation 
    through access token manipulation.

    Based on the available evidence, the incident is assessed as a high-confidence endpoint compromise involving the stages of execution,
    discovery,defence evasion, attempted privilege escalation, and persistence. While the available telemetry does not conclusively confirm
    successful privilege escalation,the observed activity warranted immediate containment and further forensic investigation.


| Investigation Outcome | Result                 |
| --------------------- | ---------------------- |
| Incident Type         | Brute Force Attack     |
| Alert Status          | True Positive          |
| Endpoint Compromised  | Yes                    |
| Initial Access        | Successful Brute Force |
| Privilege Escalation  | Attempted              |
| Persistence           | Confirmed              |
| Lateral Movement      | Not Observed           |
| Data Exfiltration     | Not Observed           |
| Overall Severity      | High                   |


Incident Details 
    
    ALERT NAME : Possible Token Manipulation Detected
    EVENT ID : 219
    Alert Time : 12:19 PM
    Hostname : LARK
    Operating System : Windows 10
    Source ip : 149.40.58.149
    Destination ip: 172.16.17.196


Alert  Analysis 

    | Field            | Value                                 |
    | ---------------- | ------------------------------------- |
    | Alert Name       | Possible Token Manipulation Detected  |
    | Detection Source | Endpoint security (LetsDefend)        |
    | Severity         | High                                  |
    | Detection Time   | 12:19 PM                              |
    | Affected Host    | LARK                                  |



Authentication Log Analysis

    Following analysis of the available authentication logs, it was determined that the internal endpoint **LARK** (IP address **172.16.17.196**) 
    received multiple authentication attempts from the external IP address **149.40.58.149**. Between **12:14 PM and 12:19 PM on 17 January 2024**,
    the source IP generated **12 consecutive authentication attempts**, resulting in multiple **Windows Security Event ID 4625** records indicating failed logon
    attempts against a valid user account.

    Subsequently, a **Windows Security Event ID 4624** was recorded, indicating a successful logon to the endpoint. Correlation of the repeated failed
    authentication attempts with the subsequent successful logon and the observed post-authentication endpoint activity indicates that the attacker
    successfully compromised the endpoint through a brute-force attack.



Source IP Analysis

    Overall Reputation Assessment :(malicious)  
    
        The source IP address (149.40.58.149) was assessed using multiple threat intelligence platforms, including VirusTotal, ANY.RUN, and AbuseIPDB.
        VirusTotal returned limited intelligence, with 1 of 91 security vendors identifying the IP as malicious.
        While this alone is insufficient to classify the IP as malicious, AbuseIPDB contained 72 abuse reports associated with the address.

        Review of the AbuseIPDB reports indicated recurring malicious activity, including brute-force authentication attempts, web application attacks,
        and botnet-related activity associated with distributed denial-of-service (DDoS) campaigns. These historical reports are consistent with the brute-force 
        activity observed during this investigation. Based on the correlation between external threat intelligence and the authentication logs,
        the source IP is assessed as malicious and likely associated with ongoing malicious scanning and credential attack activity.
        
    
    WhoIs Information:
        Organization:   Cogent Communications, LLC (COGC)
        OrgId:          COGC
        State:          Texas
        City:           Houston
        Country:        United States
        RegDate:        2000-05-30
        Updated:        2025-09-23
        OrgTechPhone:  +1-877-875-4311 
        OrgTechEmail:  ipalloc@cogentco.com

    Assessment

        The source IP address 149.40.58.149 is assessed as a malicious external host based on its observed brute-force activity against the affected endpoint,
        corroborated by historical abuse reports documenting similar malicious behaviour. Although VirusTotal returned limited detections,
        the combination of authentication log evidence and AbuseIPDB intelligence provides sufficient evidence to classify the IP as a malicious indicator of compromise (IOC).


EndPoint Activity Analysis

    User Logon Activity : 
        Analysis of the authentication logs revealed that the external IP address (149.40.58.149) performed 12 consecutive failed authentication attempts
        against the endpoint LARK (IP address 172.16.17.196). Each failed attempt generated a Windows Security Event ID 4625, indicating that authentication
        failed due to an incorrect password while targeting a valid user account.

        Subsequently, a Windows Security Event ID 4624 was recorded at 12:15 PM on 17 January 2024, indicating a successful logon to the endpoint.
        When correlated with the preceding sequence of failed authentication attempts from the same external IP address, the authentication events 
        indicate that the attacker successfully authenticated to the endpoint through a brute-force attack, establishing the initial access that preceded 
        the observed post-compromise activity.


    Proccess Creation: 
      
        Endpoint process telemetry was reviewed to determine the execution sequence following the successful authentication. Analysis showed that a PowerShell process
        (`powershell.exe`) was launched from an interactive user session initiated by `explorer.exe`, indicating that the activity was executed under the compromised user account.
        While the parent-child relationship between `explorer.exe` and `powershell.exe` is common in legitimate Windows environments, the commands executed within the
        PowerShell session were consistent with malicious post-compromise activity.

        Subsequent process activity revealed execution of multiple PowerShell and command-line operations associated with system discovery, privilege enumeration, defense evasion, 
        and persistence. Observed commands included `whoami`, `whoami /groups`, and `whoami /priv`, which were used to enumerate the current user, security group memberships, and assigned privileges.
        Additional PowerShell commands modified the endpoint's security configuration by bypassing PowerShell execution restrictions and disabling Microsoft Defender Antivirus real-time monitoring,
        script scanning, and Windows Firewall protections.

        Further analysis identified the execution of a PowerShell script (`Script.ps1`) and the creation of a scheduled task (`CheckUserLogTask`) configured to execute the script automatically at system startup. 
        Endpoint telemetry also recorded execution of `Get-System -Technique Token -Verbose`, followed by an EDR alert indicating **Possible Token Manipulation Detected**,
        which is consistent with an attempted privilege escalation through access token manipulation.

        Overall, the observed process activity demonstrates a structured sequence of post-compromise actions designed to enumerate the endpoint, weaken host security controls,
        attempt privilege escalation, and establish persistence. These activities, when correlated with the preceding brute-force authentication attack, provide strong evidence
        of a coordinated endpoint compromise.


    Commmand Line Activity:
    
        Analysis of the command-line history revealed extensive use of **Windows PowerShell (`powershell.exe`)** following the successful authentication. Multiple PowerShell sessions were observed executing 
        a sequence of commands consistent with post-compromise activity rather than routine administrative operations.

        The observed PowerShell activity included modification of the PowerShell execution policy to bypass script execution restrictions, execution of system and privilege discovery commands
        (`whoami`, `whoami /groups`, and `whoami /priv`), attempted privilege escalation using `Get-System -Technique Token -Verbose`, modification of Microsoft Defender Antivirus and Windows Firewall settings 
        to weaken endpoint security, creation of a PowerShell script (`Script.ps1`), and registration of a scheduled task (`CheckUserLogTask`) configured to execute the script at system startup.

        The sequence and nature of these commands demonstrate a structured post-compromise workflow in which PowerShell was used as the primary execution tool to enumerate the environment, 
        evade security controls, attempt privilege escalation, and establish persistence on the compromised endpoint.


    Security Control Modification:

        Analysis of the executed PowerShell commands revealed that the attacker attempted to weaken the endpoint's security controls by executing the `Set-MpPreference -DisableRealtimeMonitoring $true` command.
        Additional PowerShell commands were used to disable Microsoft Defender Antivirus script scanning and turn off Windows Firewall protections across all profiles. 
        These actions are consistent with defense evasion techniques intended to reduce the effectiveness of host-based security controls and
        allow subsequent malicious activity to proceed with a lower likelihood of detection or prevention.


    Scheduled Task Activity:
        Analysis of the endpoint telemetry identified the creation of a scheduled task named CheckUserLogTask, corresponding to a Sysmon Event ID 11 (File Created) within the Windows Task Scheduler directory
        (C:\Windows\System32\Tasks\CheckUserLogTask). Further investigation of the associated PowerShell commands revealed that the attacker generated a PowerShell script (Script.ps1) and configured the 
        scheduled task to execute the script automatically at system startup using Register-ScheduledTask.

        The scheduled task was configured with a startup trigger (New-ScheduledTaskTrigger -AtStartup), ensuring that the PowerShell script would execute each time the operating system booted.
        This behaviour is consistent with the establishment of persistence, allowing the attacker to maintain continued execution of the malicious script across system reboots.


    Network Activity :

        Review of the endpoint's command-line activity and network-related events identified outbound connectivity to the external IP address 216.218.206.80 through the PowerShell Test-Connection cmdlet.
        Analysis of the associated PowerShell script showed that the connectivity check was performed when no recent Windows Security log activity was detected for the targeted user account.
        Although the script used the external IP as a network connectivity check, the available telemetry did not provide sufficient evidence to determine whether the address functioned
        as a command-and-control (C2) server or served another operational purpose.

        Additional network activity observed during the investigation was limited, and no evidence of sustained command-and-control communications, lateral movement,
        or data exfiltration was identified from the available endpoint telemetry. Consequently, the observed network activity is assessed as consistent with connectivity verification
        rather than confirmed malicious communications.

    Endpoint Assessment:

        Analysis of the authentication logs, endpoint telemetry, process activity, command-line history, and system events indicates that the brute-force attack against the endpoint **LARK** was successful, 
        resulting in unauthorized access to the system. Subsequent endpoint activity, including PowerShell execution, system and privilege enumeration, security control modifications,
         attempted privilege escalation, and the establishment of persistence through a scheduled task, confirms that the endpoint was compromised.

        Based on the observed post-authentication activity, the incident is assessed as a confirmed endpoint compromise. Immediate containment of the affected host is recommended to prevent further attacker activity,
        persistence, or potential lateral movement within the environment. Although no evidence of lateral movement or data exfiltration was identified during the investigation, 
        the extent of the compromise warrants further forensic analysis and continuous monitoring before the endpoint is returned to normal operation.


Threat Intelligence:

    MITRE ATT&CK Mapping : 

        | Tactic               | Technique                             | Technique ID | Evidence                                                                                                                                  |
        | -------------------- | ------------------------------------- | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
        | Initial Access       | Brute Force                           | T1110        | Multiple failed authentication attempts (Event ID 4625) followed by a successful logon (Event ID 4624) from the same external IP address. |
        | Execution            | PowerShell                            | T1059.001    | Extensive use of PowerShell to execute commands and scripts following successful authentication.                                          |
        | Discovery            | System Owner/User Discovery           | T1033        | Execution of the `whoami` command to identify the current user.                                                                           |
        | Discovery            | Permission Groups Discovery           | T1069        | Execution of `whoami /groups` to enumerate group memberships.                                                                             |
        | Discovery            | System Information Discovery          | T1082        | Execution of discovery commands to identify system information and privileges.                                                            |
        | Privilege Escalation | Access Token Manipulation (Attempted) | T1134        | Execution of `Get-System -Technique Token -Verbose` and the corresponding EDR alert **Possible Token Manipulation Detected**.             |
        | Defense Evasion      | Impair Defenses                       | T1562.001    | Microsoft Defender Antivirus real-time monitoring and script scanning were disabled using PowerShell.                                     |
        | Defense Evasion      | Impair Defenses                       | T1562.004    | Windows Firewall protections were disabled using `netsh advfirewall`.                                                                     |
        | Persistence          | Scheduled Task/Job: Scheduled Task    | T1053.005    | Creation of the scheduled task **CheckUserLogTask** configured to execute `Script.ps1` at system startup.                                 |

    Indicators of Compromise :
        
        | Indicator                                   | Type                | Description                                                                                  | Assessment                                 |
        | ------------------------------------------- | ------------------- | -------------------------------------------------------------------------------------------- | ------------------------------------------ |
        | 149.40.58.149                               | External IP Address | Source IP responsible for repeated brute-force authentication attempts against the endpoint. | Malicious                                  |
        | 172.16.17.196                               | Internal IP Address | IP address of the compromised endpoint (LARK).                                               | Affected Asset                             |
        | LARK                                        | Hostname            | Compromised Windows endpoint targeted during the attack.                                     | Affected Asset                             |
        | Script.ps1                                  | PowerShell Script   | Script created on the endpoint and configured for execution through a scheduled task.        | Malicious                                  |
        | CheckUserLogTask                            | Scheduled Task      | Scheduled task created to execute `Script.ps1` at system startup, establishing persistence.  | Malicious                                  |
        | powershell.exe                              | Process             | Used extensively to execute post-compromise commands and scripts.                            | Suspicious in Context                      |
        | cmd.exe                                     | Process             | Executed during post-compromise command-line activity.                                       | Suspicious in Context                      |
        | Get-System -Technique Token -Verbose        | PowerShell Command  | Attempted token-based privilege escalation.                                                  | Malicious                                  |
        | Set-ExecutionPolicy Bypass                  | PowerShell Command  | Modified PowerShell execution policy to permit unrestricted script execution.                | Malicious                                  |
        | Set-MpPreference -DisableRealtimeMonitoring | PowerShell Command  | Disabled Microsoft Defender real-time protection.                                            | Malicious                                  |
        | Set-MpPreference -DisableScriptScanning     | PowerShell Command  | Disabled Microsoft Defender script scanning.                                                 | Malicious                                  |
        | netsh advfirewall set allprofiles state off | Command             | Disabled Windows Firewall protection across all profiles.                                    | Malicious                                  |
        | whoami                                      | Command             | Enumerated the current user account.                                                         | Suspicious in Context                      |
        | whoami /groups                              | Command             | Enumerated security group memberships.                                                       | Suspicious in Context                      |
        | whoami /priv                                | Command             | Enumerated assigned user privileges.                                                         | Suspicious in Context                      |
        | 216.218.206.80                              | External IP Address | External host contacted by the PowerShell script using `Test-Connection`.                    | Suspicious (Further Verification Required) |



    Historical Malicious Activity:

        Threat intelligence analysis of the source IP address **149.40.58.149** identified a history of malicious activity documented by multiple community intelligence sources. Although VirusTotal provided limited detections,
        AbuseIPDB contained numerous historical abuse reports associated with the IP address.

        Analysis of the available intelligence revealed that the IP address has previously been reported for activities including brute-force authentication attacks, web application attacks, botnet-related activity,
        and Distributed Denial-of-Service (DDoS) campaigns. The observed brute-force behaviour during this investigation is consistent with the historical activity associated with the IP address,
        strengthening the assessment that the source host is malicious.

        While the available intelligence supports a high-confidence assessment of malicious activity, it does not provide sufficient evidence to attribute the observed behaviour to a specific threat actor or organised threat group.
        The IP address should therefore be treated as a malicious indicator of compromise and considered for blocking, monitoring, and inclusion in threat intelligence watchlists.


Incident Classification :
    TRUE POSITIVE 
        The incident is classified as a High Severity, True Positive endpoint compromise initiated through a successful brute-force authentication attack. Following unauthorized access,
        the threat actor executed multiple PowerShell commands to perform system discovery, enumerate privileges, weaken endpoint security controls, attempt privilege escalation, 
        and establish persistence through a scheduled task. Although no evidence of lateral movement or data exfiltration was identified during the investigation, the confirmed compromise
         of the endpoint and the observed post-compromise activity warranted immediate containment and further forensic investigation.


RESPONSE:
 ## Response

     Containment

        The affected endpoint (**LARK**) should be immediately isolated from the network to prevent further malicious activity and limit the risk of lateral movement. 
        The compromised user account should be disabled or have its credentials reset, and all active sessions should be terminated.
        Any additional endpoints exhibiting similar indicators of compromise should also be identified and isolated pending investigation.

    Eradication

        A comprehensive forensic examination should be performed to identify and remove all malicious artifacts, including unauthorized PowerShell scripts, scheduled tasks,
        and any other persistence mechanisms established by the attacker. Microsoft Defender Antivirus and Windows Firewall protections should be restored to their secure configurations,
        and all compromised credentials should be reset. The environment should also be scanned to verify that no additional compromised hosts or malicious processes remain.

    Recovery

        Following eradication, the affected endpoint should only be returned to production after confirming that it is free of malicious activity.
        Security patches and system updates should be applied where necessary, and enhanced monitoring should be implemented to detect any signs of recurring attacker activity.
        A review should also be conducted to determine whether any sensitive data or systems were compromised during the incident,
        and appropriate protective measures should be implemented to mitigate any identified risks.


lessons Learned 
    
    Enforce strong password policies and account lockout thresholds.
    Enable MFA for privileged and remote access accounts.
    Monitor Event IDs 4624 and 4625 for brute-force patterns.
    Audit PowerShell activity using Script Block Logging.
    Investigate attempts to disable security controls immediately.
    Monitor scheduled task creation as a persistence indicator.
    Enrich investigations using threat intelligence.