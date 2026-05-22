# AI in a Shell

<p align="center">
  <img src="logo.svg" alt="ai — shell commands from AI" width="480">
</p>

<p align="center">
  <strong>Describe what you need. Get the command. Hit ↑ to run it.</strong>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="MIT License"></a>
  <img src="https://img.shields.io/badge/shell-bash%20%7C%20zsh-green" alt="bash | zsh">
  <img src="https://img.shields.io/badge/providers-OpenAI%20%7C%20Anthropic%20%7C%20Gemini%20%7C%20Custom-orange" alt="Providers">
</p>

---

<p align="center">
  <img src="demo.gif" alt="ai demo" width="640">
</p>

---

## What is this?

`ai` is a single-file shell script that turns plain English into shell commands. Ask a question, review the result, press Enter, and it's in your history — ready to run with ↑.

No dependencies beyond `curl`. No package managers. No runtimes. One file, drop it in your `$PATH`, done.

### Highlights

- 🔌 **Multi-provider** — OpenAI, Anthropic, Gemini, or any OpenAI-compatible endpoint
- 📂 **Context-aware** — automatically includes your cwd, git branch, and modified files
- ⚡ **`--run`** — skip confirmation and execute immediately
- 🔗 **`--silent`** — raw output for piping and composition
- ✏️ **`$EDITOR` integration** — multi-line results open in your editor for review
- 🎯 **History injection** — confirmed commands go straight into shell history

## Requirements

**bash** or **zsh** required. Does not run in PowerShell, CMD, or Fish.

On Windows, use [Git Bash](https://gitforwindows.org/), [WSL](https://learn.microsoft.com/en-us/windows/wsl/), [MSYS2](https://www.msys2.org/), or Cygwin.

## Install

```bash
mkdir -p ~/bin && curl -L https://raw.githubusercontent.com/Omodaka9375/ai-in-a-shell/main/ai -o ~/bin/ai && chmod 755 ~/bin/ai
```

Add the alias to your shell config (`~/.bashrc` or `~/.zshrc`):

```bash
alias ai='. ~/bin/ai'
```

## Quick Start

```bash
# 1. Configure your API key
ai --setup

# 2. Ask away
ai show me which process is running on port 8000
```

`--setup` creates `~/.ai` with a commented template and opens it in `$EDITOR`. Uncomment the key for your provider, save, and you're ready.

## Usage

```
ai [OPTIONS] QUERY
```

| Option | Description |
|-----------|----------------------------------------------|
| `--run` | Execute the command immediately (skip confirmation). |
| `--silent`| Output raw command only, for piping. Implies `--run`. |
| `--setup` | Create or edit the `~/.ai` config file. |
| `-h`      | Show help. |

```bash
# Standard — confirm before adding to history
ai rename all .txt files to .md

# Auto-run — execute without confirmation
ai --run list all docker containers

# Silent — compose with other commands
echo "My IP is: $(ai --silent get my public ip address)"
```

## Configuration

All options live in `~/.ai` or can be exported as environment variables:

```bash
AI_PROVIDER="anthropic"               # openai | anthropic | gemini | custom
AI_MODEL="claude-haiku-4-5"           # override the default model
AI_SHELL="zsh"                        # override detected shell
AI_OS="macOS"                         # override detected OS
AI_CONTEXT=1                          # include cwd & git context (default: 1)
AI_EDITOR_MODE=1                      # open multi-line results in $EDITOR (default: 1)
AI_TIMEOUT=10                         # curl timeout in seconds (default: 10)
```

### API Keys

The easiest way is `ai --setup`. Or edit `~/.ai` directly:

```bash
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
GEMINI_API_KEY=...

# Custom OpenAI-compatible endpoint:
AI_CUSTOM_API_URL="https://your-endpoint/v1/chat/completions"
AI_CUSTOM_API_KEY="your-key"
```

### Context Awareness

When `AI_CONTEXT=1` (default), the prompt automatically includes:
- Current working directory
- Git branch name (if in a repo)
- Modified and staged files (up to 10 each)

This helps the AI generate more relevant commands for your environment.

### Editor Integration

Multi-line results open in `$EDITOR` for review before use. Save and exit to accept, or discard to cancel. Disable with `AI_EDITOR_MODE=0`.

> **Tip:** For VS Code, set `EDITOR="code --wait"` so the script waits for you to close the file.

## Examples

Just prefix with `ai`:

**General**
- `which process is running on port 8000`
- `start a simple webserver using python3`
- `generate a random 32 character alphanumeric password`
- `numeric for loop boilerplate going from 5 to 15`

**Files & search**
- `rename every file in ./images from .jpg to .png`
- `find all files modified in the last 24 hours`
- `count lines of code in all .py files recursively`
- `find duplicate files by checksum in the current directory`
- `replace all occurrences of "foo" with "bar" across all .js files`
- `show the 10 largest files in this directory tree`

**Git**
- `show me the diff stats for the last 5 commits`
- `undo the last commit but keep the changes`
- `find all commits that touched config.yaml in the last month`
- `squash the last 3 commits into one`
- `list branches merged into main that can be deleted`

**Networking & system**
- `list all network interfaces and their ip addresses`
- `show all open TCP connections sorted by state`
- `test if ports 80 443 and 8080 are open on example.com`
- `show memory usage of the top 10 processes`
- `download a website recursively for offline reading`

**Docker & devops**
- `delete all unused docker images`
- `show logs from the last 30 minutes for the api container`
- `list all containers with their IP addresses`
- `remove all stopped containers and dangling volumes`
- `build and tag an image from the current directory as app:latest`

**Text & data**
- `extract all email addresses from access.log`
- `convert a csv file to json using jq`
- `show a sorted frequency count of HTTP status codes in access.log`
- `diff two json files ignoring key order`

**Media**
- `concatenate two .mkv video files`
- `use ffmpeg to shrink an mp4 file's size`
- `use imagemagick change the white background in screenshot.png to transparent`
- `resize all images in ./images to a maximum of 100 pixels in any dimension`


## ⚠️ Disclaimer

Always review AI-generated commands before running them. You are fully responsible for any commands you execute. This software comes with **absolutely no warranty**, to the extent permitted by applicable law.


## License

[MIT](LICENSE)
