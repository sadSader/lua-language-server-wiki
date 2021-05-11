* _Disclaimer: This article was written by a user in an effort to get basic documentation._

[EmmyLua](https://github.com/EmmyLua) annotations are doc comments similar to [LDoc](https://stevedonovan.github.io/ldoc/manual/doc.md.html) tags, but besides adding documentation they are used to improve features like code completion and signature information. Also refer to the [official documentation](https://emmylua.github.io/annotation.html), although Sumneko's implementation might not necessarily be the same.

![](https://user-images.githubusercontent.com/1073877/111884243-a337c780-89c0-11eb-856e-b6c3b1042810.gif)

## Annotations
* [@param](#param)
* [@return](#return)
* [@class](#class)
* [@field](#field)
* [Types and @type](#types-and-type)
* [Comments](#Comments)
* [Optional Params](#optional-params)
* [@vararg](#vararg)
* [@alias](#alias)
* [@overload](#overload)
* [@generic](#generic)
* [@diagnostic](#diagnostic)
* [@version](#version)
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
---@return string firstName
---@return string middleName
---@return string lastName
function GetName() end
```
```lua
---@return string, string, string 
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
---@field legs number
local Animal = {}
function Animal:Walk() end

---@class Pet
---@field ownerName string
local Pet = {}

---@class Cat : Animal, Pet
local Cat = {}

---@type Cat
local gamercat = {}
```
![](https://user-images.githubusercontent.com/1073877/114530434-3627f280-9c4b-11eb-9f8d-358e9aadde87.png)

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

---@class BNetGameAccountInfo
---@field gameAccountID number
---@field characterName string

local info = GetAccountInfoByID(1)
print(info.gameAccountInfo.characterName)
```
![](https://user-images.githubusercontent.com/1073877/114519513-c6146f00-9c40-11eb-8866-9ab6f6df5a27.png)

![](https://user-images.githubusercontent.com/1073877/114520809-19d38800-9c42-11eb-9bbe-4310e5b0b986.gif)

#### Types and `@type`
Known types are: `nil, any, boolean, string, number, integer, function, table, thread, ..., userdata, lightuserdata`
* Classes can be [used](EmmyLua-Annotations#class) and passed or [returned](EmmyLua-Annotations#field) as a type.
* Multiple types are separated with `|`
```lua
---@param nameOrIndex string|number
---@return table|nil info
function GetQuestInfo(nameOrIndex) end
```
![](https://user-images.githubusercontent.com/1073877/114528298-2c9d8b00-9c49-11eb-9d37-9219f8df5a0d.png)

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

* Functions are formatted as `fun(param: VALUE_TYPE): RETURN_TYPE` and are used in e.g. [@overload](EmmyLua-Annotations#overload)
```lua
fun(x: number): number
```
### Comments
There are multiple ways to format comments. The `@` and `#` symbols can be used to explicitly begin an annotation comment. This is useful for `@return` if you don't want to specify a param name but do want to add a comment.
```lua
--this is a valid comment
--- this is also valid
---@param apple string comment 1
---@return string banana comment 2
---@return string @comment 3
---@return string #comment 4
function Foo(apple) end
```
![](https://user-images.githubusercontent.com/1073877/114513477-85195c00-9c3a-11eb-85d7-124488d4b00b.png)

Comments support markdown formatting.
```lua
--- This is **bolded** and this is *cursive* text
---
--- [Click me](https://www.google.com/)
--- ```
--- for i, v in ipairs(tbl) do
--- 	-- do stuff
--- end
--- ```
--- - item 1
--- - item 2
---   - level 2
---@param tbl table comment for `tbl`
function Foo(tbl) end
```
![image](https://user-images.githubusercontent.com/1073877/114515870-1e497200-9c3d-11eb-808c-acd74db824de.png)

### Optional Params
Appending a question mark (after the first word) marks a param optional/nilable. Another option is to instead use `|nil` as a second type.
```lua
---@param prog  string
---@param mode? string
---@return file*?
---@return string? errmsg
function io.popen2(prog, mode) end
```
![](https://user-images.githubusercontent.com/1073877/114528633-7be3bb80-9c49-11eb-8d34-a66db3c9e449.png)

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
---@alias exitcode2 '"exit"' | '"signal"'

---@return exitcode2
function io.close2() end

---@return exitcode2
function file:close2() end
```

String literals for an alias can be listed on multiple lines and commented with `#`
```lua
---@alias popenmode3
---| '"r"' # Read data from this program by `file`.
---| '"w"' # Write data to this program by `file`.

---@param prog string
---@param mode popenmode3
function io.popen3(prog, mode) end
```
![](https://user-images.githubusercontent.com/1073877/114529340-252ab180-9c4a-11eb-8f08-f34a3e94957b.png)

#### `@overload`
Specifies multiple signatures.
```lua
---@param tbl table
---@param name string
---@param hook function
---@overload fun(name: string, hook: function)
function hooksecurefunc(tbl, name, hook) end
```
![](https://user-images.githubusercontent.com/1073877/114531962-a84d0700-9c4c-11eb-90ef-ea1fdddb9137.png)

#### `@generic`
Simulates generics.
```lua
---@class Foo
local Foo = {}
function Foo:Bar() end

---@generic T
---@param name `T`
---@return T
function Generic(name) end

local v = Generic("Foo") -- v is an object of Foo
```
![](https://user-images.githubusercontent.com/1073877/114521804-0d9bfa80-9c43-11eb-81cb-61aa9d281f40.png)

#### `@diagnostic`
Controls diagnostics for errors, warnings, information and hints ([script/proto/define.lua](https://github.com/sumneko/lua-language-server/blob/1.19.0/script/proto/define.lua))
* `disable-next-line` - Disables diagnostics for the next line.
* `disable-line`
* `disable` - Disables diagnostics for the file.
* `enable` - Enables diagnostics for the file.
```lua
---@diagnostic disable-next-line: unused-local
function hello(test) end
```
![](https://user-images.githubusercontent.com/1073877/112365313-cc659a00-8cd7-11eb-99be-722fa32d4491.gif)

![](https://user-images.githubusercontent.com/1073877/112364413-c4f1c100-8cd6-11eb-88a0-e45a56953e76.gif)

The diagnostics state behaves as a toggle.

![](https://user-images.githubusercontent.com/1073877/114522605-d0843800-9c43-11eb-878b-c5c67166260f.png)

#### `@version`
Marks if a function or class is exclusive to specific Lua versions: `5.1, 5.2, 5.3, 5.4, JIT`. Requires configuring `Diagnostics: Needed File Status` ([#494](https://github.com/sumneko/lua-language-server/issues/494)).
![](https://user-images.githubusercontent.com/75196080/115882223-3dbe7700-a455-11eb-892c-6b67ce17029f.png)
```lua
---@version >5.2,JIT
function hello() end
```
```lua
---@version 5.4
function Hello() end

print(Hello())
```
![](https://user-images.githubusercontent.com/1073877/117734858-a008cd00-b1f4-11eb-9113-8392129e2b5c.png)

#### `@deprecated`
Visibly marks a function as deprecated.

![](https://user-images.githubusercontent.com/75196080/112711806-35b5fa80-8edc-11eb-9a06-41a41545c686.gif)

#### `@see`
Functionally the same as an annotation comment.

#### `@meta`
This is for internal use by Sumneko. Files marked with this will be ignored ([#370](https://github.com/sumneko/lua-language-server/issues/370#issuecomment-770678133)).

### References
* EmmyLua: https://emmylua.github.io/annotation.html
* Examples: https://github.com/sumneko/lua-language-server/tree/master/meta
* Tests: https://github.com/sumneko/lua-language-server/blob/master/test/definition/luadoc.lua
* Issues: https://github.com/sumneko/lua-language-server/issues?q=label%3AEmmyLua
* Discussion: https://github.com/sumneko/lua-language-server/discussions/470