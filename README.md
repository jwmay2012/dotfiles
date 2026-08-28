# dotfiles

Small, additive configuration for a terminal-first agent workflow on macOS.

This is deliberately not a copy of the current home directory. It contains
only new configuration we intend to adopt, and excludes identity, SSH, Git,
shell history, credentials, host inventories, and employer-specific settings.

Nothing in this repository installs itself.

## tmux

`.tmux.conf` keeps the useful part of the reference setup:

- Caps Lock can act as Control, making `Caps Lock` + `A` the tmux prefix;
- `prefix`, then `a`, sends `C-a` through to a nested tmux session;
- windows and splits inherit the active working directory;
- mouse, focus events, true color, useful scrollback, and stable names are on;
- no theme, plugin manager, shell replacement, workspace convention, or agent
  launcher is included.

The configuration does not require per-project directories. A single session
started from `~/code` is a valid default:

```zsh
tmux new-session -As work -c "$HOME/code"
```

Activation will be a separate, reviewed step. The intended link is:

```zsh
ln -s /path/to/dotfiles/.tmux.conf "$HOME/.tmux.conf"
```

## Caps Lock as Control

`macos/caps-lock-control` defaults to a read-only status check. Its explicit
`--apply` action uses macOS `hidutil` to map Caps Lock to left Control, but
refuses to replace any existing key map. It is not run automatically.

```zsh
./macos/caps-lock-control --check
./macos/caps-lock-control --apply
```

The `hidutil` mapping is useful for testing but macOS can lose it after a
restart or when the keyboard service is removed. The checked-in user
LaunchAgent reapplies it at login without embedding a home path. A keyboard
disconnected after login can be restored with the explicit `--apply` command.

## Easement and Shotgun

`codex/easement.toml` is a reviewable fragment, not an active Codex
configuration. It gives every Codex session one shared `code` browser context
at the default loopback Easement endpoint. This matches a global `~/code`
workflow and deliberately avoids deriving browser identity from directories or
tmux window names.

The dynamic discovery tools are allowed automatically. Actual delegated calls
remain prompts, providing a small accident-prevention boundary without treating
local agents as hostile tenants.

## Muster

The tmux `prefix`, then `C`, binding opens the persistent Codex popup supplied
by the `jwmay2012/muster` fork. A first invocation creates an unnamed Codex
thread at `~/code`; closing the popup does not stop it. Inside Codex, use a
unique lowercase kebab-case name whenever the work becomes clear:

```text
/rename browser-work
```

That exact name is then the Muster slug:

```zsh
muster codex popup --slug browser-work
muster codex tui --slug browser-work
print 'Review the queued message.' | muster codex nudge --slug browser-work
```

The popup's stable thread UUID remains associated with its outer tmux window,
so the no-argument binding continues to work before and after a rename. Codex
owns the name registry; changing `/rename` automatically retires the old Muster
address. All sessions work at `~/code` and share the Shotgun browser context
`code`.

The activation sequence will be:

1. build and start Easement on loopback;
2. load Shotgun unpacked in Brave and confirm it registers with Easement;
3. merge the reviewed Codex fragment into the existing configuration;
4. exercise DOM read, navigation, screenshot, JavaScript, and network capture;
5. keep the existing browser-debug launch available until that exercise passes.

Missive has no dotfile in this repository. Its client remains an independently
linked command with its own identity-bound session.

## Dependencies

`Brewfile` declares only the new terminal and Muster runtime layer. It is not
applied automatically.

```zsh
brew bundle check --file Brewfile
brew bundle install --file Brewfile
```
