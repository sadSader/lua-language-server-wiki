### quick debug

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

### Debugger