# Diagnostics
There are many different diagnostics that can be used to report information at different severities such as `Information`, `Warning`, `Error`, etc.

Also see [syntax errors](https://github.com/sumneko/lua-language-server/wiki/Syntax-Errors).

## List of all diagnostics
Below is a list of all diagnostics. They are organized into groups that are used by [`diagnostics.groupFileStatus`](https://github.com/sumneko/lua-language-server/wiki/Settings#diagnosticsgroupfilestatus) and [`diagnostics.groupSeverity`](https://github.com/sumneko/lua-language-server/wiki/Settings#diagnosticsgroupseverity).


### ambiguity
The ambiguity diagnostic group contains diagnostics that have to do with ambiguous cases.

<br>

#### `ambiguity-1`
**Default Severity:** `Warning`
Triggered when there is an ambiguous statement that may need some brackets in order to correct the order of operations.

<!-- ambiguity-1.png -->

<br>

#### `count-down-loop`
**Default Severity:** `Warning`
Triggers when a `for` loop will never reach its max/limit because it is incrementing when it should be decrementing.

```lua
for i=10, 1 do end
```

<br>

#### `different-requires`
**Default Severity:** `Warning`
Triggered when a file is required under two different names. This can happen when requiring a file in two different directories:

```txt
📦 myProject/
    ├── 📂 helpers/
    │   ├── 📜 strings.lua
    │   └── 📜 pretty.lua
    └── 📜 main.lua
```
```lua
-- main.lua
local strings = require("helpers.strings")
local pretty = require("helpers.pretty")
```
```lua
-- helpers/pretty.lua
local strings = require("strings")
```

<br>

#### `newfield-call`
**Default Severity:** `Warning`
Triggered when calling a function across two lines from within a table. This may be unwanted and you may want to separate the fields with a comma (`,`) or semicolon (`;`) - unless you want to call the first field as a function.

```lua
local myTable = {
  myFunc
  ("param")
}
```

<br>

#### `newline-call`
**Default Severity:** `Warning`
Triggered when calling a function from the next line. This may be unintended and you may want to add a semicolon `;` to end the line.

```lua
print(1)
('x'):sub(1, 2)
```

---

### await
The await group contains diagnostics for asynchronous code.

<br>

#### `await-in-sync`
**Default Severity:** `Warning`
**Default File Status:** `"None"`
Triggered when calling an asynchronous function from within a synchronous function.

<br>

#### `not-yieldable`
**Default Severity:** `Warning`
**Default File Status:** `"None"`
Triggered when attempting to call `coroutine.yield()` when it is not permitted.

---

### codestyle
The codestyle group contains diagnostics for maintaining a good code style.

<br>

#### `codestyle-check`
**Default Severity:** `Warning`
**Default File Status:** `"None"`
Triggered when the opinionated style checking detects an incorrectly styled line.

<br>

#### `spell-check`
**Default Severity:** `Information`
**Default File Status:** `"None"`
Triggered when a typo is detected in a string. The dictionary can be customized using the [`Lua.spell.dict` setting](https://github.com/sumneko/lua-language-server/wiki/Settings#spelldict).

---

### duplicate
The duplicate group contains diagnostics for duplicate indexes and names.

<br>

#### `duplicate-index`
**Default Severity:** `Warning`
Triggered when there are duplicate indexes.

```lua
local t = {
    -- triggered by indexes
    [1] = "a",
    [1] = "b",

    -- triggered by keys as well
    two = "c",
    two = "d"
}
```

<br>

#### `duplicate-set-field`
**Default Severity:** `Warning`
Triggered when setting the same field in a class more than once. Check the names of your fields.

```lua
---@class myClass
local myTable = {}

function myTable:x() end

function myTable:x() end
```

---

### global
The global group contains diagnostics that deal with the global scope.

<br>

#### `global-in-nil-env`
**Default Severity:** `Warning`
Triggered when a global variable is defined and the environment (`_ENV`) is `nil`.

```lua
goto jump

local a = 10

::jump::
-- a is jumped over by this label,
-- meaning it is never defined

print(a)
```

<br>

#### `lowercase-global`
**Default Severity:** `Information`
Triggered when a global variable starts with a lowercase character. This is mainly for maintaining good practice.

<br>

#### `undefined-env-child`
**Default Severity:** `Information`
Triggered when the environment (`_ENV`) is mutated and a previously global variable is no longer usable.

```lua
local A
---@type iolib
_ENV = {}
print(A)
```

<br>

#### `undefined-global`
**Default Severity:** `Warning`
Triggered when referencing an undefined global (assumed to be global). The typical "does not exist" warning. Double check that you have spelled things correctly and that the variable exists.

---

### luadoc
The luadoc group contains diagnostics for the [annotations](https://github.com/sumneko/lua-language-server/wiki/Annotations) used to document your code.

<br>

#### `cast-type-mismatch`
**Default Severity:** `Warning`
Triggered when casting a variable to a type that does not match its initial type.

```lua
---@type boolean
local e = nil

---@cast e integer
```

<br>

#### `circle-doc-class`
**Default Severity:** `Warning`
Triggered when two classes inherit each other forming a never ending loop of inheritance.

```lua
---@class Car:Vehicle

---@class Vehicle:Car
```

<br>

#### `doc-field-no-class`
**Default Severity:** `Warning`
Triggered when a [`@field`](https://github.com/sumneko/lua-language-server/wiki/Annotations#field) is specified for a non-existent [`@class`](https://github.com/sumneko/lua-language-server/wiki/Annotations#class).

<br>

#### `duplicate-doc-alias`
**Default Severity:** `Warning`
Triggered when there are two [`@alias`](https://github.com/sumneko/lua-language-server/wiki/Annotations#alias) annotations with matching names.

<br>

#### `duplicate-doc-field`
**Default Severity:** `Warning`
Triggered when there are two [`@field`](https://github.com/sumneko/lua-language-server/wiki/Annotations#field) annotations with matching key values.

<br>

#### `duplicate-doc-param`
**Default Severity:** `Warning`
Triggered when there are two [`@param`](https://github.com/sumneko/lua-language-server/wiki/Annotations#param) annotations with matching names.

<br>

#### `undefined-doc-class`
**Default Severity:** `Warning`
Triggered when referencing an undefined class in a [`@class`](https://github.com/sumneko/lua-language-server/wiki/Annotations#class) annotation.

<br>

#### `undefined-doc-name`
**Default Severity:** `Warning`
Triggered when referencing an undefined type or [`@alias`](https://github.com/sumneko/lua-language-server/wiki/Annotations#alias) in a [`@type`](https://github.com/sumneko/lua-language-server/wiki/Annotations#type) annotation.

<br>

#### `undefined-doc-param`
**Default Severity:** `Warning`
Triggered when referencing an undefined parameter from a function declartion.

```lua
---@param doesNotExist number
function subtract(a, b) end
```

<br>

#### `unknown-cast-variable`
**Default Severity:** `Warning`
Triggered when attempting to cast an undefined variable. Double check that you have spelled things correctly and that the variable exists. Appears when using [`@cast`](https://github.com/sumneko/lua-language-server/wiki/Annotations#cast).

<br>

#### `unknown-diag-code`
**Default Severity:** `Warning`
Triggered when entering an invalid diagnostic code. A diagnostic code is like one of the many diagnosis codes found on this page.

```lua
---@diagnostic disable: doesNotExist
```

<br>

#### `unknown-operator`
**Default Severity:** `Warning`
Triggered when an unknown operator is found like `**`.

---

### redefined
The redefined group contains diagnostics that warn when variables are being redefined.

<br>

#### `redefined-local`
**Default Severity:** `Hint`
Triggered when a local variable is being redefined. This will result in the redefinition being underlined. While still legal, it could cause confusion when trying to use the previously defined version of the local variable that may have since been mutated. It is good practice not to re-use local variable names for this reason.

---

### strict
The strict group contains diagnostics considered "strict". These can help you write better code but may require more work to follow.

<br>

#### `close-non-object`
**Default Severity:** `Warning`
Triggered when attempting to [close a variable](https://www.lua.org/manual/5.4/manual.html#3.3.8) with a non-object. The value of the variable must be an object with the `__close` metamethod (a Lua 5.4 feature).

```lua
local x <close> = 1
```

<br>

#### `deprecated`
**Default Severity:** `Warning`
Triggered when a variable has been marked as deprecated yet is still being used. The variable in question will also be ~~struck through~~.

<br>

#### `discard-returns`
**Default Severity:** `Warning`
Triggered when the returns of a function are being ignored when it is not permitted. Functions can specify that their returns cannot be ignored with [`@nodiscard`](https://github.com/sumneko/lua-language-server/wiki/Annotations#nodiscard).

---

### strong
The strong group contains diagnostics considered "strong". These can help you write better code but may require more work to follow.

<br>

#### `no-unknown`
**Default Severity:** `Warning`
**Default File Status:** `"None"`
Triggered when a variable has an unknown type that cannot be inferred. Useful for more strict typing.

---

### type-check
The type-check group contains diagnostics that have to do with type checking.

<br>

#### `assign-type-mismatch`
**Default Severity:** `Warning`
Triggered when assigning a value, in which its type does not match the type of the variable it is being assigned to.

The below will trigger this diagnostic because we are assigning a `boolean` to a `number`:
```lua
---@type number
local isNum = false
```

<br>

#### `cast-local-type`
**Default Severity:** `Warning`
Triggered when a local variable is being cast to a different value than it was defined as.

```lua
---@type boolean
local myBool

myBool = {}
```

<br>

#### `cast-type-mismatch`
**Default Severity:** `Warning`
Triggered when casting a variable to a type that does not match its initial type.

```lua
---@type boolean
local e = nil

---@cast e integer
```

<br>

#### `need-check-nil`
**Default Severity:** `Warning`
Triggered when indexing a possibly `nil` object. Serves as a reminder to confirm the object is not nil before indexing - which would throw an error on execution.

```lua
---@class Bicycle
---@field move function

---@type Bicycle|nil
local bicycle

-- need to make sure bicycle isn't nil first
bicycle.move()
```

<br>

#### `param-type-mismatch`
**Default Severity:** `Warning`
Triggered when the type of the provided parameter does not match the type requested by the function definition. Uses information defined with [`@param`](https://github.com/sumneko/lua-language-server/wiki/Annotations#param).

<br>

#### `return-type-mismatch`
**Default Severity:** `Warning`
Triggered when the provided `return` value is not of the same type that the function expected.

```lua
---@return number sum
function add(a, b)
    return false
end
```

<br>

#### `undefined-field`
**Default Severity:** `Warning`
Triggered when referencing an undefined field.

```lua
---@class myClass
local myClass = {}

-- undefined field "hello"
myClass.hello()
```

---

### unbalanced
The unbalanced group contains diagnostics that deal with too few or too many of an item being given - like too few required parameters.

<br>

#### `missing-parameter`
**Default Severity:** `Warning`
Triggered when a required parameter is not supplied when calling a function. Uses information defined with [`@param`](https://github.com/sumneko/lua-language-server/wiki/Annotation#param).

<br>

#### `missing-return`
**Default Severity:** `Warning`
Triggered when a required `return` is not supplied from within a function. Uses information defined with [`@return`](https://github.com/sumneko/lua-language-server/wiki/Annotations#return).

<br>

#### `missing-return-value`
**Default Severity:** `Warning`
Triggered when a `return` is specified but the return value is not. Uses information defined with [`@return`](https://github.com/sumneko/lua-language-server/wiki/Annotations#return).

<br>

#### `redundant-parameter`
**Default Severity:** `Warning`
Triggered when providing an extra parameter that a function does not ask for. Uses information defined with [`@param`](https://github.com/sumneko/lua-language-server/wiki/Annotations#param).

<br>

#### `redundant-return-value`
**Default Severity:** `Warning`
Triggered when a `return` is returning an extra value that the function has not requested. Uses information defined with [`@return`](https://github.com/sumneko/lua-language-server/wiki/Annotations#return).

<br>

#### `redundant-value`
**Default Severity:** `Warning`
Triggered when providing an extra value to an assignment operation that will go unused.

```lua
local a, b = 1, 2, 3
```

<br>

#### `unbalanced-assignments`
**Default Severity:** `Warning`
Triggered when there are more variables being assigned than values to assign them.

```lua
local a, b, c = 1, 2
```

---

### unused
The unused group contains diagnostics that report unused or unnecessary items.

<br>

#### `code-after-break`
**Default Severity:** `Hint`
Triggered when unreachable code is added after a `break` in a loop. This will result in the affected code becoming slightly transparent.

<br>

#### `empty-block`
**Default Severity:** `Hint`
Triggered when a code block is left empty. This will result in the code block becoming slightly transparent.

<br>

#### `redundant-return`
**Default Severity:** `Hint`
Triggered when placing a `return` that is not needed as the function would exit on its own.

<br>

#### `trailing-space`
**Default Severity:** `Hint`
Triggered when a trailing space is detected. This will result in the trailing space being underlined.

<br>

#### `unreachable-code`
**Default Severity:** `Hint`
Triggered when a section of code can never be reached. This will result in the affected code becoming slightly transparent.

<br>

#### `unused-function`
**Default Severity:** `Hint`
Triggered when a function is defined but never called. This results in the function becoming slightly transparent.

<br>

#### `unused-label`
**Default Severity:** `Hint`
Triggered when a label is defined but never used. This results in the label becoming slightly transparent.

<br>

#### `unused-local`
**Default Severity:** `Hint`
Triggered when a `local` variable is defined but never referenced. This results in the variable becoming slightly transparent.

<br>

#### `unused-vararg`
**Default Severity:** `Hint`
Triggered when the variable arguments symbol (`...`) in a function is unused. This results in the symbol becoming slightly transparent.

---
