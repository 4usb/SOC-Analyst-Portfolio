6.1 Validation Objectives
    Security validation was conducted to determine whether the completed laboratory architecture achieved its intended security and connectivity objectives.

    The validation focused on five distinct areas:

    Verification of the WireGuard VPN tunnel state.

    Verification of Layer 3 routing between the HQ and Branch networks.

    Verification of end-to-end host connectivity.

    Verification of Wazuh telemetry transport across the VPN.

    Examination of traffic visibility and payload confidentiality on the simulated WAN.

6.2 WireGuard Tunnel Validation
    The WireGuard tunnel was first validated to confirm that the HQ and Branch gateways had established bidirectional communication.

    The WireGuard status was examined on both gateways using:

        sudo wg show

    The output provided real-time telemetry regarding the logical interface and its peers. Key validation indicators included:

        Active interface configuration (wg0).

        Verified peer public key exchange.

        Recent cryptographic handshake timestamp.

        Non-zero Received (RX) and Transmitted (TX) byte counters.

    Handshake Verification
        A recent handshake (within the last few minutes/seconds) between the HQ and Branch peers confirmed that the two WireGuard gateways successfully completed an initial exchange across the simulated WAN (192.168.56.0/24).

        RX/TX Verification
        The WireGuard byte counters were monitored while generating test traffic between the connected sites. A steady increase in both transfer: [RX] and transfer: [TX] confirmed that encapsulated frames were traversing the virtual tunnel interface.

        Evidence: evidence/04-vpn-validation/wireguard-status.png

6.3 Network Routing Validation
    After confirming the tunnel was operational, the routing tables across all nodes were inspected to verify valid Layer 3 forwarding paths.

    Routing tables were checked using:

  
    # Ubuntu Gateways & Wazuh Manager
        ip route
        DOS
        :: Windows Endpoint
        route print
    The validation confirmed that traffic destined for remote subnets was correctly mapped to the appropriate gateway interfaces:

    HQ Subnet: 10.10.10.0/24 routed locally via enp0s8 on the HQ gateway.

    Branch Subnet: 10.20.20.0/24 routed out of wg0 toward the Branch gateway.

    Wazuh Manager: Maintained a dedicated static route directing 10.20.20.0/24 to the HQ gateway (10.10.10.1).

    
    [HQ Network: 10.10.10.0/24] <--- (wg0 Tunnel) ---> [Branch Network: 10.20.20.0/24]
    Validation Result
    The routing tables across all systems established an unambiguous, deterministic path between the simulated enterprise sites without routing loops or dropped subnets.


6.4 End-to-End Connectivity Validation
    End-to-end connectivity was evaluated using ICMP to ensure packets traveled beyond the boundary gateways into the internal host subnets.

    The principal test path originated from the Windows endpoint to the Wazuh Manager:


    Windows Endpoint (10.20.20.5)
            │
            ▼
    Branch Gateway (10.20.20.1)
            │
            ▼
    WireGuard Tunnel (wg0: 10.0.0.2 -> 10.0.0.1)
            │
            ▼
    HQ Gateway (10.10.10.1)
            │
            ▼
    Wazuh Manager (10.10.10.2)
    Successful ICMP echo requests and replies confirmed that:

    Kernel IPv4 forwarding was active on both gateways.

    Intermediate static routes handled encapsulated and decapsulated packets correctly.

    Inter-site communication was established without requiring Network Address Translation (NAT) between the internal networks.

6.5 Wazuh Telemetry Validation
    The next validation stage evaluated whether application-layer SOC operations functioned across the VPN link.

    The Windows endpoint hosted an active Wazuh Agent communicating with the centralized Wazuh Manager (10.10.10.2) located in the remote HQ network.

    Plaintext
    Windows Endpoint (Agent)
            │
            ▼
    Branch Gateway (wg0)
            │
            ▼
    HQ Gateway (wg0)
            │
            ▼
    Wazuh Manager (Dashboard: Port 443 / Telemetry: Port 1514)

    Agent Status
        Inspection of the Wazuh Dashboard confirmed that the Windows endpoint was registered, displayed an Active status, and maintained persistent keep-alive communication with the manager over TCP port 1514.

    Controlled Event Generation
        A controlled security event was initiated on the Windows endpoint to test the end-to-end detection pipeline:


    [Generate Controlled Event]
            │
            ▼
    [Wazuh Agent Scans Event Logs]
            │
            ▼
    [Encapsulated over WireGuard VPN]
            │
            ▼
    [Wazuh Manager Ingests & Decodes]
            │
            ▼
    [Alert Visualized in Wazuh Dashboard]
    The simulated event appeared in the Wazuh event feed within seconds, verifying uninterrupted telemetry ingestion across the site-to-site architecture.


6.6 VPN Telemetry Correlation
    To correlate security event ingestion directly with VPN transport, WireGuard interface counters were inspected before and after event generation:

    Bash
    sudo wg show
    The correlation sequence followed:

    
    Record Initial RX/TX Bytes
            │
            ▼
    Trigger Controlled Windows Event
            │
            ▼
    Agent Forwards Log Data
            │
            ▼
    Record Post-Event RX/TX Bytes (Counters Increment)
            │
            ▼
    Validate Alert Timestamp in Dashboard
    Interpretation
        The observed byte increases on the wg0 interface correlated with the transmission of Wazuh event logs. While aggregate byte counters alone cannot distinguish Wazuh telemetry from background network traffic, pairing this observation with passive packet captures provided concrete proof of traffic transport.

6.7 WAN Traffic Analysis
    Passive network traffic analysis was performed on the simulated WAN segment (192.168.56.0/24) using Wireshark to determine whether sensitive internal data was exposed outside the VPN tunnel.

    Plaintext
    [Sniffing Position: Host-Only "WAN" Adapter (192.168.56.x)]
    - Filter: udp.port == 51820
    - Visible: WireGuard Handshakes, Keepalives, Transport Data
    - Hidden: IP Headers of Endpoints, Windows Event Data, Cleartext Pings
    Observations
        Captures taken from an external vantage point revealed:

        Headers: Only the WAN IP addresses of the two gateways (192.168.56.105 and 192.168.56.106) were visible.

        Transport: All frames were wrapped within UDP datagrams destined for or originating from port 51820.

        Payload: The payload was labeled by Wireshark dissectors strictly as WireGuard Transport Data. Hex byte inspection showed pseudo-random ciphertext without readable strings, internal IP addresses (10.10.10.x / 10.20.20.x), or security log parameters.

    Security Interpretation
        This validation confirmed the practical efficacy of tunnel encapsulation. An attacker or passive sniffer positioned on the WAN transit path can observe traffic volume and peer endpoints, but cannot inspect internal host addressing or eavesdrop on sensitive SOC event telemetry.

6.8 Security Validation Summary
    Security / Network Objective  |	Validation Method                                       |Result
    ------------------------------|---------------------------------------------------------|--------------|
    WireGuard Peer Communication  | Handshake timestamp verification (wg show)              | Successful
    VPN Traffic Transmission      |	   RX/TX byte counter increments                        |	Confirmed
    HQ-to-Branch Routing          |Routing table review (ip route) and ICMP validation      |Successful
    Branch-to-HQ Routing	      |Routing table review (ip route) and ICMP validation	    |Successful
    Windows-to-HQ Reachability	  |End-to-end ICMP ping across gateways               	    |Successful
    Wazuh Agent Connectivity	  |Wazuh Dashboard agent status check	                    |Successful
    Security Telemetry Transport  |Controlled Windows event generation and ingestion     	|Successful
    VPN Telemetry Correlation	  |Pre- and post-event RX/TX byte comparison	            |Supporting Evidence
    WAN Traffic Visibility	      |Passive Wireshark capture on simulated WAN	            |WireGuard transport observed
    Payload Confidentiality       |	Deep packet payload inspection in Wireshark             |No readable plaintext detected



6.9 Validation Evidence Inventory
    WireGuard Metrics: Status outputs demonstrating active peers, completed handshakes, and TX/RX data counters.

    Routing Tables: Command-line outputs from ip route (HQ, Branch, Wazuh Manager) and route print (Windows).

    Connectivity Verification: ICMP response terminals demonstrating cross-subnet reachability.

    SOC Telemetry: Wazuh agent active status screenshots and indexed event logs in the Wazuh Dashboard.

    Packet Captures: Wireshark .pcap captures of WireGuard UDP/51820 encapsulated transport frames on the WAN.



6.10 Security Validation Conclusion
    The validation phase confirms that the laboratory deployment achieved all primary networking and security objectives.

    The site-to-site WireGuard VPN created an authenticated, encrypted channel between the HQ and Branch environments. Dynamic routing and kernel forwarding enabled seamless communication across disparate subnets without compromising network isolation. Centralized security monitoring was preserved across the WAN barrier, allowing the Wazuh Manager to ingest endpoint telemetry reliably. Finally, deep packet inspection on the WAN link verified that all internal communications remained opaque to outside observers, meeting the core design criteria for secure distributed SOC operations.