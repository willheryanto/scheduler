# Scheduler

## Scheduled token refresh (Codex + OpenCode; Claude optional)

This repo includes a GitHub Actions workflow that starts a Codex token refresh at:
- 04:00 (GMT+7), targeting completion by 05:00

GitHub cron is UTC, so the workflow uses `0 21 * * *`.
The Codex step pings with `gpt-5.6-luna` at low reasoning effort, then runs `/usage`.
The (optional) `claude` section runs with `--model haiku` for faster execution.

### Required repo secret
- `CLAUDE_CREDENTIALS_JSON`: the full contents of `~/.claude/.credentials.json` (only needed if enabling the `claude` steps), for example:
  - `{"claudeAiOauth":{"accessToken":"sk-ant-oat01-...","refreshToken":"sk-ant-ort01-...","expiresAt":1748658860401,"scopes":["user:inference","user:profile"]}}`
- `CODEX_AUTH_JSON`: the full contents of `~/.codex/auth.json`
- `CODEX_KR_AUTH_JSON`: the full contents of `~/.codex-kr/auth.json` (a separate Codex profile; see `codex-kr` pattern)
- `ANTIGRAVITY_ACCOUNTS_JSON`: the full contents of `~/.config/opencode/antigravity-accounts.json`

### Optional secret (only if needed)
- `GH_PAT`: a GitHub Personal Access Token used to update repo secrets if tokens rotate.
  - If your repo allows it, the workflow will try `github.token` first; otherwise set `GH_PAT`.

### Optional repo variable
- `CLAUDE_CREDENTIALS_PATH`: where the `claude` CLI reads/writes credentials (default: `~/.claude/.credentials.json`).
- `CODEX_AUTH_PATH`: where the `codex` CLI reads/writes auth (default: `~/.codex/auth.json`).
- `CODEX_KR_HOME`: where the "KR" `codex` profile stores state (default: `~/.codex-kr`).

Workflow file: `.github/workflows/claude-scheduler.yml`
