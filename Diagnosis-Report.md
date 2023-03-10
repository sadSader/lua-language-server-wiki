# Diagnosis Report
A diagnosis report is a file that can be generated by the language server that provides the same info a client usually receives.

## Create a Report
You can use the [command line](https://github.com/LuaLS/lua-language-server/wiki/Getting-Started#command-line) to perform a diagnosis of your workspace and export the results to a file. This report will use the [`.luarc.json`](https://github.com/LuaLS/lua-language-server/wiki/Configuration-File#luarcjson) in the target workspace (unless otherwise specified with [`--configpath`](https://github.com/LuaLS/lua-language-server/wiki/Getting-Started#configpath))

The command should looks like this:

`lua-language-server --check E:\programming\myLuaProject --checklevel=warning`

The server will exit once the report is complete. The report will be written to `check.json` in your [logpath](https://github.com/LuaLS/lua-language-server/wiki/FAQ#where-can-i-find-the-log-file) (unless otherwise specified with [`--logpath`](https://github.com/LuaLS/lua-language-server/wiki/Getting-Started#logpath)).

## How it Works
The check command will start a virtual client that opens all files in the target workspace, retrieves all diagnostic info, and writes it to a file.

>ℹ️ Note: Since each file is being opened, `"Opened"` has the same effect as `"Any"` for [`Lua.diagnostics.neededFileStatus`](https://github.com/LuaLS/lua-language-server/wiki/Settings#diagnosticsneededfilestatus) and [`Lua.diagnostics.groupFileStatus`](https://github.com/LuaLS/lua-language-server/wiki/Settings#diagnosticsneededfilestatus).
