`BINRARY/lua-language-server LUA_LANGUAGE_SERVER/main.lua --logpath=D:/log --metapath=D:/meta --locale=en-us --develop=false --dbgport=11411 --dbgwait=false`

* logpath: The write location of the log, default is `LUA_LANGUAGE_SERVER/log`
* metapath: Generation location of definition file of standard library, default is `LUA_LANGUAGE_SERVER/meta`
* locale: The localized language used, will override the client's settings. Currently supports `en-us` and `zh-cn`
* develop: Enabling developer mode
* dbgport: Listing debugger port(For developer)
* dbgwait: Wait debugger connecting(For developer)