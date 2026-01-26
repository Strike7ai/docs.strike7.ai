# CLI Overview

The Strike7 CLI (`s7-cli`) is your primary interface for interacting with the Strike7 platform.

## Installation

### One-Line Install (macOS/Linux)

```bash
curl -fsSL http://129.212.229.32:8001/downloads/install.sh | bash
```

### Manual Installation

Download the appropriate binary for your system:

| Platform | Architecture | Download |
|----------|--------------|----------|
| Linux | x64 | [s7-cli-linux-amd64](http://129.212.229.32:8001/downloads/cli/s7-cli-linux-amd64) |
| Linux | ARM64 | [s7-cli-linux-arm64](http://129.212.229.32:8001/downloads/cli/s7-cli-linux-arm64) |
| macOS | Intel | [s7-cli-darwin-amd64](http://129.212.229.32:8001/downloads/cli/s7-cli-darwin-amd64) |
| macOS | Apple Silicon | [s7-cli-darwin-arm64](http://129.212.229.32:8001/downloads/cli/s7-cli-darwin-arm64) |
| Windows | x64 | [s7-cli-windows-amd64.exe](http://129.212.229.32:8001/downloads/cli/s7-cli-windows-amd64.exe) |

Then move to your PATH:

```bash
chmod +x s7-cli-*
sudo mv s7-cli-* /usr/local/bin/s7-cli
```

## Updating

The CLI can update itself:

```bash
# Check for updates
s7-cli update --check

# Update to latest version
s7-cli update

# Or with sudo if needed
sudo s7-cli update
```

### Reinstalling (for users without update command)

```bash
sudo rm -f /usr/local/bin/s7-cli
curl -fsSL http://129.212.229.32:8001/downloads/install.sh | bash
```

## Command Structure

```
s7-cli [command] [subcommand] [flags]
```

## Global Flags

| Flag | Description |
|------|-------------|
| `--config` | Config file path (default: `~/.strike7/config.yaml`) |
| `--api-url` | Strike7 API URL |
| `-v, --verbose` | Verbose output |
| `-h, --help` | Help for any command |

## Available Commands

| Command | Description |
|---------|-------------|
| [login](login.md) | Authenticate with Strike7 |
| [logout](login.md#logout) | Log out of Strike7 |
| [whoami](login.md#whoami) | Show current user info |
| [pentest](pentest.md) | Manage penetration tests |
| [worker](worker.md) | Manage hybrid workers |
| [diag](diag.md) | Run diagnostics |
| [update](update.md) | Update CLI to latest version |
| [version](version.md) | Print version number |

## Configuration

The CLI stores configuration in `~/.strike7/config.yaml`:

```yaml
api_url: http://129.212.229.32:8001
api_key: "sk_live_..."
org_id: "org_..."
email: "user@company.com"
output_format: table
color_enabled: true
```

### Environment Variables

You can also use environment variables (prefixed with `S7_`):

```bash
export S7_API_URL=http://129.212.229.32:8001
export S7_API_KEY=sk_live_...
```

## Examples

```bash
# Login and create a network pentest
s7-cli login
s7-cli pentest create --name "Q1 Assessment" --target 10.0.0.0/16

# Register and start a worker
sudo s7-cli worker register --name my-worker
sudo s7-cli worker install
sudo s7-cli worker start

# Monitor a running pentest
s7-cli pentest watch npt_abc123

# Download findings
s7-cli pentest findings npt_abc123 --severity critical,high
s7-cli pentest report npt_abc123 --format pdf -o report.pdf
```
