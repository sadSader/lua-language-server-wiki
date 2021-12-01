### Quick Debug

#### `log.debug`
```lua
local util   = require 'utility'
local client = require 'client'

function OnSetText(uri, text)
    log.debug(uri, #text, util.dump(client.info.clientInfo))
end

```

[Find log](https://github.com/sumneko/lua-language-server/wiki/Default-log-path)

#### `print`
```lua
local util   = require 'utility'
local client = require 'client'

function OnSetText(uri, text)
    print(uri, #text, util.dump(client.info.clientInfo))
end
```

Send message by [window/showMessage](https://microsoft.github.io/language-server-protocol/specifications/specification-3-17/#window_showMessage)

In VSCode, they are displayed in OUTPUT pannel.

### Debugger Attach

1. Open server folder in VSCode
2. Install [actboy168.lua-debug](https://marketplace.visualstudio.com/items?itemName=actboy168.lua-debug)
3. Copy [setting.json](https://github.com/sumneko/lua-language-server/blob/master/.vscode/launch.json) into your folder
4. The debug target uses setting:
    ```json
    "Lua.misc.parameters": [
        "--develop=true",
        "--dbgport=11413"
    ],
    ```
5. Restart debug target server (`F1` -> `Reload Window`)
6. Select `attach` and press F5 to attach target server