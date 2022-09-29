# Tips
This page includes some lesser known tips that don't really fit anywhere else in the wiki.



## Support for Hex Colours
The language server supports hexadecimal colour definitions, which allows your editor to display a colour picker.

![Colour picker in Visual Studio Code](https://user-images.githubusercontent.com/61925890/193044002-512cb379-2968-4613-ba90-2fe15d8aa876.png)

The colour picker will appear when you define an 8 character hexadecimal value in a string (with no leading `#`).



## Inlay Hints
Inlay hints are little bits of context that can be enabled to appear in your code.

![Contextual hints appearing in code](https://user-images.githubusercontent.com/61925890/193045115-317f15ed-d0b5-4240-959f-09e41200e5e7.png)

They are disabled by default but we'll walk through how to enable and configure them.

### Enable

1. If you are using VS Code, configure `editor.inlayHints.enabled` to your liking.
2. Set [`Lua.hint.enable`](https://github.com/sumneko/lua-language-server/wiki/Settings#hintenable) to true

### Configure
There are many different settings that let you change when hints appear. These settings can be seen on the [settings page](https://github.com/sumneko/lua-language-server/wiki/Settings#hint).
