01 — Project Objectives

    1.1 Project Overview

        This project implements a secure site-to-site VPN between two
        simulated network environments using WireGuard. The lab extends
        an existing Wazuh security monitoring environment by placing a
        Windows endpoint within the Branch network while the Wazuh
        Manager remains within the HQ network.

        The project demonstrates how encrypted site-to-site connectivity
        can be used to securely transport network and security telemetry
        between geographically or logically separated environments.

    1.2 Project Objectives

        The primary objectives of the project are to:

        1. Establish an encrypted site-to-site VPN between the HQ and
        Branch networks using WireGuard.

        2. Configure routing between the two networks through the
        WireGuard tunnel.

        3. Place a Windows endpoint behind the Branch VPN gateway.

        4. Maintain the Wazuh Manager within the HQ network while
        enabling the Branch Windows endpoint to communicate with it
        through the VPN.

        5. Validate connectivity between the HQ and Branch networks.

        6. Demonstrate that traffic crossing the simulated WAN is
        encapsulated within the WireGuard tunnel.

        7. Capture and analyse WAN traffic to examine what information
        is visible to an observer positioned outside the VPN tunnel.

        8. Document the implementation, validation process, security
        observations, and lessons learned.

    1.3 Security Objectives

        The security component of the project focuses on demonstrating:

        - Confidentiality of traffic transported across the simulated WAN.
        - Authentication between WireGuard peers.
        - Controlled routing between the HQ and Branch networks.
        - Secure transport of Wazuh telemetry between network segments.
        - Visibility of VPN traffic from a network monitoring perspective.

    1.4 Scope

        The project is limited to an isolated VirtualBox laboratory
        environment under the control of the project owner.

        The laboratory consists of:

        - An HQ network.
        - A Branch network.
        - WireGuard gateways at both sites.
        - A Wazuh Manager located within the HQ network.
        - A Windows endpoint located within the Branch network.
        - A simulated WAN connecting the two VPN gateways.

        All connectivity and security testing described in this project
        is performed within the isolated laboratory environment.

    1.5 Expected Outcome

        The completed laboratory should demonstrate that:

        - The HQ and Branch networks can communicate through the
        WireGuard tunnel.
        - The Branch Windows endpoint can communicate with the HQ
        Wazuh Manager.
        - Wazuh telemetry can traverse the VPN.
        - Traffic observed on the simulated WAN is represented as
        WireGuard transport traffic rather than exposed application
        traffic.