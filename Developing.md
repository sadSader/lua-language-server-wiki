# Developing
Hello! Thank you for taking an interest in helping improve the language server.

You can explore the [file structure page](https://github.com/LuaLS/lua-language-server/wiki/File-Structure) for info on what files are doing what.

<br>

## Enable Developer Mode
In Visual Studio Code, add `--develop=true` to [`Lua.misc.parameters`](https://github.com/LuaLS/lua-language-server/wiki/Settings#miscparameters).

In other clients, use `--develop=true` as a flag from the command line.

## Debugging
Debugging can be performed in a few ways. You can [do a quick `print()`](#quick-print), [write to the log file](#append-to-log-file), or [attach a debugger](#attach-debugger) to get all the info you need.

### Quick Print
You can quickly `print()` to the `OUTPUT` panel (<kbd>Ctrl + Shift + U</kbd>) in Visual Studio Code.

![outputPanel](https://user-images.githubusercontent.com/61925890/181308229-52b7e9b7-2893-429b-bca2-8386670df6b0.png)

Below is an example of how a [plugin](https://github.com/LuaLS/lua-language-server/wiki/Plugins) can be debugged.

```lua
local util   = require 'utility'
local client = require 'client'

function OnSetText(uri, text)
    print(uri, #text, util.dump(client.info.clientInfo))
end
```

### Append to Log File
You can add an entry to the [log file](https://github.com/LuaLS/lua-language-server/wiki/FAQ#where-can-i-find-the-log-file). Below is an example of how a [plugin](https://github.com/LuaLS/lua-language-server/wiki/Plugins) can be debugged.

```lua
local util   = require 'utility'
local client = require 'client'

function OnSetText(uri, text)
    log.debug(uri, #text, util.dump(client.info.clientInfo))
end
```

### Attach Debugger
This is the most advanced method, but provides all kinds of useful info and is the most "proper" way to debug the language server.

You will need two Visual Studio Code instances open:
1. The **Debug Host**
    - This instance has the language server open which can be found in one of these locations:
      - Windows: `%USERPROFILE%\.vscode\extensions`
      - Linux: `~/.vscode/extensions`
      - MacOS: `~/.vscode/extensions`
2. The **Debug Target**
    - This instance is where you will test the language server. It should have a folder opened where you can write Lua to test the various features of the language server and use it as normal.

The below steps guide you through setting up Lua debugging:

1. Install [`actboy168.lua-debug`](https://marketplace.visualstudio.com/items?itemName=actboy168.lua-debug) from the extension marketplace for Lua debugging
2. Copy [`.vscode/launch.json`](https://github.com/LuaLS/lua-language-server/blob/master/.vscode/launch.json) into **Debug Host**
3. Copy the below settings into your [`settings.json`](https://code.visualstudio.com/docs/getstarted/settings#_settingsjson) for the **Debug Target**
   ```lua
   "Lua.misc.parameters": [
    "--develop=true",
    "--dbgport=11428"
   ],
   ```
4. Restart the **Debug Target** (<kbd>F1</kbd> -> `Reload Window`)
5. Open the `Run and Debug` sidepanel (<kbd>Ctrl + Shift + D</kbd>) and select `🍄 attach`
   ![outputPanel](https://user-images.githubusercontent.com/61925890/181308530-264e8c8e-3847-4ae4-a4f0-2446c41fbfc8.png)
6. Press <kbd>F5</kbd> to begin debugging

> ℹ️ Note: If you got the server through git you will need to change the debug port in `settings.json` to `"--dbgport=XXXXX"` and address to `"address": "127.0.0.1:XXXXX"` in `launch.json`.


## Multiple Workspace Support
The server has supported multi-workspace environments since `v2.6.0`. This works when the client starts up one instance of the language server and then sends all Lua files to it (even if they are not included in the current workspaces).

> ℹ️ ~Note: The server does not support dynamically adding or removing workspaces. If the workspaces change, the client should restart the server.~ The server has supported dynamically adding or removing workspaces since `v3.5.1`.

![](https://github.com/LuaLS/vscode-lua/raw/master/images/wiki-workspace.png)

The server creates a `<fallback>` scope by default. If you start the server while in "single file mode", this `<fallback>` scope is what is being used.

Should the server be started in "workspace mode", each workspace will be given its own scope.

Linking to other files/directories outside the scope of your workspace(s) is also possible. Built-in Lua libraries are linked using the API definition files from `meta/` that correspond to the [`runtime.version`](https://github.com/LuaLS/lua-language-server/wiki/Settings#runtimeversion) you have selected. You can also specify additional files to include using [`workspace.library`](https://github.com/LuaLS/lua-language-server/wiki/Settings#workspacelibrary).

When a Lua file opened/created, the server will check all workspace scopes:

- If the file belongs to the working directory or linked directory of the scope, it will be assigned to this scope
- If all workspace scopes are not compliant, the file will be assigned to the `<fallback>` scope.

Every scope has an independent environment for separating global variables, classes, settings, requires, etc.


## Theming
Here you can find the names of the various tokens in Visual Studio Code to highlight and colour the various semantic items of Lua.

### Syntax Tokens
These tokens are being previewed in `Dark+` of Visual Studio Code as it has great support for the various tokens used by the extension.

|                     token                      |                                                            preview                                                             |
| :--------------------------------------------: | :----------------------------------------------------------------------------------------------------------------------------: |
|              `keyword.local.lua`               |              ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/keyword.local.lua.jpg?raw=true)               |
|             `keyword.control.lua`              |             ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/keyword.control.lua.jpg?raw=true)              |
|            `entity.name.class.lua`             |            ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/entity.name.class.lua.jpg?raw=true)             |
|           `entity.name.function.lua`           |           ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/entity.name.function.lua.jpg?raw=true)           |
| `punctuation.definition.parameters.begin.lua`  | ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/punctuation.definition.parameters.begin.lua.jpg?raw=true)  |
| `punctuation.definition.parameters.finish.lua` | ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/punctuation.definition.parameters.finish.lua.jpg?raw=true) |
|       `variable.parameter.function.lua`        |       ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/variable.parameter.function.lua.jpg?raw=true)        |
|     `punctuation.separator.arguments.lua`      |     ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/punctuation.separator.arguments.lua.jpg?raw=true)      |
|         `constant.numeric.integer.lua`         |         ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/constant.numeric.integer.lua.jpg?raw=true)         |
|          `constant.numeric.float.lua`          |          ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/constant.numeric.float.lua.jpg?raw=true)          |
|   `constant.numeric.integer.hexadecimal.lua`   |   ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/constant.numeric.integer.hexadecimal.lua.jpg?raw=true)   |
|    `constant.numeric.float.hexadecimal.lua`    |    ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/constant.numeric.float.hexadecimal.lua.jpg?raw=true)    |
|   `punctuation.definition.string.begin.lua`    |   ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/punctuation.definition.string.begin.lua.jpg?raw=true)    |
|    `punctuation.definition.string.end.lua`     |    ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/punctuation.definition.string.end.lua.jpg?raw=true)     |
|           `string.quoted.single.lua`           |           ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/string.quoted.single.lua.jpg?raw=true)           |
|           `string.quoted.double.lua`           |           ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/string.quoted.double.lua.jpg?raw=true)           |
|      `string.quoted.other.multiline.lua`       |      ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/string.quoted.other.multiline.lua.jpg?raw=true)       |
|        `constant.character.escape.lua`         |        ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/constant.character.escape.lua.jpg?raw=true)         |
|      `constant.character.escape.byte.lua`      |      ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/constant.character.escape.byte.lua.jpg?raw=true)      |
|    `constant.character.escape.unicode.lua`     |    ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/constant.character.escape.unicode.lua.jpg?raw=true)     |
|     `invalid.illegal.character.escape.lua`     |     ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/invalid.illegal.character.escape.lua.jpg?raw=true)     |
|      `punctuation.definition.comment.lua`      |      ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/punctuation.definition.comment.lua.jpg?raw=true)      |
|         `comment.line.double-dash.lua`         |         ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/comment.line.double-dash.lua.jpg?raw=true)         |
|   `punctuation.definition.comment.begin.lua`   |   ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/punctuation.definition.comment.begin.lua.jpg?raw=true)   |
|    `punctuation.definition.comment.end.lua`    |    ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/punctuation.definition.comment.end.lua.jpg?raw=true)    |
|              `comment.block.lua`               |              ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/comment.block.lua.jpg?raw=true)               |
|           `keyword.control.goto.lua`           |           ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/keyword.control.goto.lua.jpg?raw=true)           |
|                `string.tag.lua`                |                ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/string.tag.lua.jpg?raw=true)                |
|    `punctuation.section.embedded.begin.lua`    |    ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/punctuation.section.embedded.begin.lua.jpg?raw=true)    |
|     `punctuation.section.embedded.end.lua`     |     ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/punctuation.section.embedded.end.lua.jpg?raw=true)     |
|          `variable.language.self.lua`          |          ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/variable.language.self.lua.jpg?raw=true)          |
|             `support.function.lua`             |             ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/support.function.lua.jpg?raw=true)             |
|         `support.function.library.lua`         |         ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/support.function.library.lua.jpg?raw=true)         |
|             `keyword.operator.lua`             |             ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/keyword.operator.lua.jpg?raw=true)             |
|              `variable.other.lua`              |              ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/variable.other.lua.jpg?raw=true)              |

### Semantic Tokens

|     semantic token      |    fallen syntax token     |                                                 preview                                                 |
| :---------------------: | :------------------------: | :-----------------------------------------------------------------------------------------------------: |
|   `namespace.static`    |   `support.function.lua`   |   ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/namespace.static.jpg?raw=true)    |
|  `namespace.readonly`   |  `constant.language.lua`   |  ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/namespace.readonly.jpg?raw=true)   |
| `namespace.deprecated`  |    `entity.name.label`     | ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/namespace.deprecated.jpg?raw=true)  |
| `parameter.declaration` |    `variable.parameter`    | ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/parameter.declaration.jpg?raw=true) |
| `property.declaration`  |  `entity.other.attribute`  | ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/property.declaration.jpg?raw=true)  |
|       `variable`        |    `variable.other.lua`    |       ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/variable.jpg?raw=true)        |
| `interface.declaration` | `entity.name.function.lua` | ![](https://github.com/LuaLS/vscode-lua/blob/master/images/tokens/interface.declaration.jpg?raw=true) |
