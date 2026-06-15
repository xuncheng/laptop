# Laptop

A single shell script that turns a fresh macOS machine into a ready-to-use
web development environment. Idempotent — safe to re-run any time to pick up
new tools.

## Install

Prerequisites: macOS (Apple Silicon or Intel) and an SSH key configured for GitHub.

```bash
git clone git@github.com:xuncheng/laptop.git ~/laptop
cd ~/laptop
bash mac
```

By default this sets up the machine **without** touching your dotfiles. To also
clone and install [dotfiles](https://github.com/xuncheng/dotfiles) via dotbot:

```bash
bash mac --dotfiles
```

## What it installs

Everything below is installed through Homebrew (`brew bundle`):

| Category | Tools |
| --- | --- |
| **Terminal & editor** | Kitty · Neovim + LazyVim · tmux |
| **Modern CLI** | ripgrep · fd · fzf · bat · eza · jq · delta · zoxide · httpie · lazygit |
| **Shell** | zsh-syntax-highlighting · zsh-autosuggestions · starship |
| **Dev tooling** | luarocks · stylua · shellcheck · libyaml |
| **Languages** | Node.js LTS (via Volta) |
| **Git** | git · gh |

## What it does

1. Installs Homebrew (auto-detecting Apple Silicon vs Intel) and runs `brew doctor`
2. Installs the CLI tools and Kitty via `brew bundle`
3. Sets up fzf key bindings and shell completion
4. Installs Node.js LTS through Volta
5. Configures the shell — starship prompt, zoxide, zsh plugins, and modern aliases
6. Applies modern git defaults (delta pager, `pull.rebase`, `zdiff3`, `init.defaultBranch=main`)
7. With `--dotfiles`: clones the dotfiles repo and runs dotbot
8. Bootstraps LazyVim if no Neovim config exists yet

## Customize

Add machine-specific extras to `~/.laptop.local` — it's sourced at the end of
every run, so your additions survive re-runs:

```bash
#!/bin/bash

# Docker
brew install --cask docker

# Claude Code
npm install -g @anthropic-ai/claude-code

# Databases
brew install redis && brew services start redis
```

## Related

- [dotfiles](https://github.com/xuncheng/dotfiles) — config files managed by dotbot
