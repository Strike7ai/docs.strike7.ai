# Authentication Commands

## login

Authenticate with your Strike7 account.

### Usage

```bash
s7-cli login [flags]
```

### Flags

| Flag | Description |
|------|-------------|
| `--email` | Email address (optional, will prompt if not provided) |
| `--password` | Password (optional, will prompt securely) |
| `--api-key` | API key for non-interactive login |

### Examples

```bash
# Interactive login (recommended)
s7-cli login

# Login with email (will prompt for password)
s7-cli login --email user@company.com

# Login with API key (for CI/CD)
s7-cli login --api-key sk_live_abc123xyz
```

### Output

```
Strike7 Login
Email: user@company.com
Password: ********

Success! Logged in as user@company.com
Organization: Acme Corp
```

---

## logout

Log out and clear stored credentials.

### Usage

```bash
s7-cli logout
```

### Output

```
Logged out successfully
```

---

## whoami

Display information about the currently authenticated user.

### Usage

```bash
s7-cli whoami
```

### Output

```
Logged in as: user@company.com
Organization: Acme Corp
Plan: Enterprise
API Key: sk_live_***xyz
```

---

## Configuration File

Credentials are stored in `~/.strike7/config.yaml`:

```yaml
api_url: http://129.212.229.32:8001
api_key: "sk_live_abc123..."
org_id: "org_xyz789"
email: "user@company.com"
```

**Security Note**: The config file is created with `0600` permissions (readable only by the owner).

## Using API Keys

API keys are recommended for:
- CI/CD pipelines
- Automated scripts
- Service accounts

Generate an API key from the Strike7 dashboard:
1. Go to Settings → API Keys
2. Click "Generate New Key"
3. Copy the key (it won't be shown again)

```bash
# Use in scripts
export S7_API_KEY=sk_live_abc123xyz
s7-cli pentest list
```
