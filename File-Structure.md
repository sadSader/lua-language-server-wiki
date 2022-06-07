Thank you for reading this wiki. I will briefly describe the file construction of this language server.

# 3rd
submodules

# bin
binaries, ignored in git

# locale
locale files, if you want to support a language, you only need to name the folder with the language id

# log
[default log path](https://github.com/sumneko/lua-language-server/wiki/Default-log-path), ignored in git

# make
used for build

# meta
provide definition files.

## meta/3rd
definition files for built-in 3rd library, e.g. `love2d`, `OpenResty`.

## meta/template
definition template files for built-in library, e.g. `io`, `table`  
after the language server is started, real definition files will be generated according to your Lua version, language ID and file encoding

## meta/Lua {LUA_VERSION} {LANGUAGE_ID} {FILE_ENCODING}
definition files for built-in library, ignored in git

# script
code executed by the language server

## script/brave
sub thread workers, provide "read protocol from standard input", "read file content" and "regularly wake up the main thread"

##  script/cli
provide `--version` and `--check`, see https://github.com/sumneko/lua-language-server/wiki/Command-line

## script/config

## script/core
provide language features

the file name is the feature, so it will not be introduced separately

## script/encoder
convert encoding between `ansi`, `utf8` and `utf16`

## script/glob
[lua-glob](https://github.com/sumneko/lua-glob)  
Used to resolve `abc/*/[1-9].lua`

## script/parser
[LuaParser](https://github.com/sumneko/LuaParser)  
parsing Lua code into an abstract syntax tree

```lua
x = 1
y = 1
```

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
            finish = 5,
            [1]    = 1
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
            finish = 10005,
            [1]    = 2
        },
    },
}
```

> first line is 0, `start` is cursor position on the left and `finish` is cursor position on the right
> position = row * 10000 + col, therefore, only codes without more than 10000 bytes in a single line are supported
> these nodes are generally named `source`

most of the files are obsolete, and only the following files are in use

### script/parser/guide.lua
provide utility functions, for example `getVisibleLocals(source, position)`, `getParentFunction(source)` and `positionToOffset(state, position)`

### script/parser/luadoc.lua
parse EmmyLua from `state.comments`

### script/parser/newparser.lua
parsing Lua code into an abstract syntax tree, then wrapping into `state`

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

### script/parser/tokens.lua
split text into tokens by `LpegLabel`

## script/proto
LSP related

### script/proto/converter.lua
`50003` -> `{ line = 5, character = 3 }`

### script/proto/define.lua
consts

### script/proto/proto.lua
communication with client

## script/provider
bridging LSP requests with core features

### script/provider/diagnostic.lua
manage diagnostic push service

### script/provider/provider.lua
register server capability

## script/pub
sub thread host

## script/service
server runtime and event loop

## script/vm
semantic analysis of the abstract syntax tree, and binding status according to the workspace files

```lua
---@class myClass
local mt
```

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

### script/vm/compiler.lua
provide `vm.compileNode(source) --> node`

### script/vm/def.lua
provide `vm.getDefs(source) --> source[]`

### script/vm/doc.lua
provide EmmyLua related features

### script/vm/field.lua
provide `vm.getFields(source) --> source[]`

### script/vm/generic.lua
resolve generic by `proto`, `sign` and `call args`

### script/vm/global.lua
manager for global variables and types

> include `GlobalVar.x.y.z`

### script/vm/infer.lua
provide class `infer`: infer types of sources

### script/vm/local-id.lua
manager for local variables

> include `localVar.x.y.z`

### script/vm/node.lua
class `node`

### script/vm/ref.lua
provide `vm.getRefs(source) --> source[]`

### script/vm/runner.lua
process analysis and tracking for local variables

```lua
---@type number|nil
local x

if x then
    print(x) --> `x` is number here
end
```

### script/vm/sign.lua
create generic instance

## script/workspace
manager of workspace

### script/workspace/loading.lua
workspace loading process

### script/workspace/require-path.lua
compute require name of file

### script/workspace/scope.lua
class `scope`, see [multi workspace supports](https://github.com/sumneko/lua-language-server/wiki/Multi-workspace-supports)

### script/workspace/workspace.lua
provide workspace related features

## script/await.lua
simple coroutine library

## script/client.lua
* wrapped LSP request from server to client
* modify configuration file

## script/files.lua
manager files

## script/language.lua
provide locale supports

## script/lclient.lua
fake client for `cli` and `tests`

## script/library.lua
meta related features

## script/plugin.lua
plugin feature, see [plugin](https://github.com/sumneko/lua-language-server/wiki/Plugin)

# test

# tools

# debugger.lua
provide `debugger attach` with parameters `--develop`

# main.lua
entry file for language server

# test.lua
entry file for testing
