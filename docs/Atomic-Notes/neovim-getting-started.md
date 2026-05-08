---
tags:
  - Neovim
  - TUI
---

## Configuration

### Neovim Options
All options will be explained as concise as possible, for me own sake!
```lua
local o = vim.opt
```
`vim.opt` is being passed into `o` basically `o` becomes `vim.opt` thus, instead of calling out `neovim` option with `vim.opt` all we need is to call `o`

#### Options `o`
```lua
local o = vim.opt
o.number = true
o.relativenumber = true
o.termguicolors = true
o.clipboard = "unnamedplus" -- Use system clipboard
o.tabstop = 4
o.shiftwidth = 4
o.wrap = false 				-- Do not wrap lines by default
o.smartindent = true		-- Smart auto-indent
```
All `options` have its own comment at the end of the line explaining what they do! if more information is needed, simply run `:help <option_name>` within `neovim`

#### Neovim Vim Pack
`neovim 0.12` comes with its own `package manager` built-in, thus we no longer need a third party, this is a simple way of how it works. For more information [visit the official vim pack documentation](https://neovim.io/doc/user/pack/)

To add `plugins` using `vim pack` simply add its *addressess* as shown below
```lua
vim.pack.add({
	"https://www.github.com/echasnovski/mini.nvim", -- mini.nvim
	"https://github.com/ellisonleao/gruvbox.nvim",  -- lua gruvbox theme
})
```
*Note: once a plugin has been added, you would need to reopen `neovim` for the plugin to be installed*
