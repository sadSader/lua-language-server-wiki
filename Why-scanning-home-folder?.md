# Why scanning?
When you open a workspace, the extension needs to scan and load lua files in the workspace for features such as searching definitions across files.
According to the provisions of the [LSP](https://microsoft.github.io/language-server-protocol/specifications/specification-3-17/#initialize), the client will provide the server with the uri of the folder to be scanned (field `rootUri`). If you only opened a single file, then the uri should be null.
A common situation is that some clients or users misunderstand the meaning of `rootUri` and set this field to the uri of home, the root directory of the client, the root directory of this extension, etc. In this case, the extension will faithfully scan the file according to the provided uri.

# How to check `rootUri` at server side?
Please [find the server log](https://github.com/sumneko/lua-language-server/wiki/Default-log-path), then search `Client init` in the log, you could found the log like this:
```
[17:58:27.365][debug][#0:script\provider\client.lua:32]: Client init	{
    capabilities = {...
    },
    clientInfo = {
        name = "Visual Studio Code - Insiders",
        version = "1.55.0-insider",
    },
    locale = "zh-cn",
    processId = 21048,
    rootPath = "c:\\Users\\sumneko\\.vscode-insiders\\extensions\\vscode-lua",
    rootUri = "file:///c%3A/Users/sumneko/.vscode-insiders/extensions/vscode-lua",
    trace = "off",
    workDoneToken = "a9e94178-eb6f-4277-ab21-6d81a5871041",
    workspaceFolders = {
        [1] = {
            name = "vscode-lua",
            uri = "file:///c%3A/Users/sumneko/.vscode-insiders/extensions/vscode-lua",
        },
    },
}
```
You can see the value of the `rootUri` field provided by the client here.

# How to fix `rootUri`?
* VSCode: Well, this shouldn't happen, because the client was provided by me, so there should be no mistake. In case it does happen, please open an issue in this project.
* Non-VSCode: First, check if `rootUri` field is incorrect in your configuration. As far as I know, some clients support users to customize this `rootUri` field. If your configuration is correct or has not been configured, please report it to the client developer you are using.