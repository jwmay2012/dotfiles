# Repository Guidance

This repository is intended to be safe for a public remote.

- Keep it additive. Do not import the current home directory or unrelated
  historical configuration.
- Never add credentials, tokens, private keys, session files, employer-specific
  endpoints, host inventories, personal email addresses, or absolute home paths.
- Treat files here as staged configuration. Do not link, copy, install, or load
  them into the home directory, system settings, applications, or launchd
  unless the user explicitly authorizes activation.
- Keep `.tmux.conf` small and compatible with the checked-in dependency set.
- Validate tmux changes through a separately named disposable tmux server.
- `macos/caps-lock-control --check` and `--mapping` are read-only. Do not run
  `--apply` without explicit authorization.
- `codex/easement.toml` is a merge fragment, not a complete replacement for an
  existing Codex configuration.
