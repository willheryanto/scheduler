# Scheduler

## Claude scheduled hello (token refresh)

This repo includes a GitHub Actions workflow that runs `claude` on a schedule at:
- 06:03, 11:03, 16:03 (GMT+7)

GitHub cron is UTC, so the workflow uses `3 23,4,9 * * *`.
The workflow runs with `--model haiku` for faster execution.

### Required repo secret
- `CLAUDE_CREDENTIALS_JSON`: the full contents of `~/.claude/.credentials.json`, for example:
  - `{"claudeAiOauth":{"accessToken":"sk-ant-oat01-...","refreshToken":"sk-ant-ort01-...","expiresAt":1748658860401,"scopes":["user:inference","user:profile"]}}`

### Optional secret (only if needed)
- `GH_PAT`: a GitHub Personal Access Token used to update the `CLAUDE_CREDENTIALS_JSON` secret if the token rotates.
  - If your repo allows it, the workflow will try `github.token` first; otherwise set `GH_PAT`.

### Optional repo variable
- `CLAUDE_CREDENTIALS_PATH`: where the `claude` CLI reads/writes credentials (default: `~/.claude/.credentials.json`).

Workflow file: `.github/workflows/claude-scheduler.yml`
