## Feature
* code full text formatting
* code range formatting
* code style check 

## How to configure
supports project configuration through the `.editorconfig` file. The `.editorconfig` file must be in the project root directory.
If you want different directories under the same project to use different configurations, you can add `.editorconfig` files to different directories.
Or if you want different files in the current directory to use different configurations, you can configure different files according to the way supported by `.editorconfig`.

For configuration documents, please refer to https://github.com/CppCXY/EmmyLuaCodeStyle

## Create a template configuration
create a `.editorconfig` file in the project root and copy from [template](https://github.com/CppCXY/EmmyLuaCodeStyle/blob/master/lua.template.editorconfig)

## How to setup default format options
If you do not want to set the common format options for every project via `.editorconfig`, you can configure the default format options.

#### For Neovim

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

#### For VSCode

```json
"Lua.format.defaultConfig":{
    "indent_style":"space",
    "indent_size":"2"
}
```

`NOTE`: In fact, the indentation cannot be set by the above setting method. indentation uses the editor's indentation settings for the current file

## How to enable code style check

#### For Neovim

```lua
Lua = {
  diagnostics = {
    neededFileStatus = {
      ["codestyle-check"] = "Any",
    },
  },
}
```

## Feature request and bug reports
https://github.com/CppCXY/EmmyLuaCodeStyle/issues