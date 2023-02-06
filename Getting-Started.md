# Getting Started
This page goes over getting set up in the various environments that the Lua language server can be used in.

You'll probably want to get familiar with the [annotations](https://github.com/LuaLS/lua-language-server/wiki/Annotations).


## Visual Studio Code
Using the language server through the VS Code extension is a super easy and convenient way to get set up and start coding.

### Install
[![Install in VS Code](https://img.shields.io/badge/Install%20For-VS%20Code-blue?style=for-the-badge&logo=visualstudiocode "Install in VS Code")](https://marketplace.visualstudio.com/items?itemName=sumneko.lua)

The Visual Studio Code extension can be installed from the [marketplace](https://marketplace.visualstudio.com/items?itemName=sumneko.lua) or found in VS Code under `sumneko.lua`.

### Configuration
Configuration of the extension can be done from your VS Code settings (<kbd>Ctrl + ,</kbd>). Enter `@ext:sumneko.lua` in the search bar at the top and you will be presented with [all of the settings](https://github.com/LuaLS/lua-language-server/wiki/Settings) for the extension.


## Command Line
The Lua language server can be run straight from the command line.

You may be able to get it from your package manager:

- Windows
  - Scoop: `scoop install lua-language-server`
- MacOS
  - Homebrew: `brew install lua-language-server`
  - Macports: `sudo port install lua-language-server`

You can also find precompiled binaries attached to [each release](https://github.com/LuaLS/lua-language-server/releases) and [prerelease from the GitHub actions](https://github.com/LuaLS/lua-language-server/actions) for Windows, Linux, and MacOS.

Note that you can't simply create a symbolic link to the binary in one of the directories on your `$PATH`, since `lua-language-server` expects to find the scripts in a fixed location relative to the directory it is run from. Instead, create a wrapper script with the contents:
```bash
#!/bin/bash
exec "<path-to-directory>/bin/lua-language-server" "$@"
```

You can also build it yourself.

### Build
If you don't have a precompiled binary, you can build the server yourself.

1. Install [Ninja](https://github.com/ninja-build/ninja/wiki/Pre-built-Ninja-packages)
2. Ensure you have C++17
3. Clone the project

```bash
git clone https://github.com/LuaLS/lua-language-server
cd lua-language-server
```

*Continue below with your OS of choice.*
- [Windows](#windows)
- [Linux/MacOS](#linuxmacos)

#### Windows

```bash
.\make.bat
```

#### Linux/MacOS

```bash
.\make.sh
```


### Run

#### Windows
```bash
.\bin\lua-language-server.exe
```

#### Linux/MacOS
```bash
./bin/lua-language-server
```

#### Arguments
There are a few arguments that can be provided when running the language server from the command line.

##### entry
*optional*

Type: string

The main Lua script file from the root of [this repository](https://github.com/LuaLS/lua-language-server/blob/master/main.lua). If omitted, the application will attempt to load `bin/../main.lua`.

#### Flags
There are a few optional flags that can alter how the server operates.

##### --doc
**Type:** `string`

Where to create documentation JSON and MarkDown files.

Example: `--doc=C:/Users/Me/Documents/LuaDocuments`

##### --logpath
**Type:** `string`

Where the log should be written to. Defaults to `./log`

Example: `--logpath=D:/luaServer/logs`

##### --loglevel
**Type:** `string`

The minimum level of logging that should appear in the logfile. Can be used to log more detailed info for debugging and error reporting.

Options:
- `trace`

Example: `--loglevel=trace`

##### --metapath
**Type:** `string`

Where the standard Lua library definition files should be generated to. Defaults to `./meta`

Example: `--metapath=D:/sumnekoLua/metaDefintions`

##### --locale
**Type:** `string`

The language to use. Defaults to `en-us`. Options can be found in [`locale/`](https://github.com/LuaLS/lua-language-server/tree/master/locale)

Example: `--locale=zh-cn`

##### --configpath
**Type:** `string`

The location of the configuration file that will be loaded. Can be relative to the workspace. When provided, config files from elsewhere (such as from VS Code) will no longer be loaded.

Example: `--configpath=sumnekoLuaConfig.lua`

##### --version
**Type:** `boolean`

Get the version of the Lua language server. This will print it to the command line and immediately exit.

##### --check
**Type:** `boolean`

Perform a "[diagnosis report](https://github.com/LuaLS/lua-language-server/wiki/Diagnosis-Report)" where the results of the diagnosis are written to a file.

##### --checklevel
**Type:** `string`
**Default:** `Warning`

To be used with [`--check`](#-check). The minimum level of diagnostic that should be logged. Items with lower priority than the one listed here will not be written to the file. Options include, in order of priority:

1. `Error`
2. `Warning`
3. `Information`

Example: `--checklevel=Information`

### Configuration
The server loads its settings from one of the following sources, in order:

1. The file specified by [`--configpath`](#-configpath)
2. A `.luarc.json` file in the workspace
3. The configuration file sent from the LSP client (like from VS Code)

For more details, go to the [configuration file page](https://github.com/LuaLS/lua-language-server/wiki/Configuration-File).

<br>

---

### Common Issues
A list of common issues when executing/building from the command line.

- Compile error `/usr/bin/ld: cannot find -lstdc++` (or similar) when running `install.sh`
  - You may need to install `libstdc++`. On Fedora linux or similar run:
    ```bash
    dnf install libstdc++-static
    ```
