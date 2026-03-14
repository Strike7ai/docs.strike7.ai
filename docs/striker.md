# Striker Management

Strikers are Docker containers deployed on your network to perform penetration testing. They connect securely to the Strike7 platform via an encrypted VPN tunnel (Headscale/Tailscale) and communicate over mTLS-secured gRPC.

## Overview

The Striker lifecycle:

1. **Register** a Striker on the platform — get an install token
2. **Install** the Striker locally using the token — sets up Docker, certificates, and VPN
3. **Run pentests** — the Striker receives jobs from the platform
4. **Upgrade** or **uninstall** as needed

---

## Register a Striker

Register a new Striker with the Strike7 platform:

```bash
s7 striker register <name>
```

**Example:**

```bash
s7 striker register datacenter-striker
```

**Output:**

```
Striker registered: datacenter-striker (stk_abc12345)

Install command:
  s7 striker install --token eyJzd...
```

!!! info "Install Token"
    The install token is a Base64-encoded payload containing the Striker ID, VPN enrollment key, mTLS certificates, and short-lived Docker registry credentials. It is valid for a limited time — install promptly.

---

## Install a Striker

Install a Striker on the current machine using the token from `register`:

```bash
s7 striker install --token <TOKEN>
```

### Flags

| Flag | Description |
|------|-------------|
| `--token` | Install token from `s7 striker register` (required) |
| `--force` | Force reinstall if already installed |

### What Happens

The installer runs an 8-step process:

1. **Preflight checks** — Docker installed, disk space, kernel version, network access
2. **Create directory** — `~/.s7/strikers/<strikerId>/`
3. **Write mTLS certificates** — `server.crt`, `server.key`, `ca.crt`
4. **Generate docker-compose.yml** — with Headscale Tailscale VPN configuration
5. **Docker ECR login** — using short-lived credentials from the install token
6. **Pull Striker image** — from AWS ECR
7. **Start container** — `docker compose up -d`
8. **Health check** — polls container status (up to 60 seconds)

### Prerequisites

- **Docker** must be installed and running
- **Disk space**: at least 2 GB free
- **Network**: outbound HTTPS access (no inbound rules needed)

### One-Line Install

Install the S7 CLI and a Striker in one command:

```bash
curl -sL https://get.strike7.ai | bash -s -- --token <TOKEN>
```

---

## List Strikers

```bash
s7 striker list
```

**Output:**

```
ID        NAME                  STATUS    IP             LAST SEEN
stk_abc1  datacenter-striker    online    100.64.0.5     2m ago
stk_def2  cloud-vpc-striker     offline   -              3d ago
```

Add `--json` for JSON output.

---

## Striker Status

Check a specific Striker's health:

```bash
s7 striker status <id|name>
```

**Output:**

```
Name:      datacenter-striker
ID:        stk_abc12345-xxxx-xxxx
Status:    online
IP:        100.64.0.5
Version:   1.2.0
Last Seen: 2m ago
```

---

## Test Connection

Test the gRPC connection to a Striker:

```bash
s7 striker test <id>
```

**Output:**

```
Connection test: ok
```

---

## Upgrade a Striker

Pull the latest Docker image and restart:

```bash
s7 striker upgrade <id>
```

### Flags

| Flag | Description |
|------|-------------|
| `--path` | Path to docker-compose.yml (auto-detected if omitted) |

The upgrade fetches fresh ECR credentials from the API, pulls the latest image, and restarts the container.

### Automatic Upgrades

When you run `s7 pentest create`, the CLI automatically checks all locally-installed Strikers for newer images in ECR. If a newer image is found, the Striker is upgraded before the pentest starts — no manual intervention needed.

- Runs silently in the background before each pentest
- Only recreates the container if the image actually changed
- Failures are non-blocking (printed as warnings with `-v`)
- Use `s7 striker upgrade` for manual control when needed

---

## Uninstall a Striker

Stop the container and remove local files:

```bash
s7 striker uninstall <id>
```

This stops the Docker container, removes the compose file, certificates, and the Striker directory at `~/.s7/strikers/<id>/`.

---

## Delete a Striker

Deregister a Striker from the platform (does not touch local files):

```bash
s7 striker delete <id>
```

!!! warning
    This removes the Striker from the platform. The Striker will no longer receive jobs. To also remove local files, run `s7 striker uninstall` first.

---

## Network Requirements

Strikers only require **outbound** connectivity:

| Destination | Port | Protocol | Purpose |
|-------------|------|----------|---------|
| Strike7 API | 443 | HTTPS | API communication |
| AWS ECR | 443 | HTTPS | Docker image pulls |
| Headscale VPN | 443 | HTTPS | VPN tunnel coordination |

**No inbound firewall rules are required.** All connections are initiated by the Striker.

---

## File Locations

| Path | Purpose |
|------|---------|
| `~/.s7/strikers/<id>/` | Striker installation directory |
| `~/.s7/strikers/<id>/docker-compose.yml` | Docker Compose configuration |
| `~/.s7/strikers/<id>/certs/` | mTLS certificates |

---

## Troubleshooting

### Striker shows "offline"

1. Check if the Docker container is running: `docker ps | grep striker`
2. Check container logs: `docker logs <container-name>`
3. Run diagnostics: `s7 diag preflight`
4. Verify network access: `s7 diag firewall`

### Install fails at "Pulling image"

- Ensure Docker is running: `docker info`
- Check internet connectivity
- The ECR credentials in the token may have expired (valid for 12 hours). Re-register to get a fresh token.

### Force reinstall

```bash
s7 striker install --token <TOKEN> --force
```
