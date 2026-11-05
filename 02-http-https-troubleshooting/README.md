# HTTP/HTTPS Troubleshooting Lab

## Objective

Practice basic HTTP and HTTPS troubleshooting using command-line tools in Ubuntu.

## Tools

- `curl`
- `ping`
- `nslookup`
- `nc`

## Lab 1 — Check HTTP and HTTPS response

### Step 1: Check an HTTPS website

Run:

```bash
curl -I https://example.com
```

The `-I` option requests only the response headers, without downloading the full webpage.

### Expected observations

Record:

- The HTTP status code
- Whether the connection uses HTTP or HTTPS
- Any relevant response headers

## Results

### Lab 1 Results

#### HTTPS request

Command:

```bash
curl -I https://example.com
```
#### Detailed HTTPS connection

Command:

```bash
curl -Iv https://example.com
#### IPv4-only HTTPS test

Command:

```bash
curl -4 -Iv https://example.com
```
#### TCP port connectivity test

Command:

```bash
nc -vz example.com 443
``
## Troubleshooting notes

_To be completed during the lab._
