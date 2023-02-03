# Project File Structure
Below is an explanation of the project file structure. Hovering an item will show a description, clicking a linked item will jump to more detailed info, if available.

Files marked with `⛔` are ignored by git.

<pre>

📦 lua-language-server/
    ├── 📁 <a href="#github" title="Github-specific files">.github/</a>
    ├── 📁 <a href="#vscode" title="VS Code files for development">.vscode/</a>
    ├── 📁 <span title="Git submodule dependencies">3rd/</span>
    ├── 📁 <span title="Built binaries">bin/</span> <span title="ignored">⛔</span>
    ├── 📁 <span title="Documentation for the settings">doc/</span>
    ├── 📁 <span title="Text and translations used all over the server">locale/</span>
    ├── 📁 <span title="Default log location">log/</span> <span title="ignored">⛔</span>
    ├── 📁 <span title="Files for building">make/</span>
    ├── 📂 <span title="Lua definition files">meta/</span>
    │    ├── 📁 <a href="#3rd" title="Lua definitions for third party libraries e.g. love2d, OpenResty">3rd/</a>
    │    ├── 📁 <span title="Templates for the built-in Lua libraries that will be generated according to the requested Lua version, language ID, and file encoding">template/</span>
    │    └── 📂 <span title="Generated definition files for built-in Lua libraries. There will be a folder for each variation">Lua ${LUA_VERSION} ${LANGUAGE_ID} ${ENCODING}/</span> <span title="ignored">⛔</span>
    ├── 📂 <span title="Code executed by the language server">script/</span>
    │    ├── 📁 <span title="Sub-thread workers that provide &quot;read protocol from standard input&quot;, &quot;read file content&quot; and &quot;regularly wake up the main thread&quot;">brave/</span>
    │    ├── 📁 <span title="Provide CLI support (--version, --check)">cli/</span>
    │    ├── 📁 <span title="Configuration file handling">config/</span>
    │    ├── 📁 <span title="Provides core language features. Files are named the same as the feature they implement">core/</span>
    │    ├── 📁 <span title="Convert encodings between asni, utf8, utf16">encoder/</span>
    │    ├── 📁 <a href="https://github.com/LuaLS/lua-glob" title="Used to resolve glob patterns">glob/</a>
    │    ├── 📂 <a href="#scriptparser" title="Parses Lua code into an abstract syntax tree (AST). Most of the children files are obsolete, only the ones still in use are documented.">parser/</a>
    │    │    ├── 📜 <span title="Provide utility functions, for example getVisibleLocals(source, position), getParentFunction(source) and positionToOffset(state, position)">guide.lua</span>
    │    │    ├── 📜 <span title="Parses annotations from state.comments">luadoc.lua</span>
    │    │    ├── 📜 <a href="#scriptparsernewparserlua" title="Parses Lua code into an AST then wraps it into state">newparser.lua</a>
    │    │    └── 📜 <a href="https://github.com/sqmedeiros/lpeglabel" title="Split strings into tokens. From sqmedeiros/lpeglabel">tokens.lua</a>
    │    ├── 📂 <span title="Code for Language Server Protocol (LSP)">proto/</span>
    │    │    ├── 📜 <span title="Convert AST values into something the LSP can use. 50003 -> { line = 5, character = 3 }">converter.lua</span>
    │    │    ├── 📜 <span title="Definitions of constants">define.lua</span>
    │    │    └── 📜 <span title="Communicates with the client">proto.lua</span>
    │    ├── 📂 <span title="Bridges LSP requests with core features">provider/</span>
    │    │    ├── 📜 <span title="Manages the diagnostic push service">diagnostic.lua</span>
    │    │    └── 📜 <span title="Registers the server's capabilities with the client so it knows what is supported">provider.lua</span>
    │    ├── 📁 <span title="Host for subthreads">pub/</span>
    │    ├── 📁 <span title="Server runtime and event loop">service/</span>
    │    ├── 📁 <span title="Contains unit tests">test</span>
    │    ├── 📁 <span title="Various tools for development">tools</span>
    │    ├── 📂 <a href="#scriptvm" title="Semantic analysis of the AST and binding status according to the workspace">vm/</a>
    │    │    ├── 📜 <span title="Provides vm.compileNode(source) --> node">compiler.lua</span>
    │    │    ├── 📜 <span title="Provides vm.getDefs(source) --> source[]">def.lua</span>
    │    │    ├── 📜 <span title="Provides annotation features">doc.lua</span>
    │    │    ├── 📜 <span title="Provides vm.getFields(source) --> source[]">field.lua</span>
    │    │    ├── 📜 <span title="Resolve generics by proto, sign, and call args">generic.lua</span>
    │    │    ├── 📜 <span title="Manages global variables and types">global.lua</span>
    │    │    ├── 📜 <span title="Provides infer class for inferring types of sources">infer.lua</span>
    │    │    ├── 📜 <span title="Manages local variables">local-id.lua</span>
    │    │    ├── 📜 <span title="Provides node class">node.lua</span>
    │    │    ├── 📜 <span title="Provides vm.getRefs(source) --> source[]">ref.lua</span>
    │    │    ├── 📜 <a href="#scriptvmrunnerlua" title="Provides vm.compileNode(source) --> node">runner.lua</a>
    │    │    └── 📜 <span title="Create generic instance">sign.lua</span>
    │    ├── 📂 <span title="Manages workspace">workspace/</span>
    │    │    ├── 📜 <span title="Workspace loading process">loading.lua</span>
    │    │    ├── 📜 <span title="Compute require filename">require-path.lua</span>
    │    │    ├── 📜 <span title="Provides scope class, adds support for multiple workspaces">scope.lua</span>
    │    │    └── 📜 <span title="Provides workspace features">workspace.lua</span>
    │    ├── 📜 <span title="Simple coroutine library">await.lua</span>
    │    ├── 📜 <span title="Contains wrapped request from server to client.Modifies configuration file">client.lua</span>
    │    ├── 📜 <span title="Manages files">files.lua</span>
    │    ├── 📜 <span title="Provide support for multiple languages">language.lua</span>
    │    ├── 📜 <span title="Fake client for cli and tests">lclient.lua</span>
    │    ├── 📜 <span title="Meta related features">library.lua</span>
    │    ├── 📜 <a href="https://github.com/LuaLS/lua-language-server/wiki/Plugins" title="Adds support for plugins">plugin.lua</a>
    ├── 📜 <span title="Is used when attaching debugger with --develop parameter">debugger.lua</span>
    ├── 📜 <span title="Entry file for testing">test.lua</span>
    └── 📜 main.lua

</pre>

## `.github/`
Github-specific files for metadata, issue templates, etc.

[Return to tree](#project-file-structure)

## `.vscode/`
Visual Studio Code specific files for development.

[Return to tree](#project-file-structure)

## `3rd/`
Contains Lua defintion files for various included [libraries](https://github.com/LuaLS/lua-language-server/wiki/Libraries) like `love2d` and `OpenResty`.

[Return to tree](#project-file-structure)

## `script/parser/`
Parses Lua code into an abstract syntax tree (AST).

Turns:
```lua
x = 10
y = 20
```
into:
```lua
{
    type   = 'main',
    start  = 0,
    finish = 20000,
    [1] = {
        type   = 'setglobal',
        start  = 0,
        finish = 1,
        range  = 5,
        [1]    = 'x',
        value  = {
            type   = 'integer',
            start  = 4,
            finish = 6,
            [1]    = 10
        },
    },
    [2] = {
        type   = 'setglobal',
        start  = 10000,
        finish = 10001,
        range  = 10005,
        [1]    = 'y',
        value  = {
            type   = 'integer',
            start  = 10004,
            finish = 10006,
            [1]    = 20
        },
    },
}
```

> ℹ️ Note: first line is `0`, start is cursor position on the left and finish is cursor position on the right.

> ℹ️ Note: `position = row * 10000 + col`, therefore, only codes with fewer than 10000 bytes in a single line are supported. These nodes are generally named source.

> ℹ️ Note: Most of the children files are obsolete, only the ones still in use are documented.

[Return to tree](#project-file-structure)


## `script/parser/newparser.lua`
Parses Lua code into an AST then wraps it into `state`.

```lua
local state = {
    version = 'Lua 5.4',
    lua     = [[local x = 1]],
    ast     = { ... },
    errs    = { ... }, -- syntax errors
    comms   = { ... }, -- comments
    lines   = { ... }, -- map of offset and position
}
```

[Return to tree](#project-file-structure)

## `script/vm/`
Semantic analysis of the AST and binding status according to the workspace.

Turns:
```lua
---@class myClass
local mt
```

into:

```lua
vm.compileNode('mt')

-->

node: {
    [1] = {
        type = 'local',
        [1]  = 'mt',
    },
    [2] = {
        type = 'global',
        cate = 'type',
        name = 'myClass',
    },
}
```

[Return to tree](#project-file-structure)

## `script/vm/runner.lua`
Process analysis and tracking of local variables

```lua
---@type number|nil
local x

if x then
    print(x) --> `x` is number here
end
```

[Return to tree](#project-file-structure)
