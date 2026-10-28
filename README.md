# dev-tool-caffeinate

Shell aliases around macOS `caffeinate` so your Mac stays awake while a long build, download, or
remote session runs. All aliases are prefixed `coffee_` and live in a single file, [`coffee.sh`](coffee.sh).

## Aliases

| Alias | Runs | What it does |
|---|---|---|
| `coffee_one_hour` | `caffeinate -u -t 3600 &` | Keeps the system awake for 1 hour, in the background. |
| `coffee_no_idle_sleep` | `caffeinate -i` | Prevents idle sleep until you press `Ctrl-C`. |
| `coffee_no_sleep` | `caffeinate -d` | Prevents the display from sleeping until you press `Ctrl-C`. |
| `coffee_awake` | `caffeinate` | Prevents idle sleep (default assertion) until you press `Ctrl-C`. |
| `coffee_iv_drip` | `caffeinate -disu &` | Display, idle, system and user-activity assertions, in the background, until killed. |
| `coffee_processes` | `ps aux \| grep caffeinate` | Lists running `caffeinate` processes via `ps`. |
| `coffee_pgrep` | `pgrep caffeinate` | Lists running `caffeinate` PIDs via `pgrep`. |
| `coffee_crash` | `pkill caffeinate` | Terminates every running `caffeinate` process. |
| `coffee_type` | `pmset -g assertions` | Shows the current power-management assertions. |

Aliases ending in `&` return immediately and keep running in the background. Stop them with
`coffee_crash`.

## Install

The aliases are sourced from `~/.zshrc`. The recommended location for the file is
`/Users/Shared/LDE/cli-aliases/coffee.sh` so it is shared across users on the machine, but any
path works.

```sh
mkdir -p /Users/Shared/LDE/cli-aliases
curl -fsSL https://raw.githubusercontent.com/carlosmarte/dev-tool-caffeinate/main/coffee.sh \
  -o /Users/Shared/LDE/cli-aliases/coffee.sh

# add once to ~/.zshrc
cat >> ~/.zshrc <<'ZSH'

# dev-tool-caffeinate aliases
[ -f /Users/Shared/LDE/cli-aliases/coffee.sh ] && source /Users/Shared/LDE/cli-aliases/coffee.sh
ZSH

source ~/.zshrc
```

Or, if you have this repo cloned, source it straight from the checkout:

```sh
echo 'source /path/to/dev-tool-caffeinate/coffee.sh' >> ~/.zshrc
```

### Installing with an AI agent

[`AGENTS.md`](AGENTS.md) contains step-by-step instructions for a coding agent (Claude Code,
Codex, Cursor, etc.) to install `coffee.sh` into your `~/.zshrc`. The agent defaults to the
shared path above and asks for confirmation before downloading or editing anything.

## Update

Re-run the `curl` command above to overwrite the file with the latest version, then
`source ~/.zshrc` or open a new shell.

## Uninstall

Remove the `source` line from `~/.zshrc` and delete the file:

```sh
rm /Users/Shared/LDE/cli-aliases/coffee.sh
```

## Verify

```sh
alias | grep coffee_
coffee_one_hour
coffee_processes   # should show one caffeinate process
coffee_crash
```

## License

[MIT](LICENSE)
