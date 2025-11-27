# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a personal Neovim configuration based on kickstart.nvim with a modular structure. The configuration uses lazy.nvim for plugin management and focuses on LSP integration, modern completion (blink.cmp), and markdown-centric editing.

## Configuration Architecture

### Entry Point
- `init.lua` - Main configuration file that:
  - Loads core modules (`core.options`, `core.keymaps`)
  - Sets up lazy.nvim plugin manager
  - Defines all plugin specifications inline
  - Configures autocommands (auto-reload on save, yank highlighting, markdown startup)
  - Contains LSP configuration and keymaps

### Core Modules (lua/core/)
- `options.lua` - All Neovim options (indent settings, UI preferences, etc.)
- `keymaps.lua` - Global keymaps and custom key bindings

### Plugin Organization
- Plugins are configured inline within `init.lua` using lazy.nvim's `setup()` call
- Additional kickstart plugins in `lua/kickstart/plugins/` (neo-tree, gitsigns are loaded)
- Plugin directory `lua/plugins/` exists but is not currently used

### Key Architectural Decisions

**Lazy Loading**: Plugins use events (`VimEnter`, `BufWritePre`) and filetypes (`lua`) to optimize startup time

**LSP Setup**: Uses mason.nvim + mason-lspconfig.nvim + nvim-lspconfig chain:
- Mason installs LSP servers
- mason-tool-installer ensures required tools are present
- LSP capabilities are enhanced by blink.cmp
- LspAttach autocommand sets up buffer-local keymaps

**Completion**: Uses blink.cmp (not nvim-cmp) with LuaSnip for snippets

**Colorscheme**: Catppuccin (macchiato flavor) with transparent background is active

## Common Development Tasks

### Plugin Management
```bash
# Open lazy.nvim UI
nvim
:Lazy

# Update all plugins
:Lazy update

# Install new plugins (add to init.lua, then run)
:Lazy install
```

### LSP and Tool Management
```bash
# Open Mason to manage LSP servers and tools
:Mason

# Currently configured LSP servers are defined in init.lua servers table
# Automatic installation via mason-tool-installer
```

### Formatting
- Uses conform.nvim for formatting
- Auto-format on save enabled (except for C/C++)
- Manual format: `<leader>f` in any mode
- Check formatter status: `:ConformInfo`

### Code Navigation
LSP keymaps (work in buffers with attached LSP):
- `grd` - Go to definition (telescope)
- `grr` - Find references (telescope)
- `gri` - Go to implementation (telescope)
- `grt` - Go to type definition (telescope)
- `grn` - Rename symbol
- `gra` - Code action
- `gO` - Document symbols (telescope)
- `gW` - Workspace symbols (telescope)

### Telescope Keymaps
- `<leader>sf` - Search files
- `<leader>sg` - Live grep
- `<leader>sh` - Search help
- `<leader>sw` - Search current word
- `<leader>sd` - Search diagnostics
- `<leader>sn` - Search neovim config files
- `<leader><leader>` - Find buffers
- `<leader>/` - Fuzzy find in current buffer

### Key Custom Behaviors
- **Startup**: Opens new markdown buffer in insert mode when nvim starts with no arguments
- **Auto-reload**: `init.lua` automatically sources itself on save
- **Clipboard**: Synced with system clipboard via `unnamedplus`
- **Leader keys**: Space is both leader and localleader
- **Markdown Tab behavior**: Tab/Shift-Tab mapped to indent/outdent in markdown files

### File Structure Conventions
- Core settings belong in `lua/core/`
- Additional plugins can be added to `init.lua` inline or required from `lua/kickstart/plugins/`
- Session files saved as `.session.vim` in current directory

### Testing Configuration Changes
After editing configuration files, changes take effect:
- `init.lua` - automatically reloaded on save
- Other lua files - restart nvim or `:source $MYVIMRC`

### Diagnostics
- `[d` / `]d` - Jump to previous/next diagnostic
- `<leader>d` - Open floating diagnostic
- `<leader>q` - Open diagnostics quickfix list
- `<leader>do` - Toggle diagnostics on/off

## Plugin Dependencies
External tools required:
- `git`, `make`, `gcc` - Build tools
- `ripgrep` - Required by telescope live_grep
- `fd` (optional) - Better file finding
- `xclip` - Clipboard integration on Linux
- Nerd Font - For icons (have_nerd_font = true)

## Notes
- Configuration uses Neovim 0.10+ features (may require nightly for some capabilities)
- LSP servers must be added to the `servers` table in init.lua to be auto-installed
- Formatters must be added to `formatters_by_ft` table in conform.nvim config
- Uses `vim.keymap.set()` modern API instead of legacy `vim.api.nvim_set_keymap()`
