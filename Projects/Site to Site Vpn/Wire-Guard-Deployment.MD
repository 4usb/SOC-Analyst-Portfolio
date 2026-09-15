03 — WireGuard Deployment

    3.1 WireGuard Installation

        WireGuard was deployed on both the HQ and Branch Ubuntu servers to provide an encrypted site-to-site VPN between the two simulated network environments.

        The Ubuntu servers function as both WireGuard VPN gateways and routers for their respective internal networks. The required WireGuard packages were installed on both gateways:

        ```bash
        sudo apt update
        sudo apt install wireguard wireguard-tools

        ```

        The installation provided the WireGuard kernel interface and command-line management utilities required to create, configure, and verify the VPN tunnel.

        WireGuard was configured to use the `wg0` interface on both gateways. The VPN uses UDP port `51820` for WireGuard communication across the simulated WAN, and this port was explicitly permitted through the Uncomplicated Firewall (UFW) on both gateways to ensure unhindered encrypted transport.

        ---

    3.2 Key Generation

            WireGuard uses public-key cryptography to authenticate VPN peers. A unique private/public key pair was generated for each WireGuard gateway. The private key remains on the respective gateway, while the corresponding public key is exchanged with the remote peer.

            Key generation was performed using the WireGuard utility:

            ```bash
            wg genkey | tee privatekey | wg pubkey > publickey

            ```

            This produced:

            * A private key for the local WireGuard gateway.
            * A corresponding public key used to identify the gateway to its peer.

            The HQ gateway was configured with the Branch gateway's public key, while the Branch gateway was configured with the HQ gateway's public key. This establishes peer authentication within the WireGuard tunnel.

            > **Security Consideration:** Private WireGuard keys are sensitive credentials and were strictly excluded from the project repository. Only sanitized configuration examples are included in the public documentation. Any private keys, credentials, or other secrets have been removed to adhere to operational security best practices.

    

    3.3 HQ Gateway Configuration

            The HQ Ubuntu server functions as the WireGuard gateway for the Headquarters network. Its internal HQ interface is `enp0s8 — 10.10.10.1`. The HQ gateway is responsible for routing traffic between the HQ LAN and the WireGuard tunnel.

        IPv4 Forwarding

            IPv4 forwarding was enabled on the HQ gateway to allow the server to route traffic between network interfaces. The current forwarding state can be checked using:

            ```bash
            sysctl net.ipv4.ip_forward

            ```

            A value of `net.ipv4.ip_forward = 1` confirms that IPv4 forwarding is enabled. IPv4 forwarding is required because the HQ server acts as a router rather than simply terminating the VPN connection.

        WireGuard Interface

            The WireGuard interface was configured as `wg0` and assigned the internal tunnel IP address of `10.0.0.1/24`. This interface provides the logical VPN connection between the HQ and Branch gateways.

            The configuration was stored in the standard WireGuard configuration location (`/etc/wireguard/wg0.conf`). The interface can be started with `sudo wg-quick up wg0` and configured to start automatically as a system service with `sudo systemctl enable wg-quick@wg0`.

        Branch Peer

            The HQ gateway was configured with the Branch gateway as a WireGuard peer. The Branch peer configuration specifies the Branch internal network:

            `AllowedIPs = 10.20.20.0/24`

            This informs the HQ gateway that traffic destined for the Branch network should be sent through the WireGuard peer.

 

    3.4 Branch Gateway Configuration

            The Branch Ubuntu server functions as the WireGuard gateway for the Branch network. Its internal Branch interface is `enp0s3 — 10.20.20.1`. The Branch gateway provides routing between the Branch LAN and the WireGuard tunnel.

        IPv4 Forwarding

            IPv4 forwarding was enabled on the Branch gateway to allow traffic to be routed between the Branch LAN and WireGuard interface. The forwarding state was verified using `sysctl net.ipv4.ip_forward`, expecting a result of `1`.

            ### WireGuard Interface

            The Branch gateway uses the `wg0` interface for the VPN connection and was assigned the internal tunnel IP address of `10.0.0.2/24`. The configuration is stored at `/etc/wireguard/wg0.conf` and managed via `wg-quick`.

        HQ Peer

            The Branch gateway was configured with the HQ gateway as its WireGuard peer. The peer configuration specifies the HQ internal network:

            `AllowedIPs = 10.10.10.0/24`

            This ensures that traffic destined for the HQ network is directed through the WireGuard tunnel.

            

    3.5 Internal Network Routing

            Establishing the WireGuard tunnel alone does not provide complete site-to-site connectivity. Additional routing was required so that hosts within each internal network knew how to reach the remote network.

        Wazuh Manager Route

            The Wazuh Manager is located within the HQ network with the address `10.10.10.2`. A static route was configured on the Wazuh Manager for the Branch network:

            `10.20.20.0/24 via 10.10.10.1`

            This directs Branch-bound traffic to the HQ Ubuntu server, which then forwards the traffic through the WireGuard tunnel. The route allows the Wazuh Manager to reach systems located within the Branch network.

        Windows Endpoint Default Gateway

            The Windows endpoint is located within the Branch network with the address `10.20.20.5`. The Branch Ubuntu gateway uses `10.20.20.1` as the Branch network gateway.

            Traffic from the Windows endpoint destined for networks outside its local subnet is forwarded through the Branch gateway. This routing configuration is necessary for the Windows endpoint to securely communicate with resources located within the HQ network, including the Wazuh Manager.

 

    3.6 Tunnel Verification

            After configuring both WireGuard gateways and the required routing, the VPN tunnel was verified using WireGuard status information, packet captures, and end-to-end connectivity tests.

        WireGuard Status and Handshake

            The WireGuard interface and peer status were checked using `sudo wg show`. This command provided confirmation of the WireGuard interface status, configured peer, peer public key, latest handshake, and data transferred. A successful and recent handshake confirmed that the two WireGuard peers established secure communication across the simulated WAN.

        RX/TX Counters and Packet Captures

            The WireGuard status output provided received (`RX`) and transmitted (`TX`) byte counters, confirming that traffic was actively passing through the VPN tunnel.

            To definitively prove the cryptographic integrity of the tunnel, network traffic captures were performed using **Wireshark** and `tcpdump`. Captures taken on the external WAN interface (`enp0s3`/`enp0s9`) successfully demonstrated that all traversing Wazuh telemetry and ICMP traffic was heavily encrypted and obfuscated by WireGuard. Subsequent captures on the internal `wg0` interface confirmed the readable plain-text payload was being successfully encapsulated.

        End-to-End Connectivity

        Connectivity between the two internal networks was tested using ICMP. The Branch Windows endpoint (`10.20.20.5`) was tested against the HQ Wazuh Manager (`10.10.10.2`). Successful ping replies confirmed that:

        * The WireGuard peers could communicate.
        * IP forwarding was operational.
        * The required internal routes were present.

   

    3.7 Deployment Result

        The WireGuard deployment successfully established an encrypted site-to-site VPN between the simulated HQ and Branch networks. The HQ gateway provides routing for the HQ environment, while the Branch gateway provides routing for the Branch environment.

        The configured `AllowedIPs` entries define the remote networks reachable through the WireGuard peer:

        * **HQ peer configuration:** `10.20.20.0/24`
        * **Branch peer configuration:** `10.10.10.0/24`

        The Wazuh Manager was provided with a route to the Branch network through the HQ gateway, while the Windows endpoint uses the Branch gateway for traffic leaving the local Branch subnet. The resulting architecture allows the Branch Windows endpoint to securely communicate with the HQ Wazuh Manager through the encrypted WireGuard tunnel. The deployment was thoroughly validated through WireGuard handshake verification, RX/TX traffic counters, live packet captures, network routing checks, and end-to-end connectivity testing.