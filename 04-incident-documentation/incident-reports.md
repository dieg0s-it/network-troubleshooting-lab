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

- `nslookup`
- `dig`
- `resolvectl`
- `ping`
- `curl`
