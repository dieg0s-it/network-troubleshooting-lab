# Incident Report: DNS and HTTPS Connectivity — Baseline Test

## 1. Summary

A baseline network troubleshooting test was performed on an Ubuntu workstation to verify DNS resolution, IP connectivity, and HTTPS access.

No active connectivity issue was reproduced during testing.

## 2. Environment

- Operating system: Ubuntu
- Network interface: `wlp3s0`
- Local DNS resolver: `127.0.0.53`
- Configured DNS server: `192.168.1.1`
- Public DNS server tested: `8.8.8.8`

## 3. Tests and Results

| Test | Command | Result |
|---|---|---|
| DNS lookup | `nslookup google.com` | Successful |
| IPv4 DNS query | `dig google.com A` | `NOERROR` |
| IPv6 DNS query | `dig google.com AAAA` | `NOERROR` |
| DNS configuration | `resolvectl status` | DNS server identified as `192.168.1.1` |
| IP connectivity | `ping -c 4 8.8.8.8` | 4 packets received, 0% packet loss |
| Domain connectivity | `ping -c 4 google.com` | 4 packets received, 0% packet loss |
| Router DNS query | `dig @192.168.1.1 google.com A` | `NOERROR` |
| Public DNS query | `dig @8.8.8.8 google.com A` | `NOERROR` |
| HTTPS request | `curl -I https://google.com` | `301` redirect to `https://www.google.com/` |
| HTTPS destination | `curl -I https://www.google.com/` | `200 OK` |

## 4. Findings

- The workstation successfully resolved `google.com` to IPv4 and IPv6 addresses.
- The configured DNS server responded successfully.
- The public DNS server `8.8.8.8` also resolved the domain.
- Basic IP connectivity was successful, with no packet loss in the tests performed.
- HTTPS access to Google was successful.
- The `301` response was a normal redirect to the canonical URL.
- The final `200` response confirmed successful access to the destination.

## 5. Conclusion

DNS resolution, basic IP connectivity, and HTTPS access were functioning during the test.

No active incident was identified. These results provide a baseline for comparison when troubleshooting future DNS or connectivity reports.

## 6. Tools Used

## 6. Tools Used

- `nslookup`
- `dig`
- `resolvectl`
- `ping`
- `curl`

# Simulated Incident: Corporate Portal HTTPS Access

> **Note:** This is a simulated troubleshooting scenario for learning purposes. No real corporate incident was investigated.

## 1. Incident Summary

A user reported that they could not access a corporate portal over HTTPS, while other websites were working.
> The domain `portal.empresa.example` is a placeholder used exclusively for this simulated exercise.

## 2. Reported Symptoms

- The user could access other websites.
- The corporate portal did not load in the browser.
- The issue was reported by one user.
- The user stated that the portal domain was correct.

## 3. Troubleshooting Steps

### Step 1: Test basic IP connectivity

Command:

```bash
ping -c 4 8.8.8.8
```

Simulated result:

- 4 packets received
- 0% packet loss

Interpretation: Basic IP connectivity to the tested destination was available.

### Step 2: Verify DNS resolution

Command:

```bash
nslookup portal.empresa.example
```

Simulated result: The valid corporate portal domain resolved to an IP address.

Interpretation: DNS resolution was successful.

### Step 3: Test connectivity to the portal

Command:

```bash
ping -c 4 portal.empresa.example
```

Simulated result:

- The domain resolved to an IP address.
- The destination responded to ICMP.

Interpretation: The destination was reachable through ICMP during the test.

### Step 4: Test HTTPS response

Command:

```bash
curl -I https://portal.empresa.example
```

Simulated result: SSL certificate problem: certificate has expired.

Interpretation: The HTTPS request failed because certificate validation was unsuccessful. Further investigation was required to identify the exact TLS certificate issue.

### Step 5: Test TCP connectivity to port 443

Command:

```bash
nc -vz portal.empresa.example 443
```

Simulated result: The TCP connection to port 443 succeeded.

Interpretation: The HTTPS port was reachable during the test.

### Step 6: Inspect TLS details

Command:

```bash
curl -Iv https://portal.empresa.example
```

Simulated result:

```text
SSL certificate problem: certificate has expired
```

Interpretation: The simulated error indicated an expired TLS certificate.

## 4. Findings

- Basic IP connectivity was available.
- DNS resolution was successful.
- The destination responded to ICMP.
- TCP connectivity to port 443 was successful.
- The HTTPS request failed during certificate validation.
- The simulated evidence pointed to a TLS certificate issue rather than a DNS or basic connectivity problem.

## 5. Resolution / Next Action

The issue was documented and escalated to the team responsible for the corporate portal and its TLS certificate.

No network configuration was changed, and certificate validation was not disabled.

## 6. Ticket Status

**Escalated — pending certificate review.**

## 7. Tools Used

- `ping`
- `nslookup`
- `curl`
- `nc`
