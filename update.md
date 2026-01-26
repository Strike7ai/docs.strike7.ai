# Update Command

Keep your CLI up to date with the latest features and fixes.

## update

Update s7-cli to the latest version.

### Usage

```bash
s7-cli update [flags]
```

### Flags

| Flag | Description |
|------|-------------|
| `--check` | Only check for updates, don't install |

### Examples

```bash
# Check for updates
s7-cli update --check

# Update to latest version
s7-cli update

# Update with sudo (if installed in /usr/local/bin)
sudo s7-cli update
```

### Output

```
Current version: 1.0.0
OS/Arch: darwin/arm64

Checking for updates... latest: 1.0.1

New version available: 1.0.0 -> 1.0.1

Downloading s7-cli-darwin-arm64...
Downloaded: 11.45 MB
Verifying new binary... OK
  s7-cli version 1.0.1
Installing update... OK

Success! Updated successfully!
  1.0.0 -> s7-cli version 1.0.1
```

### Already Up to Date

```
Current version: 1.0.1
OS/Arch: darwin/arm64

Checking for updates... latest: 1.0.1

Up to date! You're already on the latest version!
```

---

## Manual Update

If the update command fails or you have an older version without it:

### macOS / Linux

```bash
# Remove old version
sudo rm -f /usr/local/bin/s7-cli

# Download and install new version
curl -fsSL http://129.212.229.32:8001/downloads/install.sh | bash
```

### Windows (PowerShell)

```powershell
# Remove old version
Remove-Item $env:USERPROFILE\s7-cli.exe -ErrorAction SilentlyContinue

# Download new version
Invoke-WebRequest -Uri "http://129.212.229.32:8001/downloads/cli/s7-cli-windows-amd64.exe" -OutFile "$env:USERPROFILE\s7-cli.exe"
```

---

## Version Information

```bash
# Check current version
s7-cli version

# Output
s7-cli version 1.0.1
```

---

## Changelog

View release notes at: https://github.com/strike7ai/s7-cli/releases
