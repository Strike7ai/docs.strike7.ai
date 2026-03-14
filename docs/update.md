# Updating the CLI

Keep your S7 CLI up to date to get the latest features and bug fixes.

## Check for Updates

```bash
s7 update --check
```

**Output (update available):**

```
Update available: 1.0.0 → 1.1.0
Run 's7 update' to install.
```

**Output (up to date):**

```
Up to date: 1.1.0
```

---

## Update to Latest

```bash
s7 update
```

**Output:**

```
Updated: 1.0.0 → 1.1.0
```

If the CLI is installed in `/usr/local/bin` and you don't have write permissions:

```bash
sudo s7 update
```

---

## Manual Update

If the update command fails or you have a very old version:

```bash
# Remove old version
sudo rm -f /usr/local/bin/s7

# Download and install latest
curl -sL https://get.strike7.ai | bash
```

---

## Version Information

```bash
s7 version
```

**Output:**

```
s7 version 1.1.0 (commit: abc1234, built: 2026-03-14)
```

---

## Changelog

View release notes at the [S7 CLI releases page](https://github.com/Strike7ai/s7-cc/releases).
