# The Configuration File
There are multiple ways to provide a configuration file to customize how the language server operates. A full list of the available configuration settings can be found on the [settings page](https://github.com/sumneko/lua-language-server/wiki/Settings).

## Visual Studio Code
In Visual Studio Code, you can use [the settings UI](https://github.com/sumneko/lua-language-server/wiki/Getting-Started#configuration) or write directly to the `settings.json` file. You can also use a [`.luarc.json` file](#luarcjson).

The `settings.json` file can be found in the following OS-dependent locations:

|   OS    | Path                                                         |
| :-----: | :----------------------------------------------------------- |
| Windows | `%APPDATA%\Code\User\settings.json`                          |
|  Linux  | `$HOME/.config/Code/User/settings.json`                      |
|  MacOS  | `$HOME/Library/Application\ Support/Code/User/settings.json` |

For more details, read the [VS Code documentation](https://code.visualstudio.com/docs/getstarted/settings).

## JSON Schema
[A JSON schema](https://github.com/sumneko/vscode-lua/tree/master/setting) is available in multiple languages to help make the creation of a JSON configuration file easier.

In VS Code, this schema is already injected into `.vscode/settings.json` and `.luarc.json` but for other editors where this is not the case, it can be added manually by adding the following to the start of your JSON file:

```json
{
    "$schema": "https://raw.githubusercontent.com/sumneko/vscode-lua/master/setting/schema.json"
}
```

## .luarc.json
A `.luarc.json` file can be added to your workspace to apply a certain configuration to the server. This file must be written in JSON and can use the same [JSON schema](#json-schema) and settings as the other configuration files.

## Custom Config File
If you want to use your own custom configuration file, with whatever name you please, that is also an option - although it must be written in Lua or JSON.

This file can then be loaded by using the [`--configpath` flag](https://github.com/sumneko/lua-language-server/wiki/Getting-Started#-configpath).

If writing the file in Lua, the configuration file can consist of nested tables;

```lua
return {
    Lua = {
        runtime = {
            version = "Lua 5.1"
        }
    }
}
```

keys;

```lua
return {
    ["Lua.runtime.version"] = "Lua 5.1"
}
```

or even a combination of both!

```lua
return {
    Lua = {
        runtime = {
            version = "Lua 5.1"
        },
        ["completion.enable"] = false
    }
}
```

## Other Clients
Each client may handle the configuration file differently, below are some examples of how to get going with other clients.

### Neovim with built-in LSP client
This guide assumes that you are already familiar with how the [built-in client](https://neovim.io/doc/user/lsp.html) works.

Settings are defined as a nested Lua table, like so:

```lua
require'lspconfig'.sumneko_lua.setup {
    settings = {
        -- Settings go here!
    }
}
```

Because the settings must be a nested table, this means when you see `"Lua.runtime.version": "5.3"` in the [JSON schema](#json-schema), you must write:

```lua
Lua = {
    runtime = {
        version = "Lua 5.1"
    }
}
```

Here is an example configuration file that can be used when writing standalone Lua scripts with [luarocks](https://luarocks.org/) libraries:

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
        vim.fn.expand'~/.luarocks/share/lua/5.3',
        '/usr/share/lua/5.3'
      }
    }
  }
}
```

> ℹ️ Notice that we are able to execute Lua like `vim.fn.expand` to dynamically build the correct value.


### Neovim with [coc.nvim](https://github.com/neoclide/coc.nvim)
*Thanks to [gustavo-hms](https://github.com/sumneko/lua-language-server/issues/154#issuecomment-621203055)*

1. Execute `:CocConfig;`
2. A JSON file will open
3. Write your settings in the opened JSON file

<details>
<summary>Example</summary>

```json
{
    // ... many unrelated options here ...
    "languageserver": {
        "lua": {
            "cwd": "full path of lua-language-server directory", // (not sure this one is really necessary)
            "command": "full path to lua-language-server executable",
            "filetypes": ["lua"],
            "rootPatterns": [".git/"]
        }
    },
    "settings": {
        "Lua": {
            "workspace": {
                "library": [
                    "/path/to/hammerspoon-completion/build/stubs",
                    "/path/to/neovim/runtime/lua"
                ],
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
```

</details>

### [Kakoune](https://github.com/mawww/kakoune) with [kak-lsp](https://github.com/kak-lsp/kak-lsp)
Make sure you have `kak-lsp` installed.

#### Installation
If using [`plug.kak`](https://github.com/andreyorst/plug.kak), place the following in your `kakrc`:

```kakounescript
plug "kak-lsp/kak-lsp" do %{ cargo install --locked --force --path . }
```

If running standalone, place the `kak-lsp` binary on your `PATH` and then place the following in your `kakrc`:

```kakounescript
evaluate-commands %sh{
    kak-lsp --kakoune -s $kak_session
}
```

#### Initialization
Place the following in your `kakrc`:

```kakounescript
# Enable kak-lsp for Lua files
hook global WinSetOption filetype=lua %{
    lsp-enable-window
}
# Close kak-lsp when kakoune is terminated
hook global KakEnd .* lsp-exit
```

And the below in your `kak-lsp.toml` to inform it of the existence of the language server:

```toml
[language.lua]
filetypes = ["lua"]
roots = [".git/"]
command = "lua-language-server"
```

#### Configuration
To define server settings, place them under `[language.lua.settings]` in your `kak-lsp.toml`:

```toml
[language.lua.settings]
Lua.diagnostics.severity = { undefined-global = "Error" }
Lua.runtime.version = "Lua 5.2"
Lua.telemetry.enable = false
```
