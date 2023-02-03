# Dynamic Type Checking
The Lua language server offers type checking that helps you keep track of the types in your code and warn you when a potential type error is arising.

## Background
Lua is a dynamically typed language, which means the following is completely valid:

```lua
local userInput = nil

userInput = "Hello World!"

userInput = 38

userInput = false
```

The `userInput` variable just changed its type 3 times which is perfectly acceptable in Lua. In statically typed languages, this is not okay - once a variable is assigned a type, it is always that type.

There are pros and cons to each system, which we won't really get into here. However, one thing that is very useful with a statically typed language is you always know what the type of a certain variable is, which makes it easy to tell when the wrong type is being used somewhere. This can also help with debugging as type errors can be reported before the code is executed, a luxury that is missing with Lua, an interpreted dynamically-typed language, until now.

The Lua language server has had a type checking feature since `v3.4.0` that brings Lua *closer* to being a statically typed language.

## How it Works
The language server is able to keep track of types, like in the below example:

```lua
local myString = "Hello World"

myString = false
```

This will trigger a warning [diagnostic](https://github.com/LuaLS/lua-language-server/wiki/Diagnostics), in this case the [`cast-local-type`](https://github.com/LuaLS/lua-language-server/wiki/Diagnostics#cast-local-type) warning. This is of course all [customizable](https://github.com/LuaLS/lua-language-server/wiki/Settings) and you can have it be [reported as an error instead of a warning](https://github.com/LuaLS/lua-language-server/wiki/Settings#neededfilestatus), or not at all!

To get even better diagnostics, you can use [annotations](https://github.com/LuaLS/lua-language-server/wiki/Annotations) to provide more context for the language server to work with.

```lua
local settings = {}

---@param settingName string The name of the setting to set the value of
---@param value any The value of this setting
---@return boolean success If the setting's value was changed successfully
function settings.set(settingName, value) end


settings.set(0, false)
```

That last line will generate a warning because the first parameter, `0`, is not a string, which is what the annotations say the function requires.

While this works great and is way better than the normal "wild west" of Lua, it is not a full type system like the one TypeScript implements. The intention with the type checking in the language server is to not implement such a type checking system. Implementing a similar system would have a large impact on performance and would require a **lot** of work to design and implement (there is a reason TypeScript is developed by [hundreds of people](https://github.com/microsoft/TypeScript/graphs/contributors)).

## Examples
Here are some examples for how the type checking feature works.

```lua
---@type Cat | Dog
local pet

if isCat(pet) then
    ---@cast pet Cat

    -- pet is Cat in this scope
elseif isDog(pet) then
    ---@cast pet Dog

    -- pet is dog in this scope
end
```

```lua
---@class Cat

---@type Cat?
local pet

if not isNull(pet) then
    ---@cast pet -?

    -- pet is not null in this scope
end
```

```lua
---@class Vector

---@type Vector
local v1 = vector(1, 1, 1)
---@type Vector
local v2 = vector(2, 2, 2)

local x, y, z = getXYZ(v1 + v2 --[[@as Vector]])
```

