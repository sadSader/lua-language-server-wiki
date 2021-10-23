## Sources
This extensin loads setting from these sources:

1. The configuration file specified by the command line `--configpath="config.json"`
2. `.luarc.json` in your workspace.
3. The client sends it via LSP

The extension will try to load the configuration file in this order, and once it is successfully loaded, it will not try the following sources again.

## Configuration file
You could load local configuration file by [command line](https://github.com/sumneko/lua-language-server/wiki/Command-line) `--configpath="config.json"`. the path can be related to workspace.

You can omit the beginning of `Lua.` in this way, which means that `"runtime.version": "Lua 5.1"` and `"Lua.runtime.version": "Lua 5.1"` are completely equivalent.

configuration file should be `Lua` or `Json`:

```lua
return {
    ['runtime.version'] = 'Lua 5.1',
    diagnostics = {
        enable = false
    }
}
```

```json
{
    "runtime.version" : "Lua 5.1",
    "diagnostics": {
        "enable": false
    }
}
```

`.luarc.json` can only be json.

## Json Schema
Here is a [json schema](https://raw.githubusercontent.com/sumneko/vscode-lua/master/setting/schema.json) file to help you configure setting manually.

You could download a template `setting.json` from https://github.com/sumneko/vscode-lua/blob/master/setting/setting.json.

If your editor supports `jsonschema`, it would prompt you all the settings and options like this:

![setting-without-vscode](https://github.com/sumneko/vscode-lua/blob/master/images/setting-without-vscode.gif?raw=true)

(对于中文用户，使用 `https://raw.githubusercontent.com/sumneko/vscode-lua/master/setting/schema-zh-cn.json` 可以看到中文的提示）

You can find the description of the settings here: https://github.com/sumneko/vscode-lua/blob/master/setting/schema.json


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

## Instructions for [kakoune](https://github.com/mawww/kakoune) using [kak-lsp](https://github.com/kak-lsp/kak-lsp)

### Installation

Install `lua-language-server` via your package manager or build it manually.

Then, install `kak-lsp`:

#### [plug.kak](https://github.com/andreyorst/plug.kak)

Stick the following in your `kakrc`:

```kakounescript
plug "kak-lsp/kak-lsp" do %{ cargo install --locked --force --path . }
```

#### Standalone

Put the `kak-lsp` binary on your `PATH`, then put the following in your `kakrc`:

```kakounescript
evaluate-commands %sh{
    kak-lsp --kakoune -s $kak_session
}
```

### Initialization

Stick the following in your `kakrc`:

```kakounescript
# Enable kak-lsp for Lua files
hook global WinSetOption filetype=lua %{
    lsp-enable-window
}
# Close kak-lsp when kakoune is terminated
hook global KakEnd .* lsp-exit
```

Stick the following in your `kak-lsp.toml` to inform it about the language server:

```toml
[language.lua]
filetypes = ["lua"]
roots = [".git/"]
command = "lua-language-server"
args = ["-E", "/usr/share/lua-language-server/main.lua"]
```

### Configuration

To define server settings, put them under `[language.lua.settings]` in your `kak-lsp.toml`:

```toml
[language.lua.settings]
Lua.diagnostics.severity = { undefined-global = "Error" }
Lua.runtime.version = "Lua 5.2"
Lua.telemetry.enable = false
```