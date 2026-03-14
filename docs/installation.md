# Installation

The S7 CLI (`s7`) is your primary interface for interacting with the Strike7 platform. It supports macOS and Linux on both Intel and ARM architectures.

## One-Line Install

```bash
curl -sL https://get.strike7.ai | bash
```

This automatically detects your platform and architecture, downloads the correct binary, verifies the SHA256 checksum, and installs to `/usr/local/bin/s7`.

### Install + Striker Setup

If you already have an install token from `s7 striker register`, you can install the CLI and set up a Striker in one step:

```bash
curl -sL https://get.strike7.ai | bash -s -- --token <TOKEN>
```

## Manual Installation

Download the binary for your platform:

| Platform | Architecture | Binary |
|----------|-------------|--------|
| Linux | x86-64 (amd64) | `s7_linux_amd64` |
| Linux | ARM64 | `s7_linux_arm64` |
| macOS | Intel (amd64) | `s7_darwin_amd64` |
| macOS | Apple Silicon (arm64) | `s7_darwin_arm64` |

Binaries are hosted on S3: `https://s7-cli-releases.s3.us-east-1.amazonaws.com/latest/s7_<os>_<arch>`

```bash
# Example: macOS Apple Silicon
curl -sL https://s7-cli-releases.s3.us-east-1.amazonaws.com/latest/s7_darwin_arm64 -o s7
chmod +x s7
sudo mv s7 /usr/local/bin/s7
```

### Checksum Verification

Each binary has a matching `.sha256` file:

```bash
curl -sL https://s7-cli-releases.s3.us-east-1.amazonaws.com/latest/s7_darwin_arm64.sha256
# Verify:
shasum -a 256 s7
```

## Verify Installation

```bash
s7 version
s7 --help
```

## Command Structure

```
s7 [command] [subcommand] [flags]
```

### Global Flags

| Flag | Description |
|------|-------------|
| `--org-id` | Organization ID (overrides config) |
| `--api-url` | API base URL (overrides config) |
| `--json` | Output as JSON |
| `-v, --verbose` | Verbose output |
| `-h, --help` | Help for any command |

### Available Commands

| Command | Description |
|---------|-------------|
| [`auth`](login.md) | Authentication (login, logout, status) |
| [`striker`](striker.md) | Manage on-prem Striker nodes |
| [`pentest`](pentest.md) | Create and manage pentests |
| [`scope`](scope.md) | Manage pentest scope and targets |
| [`config`](config.md) | CLI configuration |
| [`diag`](diag.md) | Run diagnostic checks |
| [`update`](update.md) | Update CLI to latest version |
| `version` | Print version and build info |

## Updating

```bash
# Check for updates
s7 update --check

# Update to latest version
s7 update
```

See [Update](update.md) for more details.

## Uninstalling

```bash
sudo rm /usr/local/bin/s7
rm -rf ~/.s7
```

## Next Steps

1. [Authenticate](login.md) with your Strike7 account
2. [Register a Striker](striker.md) on your network
3. [Create a pentest](pentest.md) and start testing
