# Local Agent Revival Notes

_Last updated: 2026-05-14 10:07:29 CST_

This file records the stable local memory and operating workflow needed to redeploy this modified `myhermes` Hermes Agent instance on another server. It intentionally avoids API keys, tokens, cookies, and other secrets.

## User profile and preferences

- The user prefers to be called **锋哥**.
- Communication can be direct and action-oriented; when a task is clear, proceed instead of asking unnecessary confirmation.
- The primary stock/market workflow focuses on **US equities first**, then **Hong Kong equities**.
- Market-risk preference is **neutral**.
- Paper-trading capital baseline is **40,000 RMB**.
- The stock-tool project path on the Mac workstation is:
  `/Users/chenchen/Documents/Codex/2026-05-11/github-myhermes/directional-ai-executor`
- X/Twitter research may reference AI/stock-related accounts such as `dexeteryy`.

## Local environment facts

- This checkout is a modified Hermes Agent codebase, run directly from the `myhermes` repository/command line rather than only through the stock installed CLI.
- Main local workspace root on the Mac workstation:
  `/Users/chenchen/Documents/Codex/2026-05-11/github-myhermes`
- Active project path for this repo:
  `/Users/chenchen/Documents/Codex/2026-05-11/github-myhermes/myhermes`
- Hermes home / gateway state path used by this setup:
  `/Users/chenchen/Documents/Codex/2026-05-11/github-myhermes/hermes-home`
- Host observed in this session: macOS 15.7.5.
- Feishu/Lark is connected as a gateway platform; the current home chat is the Feishu DM with 陈庆锋.

## Operational workflow

### General agent behavior

1. Load relevant skills before acting when a skill exists for the task.
2. Prefer tool-backed facts over memory for current state: time, OS, git status, files, processes, versions, and remote state should be checked live.
3. Do not hallucinate missing details. If information can be retrieved from files, git, logs, sessions, or tools, retrieve it.
4. Protect user work in the repo: check `git status` before editing, avoid touching unrelated modified files, and stage only files changed for the current task.
5. For repo changes, verify with an appropriate lightweight check, inspect `git diff`, commit with a conventional commit message, then push.

### Hermes-specific workflow

- When configuring, setting up, modifying, troubleshooting, or documenting Hermes Agent itself, load the `hermes-agent` skill first.
- For gateway/Feishu issues on macOS, inspect logs before prescribing a fix:
  - Hermes home logs under the configured Hermes home, especially `logs/gateway.log`, `logs/agent.log`, and `logs/errors.log`.
  - Distinguish transient Feishu websocket reconnects from actual gateway process exits.
- On macOS, long-running gateway or market-monitor processes may be interrupted by system sleep/darkwake. Use `caffeinate -dimsu` around long-lived runs when appropriate.
- Prefer profile-aware Hermes paths and code helpers such as `get_hermes_home()` instead of hardcoding `~/.hermes` in source changes.
- Toolset/skill/config changes generally require a fresh Hermes session or gateway restart to take effect.

### GitHub workflow

1. Check auth and remotes (`git status`, `git branch --show-current`, `git remote -v`).
2. Preserve unrelated local changes.
3. Commit only the intended files.
4. Push the current branch to `origin`.
5. If creating PRs or monitoring CI, use `gh` when authenticated; otherwise fall back to git/curl with a GitHub token from the environment or credential store.

## Deployment/revival checklist for another server

1. Clone this repository from GitHub.
2. Install Hermes Agent dependencies following the repo docs and current `AGENTS.md`.
3. Restore or recreate configuration under the target Hermes home. Do **not** commit secrets; provide API keys and platform credentials through the server's private config/environment.
4. Configure the desired model/provider and toolsets.
5. Configure Feishu/Lark gateway credentials on the server if the new deployment should receive Feishu messages.
6. Restore non-secret local state if desired:
   - Cron definitions: `docs/revival/cron-jobs.json` and human-readable `docs/revival/cron-jobs.md`.
   - Paper-trading state snapshot: `docs/revival/stock_ai_system/`.
   - Copy the snapshot into the new Hermes home as `stock_ai_system/` before recreating the market cron jobs if continuing the same experiment.
7. Start the gateway under a durable supervisor for the target OS:
   - macOS: prefer LaunchAgent or a `caffeinate -dimsu`-protected long-running process.
   - Linux server: prefer systemd/user service or another supervisor that survives SSH logout.
8. Verify with:
   - `hermes doctor` or the equivalent project command.
   - `hermes status` / gateway status.
   - `hermes cron list` after recreating cron jobs.
   - A test Feishu DM round trip if Feishu is enabled.
9. After startup, check logs for provider, tool, memory, and gateway errors.

## Safety boundaries

- Do not store API keys, auth tokens, Feishu secrets, GitHub tokens, cookies, or private key material in this file or in git.
- If a deployment needs secrets, copy them through an out-of-band secret manager or server-local `.env`/config file excluded from git.
- Treat this document as a memory/workflow bootstrap only, not as a credential backup.
