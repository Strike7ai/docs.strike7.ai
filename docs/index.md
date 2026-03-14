# Welcome to Strike7

Strike7 is an autonomous penetration testing platform. Deploy on-prem Striker nodes on your network, launch AI-driven pentests, and get actionable vulnerability findings — all from the command line.

## Quick Start

```bash
# Install the S7 CLI
curl -sL https://get.strike7.ai | bash

# Authenticate
s7 auth login

# Register and install a Striker on your network
s7 striker register my-striker
# Copy the install token from the output, then:
s7 striker install --token <TOKEN>

# Launch a pentest
s7 pentest create --target 10.0.0.0/24 --type network

# Watch it in real-time
s7 pentest watch <strike-id>

# View findings
s7 pentest findings <strike-id>
```

## Documentation

- [Installation](installation.md) — Install the S7 CLI on macOS or Linux
- [Authentication](login.md) — Login via browser or API key
- [Configuration](config.md) — CLI configuration and global flags
- [Striker](striker.md) — Deploy and manage on-prem Striker nodes
- [Pentest](pentest.md) — Create, monitor, and review pentests
- [Scope](scope.md) — Manage pentest targets and scope
- [Diagnostics](diag.md) — Troubleshoot connectivity and configuration
- [Update](update.md) — Keep the CLI up to date

## Architecture

```
┌──────────────┐        ┌────────────────────┐        ┌──────────────┐
│   S7 CLI     │◄──────►│  Strike7 Platform  │◄──────►│   Striker    │
│ (your laptop)│  HTTPS  │   (Cloud SaaS)     │   VPN  │ (your network)│
└──────────────┘        └────────────────────┘        └──────────────┘
```

The **S7 CLI** is your control interface. **Strikers** are Docker containers deployed inside your network that perform the actual pentesting. They connect securely to the Strike7 platform via an encrypted VPN tunnel.
