* _Disclaimer: This article was written by a user in an effort to get basic documentation. It has only minimal examples._

[EmmyLua](https://github.com/EmmyLua) annotations are doc comments similar to [LDoc](https://stevedonovan.github.io/ldoc/manual/doc.md.html) tags, but instead of generating documentation they are used to improve features like signature information.  
Also refer to the [official documentation](https://emmylua.github.io/), although Sumneko's implementation might not necessarily be the same.

![](https://user-images.githubusercontent.com/1073877/111884243-a337c780-89c0-11eb-856e-b6c3b1042810.gif)

### Annotations
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
![](https://user-images.githubusercontent.com/1073877/111887515-d6388600-89d5-11eb-9b16-329b3618e339.png)

#### `@field`
Adds a field to a class.

#### `@type`
Specifies the type of a variable.

#### `@generic`
Simulates generics.

#### `@vararg`
Indicates a function takes multiple variable arguments.
```lua
---@vararg string
---@return string
function strconcat(...) end
```
Note that for returning a vararg you can use `...` as a type.
```lua
---@vararg string
---@return ...
function tostringall(...) end
```

#### `@alias`
An alias is useful for reusing types or a function in multiple places.
```lua
---@alias exitcode '"exit"'|'"signal"'

---@return exitcode? exitcode
function file:close() end

---@return exitcode? exitcode
function io.close(file) end
```
Types for an alias can span multiple lines and be commented with `#`.
```lua
---@alias popenmode
---| '"r"' # Read data from this program by `file`.
---| '"w"' # Write data to this program by `file`.

---@param prog string
---@param mode popenmode
function io.popen(prog, mode) end
```
![](https://user-images.githubusercontent.com/1073877/111887908-7b545e00-89d8-11eb-8e62-c6902d58e292.png)

#### `@overload`
```lua
---@overload fun(name: string, hook: function)
---@param tbl table
---@param name string
---@param hook function
function hooksecurefunc(tbl, name, hook) end
```
![](https://user-images.githubusercontent.com/1073877/111889021-0128d700-89e2-11eb-9091-01b991b017af.png)

### Comments
Annotation comments start with `@`.
```lua
--this is a valid comment
---@param msg string @the message to show
local function greet(msg)
	print(msg)
end
```
![](https://user-images.githubusercontent.com/1073877/111888658-d25d3180-89de-11eb-9e05-10bc61627f2a.png)

Markdown formatting is supported.
```lua
--- This is **bolded text**
--- [click me](https://www.google.com/)
--- ```
--- for i, v in ipairs(tbl) do body end
--- ```
---@param msg string
local function greet(msg)
	print(msg)
end
```
![](https://user-images.githubusercontent.com/1073877/111888172-64af0680-89da-11eb-9c49-26a69642d74b.png)

### Types
#### Nilable Types
Appending a question mark (after the first word) marks a type optional/nilable.
```lua
---@param prog  string
---@param mode? popenmode
---@return file*?
---@return string? errmsg
function io.popen(prog, mode) end
```
![](https://user-images.githubusercontent.com/1073877/111886711-139a1500-89d0-11eb-9c90-f3f8007ef750.png)

Alternatively, use `nil` as an extra type.
```lua
---@return string|nil name
function GetName() end
```
![](https://user-images.githubusercontent.com/1073877/111888499-5f9f8680-89dd-11eb-8185-5890da3f6abf.png)

#### array
```lua
---@type string[]
```

#### table
```lua
---@type table<string, number>
```

#### function
```lua
---@type fun(x: number): number
```

### References
* EmmyLua: https://emmylua.github.io/
* Examples: https://github.com/sumneko/lua-language-server/tree/master/meta
* Tests: https://github.com/sumneko/lua-language-server/blob/master/test/definition/luadoc.lua