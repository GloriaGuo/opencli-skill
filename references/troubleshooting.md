# OpenCLI Troubleshooting

## First Checks

```bash
command -v opencli
node -v
opencli doctor
opencli list -f yaml
```

Expected baseline:

- `opencli` is installed
- Node.js is `>= 20`
- Chrome is running for browser-backed commands
- The target site is already logged in inside Chrome

## Common Failures

| Symptom | Likely cause | Action |
| --- | --- | --- |
| `opencli: command not found` | Package not installed | Run `npm install -g @jackwener/opencli` |
| `Extension not connected` | Browser Bridge missing or disabled | Install or enable the OpenCLI Chrome extension, then rerun `opencli doctor` |
| `attach failed: Cannot access a chrome-extension:// URL` | Another Chrome extension is conflicting | Temporarily disable conflicting extensions and retry |
| Empty data or `Unauthorized` | Chrome login expired | Reopen the site in Chrome, log in again, refresh, then rerun the command |
| Node API errors such as `parseArgs` or `fs` issues | Unsupported Node version | Upgrade Node to version 20 or newer |
| Browser-backed command still fails after login | Browser bridge or daemon issue | Rerun `opencli doctor`, then inspect `curl localhost:19825/status` and `curl localhost:19825/logs` |

## Browser Command Checklist

1. Confirm the command really is browser-backed.
2. Confirm Chrome is open.
3. Confirm the site is open in Chrome and logged in.
4. Run `opencli doctor`.
5. Retry with `-v` if the failure is still unclear.

## Download Checklist

1. Verify the adapter supports download for that platform.
2. For video sites, install `yt-dlp`.
3. Use an explicit `--output` path.
4. If a browser-backed download fails, treat it as a login-state issue first.

## Escalation Rule

If setup and login both look correct but a specific adapter still fails repeatedly, stop guessing and inspect the current registry with `opencli list -f yaml` plus the adapter help output. Prefer confirming the exact command surface before editing scripts or inventing flags.
