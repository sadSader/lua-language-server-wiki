# Syntax Errors
A syntax error will appear when the Lua syntax has been violated, which will result in a error at runtime.

These work *similar* to the [diagnostics](https://github.com/sumneko/lua-language-server/wiki/).

## List of all syntax errors
Below is a list of all of the possible syntax errors that can be reported by the language server:

<br>

### `action-after-return`
Triggered when there is unreachable code after a `return` - which is invalid Lua.

<br>

### `args-after-dots`
Triggered when there is a parameter defined after a variable arguments symbol (`...`) - which is invalid Lua.

<br>

### `block-after-else`
Triggered when there is a `else` or `elseif` after a terminating `else` in an `if` statement - which is invalid Lua.

```lua
if myVar then
    print("Truthy")
else
    print("Falsy")
elseif nil then --Error!

end
```

<br>

### `break-outside`
Triggered when a `break` is placed outside of a `break`-able loop - which is invalid Lua.

<br>

### `err-assign-as-eq`
Triggered when using the equality symbol (`==`) to assign a variable. You should instead use the assignment symbol `=`.

<br>

### `err-c-long-comment`
Triggered when using `/** */` for multi-line comments instead of `--[[ ]]`, as required by Lua's syntax. You should instead use `--[[ ]]` for your multi-line comments.

<br>

### `err-comment-prefix`
Triggered when using `//` for comments instead of `--`, as required by Lua's syntax. You should instead use `--` for your comments and `---` for your annotations.

<br>

### `err-do-as-then`
Triggered when using `then` instead of `do` for a `while` loop.

<br>

### `err-eq-as-assign`
Triggered when using the assignment symbol (`=`) to test equality. You should instead use the equality symbol `==`.

<br>

### `err-esc`
Triggered when an unknown escape sequence is found, such as `"\c"`.

<br>

### `err-nonstandard-symbol`
Triggered when using a non-Lua symbol like `&&` instead of `and` or `||` instead of `or`.

<br>

### `err-then-as-do`
Triggered when using `do` instead of `then` in an `if` statement.

<br>

### `exp-in-action`
Triggered when there is an unexpected expression like in `local 3 + 2 = 10` - there is an unexpected expression during an assignment action.

<br>

### `index-in-func-name`
Triggered when there is an indexing operation taking place in a function's name e.g. `function myTable[1]() end`.

<br>

### `jump-local-scope`
Triggered when a local variable is "jumped" over using `goto` and labels. "jumping" the variable means it is never defined and will cause errors when it is referenced. This diagnostic serves to protect against such errors.

<br>

### `keyword`
Triggered when using a reserved keyword as a name e.g. `local true = "hello"`.

<br>

### `local-limit`
Triggered when the limit for `local` variables has been reached in this scope. Lua has a [hard-coded limit of 200 local variables](https://stackoverflow.com/a/66539256) in each scope.

<br>

### `malformed-number`
Triggered when a malformed number is found like `0y16` instead of `0x16` or `0.0.4`.

<br>

### `miss-end`
Triggered when an `end` is missing from an `if`, `for`, `while`, or `function`. This is usually due to nesting and a matching `end` is missing from an outer `if`, `for`, etc.

<br>

### `miss-esc-x`
Triggered when the hexadecimal digits are missing from an `\x` hexadecimal escape.

<br>

### `miss-exp`
Triggered when an expression is missing from your code. For example, forgetting the expression to an if statement, i.e. `if then end`.

<br>

### `miss-exponent`
Triggered when the exponent has been left out when representing a number in exponent form.

```lua
local incorrect = 1e
local correct = 1e10
```

<br>

### `miss-field`
Triggered when a field reference is underway but no field name has been given e.g. `print(myTable.)`.

<br>

### `miss-loop-max`
Triggered when the maximum (limit) to a `for` loop is not provided.

<br>

### `miss-loop-min`
Triggered when the minimum (start) to a `for` loop is not provided.

<br>

### `miss-method`
Triggered when a method (`:`) is being called but the method name is not provided e.g. `Class:`.

<br>

### `miss-name`
Triggered when the name is missing to a function/method.

<br>

### `miss-sep-in-table`
Triggered when a separator (`,` or `;`) is missing from a table.

<br>

### `miss-space-between`
<!-- TODO: description -->
🚧 Description needed 🚧

<br>

### `miss-symbol`
<!-- TODO: description -->
🚧 Description needed 🚧

<br>

### `set-const`
<!-- TODO: description -->
🚧 Description needed 🚧

<br>

### `unexpect-dots`
Triggered when using the variable arguments symbol (`...`) outside of a function that has variable arguments.

<br>

### `unexpect-efunc-name`
<!-- TODO: description -->
🚧 Description needed 🚧

<br>

### `unexpect-lfunc-name`
<!-- TODO: description -->
🚧 Description needed 🚧

<br>

### `unexpect-symbol`
<!-- TODO: description -->
🚧 Description needed 🚧

<br>

### `unicode-name`
Triggered when a variable name contains unicode characters. Unicode characters can be allowed by disabling this diagnostic or by enabling [`Lua.runtime.unicodeName`](https://github.com/sumneko/lua-language-server/wiki/Settings#runtimeunicodename).

<br>

### `unknown-attribute`
<!-- TODO: description -->
🚧 Description needed 🚧

<br>

### `unknown-symbol`
Triggered when an unknown symbols is found like in `local a = &`. A simple syntax error, most likely a typo.
