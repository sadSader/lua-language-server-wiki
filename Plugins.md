# Plugins
Plugins can be used to modify how the language server works.

## Usage
This does not provide hinting or reporting of custom syntax errors, however, it will let you create a custom syntax that will then be output to a separate file.

![](https://github.com/sumneko/vscode-lua/raw/master/images/plugin-diff.gif?raw=true)

## Setup
1. Add `--develop=true` to [`Lua.misc.parameters`](https://github.com/sumneko/lua-language-server/wiki/Settings#miscparameters)
   - This allows the plugin to write to [`LOGPATH/diffed.lua`](https://github.com/sumneko/lua-language-server/wiki/FAQ#where-can-i-find-the-log-file)
2. Create `.vscode/lua/plugin.lua` in your workspace (or some other absolute location)
3. Specify the path of the plugin via the [`Lua.runtime.plugin`](https://github.com/sumneko/lua-language-server/wiki/Settings#plugin) setting

[Debug the plugin](https://github.com/sumneko/lua-language-server/wiki/Developing#debugging)

## Functions


### `OnSetText(uri, text)`
This function provides the uri and text of the file that has been edited and expects a list of differences to be returned. The result will be written to `diffed.lua` in your [log location](https://github.com/sumneko/lua-language-server/wiki/FAQ#where-can-i-find-the-log-file).

#### Definition
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

## Example
![](https://github.com/sumneko/vscode-lua/raw/master/images/plugin-diff.gif?raw=true)

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
## Template
https://github.com/sumneko/sample-plugin
