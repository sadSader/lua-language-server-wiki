#### `Lua.IntelliSense.traceLocalSet`

* `true`
```lua
local x -- x is `number|boolean`
x = 1.0
x = true
```

* `false`
```lua
local x -- x is `any`
x = 1.0
x = true
```

* either `true` or `false`
```lua
local x = 1.0 -- x is `number`
```

#### `Lua.IntelliSense.traceReturn`

* `true`
```lua
local x -- find references of `x`
local function f()
    return x -- `x` can be found
end
local y = f() -- `y` can be found
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

* `true`
```lua
local x -- find references of `x`
local y = x -- `y` can be found
```

* `false`
```lua
local x -- find references of `x`
local y = x -- `y` can NOT be found
```

#### `Lua.IntelliSense.traceFieldInject`

* `true`
```lua
local t = {} -- t has field `x`
local h = t
h.x = 1
```

* `false`
```lua
local t = {} -- t is an empty table
local h = t
h.x = 1
```