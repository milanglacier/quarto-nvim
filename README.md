# quarto-nvim

Quarto-nvim provides tools for working on [quarto](https://quarto.org/) manuscripts in neovim.

**Note**: Some functionality has been refactored into its own library [otter.nvim](https://github.com/jmbuhr/otter.nvim) for better extensibility.

## Setup

Install the plugin from GitHub with your favourite neovim plugin manager e.g.

[packer.nvim](https://github.com/wbthomason/packer.nvim):

```lua
use { 'quarto-dev/quarto-nvim',
  requires = {
    'neovim/nvim-lspconfig',
    'jmbuhr/otter.nvim'
  }
}
```

or [lazy](https://github.com/folke/lazy.nvim)

```lua
{ 'quarto-dev/quarto-nvim',
  dependencies = {
    'jmbuhr/otter.nvim',
    'neovim/nvim-lspconfig'
  },
```

or [vim-plug](https://github.com/junegunn/vim-plug)

```vim
Plug 'quarto-dev/quarto-nvim'
Plug 'neovim/nvim-lspconfig'
Plug 'jmbuhr/otter.nvim'
```

## Usage

### Preview

Use the command

```vim
QuartoPreview
```

or access the function from lua, e.g. to create a keybinding:

```lua
local quarto = require'quarto'
vim.keymap.set('n', '<leader>qp', quarto.quartoPreview, {silent = true, noremap = true})
```

Then use the keyboard shortcut to open `quarto preview` for the current file or project in the active working directory in the neovim integrated terminal in a new tab.

## Configure

You can pass a lua table with options to the setup function.
It will be merged with the default options, which are shown below in the example
(i.e. if you are fine with the defaults you don't have to call the setup function).

```lua
require'quarto'.setup{
  debug = false,
  closePreviewOnExit = true,
  lspFeatures = {
    enabled = false,
    languages = { 'r', 'python', 'julia' },
    diagnostics = {
      enabled = true,
      triggers = { "BufWrite" }
    },
    completion = {
      enabled = false,
    },
  },
  keymap = {
    hover = 'K',
    definition = 'gd'
  }
}
```

## Language support (WIP)

### Demo


https://user-images.githubusercontent.com/17450586/209436101-4dd560f4-c876-4dbc-a0f4-b3a2cbff0748.mp4


### Usage

Enable quarto-nvim's lsp features by configuring it with

```lua
require'quarto'.setup{
  lspFeatures = {
    enabled = true,
  }
}
```

Or explicitly run

```vim
QuartoActivate
QuartoDiagnostics
```

or

```vim
lua require'quarto'.activate
lua require'quarto'.enableDiagnostics
```

After enabling the language features, you can open the hover documentation
for R, python and julia code chunks with `K` (or configure a different shortcut).

### Autocompletion

`quarto-nvim` now comes with a completion source for [nvim-cmp](https://github.com/hrsh7th/nvim-cmp) to deliver swift autocompletion for code in quarto code chunks.
With the quarto language features enabled, you can add the source in your `cmp` configuration:

```lua
-- ...
  sources = {
    { name = 'otter' },
  }
-- ...
```

Limitation: Currently this only works for one of the languages in a multi-language document. I am on it!

### R diagnostics configuration

To make diagnostics work with R you have to configure the linter a bit, since the language
buffers in the background separate code with blank links, which we want to ignore.
Otherwise you get a lot more diagnostics than you probably want.
Add file `.lintr` to your home folder and fill it with:

```
linters: linters_with_defaults(
    trailing_blank_lines_linter = NULL,
    trailing_whitespace_linter = NULL
  )
```

You can now also enable other lsp features, such as the show hover function
and shortcut, independent of showing diagnostics by enabling lsp features
but not enabling diagnostics.

## Available Commnds

```vim
QuartoPreview
QuartoClosePreview
QuartoHelp <..>
QuartoActivate
QuartoDiagnostics
QuartoHover
```

## Recommended Plugins

Quarto works great with a number of existing plugins in the neovim ecosystem.
You can find semi-opinionated but still minimal
configurations for `nvim` and `tmux`,
for use with quarto, R and python in these two repositories:

- <https://github.com/jmbuhr/quarto-nvim-kickstarter>
- <https://github.com/jmbuhr/tmux-kickstarter>

