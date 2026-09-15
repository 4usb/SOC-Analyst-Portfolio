 02 — Architecture and Lab Environment

    2.1 Architecture Overview

        The project consists of two simulated network environments representing a Headquarters (HQ) site and a Branch site.

        Each site contains an Ubuntu-based WireGuard gateway. The gateways establish an encrypted site-to-site VPN across a simulated WAN.

        The Wazuh Manager is located within the HQ network, while a Windows endpoint monitored by a Wazuh Agent is located within the Branch network.

        The architecture allows the Branch endpoint to communicate with the Wazuh Manager through the encrypted WireGuard tunnel.



    2.2 Network Components

        | Component                   Location                               | Function |
        |----------------------------|-----------------------|------------------------------------------------------|
        | HQ WireGuard Gateway       | HQ                    | Terminates the WireGuard tunnel and routes traffic   |
        | Branch WireGuard Gateway   | Branch                | Terminates the WireGuard tunnel and routes traffic   |
        | Wazuh Manager              | HQ                    | Collects and analyzes security telemetry             |
        | Windows Endpoint           | Branch                | Monitored endpoint generating security events        |
        | Wazuh Agent                | Branch                | Collects endpoint telemetry and forwards it to Wazuh |
        | Simulated WAN              | Between HQ and Branch | Provides the transport network for the VPN           |
        | Wireshark                  | WAN                   | Captures and analyzes network traffic                |
        | VirtualBox                 | Lab environment       | Hosts the virtual network infrastructure             |



    2.3 Network Topology

        The lab follows a site-to-site architecture:

        HQ Network
        → HQ WireGuard Gateway
        → Simulated WAN
        → Branch WireGuard Gateway
        → Branch Network
        → Windows Endpoint

        The Wazuh Manager is located within the HQ network.

        The Windows endpoint in the Branch network runs the Wazuh Agent and communicates with the Wazuh Manager through the site-to-site VPN.

        [Insert network topology diagram here]



    2.4 Traffic Flow

        Traffic originating from the Branch network is routed through the Branch WireGuard gateway.

        The Branch gateway encrypts and encapsulates the traffic before transmitting it across the simulated WAN.

        The HQ WireGuard gateway receives the encrypted traffic, decrypts it, and routes the traffic into the HQ network.

        For Wazuh monitoring, the communication path is:

        Windows Endpoint
        → Branch Gateway
        → WireGuard Tunnel
        → HQ Gateway
        → Wazuh Manager

        This design demonstrates that security telemetry from an endpoint located on a separate network can traverse an encrypted site-to-site VPN before reaching the central monitoring infrastructure.



    2.5 Addressing and Interfaces

        The following table documents the addressing used within the laboratory.

        | Device             | Interface | IP Address        | Network/Purpose  |
        |--------------------|-----------|-------------------|------------------|
        | HQ Gateway         |enp0s3     |  192.168.56.105   | Simulated WAN    |
        | HQ Gateway         | enp0s8    | 10.10.10.1        ||Hq Network       |
        | HQ Gateway         | wg0       | 10.10.10.1/24     | WireGuard Tunnel |
        | Branch Gateway     | enp0s9    | 192.168.56.106    | Simulated WAN    |
        | Branch Gateway     | enp0s3    | 10.20.20.1        | Branch Network   |
        | Branch Gateway     | wg0       | 10.0.0.2/24       | WireGuard Tunnel |
        | Wazuh Manager      | enp0s8    | 10.10.10.2        | HQ Network       |
        | Windows Endpoint   | Ethernet  | 10.20.20.5        | Branch Network   |



    2.6 Architecture Objective

        The architecture is designed to demonstrate three primary capabilities:

        1. Secure communication between separate network environments using WireGuard.
        2. Routing of traffic between the HQ and Branch networks through the VPN tunnel.
        3. Secure transmission of Wazuh security telemetry from the Branch endpoint to the HQ Wazuh Manager.

        

    2.7 Architecture Summary

        The completed architecture combines network routing, encrypted site-to-site connectivity, and centralized security monitoring.

        The HQ and Branch networks remain logically separated while communicating through the WireGuard VPN.

        The Wazuh Manager remains within the HQ network, while the monitored Windows endpoint remains within the Branch network.

        This design provides a practical demonstration of secure inter-site connectivity and security telemetry transport across an encrypted VPN.