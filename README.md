# ripeat

Set up a development machine from a list of tools, and undo it safely. Works on macOS, Linux and Windows.

This repository holds the downloads and checksums for each release. The source code is in a private repository.

**Documentation: https://ripeat.github.io** (getting started, every command, how risk is rated, and how to connect an AI assistant).

## Install

**macOS and Linux** (Homebrew):

```bash
brew install ripeat/tap/ripeat
```

**Windows** (Scoop; it needs `git` to add a bucket):

```powershell
scoop install git
scoop bucket add ripeat https://github.com/ripeat/scoop-bucket
scoop install ripeat
```

Check it worked:

```bash
ripeat version
```

## Update

```bash
brew update && brew upgrade ripeat/tap/ripeat      # macOS and Linux
```

```powershell
scoop update ripeat                                # Windows
```

## First steps

These commands only look. They change nothing:

```bash
ripeat recipes                  # the tools ripeat knows how to install
ripeat plan git node ripgrep    # what would happen, step by step
ripeat assess git node ripgrep  # how risky it is, and why
ripeat find lazygit             # where a tool you want could come from
```

When you are ready, `ripeat install git node` shows the plan and asks before it changes anything (`--yes` skips the question), and `ripeat rollback` undoes the last run. A tool ripeat has no recipe for is looked up and offered to you to add; ripeat shows the recipe and saves it only if you agree.

## Keeping it current

```bash
ripeat version --check          # is a newer release out, and how to upgrade
ripeat recipes check            # do the recipes still point at packages that exist
```

Upgrade with `brew update && brew upgrade ripeat/tap/ripeat` (macOS, Linux) or `scoop update ripeat` (Windows).

## Share a setup

```bash
ripeat profile create web git node ripgrep   # write a new list in your editor (typing just `ripeat` also asks)
ripeat profile save web ripeat.yaml            # or keep a list you wrote in a file under a name
ripeat profile export web web.yaml             # write it to a file to share (tool names only, no commands)
ripeat profile import web.yaml                 # someone else saves it (a file or an https:// address)
```

A shared list only names tools; importing it runs nothing. Check it with `ripeat assess --profile web` before installing.

## Recipe packs

A team or a community can share recipes as a GitHub repository with a `recipes/` folder:

```bash
ripeat tap add owner/repo      # shows what is in it, asks, and pins it to that commit
ripeat tap add https://host/pack.yaml   # a pack on any host, pinned by its SHA-256
ripeat tap update              # shows exactly what changed since, and asks
```

A pack cannot replace a built-in recipe or one of your own, and a recipe that runs shell commands is refused unless you allow it.

## Programs installed from a download

Most tools install through your system's package manager. Some (like ripgrep) are downloaded and checked by ripeat, then kept in ripeat's own folder, so they do not change your system. ripeat never edits your PATH, so add the folder yourself to run them:

- macOS: `~/Library/Application Support/ripeat/bin`
- Linux: `~/.config/ripeat/bin`
- Windows: `%APPDATA%\ripeat\bin`

## Use it from an AI assistant

ripeat can run as an MCP server, so an assistant such as Claude or Codex can look things up, build a plan and rate its risk for you. By default it cannot install anything: you run the install yourself. For a tool with no recipe it can ask ripeat to prepare one as a draft; the draft does nothing until you read it and run `ripeat drafts approve <name>` in a terminal.

```bash
ripeat mcp setup --client claude-code      # or claude-desktop, or codex: shows the change, keeps a backup, asks first
```

If you want the assistant to be able to install too, add `--allow-run` to that command. Nothing runs on the assistant's word: your own app shows a confirmation for every run, and ripeat refuses a high-risk plan and any step that needs administrator rights.

## Download by hand

Each release lists one archive per system (`darwin`, `linux`, `windows`, on `amd64` and `arm64`) and a `checksums.txt`. Check the download before you run it:

```bash
shasum -a 256 -c checksums.txt --ignore-missing      # macOS and Linux
```

```powershell
Get-FileHash .\ripeat_<version>_windows_amd64.zip -Algorithm SHA256   # compare with checksums.txt
```

Programs downloaded in a browser are not signed (ripeat does not do code signing), so Windows SmartScreen or macOS Gatekeeper may warn about them. Installing with Homebrew or Scoop avoids that.
