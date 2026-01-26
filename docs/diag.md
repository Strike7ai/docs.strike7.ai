# Diagnostic Commands

Run diagnostics to troubleshoot connectivity and configuration issues.

## diag connection

Test connection to Strike7 API.

### Usage

```bash
s7-cli diag connection
```

### Output

```
Testing connection to api.strike7.ai...

✓ DNS resolution: 52.7.123.45
✓ TCP connection: established
✓ TLS handshake: success (TLS 1.3)
✓ API authentication: valid
✓ Latency: 45ms

All checks passed!
```

### Failed Output Example

```
Testing connection to api.strike7.ai...

✓ DNS resolution: 52.7.123.45
✓ TCP connection: established
✗ TLS handshake: failed (certificate error)

Connection failed. Check your network settings.
```

---

## diag firewall

Check if required outbound connections are allowed.

### Usage

```bash
s7-cli diag firewall
```

### Output

```
Required outbound connections:

✓ api.strike7.ai:443 (HTTPS) - OK
✓ api.strike7.ai:443 (WSS) - OK
✓ s3.amazonaws.com:443 (file uploads) - OK

All required ports are accessible.
```

---

## diag config

Validate configuration file.

### Usage

```bash
s7-cli diag config
```

### Output

```
Configuration file: /Users/suppa/.strike7/config.yaml

✓ File exists
✓ File permissions: 0600 (secure)
✓ YAML syntax: valid
✓ API URL: http://129.212.229.32:8001
✓ API Key: present
✓ Organization ID: present

Configuration is valid.
```

---

## Common Diagnostics

### Check Everything

```bash
# Run all diagnostics
s7-cli diag connection
s7-cli diag firewall
s7-cli diag config
```

### Verbose Output

```bash
s7-cli diag connection -v
```
