### Quick Debug

#### `log.debug`
```lua
m.register 'textDocument/didOpen' {
    ---@async
    function (params)
        log.debug('didOpen:', util.dump(params))
        workspace.awaitReady()
        local doc   = params.textDocument
        local uri   = files.getRealUri(doc.uri)
        local text  = doc.text
        files.setText(uri, text, true, doc.version)
        files.open(uri)
    end
}
```

[Find log](https://github.com/sumneko/lua-language-server/wiki/Default-log-path)

#### `print`
```lua
m.register 'textDocument/didOpen' {
    ---@async
    function (params)
        print('didOpen:', util.dump(params))
        workspace.awaitReady()
        local doc   = params.textDocument
        local uri   = files.getRealUri(doc.uri)
        local text  = doc.text
        files.setText(uri, text, true, doc.version)
        files.open(uri)
    end
}
```

Send message by [window/showMessage](https://microsoft.github.io/language-server-protocol/specifications/specification-3-17/#window_showMessage)

In VSCode, they are displayed in OUTPUT pannel.

### Debugger Attach

1. Open server folder in VSCode
2. Install [actboy168.lua-debug](https://marketplace.visualstudio.com/items?itemName=actboy168.lua-debug)
3. Copy [setting.json](https://github.com/sumneko/lua-language-server/blob/master/.vscode/launch.json) into your folder
4. The debugger target uses setting:
    ```json
    "Lua.misc.parameters": [
        "--develop=true",
        "--dbgport=11413"
    ],
    ```
5. Restart target server (`F1` -> `Reload Window`)
6. Select `attach` and press F5 to attach target server