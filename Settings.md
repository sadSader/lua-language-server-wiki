# Settings
A list of all settings that can be used to customize how the language server operates. All of these settings should be prefixed with `Lua.` e.g. `Lua.completion.enable` or `Lua.diagnostics.globals` when editing a [configuration file](https://github.com/LuaLS/lua-language-server/wiki/Configuration-File).

## addonManager
Settings that affect the VS Code addon manager

<br>

### `addonManager.enable`
**Type:** `boolean`

**Default:** `true`

Set the on/off state of the addon manager. Disabling the addon manager prevents it from registering its command.

## completion
Settings that adjust how autocompletions are provided while typing.

<br>

### `completion.autoRequire`
**Type:**  `boolean`

**Default:**  `true`

When the input looks like a file name, automatically require the file.

![completion autoRequire](https://user-images.githubusercontent.com/61925890/181309077-6877717e-0884-4297-8e3b-9cbe70143e55.gif)

<br>

### `completion.callSnippet`
**Type:**  `string`

**Default:**  `"Disable"`

**Options:**

- `"Disable"` - Only show function name
- `"Both"` - Show function name and snippet
- `"Replace"` - Only show the call snippet

Whether to show call snippets or not. When disabled, only the function name will be completed. When enabled, a "more complete" snippet will be offered.

> ℹ️ Note: the below GIF is using the `"Both"` option

![completion callSnippet](https://user-images.githubusercontent.com/61925890/181309121-ccdce8bd-4d53-4c65-933f-1a754537ecf9.gif)

<br>

### `completion.displayContext`
**Type:**  `integer`

**Default:**  `0` (disabled)


When a snippet is being suggested, this setting will set the amount of lines around the snippet to preview to help you better understand its usage.

Setting to `0` will disable this feature.

![completion displayContext](https://user-images.githubusercontent.com/61925890/181309155-a3881c8d-5fef-4ec1-abfa-bc758e208ff4.png)

<br>

### `completion.enable`
**Type:**  `boolean`

**Default:**  `true`


Enable/disable completion. Completion works like any autocompletion you already know of. It helps you type less and get more done.

<br>

### `completion.keywordSnippet`
**Type:**  `string`

**Default:**  `"Replace"`

**Options:**

- `"Disable"` - Only completes the keyword
- `"Both"` - Offers a completion for the keyword and snippet
- `"Replace"` - Only shows a snippet

Whether to show a snippet for key words like `if`, `while`, etc. When disabled, only the keyword will be completed. When enabled, a "more complete" snippet will be offered.

![completion keywordSnippet](https://user-images.githubusercontent.com/61925890/181309193-bd98c7c4-e7b6-4014-b280-49beb5e9a386.gif)

<br>

### `completion.postfix`
**Type:**  `string`

**Default:**  `@`


The character to use for triggering a "postfix suggestion". A postfix allows you to write some code and then trigger a snippet after (post) to "fix" the code you have written. This can save some time as instead of typing `table.insert(myTable, )`, you can just type `myTable@`.

![completion postfix](https://user-images.githubusercontent.com/61925890/181309242-104b7cba-d2a5-496a-bde2-6ec060d95215.gif)

<br>

### `completion.requireSeparator`
**Type:**  `string`

**Default:**  `.`


The separator to use when `require`-ing a file.

<br>

### `completion.showParams`
**Type:**  `boolean`

**Default:**  `true`


Display a function's parameters in the list of completions. When a function has multiple definitions, they will be displayed separately.

<br>

### `completion.showWord`
**Type:**  `string`

**Default:**  `"Fallback"`

**Options:**

- `"Enable"` - Always show contextual words in completion suggestions
- `"Fallback"` - Only show contextual words when smart suggestions based on semantics cannot be provided
- `"Disable"` - Never show contextual words

Show "contextual words" that have to do with Lua but are not suggested based on their usefulness in the current semantic context.

<br>

### `completion.workspaceWord`
**Type:**  `boolean`

**Default:**  `true`


Whether words from other files in the workspace should be suggested as "contextual words". This can be useful for completing similar strings. [`completion.showWord`](#completionshowword) must not be disabled for this to have an effect.



## diagnostics
Settings to adjust the diagnostics for `Info`, `Warning`, and `Error`

<br>

### `diagnostics.disable`
**Type:**  <code>Array<<a href="https://github.com/LuaLS/lua-language-server/wiki/Diagnostics">diagnostic_name</a>></code>

**Default:**  `[]`


Disable certain diagnostics globally. For example, if you want all warnings for `lowercase-global` to be disabled, the value for `diagnostics.disable` would be `["lowercase-global"]`.

<br>

### `diagnostics.disableScheme`
**Type:**  `Array<string>`

**Default:**  `["git"]`


Disable diagnosis of Lua files that have the set schemes.

<br>

### `diagnostics.enable`
**Type:**  `boolean`

**Default:**  `true`


Whether all diagnostics should be enabled or not.

<br>

### `diagnostics.globals`
**Type:**  `Array<string>`

**Default:**  `[]`


An array of variable names that will be declared as global.

<br>

### `diagnostics.groupFileStatus`
**Type:**  <code>Object<<a href="https://github.com/LuaLS/lua-language-server/wiki/Diagnostics">diagnostic_group_name</a>, file_state></code>


**`file_state` Options:**
- `"Any"` - Any loaded file (workspace, library, etc.) will use this diagnostic group
- `"Opened"` - Only opened files will use this diagnostic group
- `"None"` - This diagnostic group will be disabled
- `"Fallback"` - The diagnostics in this group are controlled individually by [`diagnostics.neededFileStatus`](https://github.com/LuaLS/lua-language-server/wiki/Settings#diagnosticsneededfilestatus)

<details>

<summary>Default Value</summary>

```json
{
    /*
    * ambiguity-1
    * count-down-loop
    * different-requires
    * newfield-call
    * newline-call
    */
    "ambiguity": "Fallback",
    /*
    * await-in-sync
    * not-yieldable
    */
    "await": "Fallback",
    /*
    * codestyle-check
    * spell-check
    */
    "codestyle": "Fallback",
    /*
    * duplicate-index
    * duplicate-set-field
    */
    "duplicate": "Fallback",
    /*
    * global-in-nil-env
    * lowercase-global
    * undefined-env-child
    * undefined-global
    */
    "global": "Fallback",
    /*
    * cast-type-mismatch
    * circle-doc-class
    * doc-field-no-class
    * duplicate-doc-alias
    * duplicate-doc-field
    * duplicate-doc-param
    * undefined-doc-class
    * undefined-doc-name
    * undefined-doc-param
    * unknown-cast-variable
    * unknown-diag-code
    * unknown-operator
    */
    "luadoc": "Fallback",
    /*
    * redefined-local
    */
    "redefined": "Fallback",
    /*
    * close-non-object
    * deprecated
    * discard-returns
    */
    "strict": "Fallback",
    /*
    * no-unknown
    */
    "strong": "Fallback",
    /*
    * assign-type-mismatch
    * cast-local-type
    * cast-type-mismatch
    * need-check-nil
    * param-type-mismatch
    * return-type-mismatch
    * undefined-field
    */
    "type-check": "Fallback",
    /*
    * missing-parameter
    * missing-return
    * missing-return-value
    * redundant-parameter
    * redundant-return-value
    * redundant-value
    * unbalanced-assignments
    */
    "unbalanced": "Fallback",
    /*
    * code-after-break
    * empty-block
    * redundant-return
    * trailing-space
    * unreachable-code
    * unused-function
    * unused-label
    * unused-local
    * unused-vararg
    */
    "unused": "Fallback"
}
```

</details>
<br>

Set the file status required for each diagnostic group. This setting is an `Object` of `key`-`value` pairs where the `key` is the name of the diagnostic group and the `value` is the state that a file must be in in order for the diagnostic group to apply.


<br>

### `diagnostics.groupSeverity`
**Type:**  <code>Object<<a href="https://github.com/LuaLS/lua-language-server/wiki/Diagnostics">diagnostic_group_name</a>, severity></code>


**`severity` Options:**
- `"Error"` - An error will be raised
- `"Warning"` - A warning will be raised
- `"Information"` - An information or note will be raised
- `"Hint"` - The affected code will be "hinted" at
- `"Fallback"` - The diagnostics in this group will be controlled individually by [`diagnostics.severity`](https://github.com/LuaLS/lua-language-server/wiki/Settings#diagnosticsseverity)

<details>

<summary>Default Value</summary>

```json
{
    /*
    * ambiguity-1
    * count-down-loop
    * different-requires
    * newfield-call
    * newline-call
    */
    "ambiguity": "Fallback",
    /*
    * await-in-sync
    * not-yieldable
    */
    "await": "Fallback",
    /*
    * codestyle-check
    * spell-check
    */
    "codestyle": "Fallback",
    /*
    * duplicate-index
    * duplicate-set-field
    */
    "duplicate": "Fallback",
    /*
    * global-in-nil-env
    * lowercase-global
    * undefined-env-child
    * undefined-global
    */
    "global": "Fallback",
    /*
    * cast-type-mismatch
    * circle-doc-class
    * doc-field-no-class
    * duplicate-doc-alias
    * duplicate-doc-field
    * duplicate-doc-param
    * undefined-doc-class
    * undefined-doc-name
    * undefined-doc-param
    * unknown-cast-variable
    * unknown-diag-code
    * unknown-operator
    */
    "luadoc": "Fallback",
    /*
    * redefined-local
    */
    "redefined": "Fallback",
    /*
    * close-non-object
    * deprecated
    * discard-returns
    */
    "strict": "Fallback",
    /*
    * no-unknown
    */
    "strong": "Fallback",
    /*
    * assign-type-mismatch
    * cast-local-type
    * cast-type-mismatch
    * need-check-nil
    * param-type-mismatch
    * return-type-mismatch
    * undefined-field
    */
    "type-check": "Fallback",
    /*
    * missing-parameter
    * missing-return
    * missing-return-value
    * redundant-parameter
    * redundant-return-value
    * redundant-value
    * unbalanced-assignments
    */
    "unbalanced": "Fallback",
    /*
    * code-after-break
    * empty-block
    * redundant-return
    * trailing-space
    * unreachable-code
    * unused-function
    * unused-label
    * unused-local
    * unused-vararg
    */
    "unused": "Fallback"
}
```

</details>

<br>

### `diagnostics.ignoredFiles`
**Type:**  `string`

**Default:**  `"Opened"`

**Options:**

- `"Enable"` - Always diagnose ignored files... kind of defeats the purpose of ignoring them.
- `"Opened"` - Only diagnose ignored files when they are open
- `"Disable"` - Ignored files are fully ignored

Set how files that have been ignored should be diagnosed.

<br>

### `diagnostics.libraryFiles`
**Type:**  `string`

**Default:**  `"Opened"`

**Options:**

- `"Enable"` - Always diagnose library files
- `"Opened"` - Only diagnose library files when they are open
- `"Disable"` - Never diagnose library files

Set how files loaded with [`workspace.library`](https://github.com/LuaLS/lua-language-server/wiki/Settings#workspacelibrary) are diagnosed.

<br>

### `diagnostics.neededFileStatus`
**Type:**  <code>Object<<a href="https://github.com/LuaLS/lua-language-server/wiki/Diagnostics">diagnostic_name</a>, file_state></code>


**`file_state` Options:**
- `"Any"` - Any loaded file (workspace, library, etc.) will use this diagnostic group
- `"Opened"` - Only opened files will use this diagnostic group
- `"None"` - This diagnostic group will be disabled
- `"Any!"` - Like `"Any"` but overrides `diagnostics.groupFileStatus`
- `"Opened!"` - Like `"Opened"` but overrides `diagnostics.groupFileStatus`
- `"None!"` - Like `"None"` but overrides `diagnostics.groupFileStatus`

<details>

<summary>Default Value</summary>

```json
{
    "ambiguity-1": "Any",
    "assign-type-mismatch": "Opened",
    "await-in-sync": "None",
    "cast-local-type": "Opened",
    "cast-type-mismatch": "Any",
    "circle-doc-class": "Any",
    "close-non-object": "Any",
    "code-after-break": "Opened",
    "codestyle-check": "None",
    "count-down-loop": "Any",
    "deprecated": "Any",
    "different-requires": "Any",
    "discard-returns": "Any",
    "doc-field-no-class": "Any",
    "duplicate-doc-alias": "Any",
    "duplicate-doc-field": "Any",
    "duplicate-doc-param": "Any",
    "duplicate-index": "Any",
    "duplicate-set-field": "Any",
    "empty-block": "Opened",
    "global-in-nil-env": "Any",
    "lowercase-global": "Any",
    "missing-parameter": "Any",
    "missing-return": "Any",
    "missing-return-value": "Any",
    "need-check-nil": "Opened",
    "newfield-call": "Any",
    "newline-call": "Any",
    "no-unknown": "None",
    "not-yieldable": "None",
    "param-type-mismatch": "Opened",
    "redefined-local": "Opened",
    "redundant-parameter": "Any",
    "redundant-return": "Opened",
    "redundant-return-value": "Any",
    "redundant-value": "Any",
    "return-type-mismatch": "Opened",
    "spell-check": "None",
    "trailing-space": "Opened",
    "unbalanced-assignments": "Any",
    "undefined-doc-class": "Any",
    "undefined-doc-name": "Any",
    "undefined-doc-param": "Any",
    "undefined-env-child": "Any",
    "undefined-field": "Opened",
    "undefined-global": "Any",
    "unknown-cast-variable": "Any",
    "unknown-diag-code": "Any",
    "unknown-operator": "Any",
    "unreachable-code": "Opened",
    "unused-function": "Opened",
    "unused-label": "Opened",
    "unused-local": "Opened",
    "unused-vararg": "Opened"
}
```

</details>

<br>

### `diagnostics.severity`
**Type:**  <code>Object<<a href="https://github.com/LuaLS/lua-language-server/wiki/Diagnostics">diagnostic_name</a>, severity></code>


**`severity` Options:**
- `"Error"` - An error will be raised
- `"Warning"` - A warning will be raised
- `"Information"` - An information or note will be raised
- `"Hint"` - The affected code will be "hinted" at
- `"Error!"` - Like `"Error"` but overrides `diagnostics.groupSeverity`
- `"Warning!"` -Like `"Warning"` but overrides `diagnostics.groupSeverity`
- `"Information!"` - Like `"Information"` but overrides `diagnostics.groupSeverity`
- `"Hint!"` - Like `"Hint"` but overrides `diagnostics.groupSeverity`

<details>

<summary>Default Value</summary>

```json
{
    "ambiguity-1": "Warning",
    "assign-type-mismatch": "Warning",
    "await-in-sync": "Warning",
    "cast-local-type": "Warning",
    "cast-type-mismatch": "Warning",
    "circle-doc-class": "Warning",
    "close-non-object": "Warning",
    "code-after-break": "Hint",
    "codestyle-check": "Warning",
    "count-down-loop": "Warning",
    "deprecated": "Warning",
    "different-requires": "Warning",
    "discard-returns": "Warning",
    "doc-field-no-class": "Warning",
    "duplicate-doc-alias": "Warning",
    "duplicate-doc-field": "Warning",
    "duplicate-doc-param": "Warning",
    "duplicate-index": "Warning",
    "duplicate-set-field": "Warning",
    "empty-block": "Hint",
    "global-in-nil-env": "Warning",
    "lowercase-global": "Information",
    "missing-parameter": "Warning",
    "missing-return": "Warning",
    "missing-return-value": "Warning",
    "need-check-nil": "Warning",
    "newfield-call": "Warning",
    "newline-call": "Warning",
    "no-unknown": "Warning",
    "not-yieldable": "Warning",
    "param-type-mismatch": "Warning",
    "redefined-local": "Hint",
    "redundant-parameter": "Warning",
    "redundant-return": "Hint",
    "redundant-return-value": "Warning",
    "redundant-value": "Warning",
    "return-type-mismatch": "Warning",
    "spell-check": "Information",
    "trailing-space": "Hint",
    "unbalanced-assignments": "Warning",
    "undefined-doc-class": "Warning",
    "undefined-doc-name": "Warning",
    "undefined-doc-param": "Warning",
    "undefined-env-child": "Information",
    "undefined-field": "Warning",
    "undefined-global": "Warning",
    "unknown-cast-variable": "Warning",
    "unknown-diag-code": "Warning",
    "unknown-operator": "Warning",
    "unreachable-code": "Hint",
    "unused-function": "Hint",
    "unused-label": "Hint",
    "unused-local": "Hint",
    "unused-vararg": "Hint"
}
```

</details>

<br>

### `diagnostics.unusedLocalExclude`
**Type:**  `Array<string>`

**Default:**  `[]`


Define variable names that will not be reported as an unused local by [`unused-local`](https://github.com/LuaLS/lua-language-server/wiki/Diagnostics#unused-local).

<br>

### `diagnostics.workspaceDelay`
**Type:**  `integer`

**Default:**  `3000`


Define the delay between diagnoses of the workspace in milliseconds. Every time a file is edited, created, deleted, etc. the workspace will be re-diagnosed in the background after this delay. Setting to a negative number will disable workspace diagnostics.

<br>

### `diagnostics.workspaceEvent`
**Type:** `string`

**Default:** `"OnSave"`

**Options:**

- `"OnChange"`
- `"OnSave"`
- `"None"`

Set when the workspace diagnostics should be analyzed. It can be performed after each change, after a save, or never automatically triggered.

<br>

### `diagnostics.workspaceRate`
**Type:**  `integer`

**Default:**  `100`


Define the rate at which the workspace will be diagnosed as a percentage. `100` is 100% speed so the workspace will be diagnosed as fast as possible. The rate can be lowered to reduce CPU usage, but the diagnosis speed will also become slower. The currently opened file will always be diagnosed at `100`% speed, regardless of this setting.


## doc
Settings for configuring documentation comments.

<br>

### `doc.packageName`
**Type:** `Array<string>`

**Default:** `[]`

The pattern used for matching field names as a package-private field. Fields that match any of the patterns provided will be package-private.

<br>

### `doc.privateName`
**Type:** `Array<string>`

**Default:** `[]`

The pattern used for matching field names as a private field. Fields that match any of the patterns provided will be private to that class.

### `doc.protectedName`
**Type:** `Array<string>`

**Default:** `[]`

The pattern used for matching field names as a protected field. Fields that match any of the patterns provided will be private to that class and its child classes.


## format
Settings for configuring the built-in code formatter.

<br>

### `format.defaultConfig`
**Type:**  `Object<string, string`

**Default:**  `{}`


The default configuration for the formatter. If there is a `.editorconfig` in the workspace, it will take priority. Read more on the [formatter's GitHub page](https://github.com/CppCXY/EmmyLuaCodeStyle/tree/master/docs).

<br>

### `format.enable`
**Type:**  `boolean`

**Default:**  `true`


Whether the built-in formatted should be enabled or not.



## hint
Settings for configuring inline hints

<br>

### `hint.arrayIndex`
**Type:**  `string`

**Default:**  `"Auto"`

**Options:**

- `"Enable"` - Show hint in all tables
- `"Auto"` - Only show hint when there is more than 3 items or the table is mixed (indexes and keys)
- `"Disable"` - Disable array index hints

![hint arrayIndex](https://user-images.githubusercontent.com/61925890/181309302-ccd2be1d-6488-4d4e-b375-42cf9bc5b8cd.png)

<br>

### `hint.await`
**Type:**  `boolean`

**Default:**  `true`


If a function has been defined as [`@async`](https://github.com/LuaLS/lua-language-server/wiki/Annotations#async), display an `await` hint when it is being called.

<br>

### `hint.enable`
**Type:**  `boolean`

**Default:**  `false`


Whether inline hints should be enabled or not.

<br>

### `hint.paramName`
**Type:**  `string`

**Default:**  `"All"`

**Options:**

- `"All"` - All parameters are hinted
- `"Literal"` - Only literal type parameters are hinted
- `"Disable"` - No parameter hints are shown

Whether parameter names should be hinted when typing out a function call.

<br>

### `hint.paramType`
**Type:**  `boolean`

**Default:**  `true`


Show a hint for parameter types at a function definition. Requires the parameters to be defined with [`@param`](https://github.com/LuaLS/lua-language-server/wiki/Annotations#param)

<br>

### `hint.semicolon`
**Type:**  `string`

**Default:**  `"SameLine"`

**Options:**

- `"All"` - Show on every line
- `"SameLine"` - Show between two statements on one line
- `"Disable"` - Never hint a semicolon

Whether to show a hint to add a semicolon to the end of a statement.

<br>

### `hint.setType`
**Type:**  `boolean`

**Default:**  `false`


Show a hint to display the type being applied at assignment operations.



## hover
Setting for configuring hover tooltips. The hover tooltips can contain all kinds of useful information.

<br>

### `hover.enable`
**Type:**  `boolean`

**Default:**  `true`


Whether to enable hover tooltips or not.

<br>

### `hover.enumsLimit`
**Type:**  `integer`

**Default:**  `5`


When a value has multiple possible types, hovering it will display them. This setting limits how many will be displayed in the tooltip before they are truncated.

<br>

### `hover.expandAlias`
**Type:**  `boolean`

**Default:**  `true`


When hovering a value with an [`@alias`](https://github.com/LuaLS/lua-language-server/wiki/Annotations#alias) for its type, should the alias be expanded into its values. When enabled, `---@alias myType boolean|number` appears as `boolean|number`, otherwise it will appear as `myType`.

<br>

### `hover.previewFields`
**Type:**  `integer`

**Default:**  `50`


When a table is hovered, its fields will be displayed in the tooltip. This setting limits how many fields can be seen in the tooltip. Setting to `0` will disable this feature.

<br>

### `hover.viewNumber`
**Type:**  `boolean`

**Default:**  `true`


Enable hovering a non-decimal value to see its numeric value.

<br>

### `hover.viewString`
**Type:**  `boolean`

**Default:**  `true`


Enable hovering a string that contains an escape character to see its true string value. For example, hovering `"\x48"` will display `"H"`.

<br>

### `hover.viewStringMax`
**Type:**  `integer`

**Default:**  `1000`


The maximum number of characters that can be previewed by hovering a string before it is truncated.



## misc
Miscellaneous settings that do not belong to any of the other groups.

<br>

### `misc.parameters`
**Type:**  `Array<string>`

**Default:**  `[]`


[Command line parameters](https://github.com/LuaLS/lua-language-server/wiki/Getting-Started#run) to be passed along to the server `exe` when starting through Visual Studio Code.

<br>

### `misc.executablePath`
**Type:** `string`

**Default:** `""`

Manually specify the path for the Lua Language Server executable file.


## runtime
Settings for configuring the runtime environment.

<br>

### `runtime.builtin`
**Type:**  `Object<library, status>`

**`library` Options:**
- `"basic"`
- `"bit"`
- `"bit32"`
- `"builtin"`
- `"coroutine"`
- `"debug"`
- `"ffi"`
- `"io"`
- `"jit"`
- `"math"`
- `"os"`
- `"package"`
- `"string"`
- `"table"`
- `"table.clear"`
- `"table.new"`
- `"utf8"`

**`status` Options:**
- `"default"` - The library will be enabled if it is a part of the current [`runtime.version`](https://github.com/LuaLS/lua-language-server/wiki/Settings#runtimeversion).
- `"enable"` - Always enable this library
- `"disable"` - Always disable this library

<details>

<summary>Default Value</summary>

```json
{
    "basic": "default",
    "bit": "default",
    "bit32": "default",
    "builtin": "default",
    "coroutine": "default",
    "debug": "default",
    "ffi": "default",
    "io": "default",
    "jit": "default",
    "math": "default",
    "os": "default",
    "package": "default",
    "string": "default",
    "table": "default",
    "table.clear": "default",
    "table.new": "default",
    "utf8": "default"
}
```

</details>

Set whether each of the builtin Lua libraries is available in the current runtime environment.

<br>

### `runtime.fileEncoding`
**Type:**  `string`

**Default:**  `utf8`

**Options:**

- `"utf8"`
- `"ansi"` (only available on Windows)
- `"utf16le"`
- `"utf16be"`

<br>

### `runtime.meta`
**Type:**  `string`

**Default:**  `"${version} ${language} ${encoding}"`


<!-- TODO: description -->
🚧 Description needed 🚧

<br>

### `runtime.nonstandardSymbol`
**Type:**  `Array<string>`

**Default:**  `[]`

**Options:**

- `"//"`
- `"/**/"`
- ``"`"``
- `"+="`
- `"-="`
- `"*="`
- `"/="`
- `"%="`
- `"^="`
- `"//="`
- `"|="`
- `"&="`
- `"<<="`
- `">>="`
- `"||"`
- `"&&"`
- `"!"`
- `"!="`
- `"continue"`

Add support for non-standard symbols. Make sure to double check that your runtime environment actually supports the symbols you are permitting as standard Lua does not.

<br>

### `runtime.path`
**Type:**  `Array<string>`

**Default:**  `["?.lua", "?/init.lua"]`


Defines the paths to use when using `require`. For example, setting to `?/start.lua` will search for `<workspace>/myFile/start.lua` from the loaded files when doing `require"myFile"`. If [`runtime.pathStrict`](https://github.com/LuaLS/lua-language-server/wiki/Settings#runtimepathstrict) is `false`, `<workspace>/**/myFile/start.lua` will also be searched. To load files that are not in the current workspace, they will first need to be loaded using [`workspace.library`](https://github.com/LuaLS/lua-language-server/wiki/Settings#workspacelibrary).

<br>

### `runtime.pathStrict`
**Type:**  `boolean`

**Default:**  `false`


When enabled, [`runtime.path`](https://github.com/LuaLS/lua-language-server/wiki/Settings#runtimepath) will only search the first level of directories. See the description of `runtime.path` for more info.

<br>

### `runtime.plugin`
**Type:**  `string`

**Default:**  `""`


The path to the [plugin](https://github.com/LuaLS/lua-language-server/wiki/Plugins) to use. Blank by default for security reasons.

<br>

### `runtime.pluginArgs`
**Type:**  `Array<string>`

**Default:**  `[]`


Additional arguments that will be passed to the active [plugin](https://github.com/LuaLS/lua-language-server/wiki/Plugins).

<br>

### `runtime.special`
**Type:**  `Object<string, string>`

**Default:**  `{}`


Special variables can be set to be treated as other variables. For example, specifying `"include" : "require"` will result in `include` being treated like `require`.

<br>

### `runtime.unicodeName`
**Type:**  `boolean`

**Default:**  `false`


Whether unicode characters should be allowed in variable names or not.

<br>

### `runtime.version`
**Type:**  `string`

**Default:**  `"Lua 5.4"`

**Options:**

- `"Lua 5.1"`
- `"Lua 5.2"`
- `"Lua 5.3"`
- `"Lua 5.4"`
- `"LuaJIT"`

The Lua runtime version to use in this environment.



## semantic
The semantic group contains settings for semantic colouring in the editor.

<br>

### `semantic.annotation`
**Type:**  `boolean`

**Default:**  `true`


Whether semantic colouring should be enabled for type annotations.

<br>

### `semantic.enable`
**Type:**  `boolean`

**Default:**  `true`


Whether semantic colouring should be enabled. You may need to set `editor.semanticHighlighting.enabled` to true in order for this setting to take effect.

<br>

### `semantic.keyword`
**Type:**  `boolean`

**Default:**  `false`


Whether the server should provide semantic colouring of keywords, literals, and operators. You should only need to enable this setting if your editor is unable to highlight Lua's syntax.

<br>

### `semantic.variable`
**Type:**  `boolean`

**Default:**  `true`


Whether the server should provide semantic colouring of variables, fields, and parameters.



## signatureHelp
The signatureHelp group contains settings for helping understand signatures.

<br>

### `signatureHelp.enable`
**Type:**  `boolean`

**Default:**  `true`

<!-- TODO: description -->
🚧 Description needed 🚧



## spell
The spell group contains settings that help detect typos and misspellings.

<br>

### `spell.dict`
**Type:**  `Array<string>`

**Default:**  `[]`


A custom dictionary of words that you know are spelt correctly but are being reported as incorrect. Adding words to this dictionary will make them exempt from spell checking.



## telemetry
The telemetry group contains settings for the opt-in telemetry.

<br>

### `telemetry.enable`
**Type:**  `boolean | null`

**Default:**  `null`


The language server comes with opt-in telemetry to help improve the language server. It would be greatly appreciated if you enable this setting. Read the [privacy policy](https://github.com/LuaLS/lua-language-server/wiki/Home#privacy) before enabling.



## type
The type group contains settings for type checking.

<br>

### `type.castNumberToInteger`
**Type:**  `boolean`

**Default:**  `false`


Whether casting a `number` to an `integer` is allowed.

<br>

### `type.weakNilCheck`
**Type:**  `boolean`

**Default:**  `false`


Whether it is permitted to assign a union type that contains `nil` to a variable that does not permit it. When `false`, the following will throw an error because `number|nil` cannot be assigned to `number`.

```lua
---@alias unionType number|nil

---@type number
local num

---@cast num unionType
```

<br>

### `type.weakUnionCheck`
**Type:**  `boolean`

**Default:**  `false`


Whether it is permitted to assign a union type which only has one matching type to a variable. When `false`, the following will throw an error because `string|boolean` cannot be assigned to `string`.

```lua
---@alias unionType string|boolean

---@type string
local str = false
```


## window
The window group contains settings that modify the window in VS Code.

<br>

### `window.progressBar`
**Type:**  `boolean`

**Default:**  `true`


Show a progress bar in the bottom status bar that shows how the workspace loading is progressing.

<br>

### `window.statusBar`
**Type:**  `boolean`

**Default:**  `true`


Show a `Lua 😺` entry in the bottom status bar that can be clicked to manually perform a workspace diagnosis.



## workspace
The workspace group contains settings for configuring how the workspace diagnosis works.

<br>

### `workspace.checkThirdParty`
**Type:**  `boolean`

**Default:**  `true`


Whether [third party libraries](https://github.com/LuaLS/lua-language-server/wiki/Libraries) can be automatically detected and applied. Third party libraries can set up the environment to be as close as possible to your target runtime environment. See [`meta/3rd`](https://github.com/LuaLS/lua-language-server/tree/master/meta/3rd) to view what third party libraries are currently supported.

<br>

### `workspace.ignoreDir`
**Type:**  `Array<string>`

**Default:**  `[".vscode"]`


An array of paths that will be ignored and not included in the workspace diagnosis. Uses `.gitignore` grammar. Can be a file or directory.

<br>

### `workspace.ignoreSubmodules`
**Type:**  `boolean`

**Default:**  `true`


Whether [git submodules](https://github.blog/2016-02-01-working-with-submodules/) should be ignored and not included in the workspace diagnosis.

<br>

### `workspace.library`
**Type:**  `Array<string>`

**Default:**  `[]`


An array of abosolute or workspace-relative paths that will be added to the workspace diagnosis - meaning you will get completion and context from these library files. Can be a file or directory. Files included here will have some features disabled such as renaming fields to prevent accidentally renaming your library files. Read more on the [Libraries page](https://github.com/LuaLS/lua-language-server/wiki/Libraries).

<br>

### `workspace.maxPreload`
**Type:**  `integer`

**Default:**  `5000`


The maximum amount of files that can be diagnosed. More files will require more RAM.

<br>

### `workspace.preloadFileSize`
**Type:**  `integer`

**Default:**  `500`


Files larger than this value (in KB) will be skipped when loading for workspace diagnosis.

<br>

### `workspace.supportScheme`
**Type:**  `Array<string>`

**Default:**  `["file", "untitled", "git"]`


Lua file schemes to enable the language server for.

<br>

### `workspace.useGitIgnore`
**Type:**  `boolean`

**Default:**  `true`


Whether files that are in `.gitignore` should be ignored by the language server when performing workspace diagnosis.

<br>

### `workspace.userThirdParty`
**Type:**  `Array<string>`

**Default:**  `[]`


An array of paths to [custom third party libraries](https://github.com/LuaLS/lua-language-server/wiki/Libraries#custom). This path should point to a directory where **all** of your custom libraries are, not just to one of the libraries. If the below is your file structure, this setting should be `"myLuaLibraries"` - of course this should be an absolute path though.

```txt
📦 myLuaLibraries/
    ├── 📂 myCustomLibrary/
    │   ├── 📂 library/
    │   └── 📜 config.lua
    └── 📂 anotherCustomLibrary/
        ├── 📂 library/
        └── 📜 config.lua
```
