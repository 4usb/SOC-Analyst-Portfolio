CASE INFORMATION

    CASE ID: PHISH 002
    EVENT TYPE : Phishing URL Detection
    SEVERITY: HIGH 
    DATE : 24-07-2026
    ANALYST : Edom Chinedu Williams 
    PLATFORM : LetsDefend SIEM
    STATUS : Resolved


Executive Summary:
   
    A Security Information and Event Management (SIEM) alert identified suspected access to a known phishing URL originating from the endpoint assigned to user **ellie**. The alert was generated under the **"Phishing URL Detected"** rule and contained the source IP address, endpoint hostname, username, and the identified malicious URL.

    The investigation confirmed that the URL was malicious through threat intelligence analysis. However, attempts to corroborate the SIEM alert using endpoint browser history and network activity logs for the alert timestamp were unsuccessful, as no corresponding records were identified. Historical endpoint artifacts indicated previous access to the same URL during an earlier period, creating a discrepancy between the SIEM telemetry and endpoint-resident evidence.

    No indicators of successful compromise were observed during the investigation. There was no evidence of malware execution, persistence, command-and-control communication, credential theft, or data exfiltration. Based on the available evidence, the incident was classified as a **True Positive** detection of a malicious phishing URL, while noting that the endpoint artifacts could not independently verify the web activity recorded by the SIEM.


Incident details:

    ALERT NAME : Phishing Url Dectected 
    EVENT ID : 86
    Alert Time : 09:23 PM
    Hostname :  EmilyComp
    Operating System : Windows 10
    Source ip : 172.16.17.49
    Destination ip: 91.189.114.8


URL Anlysis :

    Full URL : http[:]//mogagrocol.ru/wp-content/plugins/akismet/fv/index[.]php?email=ellie@letsdefend[.]io
    Protocol : HTTP
    Domain : mogagrocol.ru
    Path : wp-content/plugins/akismet/fv/index[.]php
    Query Parameters : ?email=ellie@letsdefend[.]io
    HTTP Method : GET
    User-Agent :  Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.88 Safari/537.36
    Timestamp : 9:23

URL Reputation:

    VirusTotal : Malicious [13/92 vendors flagged the url as malicious ]
    URLhaus : No available data
    Hybrid Analysis : Malicious [Hybrid analysis and previous analysis show phishing activity ]
    Google Safe Browsing :No unsafe data found 

    Overall assessment : Threat intelligence enrichment indicates that the investigated URL is associated with phishing activity. VirusTotal classified the URL as malicious, while ANY.RUN behavioral analysis identified characteristics consistent with a phishing campaign. Although several additional OSINT platforms either had no available intelligence or did not classify the URL as malicious, this does not negate the findings from the corroborating sources.

    The URL also exhibits technical characteristics commonly associated with phishing attacks, including the use of an unsecured HTTP connection, an embedded email query parameter intended to personalize the phishing page, and a resource path suggestive of content hosted within a compromised WordPress installation. These observations, when considered alongside the SIEM alert identifying the URL as a phishing indicator, provide sufficient evidence to assess the URL as malicious.

    Based on the available evidence, the URL is assessed as a malicious phishing resource. However, no evidence was identified during the endpoint investigation to indicate successful malware execution, persistence, command-and-control communication, or data exfiltration following the detected activity.

    Reputation Verdict : Malicious 

Domain Analysis
    WHOIS information:
        Domain age : 6 years
        IP Address : 195.24.68.4
        Status : REGISTERED, DELEGATED, VERIFIED
        Registrar : RU-CENTER-RU
        Country: Russia 
        Hosting provider : JSC "RU-CENTER"


Endpoint investigation:
    User Activity :  An examination of the endpoint's browser history and available network activity logs did not identify evidence confirming that the user accessed the identified phishing URL at the time of the SIEM alert. Although the SIEM recorded a connection associated with the endpoint, the absence of corroborating endpoint artifacts prevented independent verification of user interaction with the URL.

    Browser Activity : Examination of the browser history did not identify any records confirming access to the reported phishing URL during the alert timeframe.

    Network Activity : Analysis of endpoint network logs did not identify suspicious outbound connections or communications associated with the reported phishing URL beyond the SIEM detection.

    PowerShell Activity : No PowerShell execution logs or evidence of suspicious PowerShell activity were identified during the investigation

    Process Execution : No suspicious or unauthorized processes were observed that could be associated with phishing-related malware execution.

    Persistence Mechanisms : The investigation did not identify evidence of persistence mechanisms such as scheduled tasks, registry Run key modifications, startup folder entries, or malicious services.

    Command and Control (C2) : No evidence of command-and-control communications was identified during the review of available endpoint and network telemetry

    Data Exfiltration : No evidence of data exfiltration or unauthorized outbound transfer of information was identified.


Threat Intelligence :

    Sandbox Behaviour: Dynamic analysis conducted in a controlled sandbox environment (Hybrid Analysis/ANY.RUN) revealed that the submitted URL established outbound communications with multiple external domains and exhibited behaviors consistent with phishing-related activity. The sandbox also identified techniques mapped to the MITRE ATT&CK framework, indicating actions commonly associated with credential harvesting and web-based phishing attacks. However, subsequent endpoint investigation found no evidence that the affected user's workstation accessed the URL or communicated with the identified domains. No DNS queries, HTTP/HTTPS requests, browser artifacts, command-and-control traffic, or other related malicious activity were observed on the endpoint. Therefore, the network activity documented in the sandbox represents the URL's potential behavior under controlled execution and was not replicated on the user's system.


    MITRE ATT&CK Mapping : 
    | ATT&CK ID | Technique                                | Observed Behaviour                                                              |
    | --------- | ---------------------------------------- | ------------------------------------------------------------------------------- |
    | T1071     | Application Layer Protocol               | Network communication over HTTP/HTTPS.                                          |
    | T1057     | Process Discovery                        | Enumerated running processes.                                                   |
    | T1082     | System Information Discovery             | Gathered host and operating system information.                                 |
    | T1083     | File and Directory Discovery             | Enumerated files and directories.                                               |
    | T1132.001 | Data Encoding: Standard Encoding         | Encoded data before transmission.                                               |
    | T1048.003 | Exfiltration Over Alternative Protocol   | Transmitted data using a non-command-and-control protocol.                      |


    Contacted Domains (Sandbox Analysis) :

    | Domain                              | Classification     | Observation            
    |_____________________________________|____________________|________________________________________________________________________________
    | aadcdn.msftauth.net                 | Legitimate         | Microsoft authentication CDN used by Microsoft   services.                                                                                     |
    | login.microsoftonline.com           | Legitimate         | Microsoft Entra ID (Azure AD) authentication service.                                                                                        |
    | ajax.googleapis.com                 | Legitimate         | Google-hosted JavaScript library CDN.                                                                                                        |
    | maxcdn.bootstrapcdn.com             | Legitimate         | Bootstrap CDN used to load CSS/JavaScript resources.                                                                                         |
    | aphonic-partition.000webhostapp.com | Suspicious         | Hosted on the free hosting platform 000webhost. Often abused for phishing pages. Requires further investigation.                             |
    | lead-repeated-prawn.glitch.me       | Suspicious         | Hosted on Glitch, a platform that can be abused for phishing and temporary malicious infrastructure.                                         |
    | mogagrocol.ru                       | High Risk          | Russian domain that should be investigated further. If VirusTotal or URLhaus flags it as malicious, classify it as malicious infrastructure. |
    | o.ss2.us                            | Unknown/Suspicious | Requires additional reputation analysis before classification.                                                                               |


    Contacted Hosts (SandBox Analysis) : 

    | IP Address    | Port | Protocol | Process                 | Associated Domain             | Country       | Assessment            
    |_______________|______|__________|_________________________|_______________________________|_______________|________________________________
    | 91.189.114.8  | 80   | TCP      | iexplore.exe (PID 3764) | mogagrocol.ru                 | Russia        | Suspicious. Associated with the identified domain and warrants further investigation.         |
    | 52.3.182.213  | 443  | TCP      | iexplore.exe (PID 3764) | Not resolved in provided data | United States | Unknown. HTTPS connection observed during sandbox execution.                                  |
    | 99.84.170.67  | 80   | TCP      | iexplore.exe (PID 3764) | o.ss2.us                      | United States | Requires additional reputation analysis. Likely served through AWS CloudFront infrastructure. |
    | 13.249.90.138 | 80   | TCP      | iexplore.exe (PID 3764) | Not resolved in provided data | United States | Likely Amazon CloudFront infrastructure used for content delivery.                            |
    | 13.249.90.10  | 80   | TCP      | iexplore.exe (PID 3764) | Not resolved in provided data | United States | Likely Amazon CloudFront infrastructure used for content delivery.                            |
    | 99.84.254.70  | 80   | TCP      | iexplore.exe (PID 3764) | Not resolved in provided data | United States | Likely Amazon CloudFront infrastructure used for content delivery.                            |

Incident Classification :

  TRUE POSITIVE 

    The incident was classified as a True Positive because the SIEM correctly identified a URL that was independently verified as malicious through threat intelligence analysis, including VirusTotal and ANY.RUN. Although the endpoint investigation did not identify evidence of successful user interaction, malware execution, persistence, command-and-control communication, or data exfiltration, the alert accurately detected a genuine phishing indicator. The investigation therefore concluded that the malicious URL posed a legitimate security risk, while finding no evidence that the endpoint was successfully compromised.


Response : 

    Containment : Blocked the malicious URL, associated domain, and IP addresses at the network perimeter

    Eradication : No eradication activities were required because the investigation found no evidence of malware execution, persistence mechanisms, or unauthorized system modifications on the endpoint.

    Response :  No recovery actions were required because the endpoint remained uncompromised. Normal operations continued after confirming the absence of malicious activity.


Lessons Learned : 

    Correlate SIEM alerts with endpoint and network telemetry to validate detected activity.
    Confirm malicious indicators using multiple threat intelligence sources rather than relying on a single platform.
    Recognize that a malicious URL does not necessarily indicate successful endpoint compromise.
    Continue monitoring endpoints after phishing detections, even when no evidence of compromise is identified.
    Strengthen preventive controls by blocking confirmed malicious URLs and reinforcing phishing awareness.
