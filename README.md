# AI in a Shell

<p align="center">
  <img src="logo.svg" alt="ai — shell commands from AI" width="480">
</p>


`ai` is a single-file shell script for getting one-liner solutions from AI right into your terminal where you need them.
It asks an LLM for a one-liner solution to your problem, shows it to you, and inserts it into your history if you confirm.
Once it's in your history you can press "up" and then run or edit the one-liner as you like.

**Features:** animated spinner, directory & git context awareness, `--run` / `--silent` flags, multi-line `$EDITOR` integration, and support for OpenAI, Anthropic, Gemini, and custom endpoints.

## Requirements

`ai` requires **bash** or **zsh**. It does not run in PowerShell, CMD, or Fish.

On Windows, use one of these bash-compatible environments:
- [Git Bash](https://gitforwindows.org/) (included with Git for Windows)
- [WSL](https://learn.microsoft.com/en-us/windows/wsl/) (Windows Subsystem for Linux)
- [MSYS2](https://www.msys2.org/) or Cygwin

## Install

```bash
curl https://raw.githubusercontent.com/Omodaka9375/ai-in-a-shell/main/ai > ~/bin/ai && chmod 755 ~/bin/ai

# or create bin folder if you haven't already

mkdir -p ~/bin && curl -L https://raw.githubusercontent.com/Omodaka9375/ai-in-a-shell/main/ai -o ~/bin/ai && chmod 755 ~/bin/ai
```

```bash
# Add this to your ~/.bashrc or ~/.zshrc to use it as 'ai':
alias ai='. ~/bin/ai'
```

## Quick Start

Run `--setup` to create a config file and add your API key:

```bash
ai --setup
```

This creates `~/.ai` with a commented template and opens it in your `$EDITOR`. Uncomment the API key for your provider, save, and you're ready:

```bash
ai show me which process is running on port 8000
```

## Usage

```
ai [OPTIONS] QUERY
```

| Option | Description |
|-----------|--------------------------------------------|
| `--run` | Execute the command immediately (skip confirmation). |
| `--silent`| Output raw command only, for piping. Implies `--run`. |
| `--setup` | Create or edit the `~/.ai` config file. |
| `-h`      | Show help. |

```bash
# Standard — confirm before adding to history
ai rename all .txt files to .md

# Auto-run — execute without confirmation
ai --run list all docker containers

# Silent — pipe the raw command to another tool
ai --silent find files larger than 100M | xargs du -sh
```

### Providers
By default, `ai` uses `anthropic`. You can switch providers by setting `AI_PROVIDER`.

```bash
# Supported providers: openai, anthropic, gemini, custom
AI_PROVIDER="anthropic"
```

### API Keys

The easiest way to configure keys is `. ai --setup`. Or manually edit `~/.ai`:

```bash
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
GEMINI_API_KEY=...

# For custom OpenAI-compatible endpoints:
AI_CUSTOM_API_URL="https://your-endpoint/v1/chat/completions"
AI_CUSTOM_API_KEY="your-key"
```

### Configuration

All options can be set in `~/.ai` or exported as environment variables:

```bash
# Target shell (default: auto-detect)
AI_SHELL="zsh"

# Target OS (default: auto-detect)
AI_OS="macOS"

# Model override (default: per-provider)
AI_MODEL="claude-haiku-4-5-20250514"

# Include cwd and git context in the AI prompt (default: 1)
AI_CONTEXT=1

# Open multi-line results in $EDITOR (default: 1)
AI_EDITOR_MODE=1

# Curl timeout in seconds (default: 10)
AI_TIMEOUT=10
```

### Context Awareness

When `AI_CONTEXT=1` (default), the AI prompt automatically includes:
- Your current working directory
- Git branch name (if in a repo)
- Modified and staged files (up to 10 each)

This helps the AI generate more relevant commands without you needing to describe your environment.

### Editor Integration

If the AI returns a multi-line command and `$EDITOR` is set, the script opens the result in your editor for review. After you save and exit, the edited version is used. Disable with `AI_EDITOR_MODE=0`.

> **Tip:** If you use VS Code, set `EDITOR="code --wait"` so the script waits for you to close the file.

## Examples
Prefix these queries with `ai` in your terminal to try them out:

- which process is running on port 8000
- numeric for loop boilerplate going from 5 to 15
- restart openssh
- start a simple webserver using python3
- rename every file in ./images from .jpg to .png
- resize all of images in ./images to a maximum of 100 pixels in any dimension
- create a small webserver with netcat
- get the ips that amazon.com resolves to and ping each one
- list all network interfaces and their ip addresses
- print out the source of function __git_ps1
- delete all unused docker images
- concatenate two .mkv video files
- use ffmpeg to shrink an mp4 file's size
- use imagemagick change the white background in screenshot.png to transparent

**WARNING**: Always carefully check the output from the AI for bugs or other issues. Do not run the output unless you completely understand what it is doing. You are fully responsible for any commands you run. **This software comes with ABSOLUTELY NO WARRANTY, to the extent permitted by applicable law.**
