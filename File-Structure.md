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

<details>
<summary>subdirectories</summary>

## 3rd
definition files for built-in 3rd library, e.g. `love2d`, `OpenResty`.

## template
definition template files for built-in library, e.g. `io`, `table`  
after the language server is started, real definition files will be generated according to your Lua version, language ID and file encoding

## Lua {LUA_VERSION} {LANGUAGE_ID} {FILE_ENCODING}
definition files for built-in library, ignored in git

</details>

# script
code executed by the language server

<details>
<summary>subdirectories</summary>

## brave
sub thread workers, provide "read protocol from standard input", "read file content" and "regularly wake up the main thread"

## cli
provide `--version` and `--check`, see https://github.com/sumneko/lua-language-server/wiki/Command-line

## config

## core
provide language features

the file name is the feature, so it will not be introduced separately

## encoder
convert encoding between `ansi`, `utf8` and `utf16`

## glob
[lua-glob](https://github.com/sumneko/lua-glob)  
Used to resolve `abc/*/[1-9].lua`

## parser
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

<details>
<summary>subdirectories</summary>

### guide.lua
provide utility functions, for example `getVisibleLocals(source, position)`, `getParentFunction(source)` and `positionToOffset(state, position)`

### luadoc.lua
parse EmmyLua from `state.comments`

### newparser.lua
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

### tokens.lua
split text into tokens by `LpegLabel`

</details>

## proto
LSP related

<details>
<summary>subdirectories</summary>

### converter.lua
`50003` -> `{ line = 5, character = 3 }`

### define.lua
consts

### proto.lua
communication with client

</details>

## provider
bridging LSP requests with core features

<details>
<summary>subdirectories</summary>

### diagnostic.lua
manage diagnostic push service

### provider.lua
register server capability

</details>

## pub
sub thread host

## service
server runtime and event loop

## vm
semantic analysis of the abstract syntax tree, and binding status according to the workspace files

```lua
---@class myClass
local mt
```

```lua
vm.compileNode('mt')

-->

{
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

<details>
<summary>subdirectories</summary>

</details>

</details>

# test

# tools

# debugger.lua
provide `debugger attach` with parameters `--develop`

# main.lua
entry file for language server

# test.lua
entry file for testing
