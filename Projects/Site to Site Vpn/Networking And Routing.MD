04 — Routing and Connectivity

    4.1 Overview

        Following the deployment of the WireGuard VPN, routing and connectivity were validated to confirm that the HQ and Branch networks could communicate securely through the encrypted tunnel.

        The validation focused on:

        * Verification of the routing tables across all critical infrastructure nodes.
        * Connectivity between the WireGuard gateways.
        * Connectivity between the HQ and Branch local area networks.
        * Connectivity between the Branch Windows endpoint and HQ resources using ICMP.
        * Application-layer validation to ensure the Wazuh Agent successfully established a TCP connection with the Wazuh Manager across the VPN.

    4.2 HQ Routing

        The HQ Ubuntu server acts as the gateway for the HQ network. Its internal network interface is assigned the address `10.10.10.1`, serving the `10.10.10.0/24` subnet.

        The HQ gateway maintains routes for its local network, the internal WireGuard tunnel (`10.0.0.0/24`), and the remote Branch network via the WireGuard peer. The routing table was verified using `ip route`, confirming that traffic destined for the Branch network is correctly routed out of the `wg0` interface:


            vboxuser@hq-gateway:~$ ip route
            10.0.0.0/24 dev wg0 proto kernel scope link src 10.0.0.1
            10.10.10.0/24 dev enp0s8 proto kernel scope link src 10.10.10.1
            10.20.20.0/24 dev wg0 scope link


    4.3 Branch Routing

        The Branch Ubuntu server acts as the gateway for the Branch network. Its internal network interface is assigned the address `10.20.20.1`, serving the `10.20.20.0/24` subnet.

        Traffic destined for the HQ network (`10.10.10.0/24`) must be routed through the WireGuard tunnel toward the HQ gateway. The routing table was validated to ensure the proper paths were established:

            vboxuser@branch-gateway:~$ ip route
            10.0.0.0/24 dev wg0 proto kernel scope link src 10.0.0.2
            10.10.10.0/24 dev wg0 scope link
            10.20.20.0/24 dev enp0s3 proto kernel scope link src 10.20.20.1



    4.4 Wazuh Manager Routing

        The Wazuh Manager is an isolated virtual machine located within the HQ network at `10.10.10.2`. Because it is not directly connected to the Branch network or the WireGuard tunnel, a static route was configured to reach the `10.20.20.0/24` subnet via the HQ gateway (`10.10.10.1`).

        This directs Branch-bound traffic to the HQ Ubuntu server, which subsequently encapsulates it into the tunnel. The presence of this required route was verified:


            vboxuser@wazuh-manager:~$ ip route
            default via 10.10.10.1 dev enp0s8
            10.10.10.0/24 dev enp0s8 proto kernel scope link src 10.10.10.2
            10.20.20.0/24 via 10.10.10.1 dev enp0s8



    4.5 Branch Windows Endpoint

        The Windows endpoint resides within the Branch network with the IP address `10.20.20.5`. The Branch Ubuntu gateway (`10.20.20.1`) provides the routing path for any traffic leaving this local subnet.

        The endpoint's network configuration was confirmed using `ipconfig`, and the routing table was examined using `route print` to ensure the default gateway was properly assigned:

        ```cmd
        C:\Users\Admin> route print
        ===========================================================================
        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0       10.20.20.1       10.20.20.5     35
            10.20.20.0    255.255.255.0         On-link        10.20.20.5    291
        ===========================================================================

   

 



    4.6 End-to-End Connectivity Testing

        Connectivity testing was performed across two layers to verify communication across the site-to-site VPN.

        **Network Layer (ICMP):**
        The primary logical path was validated by generating ping requests from the Windows Endpoint to the Wazuh Manager:
        `Windows Endpoint (10.20.20.5) → Branch Gateway (10.20.20.1) → WireGuard Tunnel → HQ Gateway (10.10.10.1) → Wazuh Manager (10.10.10.2)`
        Successful ICMP responses confirmed operational Layer 3 routing in both directions.

        **Application Layer (Wazuh Telemetry):**
        To ensure the infrastructure supported its primary security objective, application-layer connectivity was validated. The Wazuh Agent installed on the Windows endpoint successfully established a persistent TCP connection to the Wazuh Manager (ports 1514/1515). This was confirmed by the active ingestion of security event logs into the Wazuh dashboard, proving the tunnel successfully transports application payload without interference.



    4.7 Connectivity Validation Results

        The routing and connectivity tests confirmed the following operational states:

        | Test                                | Expected Result                    | Result     |
        | ----------------------------------- | ---------------------------------- | ---------- |
        | **HQ Gateway → Branch Gateway**     |Connectivity through VPN            | Successful |
        | **Branch Gateway → HQ Gateway**     |Connectivity through VPN            | Successful |
        | **HQ → Branch network**             | Remote network reachable           | Successful |
        | **Branch → HQ network**             | Remote network reachable           | Successful |
        | **Windows Endpoint → HQ resources** | ICMP traffic routed through VPN    | Successful |
        | **Wazuh Manager → Branch network**  | Branch route operational           | Successful |
        | **Wazuh Agent → Wazuh Manager**     | Application telemetry over TCP 1514| Successful |

        These results demonstrate that the WireGuard configuration is supported by functional, end-to-end routing capable of sustaining distributed security monitoring.


    4.8 Routing Validation Evidence

        The primary validation evidence (routing tables and gateway configurations) is documented directly within Sections 4.2 through 4.5. This command-line evidence is used alongside the cryptographic packet captures (detailed in Chapter 3) to conclusively demonstrate that the VPN provides highly functional, secure site-to-site connectivity for the SOC architecture.

       

        