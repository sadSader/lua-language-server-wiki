#### `Lua.IntelliSense.traceLocalSet`

* `true`
```lua
local x -- x is `number|boolean`
x = 1
x = true
```

* `false`
```lua
local x -- x is `any`
x = 1
x = true
```

#### `Lua.IntelliSense.traceReturn`

* `true`
```lua
local x -- find references of `x`
local function f()
    return x -- `x` is found
end
local y = f() -- `y` is found
```

* `false`
```lua
local x -- find references of `x`
local function f()
    return x -- `x` can be found
end
local y = f() -- `y` can NOT be found
```

#### `Lua.IntelliSense.traceBeSetted`
#### `Lua.IntelliSense.traceGlobalInject`