This server have been supporting multi-workspace by server side since 2.6.0.
The client just need to startup one server, then send all lua files to this server(even if they are not included in current workspaces).

> the server does not support dynamically add or remove workspaces. If workspaces change, client should restart server.

# Scope
![](https://github.com/sumneko/vscode-lua/blob/master/images/wiki-workspace.png)

服务器默认会创建一个 `<fallback>` Scope，当你以“单文件模式”启动时，你的文件都会视为在 `<callback>` Scope中。

如果你以工作区模式启动，那么每个工作区都会创建一个对应的Scope。

Scope可以附加一些工作区以外的文件或目录，比如你的工作区的`runtime.version`设置为`Lua 5.4`，那么就会附加上`Lua 5.4`的API目录。你也可以使用`workspace.library`设置来附加更多的目录。

当你打开/创建一个新的Lua文件时，服务器会先检查所有的工作区Scope，如果文件路径属于该Scope的工作目录或附加目录，那么就会分配到此Scope中。如果所有的工作区Scope都不符合，那么文件会被分配到 `<fallback>` Scope中。


每个Scope会拥有独立的环境，用于分离全局变量/class/设置/require可见度等。

-------------------------------------------------

The server will create a `<fallback>` scope by default. When you start in "single file mode", your files will be regarded as in the `<fallback>` scope.    
If you start in workspace mode, each workspace will create a corresponding scope.

Scope can link some files or directories outside the workspace, such as if `runtime.version` is set to `Lua 5.4`, the API directory of `Lua 5.4` will be linked. You can also use `workspace.library` setting to link more directories.

When you open / create a new Lua file, the server will first check all workspace scopes. If the file path belongs to the working directory or linked directory of the scope, it will be assigned to this scope. If all workspace scopes are not compliant, the file will be assigned to `<fallback>` scope.


Each scope will have an independent environment for separating global variables / class / settings / require visibility, etc.