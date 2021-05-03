## Json Schema
Here is a [json schema](https://raw.githubusercontent.com/sumneko/vscode-lua/master/setting/schema.json) file to help you configure setting manually.

You could download a template `setting.json` from https://github.com/sumneko/vscode-lua/blob/master/setting/setting.json.

If your editor supports `jsonschema`, it would prompt you all the settings and options like this:

![setting-without-vscode](https://github.com/sumneko/vscode-lua/blob/master/images/setting-without-vscode.gif?raw=true)

(对于中文用户，使用 `https://raw.githubusercontent.com/sumneko/vscode-lua/master/setting/schema-zh-cn.json` 可以看到中文的提示）

## Different clients load `setting.json` in different ways:

### Neovim with built-in LSP client

This guide assumes you are already familiar with how to use the built-in client
(see `:help lsp` for more information). Settings are specified as a Lua table
which is passed as the `settings` field of your configuration. Here is a
minimal example

```lua
require'nvim_lsp'.sumneko_lua.setup {
  settings = {
    -- Insert your settings here
  }
}
```

The settings are specified as a nested table. This means that when the JSON
schema says `"Lua.runtime.version": "5.3"` you have to write `{Lua = {runtime =
{version = 'Lua 5.3'}}}`. Here is a settings table suitable for writing
standalone Lua scripts with [Luarocks](https://luarocks.org/) libraries:

```lua
-- You will have to adjust your values according to your system
settings = {
  Lua = {
    runtime = {
      version = 'Lua 5.3',
      path = {
        '?.lua',
        '?/init.lua',
        vim.fn.expand'~/.luarocks/share/lua/5.3/?.lua',
        vim.fn.expand'~/.luarocks/share/lua/5.3/?/init.lua',
        '/usr/share/5.3/?.lua',
        '/usr/share/lua/5.3/?/init.lua'
      }
    },
    workspace = {
      library = {
        [vim.fn.expand'~/.luarocks/share/lua/5.3'] = true,
        ['/usr/share/lua/5.3'] = true
      }
    }
  }
}
```

Note how we are able to use Lua code such as `vim.fn.expand` in our settings in
order to dynamically build up the proper value.



### Neovim with [coc.nvim](https://github.com/neoclide/coc.nvim)

- execute :CocConfig; it'll open a json file (this is the the file containing `coc` configuration
- write the configuration for `lua-language-server` in that json.

For example, mine has the following json:
```
{
    ... many unrelated options here ...
    "languageserver": {
        "lua": {
	    "cwd": "full path of lua-language-server directory", (not sure this one is really necessary)
	    "command": "full path to lua-language-server executable",
	    "args": ["-E", "-e", "LANG=en", "[full path of lua-language-server directory]/main.lua"],
	    "filetypes": ["lua"],
	    "rootPatterns": [".git/"]
        }
    },
    "Lua.diagnostics.disable" : [
        "lowercase-global"
    ]
}
```
(Thanks to [gustavo-hms](https://github.com/sumneko/lua-language-server/issues/154#issuecomment-621203055))


---


## Instructions for coc.nvim

I'm posting this in the hope that it'll save someone else from spending an afternoon making (mostly random) changes to their `coc-settings.json` in the struggle to configure `lua-language-server` settings in vim using [coc.nvim](https://github.com/neoclide/coc.nvim).


### Two general rules:

- Any `"Lua.whatever…"` settings you may see around the 'net go _inside_ of a `settings: {…}` key (sibling to `rootPatterns`).
- Dot-separated keys representing nested objects are not supported. E.g., Instead of `"Lua.diagnostics.disable": ["lowercase-global"]`, use  `"Lua": { "diagnostics": { "disable": ["lowercase-global"] }}`

### Examples of what NOT to do

**This does NOT work**

```jsonc
    // coc-settings.json
    "languageserver": {
        "lua": {
            "command": "/path/to/lua-language-server/bin/macOS/lua-language-server",
            "args": ["-E", "/path/to/lua-language-server/main.lua"],
            "filetypes": ["lua"],
            "rootPatterns": [".git/"]
        }
    }

    "Lua.workspace.library":  {  // ← coc.nvim will warn:  "Property Lua.workspace.library is not allowed."
        "/path/to/hammerspoon-completion/build/stubs": true, 
        "/path/to/neovim/runtime/lua": true 
    },
```

---

**This also does NOT work**

```jsonc
    // coc-settings.json
    "languageserver": {
        "lua": {
            "command": "/path/to/lua-language-server/bin/macOS/lua-language-server",
            "args": ["-E", "/path/to/lua-language-server/main.lua"],
            "filetypes": ["lua"],
            "rootPatterns": [".git/"],
            "settings": {
                 "Lua.workspace.library":  {  // ← don't use dot-separated keys, properly nested keys are required.
                     "/path/to/hammerspoon-completion/build/stubs": true, 
                     "/path/to/neovim/runtime/lua": true 
               },
            }
        }
    }
```

---

### Working example

As of 2020-11-07, the following works on the system identified in the comment:


```jsonc
// vim version: NVIM v0.5.0-789-gca7449db4
// node version: v15.0.1
// coc.nvim version: 0.0.79-3e5fbe3a93
// term: xterm-kitty
// platform: darwin

    // coc-settings.json
    "languageserver": {
        "lua": {
            "command": "/path/to/lua-language-server/bin/macOS/lua-language-server",
            "args": ["-E", "/path/to/lua-language-server/main.lua"],
            "filetypes": ["lua"],
            "rootPatterns": [".git/"],
            "settings": {
                "Lua": {
                    "workspace": {
                        "library": {
                            "/path/to/hammerspoon-completion/build/stubs": true,
                            "/path/to/neovim/runtime/lua": true
                        },
                        "maxPreload": 2000,
                        "preloadFileSize": 1000
                    },
                    "runtime": {
                        "version": "Lua 5.4"
                    },
                    "diagnostics": {
                        "enable": true,
                        "globals": ["hs", "vim", "it", "describe", "before_each", "after_each"],
                        "disable": ["lowercase-global"]
                    },
                    "completion": {
                        "keywordSnippet": "Disable"
                    }
                }
            }
        }
    }
```
