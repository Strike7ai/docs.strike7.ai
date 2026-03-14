# Configuration

The S7 CLI stores configuration in `~/.s7/config.yaml`. You can manage it with the `s7 config` commands or by editing the file directly.

## Show Configuration

```bash
s7 config show
```

**Output:**

```
API URL:  https://internal.api.strike7.ai  (default)
Org ID:   xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  (config)
Config:   /Users/you/.s7/config.yaml
```

The source of each value is shown in parentheses: `default`, `config` (from config file), or `flag` (from command-line flag).

Add `--json` for JSON output.

---

## Set a Value

```bash
s7 config set <key> <value>
```

### Available Keys

| Key | Description | Example |
|-----|-------------|---------|
| `api_url` | API base URL | `https://internal.api.strike7.ai` |
| `org_id` | Organization ID | `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` |

### Examples

```bash
# Set a custom API URL
s7 config set api_url https://api.custom.example.com

# Set organization ID
s7 config set org_id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

**Output:**

```
Set api_url = https://api.custom.example.com
```

---

## Reset Configuration

Delete the config file and start fresh:

```bash
s7 config reset
```

You will be prompted for confirmation. Use `--force` to skip:

```bash
s7 config reset --force
```

**Output:**

```
Configuration reset.
```

---

## Config File Format

`~/.s7/config.yaml`:

```yaml
api_url: https://internal.api.strike7.ai
org_id: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

---

## Global Flags

These flags override config values for any command:

| Flag | Description |
|------|-------------|
| `--org-id` | Override organization ID |
| `--api-url` | Override API base URL |
| `--json` | Output as JSON |
| `-v, --verbose` | Verbose output |

**Precedence:** command-line flags > config file > defaults

### Examples

```bash
# Use a different org for one command
s7 pentest list --org-id other-org-id

# Point to a different API
s7 striker list --api-url https://staging.api.strike7.ai

# Get JSON output from any command
s7 striker list --json
```

---

## File Locations

| Path | Purpose |
|------|---------|
| `~/.s7/config.yaml` | CLI configuration |
| `~/.s7/credentials.json` | Authentication credentials |
| `~/.s7/strikers/` | Striker installations |
