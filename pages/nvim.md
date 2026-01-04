## Lua API
	- Vim-inherited API
		- [Ex-commands](https://neovim.io/doc/user/vimindex.html#Ex-commands) and [vimscript-functions](https://neovim.io/doc/user/vimfn.html#vimscript-functions)as well as [user-function](https://neovim.io/doc/user/vimeval.html#user-function)s in Vimscript.
		- accessed through [vim.cmd()](https://neovim.io/doc/user/lua.html#vim.cmd()) and [vim.fn](https://neovim.io/doc/user/lua.html#vim.fn) respectively, which are discussed under [lua-guide-vimscript](https://neovim.io/doc/user/lua-guide.html#lua-guide-vimscript)
	- Nvim API
		- written in C for use in remote plugins and GUIs; see [api](https://neovim.io/doc/user/api.html#api).
		- accessed through [vim.api](https://neovim.io/doc/user/lua.html#vim.api)
	- "Lua API"
		- written in and specifically for Lua.
		- functions accessible through `vim.*` not mentioned already; see
		     [lua-stdlib](https://neovim.io/doc/user/lua.html#lua-stdlib).
		-