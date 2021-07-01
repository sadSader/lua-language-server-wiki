`BINRARY/lua-language-server LUA_LANGUAGE_SERVER/main.lua --logpath=D:/log --metapath=D:/meta --locale=en-us --configpath="config.json"`

* logpath: The write location of the log, default is `LUA_LANGUAGE_SERVER/log`
* metapath: Generation location of definition file of standard library, default is `LUA_LANGUAGE_SERVER/meta`
* locale: The localized language used, will override the client's settings. Currently supports `en-us` and `zh-cn`
* configpath: Load local config path, the path can be related to workspace. Once you use this parameter, the settings will no longer be read from other places (for example VSCode client). [Lean more here](https://github.com/sumneko/lua-language-server/wiki/Setting-without-VSCode).