# ripeat

Set up a development machine from a list of tools, and undo it safely. Works on macOS, Linux and Windows.

This repository holds the downloads and checksums for each release. The source code is in a private repository.

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

When you are ready, `ripeat install git node` installs, and `ripeat rollback` undoes the last run.

## Programs installed from a download

Most tools install through your system's package manager. Some (like ripgrep) are downloaded and checked by ripeat, then kept in ripeat's own folder, so they do not change your system. ripeat never edits your PATH, so add the folder yourself to run them:

- macOS: `~/Library/Application Support/ripeat/bin`
- Linux: `~/.config/ripeat/bin`
- Windows: `%APPDATA%\ripeat\bin`

## Use it from an AI assistant

ripeat can run as an MCP server, so an assistant such as Claude or Codex can look things up, build a plan and rate its risk for you. It cannot install anything: you run the install yourself.

```bash
claude mcp add --scope user ripeat -- ripeat mcp
```

## Download by hand

Each release lists one archive per system (`darwin`, `linux`, `windows`, on `amd64` and `arm64`) and a `checksums.txt`. Check the download before you run it:

```bash
shasum -a 256 -c checksums.txt --ignore-missing      # macOS and Linux
```

```powershell
Get-FileHash .\ripeat_<version>_windows_amd64.zip -Algorithm SHA256   # compare with checksums.txt
```

Programs downloaded in a browser are not signed yet, so Windows SmartScreen or macOS Gatekeeper may warn about them. Installing with Homebrew or Scoop avoids that.
