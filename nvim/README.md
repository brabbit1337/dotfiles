# Neovim Configuration

![Neovim](https://img.shields.io/badge/Neovim-0.11%2B-57A143?logo=neovim&logoColor=white)
![NixOS](https://img.shields.io/badge/NixOS-26.05-5277C3?logo=nixos&logoColor=white)
![Lua](https://img.shields.io/badge/Configuration-Lua-2C2D72?logo=lua&logoColor=white)
![KDE Plasma](https://img.shields.io/badge/Desktop-KDE%20Plasma-1D99F3?logo=kde&logoColor=white)

A modular, Nordic-themed Neovim configuration focused on Linux system administration, NixOS configuration editing and general-purpose development. Designed for KDE Plasma Wayland and Konsole, with plugins managed by **lazy.nvim** and external development tools managed declaratively by **NixOS**.

The configuration does not require Home Manager, Mason or AI-assisted coding services.

## Contents

- [Features](#features)
- [Requirements](#requirements)
- [Getting Started](#getting-started)
- [Vim Essentials](#vim-essentials)
- [Directory Structure](#directory-structure)
- [Plugins](#plugins)
- [Language Support](#language-support)
- [Keybindings](#keybindings)
- [Elevated File Editing](#elevated-file-editing)
- [Maintenance](#maintenance)
- [Troubleshooting](#troubleshooting)

## Features

- Nordic dark colorscheme, Nerd Font icons, buffer tabs and minimal statusline.
- Relative line numbers, current-line highlighting, smart indentation and persistent undo history.
- Interactive dashboard, file explorer, fuzzy file finder and integrated terminal.
- Dedicated searches for the current directory, home directory, root filesystem, NixOS configuration and Neovim configuration.
- Treesitter syntax highlighting and indentation, automatic bracket pairing, surround editing and multiple cursors.
- Native LSP integration, Blink completion with a Lua fuzzy matcher, snippets and automatic formatting on standard saves.
- Git change indicators, TODO highlighting, diagnostic navigation and visual undo history.
- Wayland system clipboard integration and mouse support.
- Automatic elevated file editing through Suda Smart Edit, including standard `:w` saves for protected files opened through Suda.
- Reproducible plugin revisions through `lazy-lock.json`.

## Requirements

- Neovim 0.11 or newer, subject to the requirements of the locked plugin revisions.
- Git and an internet connection for initial plugin installation.
- A Nerd Font in the terminal for icon rendering.
- NixOS with the external tools listed below. KDE Plasma Wayland and Konsole are the intended desktop environment and terminal, but are not hard requirements of Neovim itself.

### NixOS packages

Add the following packages to your existing `users.users.<username>.packages = with pkgs; [ ... ];` list in `/etc/nixos/configuration.nix`:

```nix
# Neovim and dependencies
neovim
git
gcc
gnumake
curl
ripgrep
fd
wl-clipboard
tree-sitter

# Language servers
nixd
lua-language-server
bash-language-server
vscode-langservers-extracted
yaml-language-server

# Formatters
nixfmt
stylua
shfmt
prettier
```

For all available Nerd Fonts, add the following at module level (include `lib` in the module arguments):

```nix
fonts.packages = builtins.filter lib.isDerivation (
  builtins.attrValues pkgs.nerd-fonts
);
```

Alternatively, install only `pkgs.nerd-fonts.jetbrains-mono` through `fonts.packages` if you do not need the entire collection. Merge these settings with any existing font configuration.

Apply the system configuration:

```bash
sudo nixos-rebuild switch
```

Optionally set Neovim as your default editor:

```nix
environment.variables.EDITOR = "nvim";
environment.variables.VISUAL = "nvim";
```

### Konsole setup

Open **Settings → Edit Current Profile → Appearance → Font** and select **JetBrainsMono Nerd Font Mono**. A font size of 11 pt is a good starting point. The Neovim colorscheme controls the editor's colors independently of the Konsole profile.

## Getting Started

### First launch

```bash
nvim
```

lazy.nvim bootstraps itself and installs the configured plugins. Once installation has completed, run:

```vim
:checkhealth
```

Use `:Lazy restore` to restore revisions from the committed lockfile if necessary. Treesitter parsers are installed according to the configured language list; use `:TSUpdate` when intentionally updating them.

## Vim Essentials

### Modes

| Key | Action |
|---|---|
| `Esc` | Return to Normal mode |
| `i` / `a` | Insert before / after cursor |
| `I` / `A` | Insert at beginning / end of line |
| `o` / `O` | Open a line below / above |
| `v` / `V` | Characterwise / linewise Visual mode |
| `Ctrl + v` | Blockwise Visual mode |
| `:` | Enter command-line mode |

### Navigation and search

| Key | Action |
|---|---|
| `h` / `j` / `k` / `l` | Left / down / up / right |
| `w` / `b` / `e` | Next word / previous word / end of word |
| `0` / `^` / `$` | Start / first nonblank / end of line |
| `gg` / `G` | First / last line |
| `:42` | Jump to line 42 |
| `Ctrl + d` / `Ctrl + u` | Scroll half a page down / up |
| `/pattern` / `?pattern` | Search forward / backward |
| `n` / `N` | Next / previous match relative to search direction |
| `%` | Jump to matching bracket |

### Editing and registers

| Key | Action |
|---|---|
| `dd` / `yy` | Delete / yank current line |
| `dw` / `cw` | Delete / change word |
| `ciw` | Change inner word |
| `ci"` | Change inside double quotes |
| `p` / `P` | Paste after / before cursor |
| `x` | Delete character |
| `u` / `Ctrl + r` | Undo / redo |
| `.` | Repeat last change |
| `gcc` | Toggle line comment (Normal mode) |
| `gc` | Comment selected lines (Visual mode) |
| `ggVG` | Select entire buffer |

### Essential commands

| Command | Action |
|---|---|
| `:e path` | Open file |
| `:w` | Save |
| `:wq` | Save and quit |
| `:q!` | Quit without saving |
| `:%s/old/new/gc` | Replace throughout file with confirmation |
| `:set paste` / `:set nopaste` | Toggle paste mode if needed |
| `:help subject` | Open built-in help |
| `:messages` | Show recent messages |

### Everyday workflow

1. Launch `nvim` in Konsole.
2. Press `Space f n` to find NixOS configuration files, or `Space f f` to search the current directory.
3. Press `i` to edit, then `Esc` to return to Normal mode.
4. Press `Space c f` to format if a formatter is available.
5. Save with `:w`; protected files opened through Suda request authentication when required.
6. Press `Space q` to quit the current window.

## Directory Structure

```text
~/.config/nvim/
├── init.lua
├── lazy-lock.json
└── lua/
    ├── core/
    │   ├── options.lua
    │   ├── keymaps.lua
    │   └── lazy.lua
    └── plugins/
        ├── ui.lua
        ├── editor.lua
        ├── treesitter.lua
        ├── lsp.lua
        ├── completion.lua
        ├── formatting.lua
        └── tools.lua
```

| File | Purpose |
|---|---|
| `init.lua` | Entry point, leader keys and core module loading |
| `core/options.lua` | Editor behavior and general settings |
| `core/keymaps.lua` | Global custom keybindings |
| `core/lazy.lua` | lazy.nvim bootstrap and plugin manager configuration |
| `plugins/ui.lua` | Colorscheme, statusline, buffer tabs, icons and keybinding hints |
| `plugins/editor.lua` | File search, explorer, terminal and editing utilities |
| `plugins/treesitter.lua` | Syntax highlighting and parser management |
| `plugins/lsp.lua` | Language servers and LSP keybindings |
| `plugins/completion.lua` | Blink completion and snippets |
| `plugins/formatting.lua` | Formatting and format-on-save |
| `plugins/tools.lua` | Suda, Git signs, undo history, TODO comments and diagnostics |

## Plugins

### Plugin management

| Plugin | Purpose |
|---|---|
| [lazy.nvim](https://github.com/folke/lazy.nvim) | Plugin installation, lazy loading and revision locking |

### Appearance and UI

| Plugin | Purpose |
|---|---|
| [nordic.nvim](https://github.com/AlexvZyl/nordic.nvim) | Nordic-inspired dark colorscheme |
| [bufferline.nvim](https://github.com/akinsho/bufferline.nvim) | Tab-style buffer navigation |
| [mini.statusline](https://github.com/echasnovski/mini.statusline) | Minimal statusline |
| [nvim-web-devicons](https://github.com/nvim-tree/nvim-web-devicons) | Filetype icons |
| [which-key.nvim](https://github.com/folke/which-key.nvim) | Interactive keybinding hints |
| [nvim-highlight-colors](https://github.com/brenoprata10/nvim-highlight-colors) | Inline color previews |

### File management and editing

| Plugin | Purpose |
|---|---|
| [snacks.nvim](https://github.com/folke/snacks.nvim) | Dashboard, explorer, picker, terminal, notifications, indentation guides and other UI utilities |
| [oil.nvim](https://github.com/stevearc/oil.nvim) | Filesystem editing as a Neovim buffer |
| [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter) | Syntax highlighting and indentation using the legacy `master` configuration API |
| [mini.pairs](https://github.com/echasnovski/mini.pairs) | Automatic bracket and quote pairing |
| [mini.surround](https://github.com/echasnovski/mini.surround) | Surrounding character manipulation |
| [guess-indent.nvim](https://github.com/NMAC427/guess-indent.nvim) | Automatic indentation detection |
| [vim-visual-multi](https://github.com/mg979/vim-visual-multi) | Multiple-cursor editing |
| [undotree](https://github.com/mbbill/undotree) | Visual undo history |
| [suda.vim](https://github.com/lambdalisue/suda.vim) | Elevated file access and saving through sudo |
| [gitsigns.nvim](https://github.com/lewis6991/gitsigns.nvim) | Git change indicators |
| [todo-comments.nvim](https://github.com/folke/todo-comments.nvim) | TODO, FIXME and special comment highlighting |

### LSP, completion and formatting

| Plugin | Purpose |
|---|---|
| [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig) | Language server configurations using native Neovim LSP APIs |
| [blink.cmp](https://github.com/saghen/blink.cmp) | Autocompletion using the Lua fuzzy matcher |
| [friendly-snippets](https://github.com/rafamadriz/friendly-snippets) | Predefined code snippets |
| [conform.nvim](https://github.com/stevearc/conform.nvim) | Code formatting and format-on-save |
| [fidget.nvim](https://github.com/j-hui/fidget.nvim) | LSP progress notifications |
| [trouble.nvim](https://github.com/folke/trouble.nvim) | Diagnostic list and navigation |
| [plenary.nvim](https://github.com/nvim-lua/plenary.nvim) | Shared Lua utility dependency |

## Language Support

Language servers and formatters are installed through NixOS rather than Mason.

| Language | Language server | Formatter |
|---|---|---|
| Nix | nixd | nixfmt |
| Lua | lua-language-server | stylua |
| Bash / Shell | bash-language-server | shfmt |
| JSON / JSONC | vscode-json-language-server | prettier |
| HTML | vscode-html-language-server | prettier |
| CSS | vscode-css-language-server | prettier |
| YAML | yaml-language-server | prettier |
| JavaScript / TypeScript | — | prettier |
| Markdown | — | prettier |

Configured Treesitter parsers cover Bash, C, C++, CSS, HTML, JavaScript, TypeScript, JSON, JSONC, Lua, Markdown, Markdown Inline, Nix, Python, TOML, YAML, Vim, Vimdoc and regex. Syntax highlighting does not require a language server.

Python syntax highlighting is included, but Python LSP and formatting tools are not required by the simplified package list. Remove any leftover `pyright` or `black` entries from the LSP and formatting configuration if those tools are not installed.

## Keybindings

The leader key is **Space**. Unless otherwise noted, custom mappings are for Normal mode.

### General

| Keybinding | Action |
|---|---|
| `Space` | Display which-key hints |
| `Esc` | Clear search highlighting |
| `Space + w` | Save file |
| `Space + Shift + w` | Save file through Suda |
| `Ctrl + s` | Save file (Normal and Insert modes; leaves Insert mode) |
| `Space + q` | Quit current window |
| `Space + Shift + q` | Quit all windows |
| `Space + u` | Toggle undo history |
| `Space + t` | Toggle integrated terminal |

### File management and search

| Keybinding | Action |
|---|---|
| `Space + e` | Open file explorer |
| `Space + f + f` | Find files in current working directory |
| `Space + f + a` | Find system files from the root filesystem |
| `Space + f + n` | Find NixOS configuration files in `/etc/nixos` |
| `Space + f + h` | Find files in the home directory |
| `Space + f + c` | Find Neovim configuration files |
| `Space + f + g` | Search file contents |
| `Space + f + b` | Find open buffers |
| `Space + f + r` | Find recently opened files |
| `-` | Open parent directory with Oil |

System-wide search uses normal user permissions and is configured to exclude large or virtual system paths such as `/nix/store`, `/proc`, `/sys`, `/dev` and `/run`. It does not grant privileged read access.

### Buffers and windows

| Keybinding | Action |
|---|---|
| `Shift + h` | Previous buffer |
| `Shift + l` | Next buffer |
| `Space + b + d` | Delete current buffer |
| `Ctrl + h` | Move to left window |
| `Ctrl + j` | Move to lower window |
| `Ctrl + k` | Move to upper window |
| `Ctrl + l` | Move to right window |
| `Space + s + v` | Create vertical split |
| `Space + s + h` | Create horizontal split |

### Editing

| Keybinding | Action |
|---|---|
| `<` | Decrease indentation, preserving Visual selection |
| `>` | Increase indentation, preserving Visual selection |
| `Ctrl + n` | Start/select next matching occurrence with Visual Multi |
| `sa` | Add surrounding characters |
| `sd` | Delete surrounding characters |
| `sr` | Replace surrounding characters |
| `sf` | Find surrounding characters |
| `sF` | Find surrounding characters backwards |
| `sh` | Highlight surrounding characters |

### LSP and diagnostics

| Keybinding | Action |
|---|---|
| `gd` | Go to definition |
| `gr` | Find references |
| `K` | Show hover documentation |
| `Space + c + r` | Rename symbol |
| `Space + c + a` | Code actions |
| `Space + c + d` | Show diagnostic details |
| `Space + c + f` | Format current file |
| `Space + x + x` | Toggle diagnostic list |

LSP mappings become available when a language server attaches to the current buffer.

### Autocompletion

Blink CMP uses the `default` keymap preset.

| Keybinding | Action |
|---|---|
| `Ctrl + Space` | Show completion or toggle documentation |
| `Ctrl + n` | Select next completion item |
| `Ctrl + p` | Select previous completion item |
| `Ctrl + y` | Accept selected completion |
| `Ctrl + e` | Hide completion menu |
| `Ctrl + b` | Scroll documentation upwards |
| `Ctrl + f` | Scroll documentation downwards |
| `Tab` | Jump to next snippet placeholder when active |
| `Shift + Tab` | Jump to previous snippet placeholder when active |

Plugin-default mappings may vary by version or mode; the tables describe the configured mappings and intended defaults.

## Elevated File Editing

Suda Smart Edit is enabled at startup:

```lua
{
    "lambdalisue/suda.vim",
    lazy = false,
    init = function()
        vim.g.suda_smart_edit = 1
    end,
},
```

Open a protected system configuration without running Neovim itself as root:

```bash
nvim /etc/nixos/configuration.nix
```

If Smart Edit does not automatically redirect the file, explicitly open it through Suda:

```bash
nvim suda:///etc/nixos/configuration.nix
```

Save using the standard command:

```vim
:w
```

Suda requests sudo authentication when needed. `:SudaWrite` and `:SudaRead` remain available for explicit privileged operations. Suda URI buffers can behave differently from standard file buffers, so LSP attachment and format-on-save may not always work identically.

## Maintenance

### Useful commands

| Command | Action |
|---|---|
| `:w` | Save current file |
| `:wq` | Save and quit |
| `:Lazy` | Open plugin manager |
| `:Lazy sync` | Install, update and clean plugins |
| `:Lazy update` | Update plugins |
| `:Lazy clean` | Remove unused plugins |
| `:Lazy restore` | Restore locked plugin revisions |
| `:TSUpdate` | Update Treesitter parsers |
| `:LspInfo` | Show LSP status |
| `:ConformInfo` | Show formatter status |
| `:Oil` | Open Oil file manager |
| `:UndotreeToggle` | Toggle undo history |
| `:SudaWrite` | Save with elevated privileges |
| `:SudaRead` | Read with elevated privileges |
| `:checkhealth` | Run health checks |

### Plugin updates

Use `:Lazy update` to update plugins intentionally and `:Lazy restore` to return to revisions recorded in `lazy-lock.json`. Keep a known-good copy of the lockfile before updating. Use `:TSUpdate` when updating parsers, and `:checkhealth` to diagnose compatibility problems.

## Troubleshooting

| Issue | Check |
|---|---|
| Missing icons | Select a Nerd Font in the Konsole profile |
| Clipboard not working | Verify `wl-clipboard` is installed and check `:checkhealth` |
| File search cannot find system files | Use `Space + f + a` or `Space + f + n`; check file permissions |
| Root-owned file is read-only | Verify Suda Smart Edit or open the file using `suda:///` |
| Language server is unavailable | Verify the relevant NixOS package is installed and run `:LspInfo` |
| Formatter is unavailable | Run `:ConformInfo` and verify the formatter is on `PATH` |
| Treesitter errors after an update | Check plugin/parser compatibility; restore locked plugin revisions where appropriate |
| Plugin fails after an update | Run `:Lazy restore` using the known-good lockfile |

---

Built around Neovim, NixOS and Lua.
