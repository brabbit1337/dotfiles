## Plugins & Features

A lightweight, modular Neovim configuration designed for NixOS, running inside KDE Plasma's Konsole terminal.

### Plugin Manager

| Plugin | Description |
|---|---|
| [lazy.nvim](https://github.com/folke/lazy.nvim) | Plugin management, lazy loading and version locking |

### Appearance & User Interface

| Plugin | Description |
|---|---|
| [nordic.nvim](https://github.com/AlexvZyl/nordic.nvim) | Nordic-inspired dark colorscheme |
| [bufferline.nvim](https://github.com/akinsho/bufferline.nvim) | Tab-style buffer management |
| [mini.statusline](https://github.com/echasnovski/mini.statusline) | Minimal and informative statusline |
| [nvim-web-devicons](https://github.com/nvim-tree/nvim-web-devicons) | Nerd Font file and folder icons |
| [which-key.nvim](https://github.com/folke/which-key.nvim) | Interactive keybinding hints |
| [nvim-highlight-colors](https://github.com/brenoprata10/nvim-highlight-colors) | Inline color previews |

### File Management & Navigation

| Plugin | Description |
|---|---|
| [snacks.nvim](https://github.com/folke/snacks.nvim) | Dashboard, file explorer, fuzzy finder, terminal, notifications and indentation guides |
| [oil.nvim](https://github.com/stevearc/oil.nvim) | Edit the filesystem like a regular Neovim buffer |

### Editing & Productivity

| Plugin | Description |
|---|---|
| [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter) | Advanced syntax highlighting and indentation |
| [mini.pairs](https://github.com/echasnovski/mini.pairs) | Automatic bracket and quote pairing |
| [mini.surround](https://github.com/echasnovski/mini.surround) | Add, replace and delete surrounding characters |
| [guess-indent.nvim](https://github.com/NMAC427/guess-indent.nvim) | Automatic indentation detection |
| [vim-visual-multi](https://github.com/mg979/vim-visual-multi) | Multiple cursor editing |
| [undotree](https://github.com/mbbill/undotree) | Visual undo history |
| [suda.vim](https://github.com/lambdalisue/suda.vim) | Automatic elevated file access and saving through sudo |
| [todo-comments.nvim](https://github.com/folke/todo-comments.nvim) | Highlight TODO, FIXME and other special comments |
| [gitsigns.nvim](https://github.com/lewis6991/gitsigns.nvim) | Git integration and inline change indicators |

### LSP, Completion & Formatting

| Plugin | Description |
|---|---|
| [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig) | Native Language Server Protocol integration |
| [blink.cmp](https://github.com/saghen/blink.cmp) | Fast autocompletion with a pure Lua fuzzy matcher |
| [friendly-snippets](https://github.com/rafamadriz/friendly-snippets) | Predefined code snippets |
| [conform.nvim](https://github.com/stevearc/conform.nvim) | Code formatting and format on save |
| [fidget.nvim](https://github.com/j-hui/fidget.nvim) | LSP progress notifications |
| [trouble.nvim](https://github.com/folke/trouble.nvim) | Diagnostics and error management |
| [plenary.nvim](https://github.com/nvim-lua/plenary.nvim) | Shared Lua utility library |

### Language Support

| Language | Language Server | Formatter |
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

### Core Features

- Modular Lua configuration
- Nordic dark colorscheme
- Nerd Font icon support
- Relative line numbers
- Current line highlighting
- Persistent undo history
- System clipboard integration through Wayland
- Mouse support
- Smart indentation
- Automatic code completion
- Automatic code formatting on standard saves
- Syntax highlighting through Treesitter
- Interactive startup dashboard
- Integrated terminal
- Multiple cursor editing
- Git change indicators
- Buffer-based tab navigation
- Fuzzy file and content searching
- Root filesystem searching
- Dedicated NixOS configuration finder
- Dedicated home directory finder
- Dedicated Neovim configuration finder
- Automatic sudo access through Suda Smart Edit
- Standard `:w` support for protected files opened through Suda
- Native Neovim LSP integration
- Declarative language server and formatter installation through NixOS
- No Mason dependency
- No AI or Copilot integration
- No Home Manager requirement
