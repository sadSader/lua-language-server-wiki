你可以使用命令行对你的工作区进行离线诊断，并将诊断结果输出到文件中。

---

You can use the command line to perform offline diagnostics on your workspace and output the diagnostic results to a file

## 快速开始|Quick Start

`lua-language-server --check D:\github\lua-language-server --checklevel=Warning --locale=zh-cn`

如果诊断发现了问题，这些问题保存到 `LOGPATH/check.json` 中。  
该指令结束后服务器进程会立即退出。

---

If the diagnostics find problems, they are saved to `LOG/check.json`.  
The server process will exit immediately after the command ends.

## 诊断原理|Diagnostic Principle

该命令会启动一个虚拟的客户端，此客户端会通知服务器加载指定的工作区，并将服务器发送过来的诊断结果收集起来输出到文件中。  
因此这个诊断会使用你工作区中的 `.luarc.json` 等配置，当然你也可以通过 `--configpath="config.json"` 来指定一个额外的配置文件。  
注意，多个配置文件会按照优先级合并，如果你想要覆盖工作区中的 `.luarc.json` 里的一些配置字段，你必须在额外配置文件中也声明这些配置字段而不是省略这些字段。  
如果你想改变输出文件的路径，可以使用 `--logpath='XXXX'`。  
诊断时虚拟客户端会打开工作区中的所有文件，因此 `Lua.diagnostics.neededFileStatus` 设置中的 `Opened` 与 `Any` 效果是相同的。当然设置为 `None` 依然可以禁用诊断，不过还是推荐通过 `Lua.diagnostics.disable` 来禁用不需要的诊断。  
`--checklevel=Warning` 可以快速的去除不重要的诊断，它的可用值为 `Error`, `Warning`, `Information` 与 `Error`，默认值就是 `Warning`，因此通常情况下你不需要这个参数。

---

This command will start a virtual client, which will notify the server to load the specified workspace, and collect and output the diagnostic results sent by the server to a file.  
So this diagnostic will use `.luarc.json` in your workspace, etc. Of course you can also specify an additional config file with `--configpath="config.json"`.  
Note that multiple configuration files will be merged according to priority, if you want to override some configuration fields in `.luarc.json` in the workspace, you must also declare these configuration fields in additional configuration file instead of omitting these fields .  
If you want to change the path of the output file, you can use `--logpath='XXXX'`.  
The virtual client opens all files in the workspace when diagnosing, so `Opened` in the `Lua.diagnostics.neededFileStatus` setting has the same effect as `Any`. Of course setting to `None` can still disable diagnostics, but it is recommended to disable unnecessary diagnostics via `Lua.diagnostics.disable`.  
`--checklevel=Warning` can quickly remove unimportant diagnostics, its available values ​​are `Error`, `Warning`, `Information` and `Error`, the default value is `Warning`, so usually you do not need this parameter.

## 输出格式|Output Format

诊断结果会以json的格式保存，它的格式定义为： { \[uri: [DocumentUri](https://microsoft.github.io/language-server-protocol/specifications/specification-3-17/#documentUri)\]: [Diagnostic](https://microsoft.github.io/language-server-protocol/specifications/specification-3-17/#diagnostic)\[\]; }

---

The diagnostic result will be saved in json format, its format is defined as: { \[uri: [DocumentUri](https://microsoft.github.io/language-server-protocol/specifications/specification-3-17/#documentUri)\]: [Diagnostic](https://microsoft.github.io/language-server-protocol/specifications/specification-3-17/#diagnostic)\[\]; }


