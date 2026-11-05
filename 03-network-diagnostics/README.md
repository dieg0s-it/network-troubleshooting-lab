# Network Diagnostics Lab

## Objective

Practice basic network diagnostics in Ubuntu to identify connectivity issues and understand the path between a local system and a remote destination.

## Tools

- `ip`
- `ping`
- `tracepath`

## Lab 1 — Inspect Local Network Configuration

### Step 1: Display Network Interfaces

Run:

```bash
ip address
```

Record:
- The active network interface
- Its IPv4 address
- Its subnet information
- Whether an IPv6 address is assigned

### Step 2: Display the Routing Table

Run:

```bash
ip route
```

Record:
- The default gateway
- The interface used for the default route

## Results

### Lab 1 Results — Local Network Configuration

#### Network Interfaces

Command:

```bash
ip address
```

Observations:
- The active Wi-Fi interface is `wlp3s0`.
- The interface state is `UP` and `LOWER_UP`.
- The system has the IPv4 address `192.168.1.134/24`.
- The subnet is `192.168.1.0/24`, equivalent to subnet mask `255.255.255.0`.
- The broadcast address is `192.168.1.255`.
- The interface has a link-local IPv6 address beginning with `fe80::`.
- The loopback interface `lo` uses `127.0.0.1`.

Conclusion:

The Wi-Fi interface is active and has a private IPv4 address assigned. The local interface configuration appears operational.

#### Routing Table

Command:

```bash
ip route
```

Observations:
- The default gateway is `192.168.1.1`.
- The default route uses the Wi-Fi interface `wlp3s0`.
- The local network is `192.168.1.0/24`.
- The system's source IPv4 address is `192.168.1.134`.
- Traffic to destinations outside the local subnet is sent through the default gateway.

Conclusion:

The routing table shows a default route through the local router. The system has a defined path for reaching destinations outside its local network.

### Lab 2 Results — Gateway Connectivity

Command:

```bash
ping -c 4 192.168.1.1
```

Observations:
- The gateway responded to all 4 ICMP echo requests.
- Packet loss was 0%.
- Average round-trip time was approximately 2.24 ms.
- The gateway returned a TTL value of 64.

Conclusion:

The local system can successfully communicate with its default gateway. The test shows no packet loss or significant latency on the local network path.

### Lab 3 Results — External IP Connectivity

Command:

```bash
ping -c 4 8.8.8.8
```

Observations:
- The external IP address `8.8.8.8` responded to all 4 ICMP echo requests.
- Packet loss was 0%.
- Average round-trip time was approximately 13.15 ms.
- The received TTL value was 117.

Conclusion:

The system can successfully reach an external IP address through the local gateway. The test confirms basic external network connectivity without relying on DNS name resolution.

### Lab 4 Results — DNS and External Connectivity

Command:

```bash
ping -c 4 google.com
```

Observations:
- The hostname `google.com` resolved to `192.178.25.110`.
- The destination responded to all 4 ICMP echo requests.
- Packet loss was 0%.
- Average round-trip time was approximately 12.88 ms.

Conclusion:

The system successfully resolved the hostname and reached the resulting IP address. DNS resolution and external connectivity are working in this test.

### Lab 5 Results — Tracing the Network Path

Command:

```bash
tracepath google.com
```

Observations:
- The first hop was `GN630VE.home`, with a response time of approximately 2 ms.
- The first hop corresponds to the local router or gateway.
- Subsequent hops did not respond to the tracepath probes.
- Previous tests confirmed successful connectivity to external IP addresses and domain names.

Conclusion:

The tracepath command identified the first hop on the route to the destination. The lack of replies from later hops does not necessarily indicate a connectivity failure, because intermediate routers may filter or ignore diagnostic probes.

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
