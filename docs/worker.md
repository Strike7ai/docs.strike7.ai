# Worker Commands

Manage hybrid workers for internal network penetration testing.

## Overview

Workers are Docker containers deployed on customer networks to perform internal network pentesting. They communicate securely with Strike7 SaaS via outbound HTTPS/WebSocket connections.

## worker register

Register a new worker with Strike7.

### Usage

```bash
sudo s7-cli worker register [flags]
```

**Note**: Requires sudo to write configuration to `/etc/strike7/`.

### Flags

| Flag | Description | Required |
|------|-------------|----------|
| `--name, -n` | Worker name | Yes |
| `--type, -t` | Worker type (default: `network_pentest`) | No |
| `--pool-id` | Worker pool ID | No |

### Examples

```bash
# Register a worker
sudo s7-cli worker register --name my-datacenter-worker

# Register with specific pool
sudo s7-cli worker register --name aws-vpc-worker --pool-id pool_abc123
```

### Output

```
Registering worker 'my-datacenter-worker'...

Success! Worker registered!

  Worker ID:  wrk_abc123
  Name:       my-datacenter-worker
  API URL:    http://129.212.229.32:8001
  Expires:    2026-02-25 10:30:00

Next steps:
  1. Install the worker: s7-cli worker install
  2. Start the worker:   s7-cli worker start
```

### Configuration

Worker credentials are saved to:
- `/etc/strike7/config.yaml` - Worker authentication
- `~/.strike7/config.yaml` - User copy

---

## worker install

Install the Strike7 worker Docker container.

### Usage

```bash
sudo s7-cli worker install
```

### Prerequisites

- Docker must be installed
- Worker must be registered first

### What it does

1. Checks Docker installation
2. Verifies worker registration
3. Creates directories (`/etc/strike7`, `/var/log/strike7`, `/var/lib/strike7/outputs`)
4. Creates `docker-compose.yml` configuration
5. Pulls Docker image (`strike7/network-worker:latest`)
6. Creates systemd service (Linux)

### Output

```
========================================
  Strike7 Network Worker Installation
========================================

Checking Docker... OK
  Docker version 24.0.7, build afdd53b
Checking Docker daemon... OK
Checking worker registration... OK
  Worker: my-datacenter-worker (wrk_abc123)
Creating directories... OK
Creating Docker Compose config... OK

Pulling Strike7 Network Worker image...
Image: strike7/network-worker:latest

latest: Pulling from strike7/network-worker
...
Status: Downloaded newer image

Creating systemd service... OK

========================================
  Installation Complete!
========================================

Next steps:
  Start the worker:    s7-cli worker start
  Check status:        s7-cli worker status
  View logs:           s7-cli worker logs --follow

Or use systemctl:
  sudo systemctl start strike7-worker
  sudo journalctl -u strike7-worker -f
```

---

## worker start

Start the worker container.

### Usage

```bash
sudo s7-cli worker start
```

### Output

```
Starting Strike7 Network Worker...

Success! Worker started!

View logs with: s7-cli worker logs --follow
```

---

## worker stop

Stop the worker container.

### Usage

```bash
sudo s7-cli worker stop
```

### Output

```
Stopping Strike7 Network Worker...
Success! Worker stopped
```

---

## worker status

Show worker status.

### Usage

```bash
s7-cli worker status
```

### Output

```
Worker: my-datacenter-worker (wrk_abc123)
Status: ONLINE
Container: running
Active Pentests: 1
Completed: 15
Last Heartbeat: 8s ago
Version: 1.0.0
```

---

## worker list

List all workers in your organization.

### Usage

```bash
s7-cli worker list
```

### Output

```
NAME                  STATUS    TYPE            ACTIVE    COMPLETED
my-datacenter-worker  online    network_pentest 1         15
aws-vpc-worker        offline   network_pentest 0         8
azure-worker          online    network_pentest 2         23
```

---

## worker logs

View worker container logs.

### Usage

```bash
s7-cli worker logs [flags]
```

### Flags

| Flag | Description |
|------|-------------|
| `-f, --follow` | Follow log output |
| `--tail` | Number of lines to show (default: 100) |

### Examples

```bash
# View recent logs
s7-cli worker logs

# Follow logs in real-time
s7-cli worker logs --follow

# Show last 500 lines
s7-cli worker logs --tail 500
```

---

## worker unregister

Revoke and unregister a worker.

### Usage

```bash
sudo s7-cli worker unregister --confirm
```

### What it does

1. Revokes worker token on server
2. Removes local configuration
3. Worker container stops receiving jobs

### Output

```
This will revoke worker 'my-datacenter-worker' and remove local configuration.

Success! Worker unregistered
```

---

## Docker Compose Configuration

The worker creates `/etc/strike7/docker-compose.yml`:

```yaml
version: '3.8'
services:
  strike7-worker:
    image: strike7/network-worker:latest
    container_name: strike7-network-worker
    restart: unless-stopped
    network_mode: host
    cap_add:
      - NET_RAW
      - NET_ADMIN
    environment:
      - S7_API_URL=http://129.212.229.32:8001
      - S7_WORKER_ID=wrk_abc123
      - S7_LOG_LEVEL=info
    volumes:
      - /etc/strike7:/etc/strike7:ro
      - /var/log/strike7:/var/log/strike7
      - /var/lib/strike7/outputs:/app/outputs
    deploy:
      resources:
        limits:
          memory: 8G
          cpus: '4.0'
```

## Systemd Service

On Linux, a systemd service is created at `/etc/systemd/system/strike7-worker.service`:

```bash
# Start worker
sudo systemctl start strike7-worker

# Stop worker
sudo systemctl stop strike7-worker

# View logs
sudo journalctl -u strike7-worker -f

# Enable on boot
sudo systemctl enable strike7-worker
```

## Network Requirements

Workers only need **outbound** connectivity:

| Destination | Port | Protocol | Purpose |
|-------------|------|----------|---------|
| api.strike7.ai | 443 | HTTPS | API communication |
| api.strike7.ai | 443 | WSS | WebSocket for job control |

**No inbound firewall rules required.**
