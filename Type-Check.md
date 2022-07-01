此语言服务器从 3.4.0 版本开始提供类型检查功能。

Lua语言本身是动态类型的，而且使用动态类型开发确实很方便，因此类型检查的原则是只在必要的时候进行检查，例如函数必须要使用 `---@param` 标记过类型后，才会对其检查参数类型是否正确。

另外，出于性能与个人精力的考虑，也不打算做成类似于 `TypeScript` 那样完备的类型系统（甚至类型可以运算）。因此很多时候你必须要通过一些注解协助，才能正确的推测或收窄类型。

这里列出一些常见的问题与注解方式：

---

This language server provides a type check feature since version 3.4.0.

The Lua language itself is dynamic type, and it is really convenient to develop using dynamic types. Therefore, the principle of type inspection is to be checked only when necessary. For example, a function must be marked with `---@param` before it can be checked whether the parameter type is correct.

In addition, for the consideration of performance and personal energy, it is not intended to make a complete type system like `TypeScript` (even the type can be calculated). Therefore, many times you must use some annotations to properly infer or narrow the type.

Here are some common problems and annotations:

```lua
---@type Cat | Dog
local pet

if isCat(pet) then
    ---@cast pet Cat
elseif isDog(pet) then
    ---@cast pet Dog
end
```

```lua
---@type Cat?
local pet

if not isNull(pet) then
    ---@cast pet -?
end
```

```lua
---@type vector
local v1 = vector(1, 1, 1)
---@type vector
local v2 = vector(2, 2, 2)

local x, y, z = getXYZ(v1 + v2 --[[as vector]])
```

learn more [cast](https://github.com/sumneko/lua-language-server/wiki/EmmyLua-Annotations#cast) and [as](https://github.com/sumneko/lua-language-server/wiki/EmmyLua-Annotations#as)

Also see [castNumberToIntger](https://github.com/sumneko/lua-language-server/blob/master/doc/en-us/config.md#typecastnumbertointeger) and [weakUnionCheck](https://github.com/sumneko/lua-language-server/blob/master/doc/en-us/config.md#typeweakunioncheck)