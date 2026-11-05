# HTTP/HTTPS Troubleshooting Lab

## Objective

Practice basic HTTP and HTTPS troubleshooting using command-line tools in Ubuntu.

## Tools

- `curl`
- `nc`

## Lab 1 — Inspect HTTP and HTTPS responses

### Step 1: Check an HTTPS website

Command:

```bash
curl -I https://example.com
```

The `-I` option requests response headers only.

### Step 2: Check HTTP

Command:

```bash
curl -I http://example.com
```

### Step 3: Follow redirects

Command:

```bash
curl -IL http://example.com
```

The `-L` option tells curl to follow redirects if the server returns a redirect response.

### Results

#### HTTPS request

Command:

```bash
curl -I https://example.com
```

Result: `HTTP/2 200`

Observations:
- The HTTPS request completed successfully.
- The response included `server: cloudflare`.
- `cf-cache-status: HIT` indicated that the response was served from cache.

#### HTTP request and redirect check

Commands:

```bash
curl -I http://example.com
curl -IL http://example.com
```

Result: `HTTP/1.1 200 OK`

Observations:
- The HTTP request completed successfully.
- The server did not return a `301` or `302` redirect in this test.
- The `-L` option had no redirect to follow.

Conclusion:

This endpoint responded directly over HTTP in the observed test. Do not assume a website redirects HTTP to HTTPS; verify its actual response.

## Lab 2 — Inspect HTTPS, TLS, and IP connectivity

### Step 1: Verbose HTTPS connection

Command:

```bash
curl -Iv https://example.com
```

Observations:
- DNS resolved the hostname to IPv4 and IPv6 addresses.
- The IPv6 connection attempt failed with `Network is unreachable`.
- The connection succeeded over IPv4 on TCP port 443.
- TLS 1.3 negotiation completed successfully.
- The certificate matched `example.com` and was verified by OpenSSL.
- ALPN negotiated HTTP/2.
- The server returned `HTTP/2 200`.

Conclusion:

HTTPS connectivity to `example.com` worked over IPv4. The IPv6 connection failure did not prevent the request from succeeding.

### Step 2: Force IPv4

Command:

```bash
curl -4 -Iv https://example.com
```

Observations:
- The `-4` option forced curl to use IPv4.
- The TCP connection to port 443 succeeded.
- TLS 1.3 and certificate validation completed successfully.
- HTTP/2 was negotiated and the server returned `HTTP/2 200`.

Conclusion:

The HTTPS request succeeded over IPv4. Forcing IPv4 avoided the unsuccessful IPv6 connection attempt observed in the previous test.

## Lab 3 — Test TCP port connectivity

### Step 1: Test HTTPS port 443

Command:

```bash
nc -vz example.com 443
```

Result: `succeeded`

Observations:
- The TCP connection to port 443 was successful.
- The port was reachable from the local system.
- This test did not inspect TLS negotiation or the HTTP response.

### Step 2: Test port 81 with a timeout

Command:

```bash
nc -vz -w 5 example.com 81
```

Result:
- IPv4 connection attempts timed out.
- IPv6 attempts failed with `Network is unreachable`.

Observations:
- Port 81 did not respond within the 5-second timeout.
- A timeout does not prove that a firewall is responsible; further investigation would be required.
- IPv6 connection attempts failed because the local system had no available IPv6 route.

Conclusion:

The tests confirmed TCP connectivity to port 443 but no response from port 81 within the configured timeout.

## Troubleshooting notes

| Symptom or result | What it indicates | Next step |
|---|---|---|
| `curl: (6) Could not resolve host` | Hostname resolution failed; check for typos and DNS | Verify the hostname and test DNS resolution |
| `HTTP 200` | The HTTP request succeeded | Review response headers and content as needed |
| `301` or `302` | The server returned a redirect | Use `curl -IL` to inspect the redirect chain |
| `Network is unreachable` | No available route to that network | Check local network and IP-family connectivity |
| TLS certificate verified | Certificate validation succeeded for this connection | Continue investigating other layers if access still fails |
| TCP port `succeeded` | TCP connection to the port was established | Check TLS and application response separately |
| TCP `timed out` | No response arrived before the timeout | Investigate routing, filtering, and service availability; do not assume a firewall without more evidence |

## Overall conclusion

The lab demonstrated how to inspect HTTP responses, examine HTTPS and TLS negotiation, compare IPv4 and IPv6 connectivity, and test TCP port reachability. These checks help isolate whether a problem occurs at name resolution, network connectivity, TLS, or the HTTP application layer.

*All results above are from hands-on tests against the public `example.com` endpoint in the user's Ubuntu environment.*
