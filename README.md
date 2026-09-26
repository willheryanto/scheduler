# Scheduler

## Scheduled token refresh (Codex + Claude)

This repo includes a GitHub Actions workflow that pings Codex and Claude at:
- 04:00 (GMT+7), targeting completion by 05:00

GitHub cron is UTC, so the workflow uses `0 21 * * *`.
The `hello` job pings Codex with `gpt-6-luna` at low reasoning effort, then runs `/usage`.
The `claude` job runs `claude -p --model haiku "ping"` and fails unless Claude replies.

### Required repo secret
- `CLAUDE_CODE_OAUTH_TOKEN`: the long-lived token printed by `claude setup-token` (valid ~1 year, independent of your local login; no rotation needed).
- `CODEX_AUTH_JSON`: the full contents of `~/.codex/auth.json`
- `CODEX_KR_AUTH_JSON`: the full contents of `~/.codex-kr/auth.json` (a separate Codex profile; see `codex-kr` pattern)
- `ANTIGRAVITY_ACCOUNTS_JSON`: the full contents of `~/.config/opencode/antigravity-accounts.json`

### Optional secret (only if needed)
- `GH_PAT`: a GitHub Personal Access Token used to update repo secrets if tokens rotate.
  - If your repo allows it, the workflow will try `github.token` first; otherwise set `GH_PAT`.

### Optional repo variable
- `CODEX_AUTH_PATH`: where the `codex` CLI reads/writes auth (default: `~/.codex/auth.json`).
- `CODEX_KR_HOME`: where the "KR" `codex` profile stores state (default: `~/.codex-kr`).

Workflow file: `.github/workflows/claude-scheduler.yml`
