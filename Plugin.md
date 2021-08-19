(This wiki is translated by a translator. You are free to improve the content)

**Attention：This feature is use to support your custom syntax, it will not hint your custom syntax are errors, and will save the replaced file content to LOGPATH/diffed.lua when you set the `Lua.misc.parametes:[--develop=true ]` configuration. But it will not replace the content in the vscode's texteditor's content，which is a formatting feature now this server not have**。

Create `.vscode/lua/plugin.lua` in your workspace or other path.
You should specify this path by setting `Lua.runtime.plugin`.
For security reasons, this setting is empty by default (meaning that no local files can be loaded).

## OnSetText

The code in your workspace is converted through this function. This function can be used to parse some custom syntax.

![plugin-diff](https://github.com/sumneko/vscode-lua/blob/master/images/plugin-diff.gif?raw=true)

The code on the right side of the figure above is the code you are entering in the editor, and the code on the left side of the figure above is the code read by the extension.

Whenever you enter content, the extension calls this function and passes in the content of the entire file as a parameter. You need to generate a list of differences, which will be used by the extension to transform the contents of the file.

The effect of the figure above is achieved by the following code:

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

After enable developer mode, you can find `diffed.lua` in the log directory, the content of which is the new text after the application differences, for you to debug.
On VSCode, set 

```
  "Lua.misc.parameters": [
    "--develop=true"
  ],
``` 
to enable developer mode.
On other clients, use `--develop=true` in command line to enable developer mode.