# Diagnostic Commands

Run diagnostics to troubleshoot connectivity, firewall, configuration, and system readiness issues.

## Connection Test

Test connectivity to the Strike7 API (DNS, TCP, TLS, HTTP health):

```bash
s7 diag connection
```

**Output:**

```
Testing connectivity to https://internal.api.strike7.ai...

✓ DNS resolution
✓ TCP connection
✓ TLS handshake
✓ HTTP health check

All connection checks passed.
```

### Custom API URL

```bash
s7 diag connection --api-url https://custom-api.example.com
```

---

## Firewall Check

Verify that all required outbound endpoints are reachable:

```bash
s7 diag firewall
```

**Output:**

```
Checking outbound access...

✓ Strike7 API (HTTPS)
✓ AWS ECR (Docker pulls)
✓ AWS S3 (CLI updates)

All endpoints reachable.
```

---

## Configuration Check

Validate local configuration and credentials:

```bash
s7 diag config
```

**Output:**

```
Checking configuration in /Users/you/.s7...

✓ Config directory exists
✓ config.yaml present and valid
✓ credentials.json present
✓ File permissions secure (0600)

Configuration looks good.
```

---

## Preflight Check

Run the full system preflight — the same checks that `s7 striker install` runs before installation:

```bash
s7 diag preflight
```

**Output:**

```
Running preflight checks...

✓ Docker installed and running
✓ Docker Compose available
✓ Sufficient disk space (15 GB free)
✓ Kernel version supported
✓ Outbound HTTPS connectivity
✓ ECR registry reachable

All preflight checks passed.
```

!!! tip
    Run `s7 diag preflight` before installing a Striker to catch issues early.

---

## Run All Diagnostics

```bash
s7 diag connection
s7 diag firewall
s7 diag config
s7 diag preflight
```

### Verbose Output

Add `-v` for detailed diagnostic information:

```bash
s7 diag connection -v
```
