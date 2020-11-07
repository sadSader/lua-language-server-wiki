## Json Schema
Here is a [json schema](https://raw.githubusercontent.com/sumneko/vscode-lua/master/setting/schema.json) file to help you configure setting manually.

You could download a template `setting.json` from https://github.com/sumneko/vscode-lua/blob/master/setting/setting.json.

If your editor supports `jsonschema`, it would prompt you all the settings and options like this:

![setting-without-vscode](https://github.com/sumneko/vscode-lua/blob/master/images/setting-without-vscode.gif?raw=true)

(对于中文用户，使用 `https://raw.githubusercontent.com/sumneko/vscode-lua/master/setting/schema-zh-cn.json` 可以看到中文的提示）

## Different clients load `setting.json` in different ways:

### nvim

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


## Alternative instructions for coc.nvim

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

The following works for me using:

vim version: NVIM v0.5.0-789-gca7449db4
node version: v15.0.1
coc.nvim version: 0.0.79-3e5fbe3a93
term: xterm-kitty
platform: darwin


```jsonc
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
                        "version": "5.4.0"
                    },
                    "diagnostics": {
                        "enable": true,
                        "globals": ["hs", "vim", "it", "describe", "before_each", "after_each"],
                        "diable": ["lowercase-global"]
                    },
                    "completion": {
                        "keywordSnippet": "Disable"
                    }
                }
            }
        }
    }
```
