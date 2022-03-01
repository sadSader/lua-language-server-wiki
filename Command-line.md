`BINRARY/lua-language-server [entry] [options...]`

* entry: The main script file `LUA_LANGUAGE_SERVER/main.lua`. If not specified, it will try to load `BINRARY/../main.lua`, This is the default location relationship used by the VSCode release.
* options
    * `--logpath=D:/log`: The write location of the log, default is `LUA_LANGUAGE_SERVER/log`
    * `--metapath=D:/meta`: Generation location of definition file of standard library, default is `LUA_LANGUAGE_SERVER/meta`
    * `--locale=en-us`: The localized language used, will override the client's settings. Currently supports `en-us`, `zh-cn` and `pt-br`
    * `--configpath="config.json"`: Load local config path, the path can be related to workspace. Once you use this parameter, the settings will no longer be read from other places (for example VSCode client). [Learn more here](https://github.com/sumneko/lua-language-server/wiki/Setting-without-VSCode).
    * `--version`: Shows the version number. The server will immediately exit.
    * `--check`: Run offline diagnostic, [Learn more here](https://github.com/sumneko/lua-language-server/wiki/Offline-Diagnostic)