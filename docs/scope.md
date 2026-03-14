# Scope Management

Manage the targets and scope of a pentest. Scope entries define what the Striker is allowed to test.

## Add a Scope Entry

```bash
s7 scope add <strike-id> <type> <value>
```

### Supported Types

| Type | Description | Example |
|------|-------------|---------|
| `cidr` | IP range in CIDR notation | `10.0.0.0/24` |
| `domain` | Domain name | `example.com` |
| `ip` | Single IP address | `192.168.1.100` |
| `url` | Specific URL | `https://app.example.com` |

### Examples

```bash
# Add a CIDR range
s7 scope add stk_abc12345 cidr 10.0.0.0/24

# Add a domain
s7 scope add stk_abc12345 domain example.com

# Add a specific URL
s7 scope add stk_abc12345 url https://app.example.com/api

# JSON output
s7 scope add stk_abc12345 ip 192.168.1.100 --json
```

**Output:**

```
Scope added: cidr 10.0.0.0/24 (scp_abc1)
```

---

## List Scope Entries

```bash
s7 scope list <strike-id>
```

**Output:**

```
ID        TYPE    VALUE
scp_abc1  cidr    10.0.0.0/24
scp_def2  domain  example.com
scp_ghi3  url     https://app.example.com/api
```

Add `--json` for JSON output.

---

## Remove a Scope Entry

```bash
s7 scope remove <strike-id> <scope-id>
```

**Output:**

```
Scope entry removed.
```

---

## Example Workflow

```bash
# Create a pentest
s7 pentest create --target https://example.com

# Add additional targets to scope
s7 scope add <strike-id> domain api.example.com
s7 scope add <strike-id> cidr 10.0.1.0/24

# Verify scope
s7 scope list <strike-id>

# Start the pentest — Striker will test all scope entries
s7 pentest watch <strike-id>
```
