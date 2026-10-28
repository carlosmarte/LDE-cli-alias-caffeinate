# AGENTS.md

Instructions for AI coding agents working in or with this repository.

## What this repo is

A single shell file, `coffee.sh`, that defines `coffee_*` aliases around macOS `caffeinate`.
There is no build, no tests, and no dependencies. See `README.md` for what each alias does.

## Task: install `coffee.sh` into the user's `~/.zshrc`

When a user asks you to install, set up, or enable the caffeinate / coffee aliases, follow this
procedure. **Do not download, write, or edit anything until the user has confirmed the plan in
step 2.**

### 1. Decide the install path

- **Default:** `/Users/Shared/LDE/cli-aliases/coffee.sh`
- Use the default unless the user names a different location.
- The default directory may not exist yet. Plan to create it with `mkdir -p`.

### 2. Ask and confirm before doing anything

Present the plan to the user in one short message and wait for an explicit yes. The plan must
state:

1. The source: `https://raw.githubusercontent.com/carlosmarte/dev-tool-caffeinate/main/coffee.sh`
   (or the local checkout path if the repo is already cloned on this machine).
2. The destination file (the default path above, or the user's override).
3. That `~/.zshrc` will be appended with a `source` line guarded by a file-exists check.
4. Whether the destination file already exists and will be overwritten.
5. Whether `~/.zshrc` already contains a `coffee.sh` source line (if so, you will not add a
   duplicate).

Offer the user the chance to change the destination path. If the user declines or does not
answer, stop. Do not proceed on an assumed yes.

### 3. Pull the file

Only after confirmation:

```sh
mkdir -p "$(dirname "$DEST")"
curl -fsSL https://raw.githubusercontent.com/carlosmarte/dev-tool-caffeinate/main/coffee.sh -o "$DEST"
```

If the repo is already cloned locally, copying `coffee.sh` from the checkout is acceptable
instead of `curl`. If `curl` fails, report the error and stop. Do not fall back to writing the
aliases from memory.

Sanity-check the download before installing it: the file should be plain text containing only
`alias coffee_...` lines and comments. If it contains anything else, stop and show it to the
user.

### 4. Wire it into `~/.zshrc`

First check for an existing line:

```sh
grep -n 'coffee.sh' ~/.zshrc
```

If a line already sources the chosen destination, skip this step. If a line sources a
different path, tell the user and ask whether to replace it. Otherwise append:

```sh
cat >> ~/.zshrc <<'ZSH'

# dev-tool-caffeinate aliases
[ -f /Users/Shared/LDE/cli-aliases/coffee.sh ] && source /Users/Shared/LDE/cli-aliases/coffee.sh
ZSH
```

Substitute the real destination if the user chose a different path. Append only. Do not rewrite
or reorder other content in `~/.zshrc`, and do not touch `~/.zprofile`, `~/.bashrc`, or any other
shell file unless the user asks.

### 5. Verify

Run in a fresh zsh so the user's current shell is not affected:

```sh
zsh -ic 'alias | grep -c coffee_'
```

Expect `9`. Then tell the user to run `source ~/.zshrc` or open a new terminal, and show them
`coffee_one_hour` and `coffee_crash` as the two aliases to try first.

### Summary of guard rails

- Default destination: `/Users/Shared/LDE/cli-aliases/coffee.sh`.
- Always ask and confirm before pulling the file and before editing `~/.zshrc`.
- Never overwrite an existing destination file or `~/.zshrc` line without saying so first.
- Append to `~/.zshrc`; never rewrite it.
- macOS only. `caffeinate` does not exist on Linux; say so and stop if the host is not macOS.

## Task: update an existing install

Re-run step 3 to the same destination (confirm first, since it overwrites). `~/.zshrc` does not
need to change.

## Task: uninstall

Confirm first, then remove the `source` block from `~/.zshrc` and delete the destination file.

## Editing `coffee.sh`

- Keep every alias prefixed `coffee_` and on one line with a trailing `#` comment.
- Keep the README alias table in sync with the file.
- Do not add dependencies beyond what ships with macOS.
