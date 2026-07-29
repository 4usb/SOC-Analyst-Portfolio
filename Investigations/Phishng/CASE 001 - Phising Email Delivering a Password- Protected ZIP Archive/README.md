CASE INFORMATION 

    CASE ID: PHISH 001
    EVENT TYPE: Phishing Email Containing a Malicious Download Link
    SEVERITY: HIGH 
    DATE: 2026-08-20
    ANALYST:  Edom Chinedu Willimas 
    PLATFORM: LetsDefend SEIM
    STATUS: Resolved 

Executive Summary

    A high-severity phishing alert was generated after an email containing a malicious download link that delivered a password-protected ZIP archive was detected within the organization's environment. Analysis of the email, embedded URL, attachment, endpoint telemetry, and network activity confirmed that the email was malicious. However, no evidence indicated that the recipient opened the attachment, accessed the embedded URL, or executed malicious content. As a result, no endpoint compromise, command-and-control communication, persistence mechanisms, or data exfiltration were observed. The malicious email was quarantined, and the associated indicators were blocked to mitigate future exposure.



Incident details

    ALERT NAME: Phising Mail Detected 
    EVENT ID : 93
    Alert Time : 02:13 PM
    Hostname: LarsPRD
    Operating System : WIndows 10
    Source ip: 50.6.153.142
    Destinatination ip : 172.16.17.57

Email Analysis

    SPF: Not verifiable (original email headers unavailable)
    DKIM: Not verifiable (no DKIM record confirmed during investigation)
    DMARC: Not verifiable (no DMARC record identified during investigation)
    RETURN PATH: Not available (original email headers unavailable)
    SMTP IP: 50.6.153.142
    SENDER: trenton@tritowncomputers.com
    RECIVER: lars@letsdefend.io

DOWNLOAD URL ANALYSIS 

    DOMAIN REPUTATION : Threat intelligence platforms flagged the URL contained in the email as malicious, indicating a poor reputation and prior association with malicious activity.
    
    REDIRECTS : No HTTP redirects were observed

    VIRUS TOTAL : Virus Total classified submitted URL malicious based on          
    detections from multiple security vendors, indicating that it has been   
    associated with malicious activity.

    WHOIS: The domain was registered in 2024. WHOIS records do not indicate any unusual registration characteristics.

ATTACHMENT ANALYSIS 

    FILE NAME : 11f44531fb088d31307d87b01e8eabff
    
    FILE TYPE: Password-protected document 

    Password: Provided within body

    SHA256: 38b01a12b8dcd39ebdcf9e97772e848237330eb227e1ccee80125564b27377e5

    VirusTotal:
    The password-protected ZIP archive was detected as malicious by multiple security vendors.

    Remarks:
    The archive was not extracted locally to avoid executing potentially malicious content outside an isolated analysis environment.



ENDPOINT INVESTIATION 

    USER ACTIVITY: No evidence of user interaction with the download URL was identified from the available endpoint and network logs.

    BROWSER HISTIRY :Available endpoint and network logs do not indicate that the endpoint connected to any IP address associated with the malicious URL.

    POWERSHELL: No PowerShell activity associated with the suspected malicious file or URL was identified.

    RUNNING PROCESSES:No suspicious or unauthorized processes related to the suspected malicious file were observed.

    REGISTRY :No registry modifications associated with the suspected malicious file or its execution were identified.

    SCHEDULLED TASKS :No scheduled tasks associated with the suspected malicious file or any persistence mechanism were identified.


NETWORK INVESTIGATION

    DNS QUERIES :No DNS queries associated with the malicious URL or its associated domains were identified, indicating that the endpoint did not attempt to resolve or access the URL

    HTTP REQUESTS: No HTTP requests to the malicious url were identified during the investigation. 

    HTTPS REQUESTS: No HTTPS requests to the malicious URL were identifies during the investigation.

    Data exfiltration : No evidence of unauthorized data transfer was identified.

    C2 : No command-and-control (C2) communication was identified. Network telemetry did not reveal any outbound connections to known malicious infrastructure.

INCIDENT CLASSIFICATION : 

True Positive : 
   
    Endpoint and network telemetry found no evidence that the recipient downloaded, opened, or executed the attachment. No DNS queries, HTTP/HTTPS requests, PowerShell activity, suspicious processes, persistence mechanisms, command-and-control communication, or data exfiltration were observed.
    The incident is therefore classified as a "True Positive phishing attempt with no successful endpoint compromise".


RESPONSE

    CONTAINMENT: Blocked the malicious URL, associated domain, and IP addresses at the network perimeter 

    Quarantined the phishing email from all affected mailboxes

    ERADICATION: No eradication actions were required because na evidence off compromised was identified 

    RECOVERY:No recovery actions were required because the investigation found no evidence that the endpoint had been compromised. Normal business operations continued without interruption.

    
LESSONS LEARNED

    Prompt reporting and investigation of suspicious emails can prevent successful phishing attacks.

    Correlating email, network, and endpoint telemetry provides confidence when determining whether user interaction occurred.

    Threat intelligence platforms such as VirusTotal and Hybrid Analysis are valuable for assessing the reputation of URLs and domains but should be used alongside network and endpoint evidence.

    Blocking known malicious domains and URLs at the email and network layers reduces the likelihood of future exposure.

    Regular phishing awareness training remains essential to help users recognize and report suspicious emails.



