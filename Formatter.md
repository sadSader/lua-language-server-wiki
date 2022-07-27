# Formatter
The language server also offers a code formatter so you can easily make your code prettier.

It also offers [code style checking](#)

This functionality is implemented by [`CppCXY/EmmyLuaCodeStyle`](https://github.com/CppCXY/EmmyLuaCodeStyle).

For any issues related to the formatter, please create them in the [respective repository](https://github.com/CppCXY/EmmyLuaCodeStyle/issues).

## Configuration
The configuration file must be called `.editorconfig` and uses YAML syntax. This file must be in the project root, however, you can have multiple in different directories if you want the various directories to format differently.

A template `.editorconfig` can be found at [`CppCXY/EmmyLuaCodeStyle/lua.template.editorconfig`](https://github.com/CppCXY/EmmyLuaCodeStyle/blob/master/lua.template.editorconfig).

For more info, refer to [`CppCXY/EmmyLuaCodeStyle`](https://github.com/CppCXY/EmmyLuaCodeStyle).

### Default Configuration
To set a default global configuration across projects, navigate to your [configuration file](https://github.com/sumneko/lua-language-server/wiki/Configuration-File) and perform the below:

### JSON
This format is used by Visual Studio Code's `settings.json` and `.luarc.json` files.
```json
"Lua.format.defaultConfig":{
    "indent_style":"space",
    "indent_size":"2"
}
```

### Lua
This format is used by custom configuration files, often used with Neovim.
```lua
Lua = {
  format = {
    enable = true,
    -- Put format options here
    -- NOTE: the value should be STRING!!
    defaultConfig = {
      indent_style = "space",
      indent_size = "2",
    }
  },
}
```

> ℹ️ Note: Although indentation is used in the examples above, it cannot be customized using the above method as the editor's indentation settings will be used instead.


# Code Style Checking
To enable code style checking, you need to add the following entry to [`diagnostics.neededFileStatus`](https://github.com/sumneko/lua-language-server/wiki/Settings#diagnosticsneededfilestatus):

## JSON
```json
["codestyle-check"]: "Any"
```

## Lua
```lua
["codestyle-check"] = "Any"
```
