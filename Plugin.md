(This wiki is translated by a machine translator. You are free to improve the content!)

**Notice：This feature allows you to add your own custom doc syntax format to the language server. It will not hint your custom syntax as errors, and will save the replaced file content to `LOGPATH/diffed.lua` when you set `Lua.misc.parameters:[--develop=true ]` in configuration. But it will not replace the content in VS Code's text editor, which is a formatting feature this server does not implement.

Create `.vscode/lua/plugin.lua` in your workspace (or other path), specifying this path via the setting `Lua.runtime.plugin`.
For security reasons, this setting is empty by default (meaning that no local files can be loaded).

[Debug your plugin](https://github.com/sumneko/lua-language-server/wiki/Debug)

## OnSetText

The `OnSetText(uri, text)` function virtually transforms the code in your workspace for the language server, allowing you to parse custom syntax. The code being entered in the editor is shown on the right side of the below figure, and the result after transformation (i.e. the code read by the language server) is on the left:

![plugin-diff](https://github.com/sumneko/vscode-lua/blob/master/images/plugin-diff.gif?raw=true)

Whenever you enter new content, the extension calls this function and passes the entire file content as a parameter. You need to generate a list of differences, which will be used by the extension to transform the contents of the file.

The effects shown in the demo figure above is achieved by the following code:

```lua
function OnSetText(uri, text)
    if text:sub(1, 4) ~= '--##' then
        return nil
    end
    local diffs = {}
    diffs[#diffs+1] = {
        start  = 1,
        finish = 4,
        text   = '',
    }

    for localPos, colonPos, typeName, finish in text:gmatch '()local%s+[%w_]+()%s*%:%s*([%w_]+)()' do
        diffs[#diffs+1] = {
            start  = localPos,
            finish = localPos - 1,
            text   = ('---@type %s\n'):format(typeName),
        }
        diffs[#diffs+1] = {
            start  = colonPos,
            finish = finish - 1,
            text   = '',
        }
    end

    return diffs
end
```

The complete definition is as follows:

```lua
---@class diff
---@field start  integer # The number of bytes at the beginning of the replacement
---@field finish integer # The number of bytes at the end of the replacement
---@field text   string  # What to replace

---@param  uri  string # The uri of file
---@param  text string # The content of file
---@return nil|diff[]
function OnSetText(uri, text) end
```

After enabling developer mode, you can find `diffed.lua` in the log directory, the content of which is the new text after the application differences, for you to debug.
In VSCode, set

```
  "Lua.misc.parameters": [
    "--develop=true"
  ],
``` 
to enable developer mode.
On other clients, use `--develop=true` in the command line to enable developer mode.

Sample: https://github.com/sumneko/sample-plugin