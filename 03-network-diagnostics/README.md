# Network Diagnostics Lab

## Objective

Practice basic network diagnostics in Ubuntu to identify connectivity issues and understand the path between a local system and a remote destination.

## Tools

- `ip`
- `ping`
- `traceroute`

## Lab 1 — Inspect local network configuration

### Step 1: Display network interfaces

Run:

```bash
ip address
```

Record:
- The active network interface
- Its IPv4 address
- Its subnet information
- Whether an IPv6 address is assigned

### Step 2: Display the routing table

Run:

```bash
ip route
```

Record:
- The default gateway
- The interface used for the default route

## Results
### Lab 1 Results — Local network configuration

#### Network interfaces

Command:

```bash
ip address
#### Routing table

Command:

```bash
ip route
```
### Lab 2 Results — Gateway connectivity

Command:

```bash
ping -c 4 192.168.1.1
``` 
### Lab 3 Results — External IP connectivity

Command:

```bash
ping -c 4 8.8.8.8
```
### Lab 4 Results — DNS and external connectivity

Command:

```bash
ping -c 4 google.com
```
### Lab 5 Results — Tracing the network path

Command:

```bash
tracepath google.com
```
## Troubleshooting Summary

| Test | Command | Result | Interpretation |
|---|---|---|---|
| Local network configuration | `ip address` | Wi-Fi interface `wlp3s0` is active with IP `192.168.1.134/24` | The interface has an assigned IPv4 address |
| Routing table | `ip route` | Default gateway is `192.168.1.1` | The system has a route to external networks |
| Gateway connectivity | `ping -c 4 192.168.1.1` | 0% packet loss, average 2.24 ms | Local connectivity is working |
| External IP connectivity | `ping -c 4 8.8.8.8` | 0% packet loss, average 13.15 ms | External IP connectivity is working |
| DNS and connectivity | `ping -c 4 google.com` | Hostname resolved; 0% packet loss | DNS resolution and external connectivity are working |
| Network path | `tracepath google.com` | First hop responded; later hops did not reply | Intermediate routers may filter diagnostic probes |

## Overall Conclusion

The diagnostic tests confirmed that the system has an active network interface, a valid local IPv4 configuration, and a default route through the local gateway.

The system successfully reached both an external IP address and a hostname, confirming basic external connectivity and DNS resolution.

The tracepath test identified the local router as the first hop. Later hops did not respond, but this does not necessarily indicate a network failure because intermediate routers may filter diagnostic probes.

This lab demonstrates a basic troubleshooting sequence: inspect local configuration, verify routing, test the gateway, test external IP connectivity, verify DNS resolution, and inspect the network path.

_To be completed during the lab._
