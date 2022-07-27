# Frequently Asked Questions

## Where Can I Find the Log File?

For debugging and a more detailed log file, you can add [`--loglevel=trace`](https://github.com/sumneko/lua-language-server/wiki/Getting-Started#loglevel) to [`misc.parameters`](https://github.com/sumneko/lua-language-server/wiki/Settings#miscparameters) or directly to the command line, if using it. If you are filing a bug report, please do so as it provides more info for the development team.

**VSCode**
- Windows: `%homepath%\.vscode\extensions\sumneko.lua-X.X.X\server\log\`
- WSL: `\\wsl$\Ubuntu-20.04\home\USERNAME\.vscode-server\extensions\sumneko.lua-X.X.X\server\log\`
- Linux/MacOS: `~/.vscode/extensions/sumneko.lua-X.X.X/server/log/`

**Other**

- `lua-language-server/log/`
- `sumneko_lua/log/`

You can also specify a custom location for the logs through the [command line](https://github.com/sumneko/lua-language-server/wiki/Getting-Started#logpath). This can be specified using the [`misc.parameters`](https://github.com/sumneko/lua-language-server/wiki/Settings#miscparameters) setting when using Visual Studio Code.


## Why is the Server Scanning the Wrong Folder?
When a workspace is opened, the client will send the URI of the directory to be scanned. When you open a single file, the client is supposed to send `null` for the URI as there is no workspace, just a single file. However, some clients will mistakenly send the URI of the extension, or worse, the home directory. The server will do as it is told and scan what is sent, which can obviously cause issues should the home directory be sent.

### How to Check the URI Being Sent
[Find the server log](#where-can-i-find-the-log-file) for your OS and then search for `Client init`. You should find something similar to the below with the `rootUri` field.

```log
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

### How Do I Fix This?
In Visual Studio Code, this should never happen as the `sumneko.lua` client is provided by the owner of this language server. Should this be the case, [please open an issue](https://github.com/sumneko/lua-language-server/issues/new?title=rootUri%20is%20incorrect%20in%20VS%20Code%20extension).

When not using Visual Studio Code, check the `rootUri` field in your configuration - some clients allow users to customize this value. If the value is correct or cannot be modified, please report the issue to the developer of the client you are using.


## How Can I Improve Startup Speeds?

**TL;DR:** Only include necessary libraries and ignore as many files/directories as you can.

The most effective (and obvious) way to improve startup times is to load fewer files. The startup time of the server is proportional to the total size of all Lua files in your workspace. Try excluding unnecessary directories and files using [`workspace.ignoreDir`](https://github.com/sumneko/lua-language-server/wiki/Settings#workspaceignoredir).

When using Visual Studio Code, you can easily specify different settings for each project/workspace by adding a `.vscode/settings.json` file. This allows you to include specific libraries per project, removing the need to include libraries globally, which slow down **all** projects, regardless of if they need the library or not.

When not using Visual Studio Code (or even with), you can accomplish the same with a [`.luarc.json` file](https://github.com/sumneko/lua-language-server/wiki/Configuration-File#luarcjson), otherwise, you will have to define libraries globally and this can have a **severe** impact on performance if the library is large.

> To get a better understaning of startup times, you may want to view the [performance benchmarking test](https://github.com/sumneko/lua-language-server/wiki/Benchmark).

<br>

---

Question still unanswered? Ask away on the [discussions page](https://github.com/sumneko/lua-language-server/discussions/categories/q-a)!

> Please make sure to check for duplicate discussions first :heart:
>
> It can help you find an answer quicker and helps keeps things organized.
