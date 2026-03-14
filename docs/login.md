# Authentication

All S7 CLI commands require authentication. The CLI supports two methods: browser-based OAuth login and API key authentication.

## Login

### Browser Login (Recommended)

Opens your default browser for secure OAuth authentication:

```bash
s7 auth login
```

**Output:**

```
Opening browser for authentication...
Waiting for login... (or visit: https://game-redbird-99.clerk.accounts.dev/...)

Logged in successfully (org: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)
```

### API Key Login (Headless)

For servers, CI/CD pipelines, or machines without a browser:

```bash
s7 auth login --api-key sk_abc123...
```

**Output:**

```
Validating API key...
Logged in successfully (org: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)
```

!!! tip "API Key Generation"
    Generate an API key from the Strike7 dashboard under **Settings > API Keys**.

---

## Logout

Remove stored credentials:

```bash
s7 auth logout
```

**Output:**

```
Logged out.
```

---

## Status

Check your current authentication status:

```bash
s7 auth status
```

**Output:**

```
Logged in (org: xxxxxxxx-xxxx, type: browser, expires: 2026-04-14 10:30)
```

---

## Whoami

Alias for `s7 auth status`:

```bash
s7 auth whoami
```

---

## Credential Storage

Credentials are stored in two files under `~/.s7/`:

| File | Purpose |
|------|---------|
| `~/.s7/credentials.json` | Auth token and org ID |
| `~/.s7/config.yaml` | API URL and org settings |

Both files are created with `0600` permissions (readable only by the owner).

**credentials.json format:**

```json
{
  "token": "sk_... or JWT",
  "orgId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "expiresAt": "2026-04-14T10:30:00Z"
}
```

---

## Using API Keys

API keys are recommended for:

- CI/CD pipelines
- Automated scripts
- Headless servers and Strikers
- Service accounts

```bash
# Set API key and use the CLI
s7 auth login --api-key sk_abc123...
s7 pentest list
```

!!! warning "Security"
    Keep your API key secure. It provides full access to your organization's pentests and Strikers.
