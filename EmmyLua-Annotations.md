* _Disclaimer: This article was written by a user in an effort to get basic documentation._

[EmmyLua](https://github.com/EmmyLua) annotations are doc comments similar to [LDoc](https://stevedonovan.github.io/ldoc/manual/doc.md.html) tags, but instead of generating documentation they are used to improve features like code completion and signature information. Also refer to the [official documentation](https://emmylua.github.io/annotation.html), although Sumneko's implementation might not necessarily be the same.

![](https://user-images.githubusercontent.com/1073877/111884243-a337c780-89c0-11eb-856e-b6c3b1042810.gif)

## Annotations
* [@param](#param)
* [@return](#return)
* [@class](#class)
* [@field](#field)
* [Types and @type](#types-and-type)
* [Comments](#Comments)
* [Nilable Params](#nilable-params)
* [@vararg](#vararg)
* [@alias](#alias)
* [@overload](#overload)
* [@generic](#generic)
* [@version](#version)
* [@diagnostic](#diagnostic)
* [@deprecated](#deprecated)
* [@meta](#meta)
* [@see](#see)

#### `@param`
Specifies the type of function params.
```lua
---@param r number
---@param g number
---@param b number
function SetColor(r, g, b) end
```

#### `@return`
Specifies the type of function returns. Note that the return name is optional.
```lua
---@return string, string, string 
function GetName() end
```
```lua
---@return string firstName
---@return string middleName
---@return string lastName
function GetName() end
```
```lua
---@return string firstName, string middleName, string lastName
function GetName() end
```

#### `@class`
Simulates classes, supporting OOP inheritance.
```lua
---@class Animal
---@field weight number
local animal = {}

function animal:eat()
end

---@class Dog : Animal
local dog = {}

---@type Dog
local dog2 = {}
```
![](https://user-images.githubusercontent.com/1073877/112010965-14de5580-8b28-11eb-8f97-31851ba0cc1d.png)

Classes can support multiple inheritance.
```lua
---@class ScriptObject
local ScriptObject = {}
function ScriptObject:GetScript() end

---@class UIObject
local UIObject = {}
function UIObject:GetName() end

---@class Frame : ScriptObject, UIObject
local Frame = {}
```
![](https://user-images.githubusercontent.com/1073877/111890328-bced0500-89e8-11eb-9129-81e220e8428f.png)

#### `@field`
Declares a field on a class. For example a table/structure can be annotated as a class with fields.
```lua
---@param id number
---@return BNetAccountInfo accountInfo
function GetAccountInfoByID(id) end

---@class BNetAccountInfo
---@field accountName string
---@field isFriend boolean
---@field gameAccountInfo BNetGameAccountInfo
local BNetAccountInfo = {}

---@class BNetGameAccountInfo
---@field gameAccountID number
---@field characterName string
local BNetGameAccountInfo = {}

local info = GetAccountInfoByID(1)
print(info.gameAccountInfo.characterName)
```
![](https://user-images.githubusercontent.com/1073877/112010872-fe37fe80-8b27-11eb-9180-65f3903b2203.png)

![](https://user-images.githubusercontent.com/1073877/112011546-a77ef480-8b28-11eb-8f98-8fe8f0c09385.png)

#### Types and `@type`
Known types are: `nil, any, boolean, string, number, integer, function, table, thread, ..., userdata, lightuserdata`
* Classes can be [used](EmmyLua-Annotations#class) and passed or [returned](EmmyLua-Annotations#field) as a type.
* Multiple types are separated with `|`
```lua
---@param nameOrIndex string|number
---@return table|nil info
function GetQuestInfo(nameOrIndex) end
```
![](https://user-images.githubusercontent.com/1073877/111910355-5526d080-8a61-11eb-9c6e-26a30604258e.png)
* `@type` specifies the type of a variable. 
* Arrays are indicated with a `[]`
```lua
---@type string[]
local msg = {"hello", "world"}
```

* Tables are formatted as `table<KEY_TYPE, VALUE_TYPE>`
```lua
---@type table<string, number>
local CalendarStatus = {
	Invited = 0,
	Available = 1,
	Declined = 2,
	Confirmed = 3,
}
```

* Functions are formatted as `fun(param: VALUE_TYPE): RETURN_TYPE` and can be used in e.g. [@overload](EmmyLua-Annotations#overload)
```lua
fun(x: number): number
```

### Comments
Annotation comments start with `@`
```lua
--this is a valid comment
---@param msg string @the message to show
function greet(msg) end
```
![](https://user-images.githubusercontent.com/1073877/111888658-d25d3180-89de-11eb-9e05-10bc61627f2a.png)

Comments have markdown formatting.
```lua
--- This is **bolded text**
--- [click me](https://www.google.com/)
--- ```
--- for i, v in ipairs(tbl) do body end
--- ```
---@param msg string
function greet(msg) end
```
![](https://user-images.githubusercontent.com/1073877/111888172-64af0680-89da-11eb-9c49-26a69642d74b.png)

### Nilable Params
Appending a question mark (after the first word) marks a param optional/nilable.
```lua
---@param prog  string
---@param mode? string
---@return file*?
---@return string? errmsg
function io.popen3(prog, mode) end
```
![](https://user-images.githubusercontent.com/1073877/112008831-21fa4500-8b26-11eb-999b-b7dab2fc8298.png)

#### `@vararg`
Indicates a function has multiple variable arguments.
```lua
---@vararg string
---@return string
function strconcat(...) end
```
For returning a vararg you can use `...` as a type.
```lua
---@vararg string
---@return ...
function tostringall(...) end
```

#### `@alias`
Aliases are useful for reusing param types e.g. a function or string literals.
```lua
---@alias exitcode '"exit"'|'"signal"'

---@return exitcode exitcode
function file:close() end

---@return exitcode exitcode
function io.close(file) end
```

String literals for an alias can be put on multiple lines and commented with `#`
```lua
---@alias popenmode2
---| '"r"' # Read data from this program by `file`.
---| '"w"' # Write data to this program by `file`.

---@param prog string
---@param mode popenmode2
function io.popen2(prog, mode) end
```
![](https://user-images.githubusercontent.com/1073877/112009178-743b6600-8b26-11eb-95b2-5f127a58b1be.png)

#### `@overload`
Specifies multiple signatures.
```lua
---@overload fun(name: string, hook: function)
---@param tbl table
---@param name string
---@param hook function
function hooksecurefunc(tbl, name, hook) end
```
![](https://user-images.githubusercontent.com/1073877/111889021-0128d700-89e2-11eb-9091-01b991b017af.png)

#### `@generic`
Simulates generics.
```lua
---@class Foo
local Foo = {}

function Foo:bar1() end

---@generic T
---@param arg1 `T`
---@return T
function Generic(arg1) print(arg1) end

local v1 = Generic("Foo") -- v1 is an object of the Foo class
print(v1.bar1) -- function
```
![](https://user-images.githubusercontent.com/1073877/112006145-bc0cbe00-8b23-11eb-8b59-ffe4cc4c6904.png)

#### `@version`
Marks if a function or class is exclusive to specific Lua versions: `5.1, 5.2, 5.3, 5.4, JIT`
```lua
---@version 5.1
function hello() end
```
```lua
---@version >5.2,JIT
function hello() end
```

#### `@diagnostic`
Controls diagnostics for errors, warnings, information and hints ([script/proto/define.lua](https://github.com/sumneko/lua-language-server/blob/1.19.0/script/proto/define.lua))
* `disable-next-line` - Disables diagnostics for the below line.
* `disable-line`
* `disable` - Disables diagnostics for the file.
* `enable` - Enables diagnostics for the file.
```lua
---@diagnostic disable-next-line: unused-local
function hello(test) end
```
![](https://user-images.githubusercontent.com/1073877/112365313-cc659a00-8cd7-11eb-99be-722fa32d4491.gif)

![](https://user-images.githubusercontent.com/1073877/112364413-c4f1c100-8cd6-11eb-88a0-e45a56953e76.gif)

#### `@deprecated`
#### `@meta`
#### `@see`

### References
* EmmyLua: https://emmylua.github.io/annotation.html
* Examples: https://github.com/sumneko/lua-language-server/tree/master/meta
* Tests: https://github.com/sumneko/lua-language-server/blob/master/test/definition/luadoc.lua
* Issues: https://github.com/sumneko/lua-language-server/issues?q=label%3AEmmyLua
* Discussion: https://github.com/sumneko/lua-language-server/discussions/470