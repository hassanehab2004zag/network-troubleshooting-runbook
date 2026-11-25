# Network troubleshooting runbook

Written practice notes. This is a guide, not tested code. Adjust commands to your own devices.

## 1. Define the problem
Who is affected (one user, one VLAN, one site, everyone)? Since when? What changed? Can the user reach the gateway, an internal server, the internet?

## 2. Work up the layers
1. **Physical:** cable, port LEDs, `show interfaces` for errors and flaps.
2. **Data link:** VLAN on the port, trunk allowed list, MAC in the table, STP state.
3. **IP:** address, mask, gateway (`ipconfig /all`, `ip a`), duplicate IP (`arp -a`), DHCP lease.
4. **Routing:** `tracert` or `traceroute`, routing table on the gateway, return path.
5. **Name resolution:** `nslookup name` against two DNS servers.
6. **Transport and application:** can you open the TCP port (`Test-NetConnection host -Port 443`)? Does the app answer?
7. **Firewall:** policy hit counters, log the denied traffic, NAT and port forwarding rules.

## 3. Site-to-site VPN down
Check the tunnel phase 1 and phase 2 status, matching proposals and pre-shared key, both ends reachable on the public IP, NAT-T, and that the local and remote subnets are not overlapping.

## 4. Wireshark filters worth remembering
`ip.addr == 10.0.0.5`, `tcp.port == 443`, `dns`, `tcp.flags.syn == 1 && tcp.flags.ack == 0` (new connections), `tcp.analysis.retransmission`, `icmp`.

## 5. Root cause report template
- Summary and impact (who, how long)
- Timeline
- Root cause
- Fix applied
- How to prevent it
