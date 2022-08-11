# Libraries
Libraries can be used to provide definitions that allow you to very closely emulate your target environment without actually `require`-ing them.

They can be extremely powerful, but they can also come with [some performance issues](https://github.com/sumneko/lua-language-server/wiki/FAQ#how-can-i-improve-startup-speeds) when they are very large or there are many being included.


## Built-in Libraries
There are a number of built-in third party libraries that can be found in [`meta/3rd/`](https://github.com/sumneko/lua-language-server/tree/master/meta/3rd). These include:

- `Cocos 4.0`
- `Jass`
- `OpenResty`
- `lfs`
- `love2d`
- `lovr`
- `skynet`

<br>

## Custom
You can create your own libraries through various methods.

The definition files can be created using the same [annotations](https://github.com/sumneko/lua-language-server/wiki/Annotations) you use in your Lua scripts. Make sure to include a [`@meta`](https://github.com/sumneko/lua-language-server/wiki/Annotations#meta) tag in your definition files.

<br>

### Placing in Your Workspace
**Difficulty:** Easy

**Use Case:** Not Recommended

This is the easiest method, as you only need to drop your library definitions and/or source code into your workspace directory (under a `definitions/` directory is recommended).

This may be usable for a very small project, however, for larger ones, especially those with source control, **it is not recommended.** This is because you now have your development definitions in with your source code making them hard to re-use and they will need to be git ignored anyways.

<br>

### Link to Workspace
**Difficulty:** Easy

**Use Case:** Any Workspace

This method uses the [`workspace.library`](https://github.com/sumneko/lua-language-server/wiki/Settings#workspacelibrary) setting to link the requested library to your workspace scope. This is great for allowing you to store your libraries elsewhere and then include them when needed through your [configuration file](https://github.com/sumneko/lua-language-server/wiki/Configuration-File).

Unless you **always** use a certain library, it is strongly recommended you only define `workspace.library` in each project/workspace using `.vscode/settings.json` or `.luarc.json` in order to prevent slowdowns from loading unnecessary libraries.

<br>

### Environment Emulation
**Difficulty:** Medium

**Use Case:** Proper Emulation of Target Environment

This method is the most in-depth and allows you to very closely emulate your target environment for many projects with easy repeatability. This is how the [built-in libraries](#built-in-libraries) are implemented. If your target environment is a game engine where you may not have access to all of Lua, this is a great option.

As well as providing defintions, you can also define when to suggest setting up the environment for this library, what changes to apply to the server's [configuration](https://github.com/sumneko/lua-language-server/wiki/Configuration-File), and what [plugins](https://github.com/sumneko/lua-language-server/wiki/Plugins) to use.

#### Setup
To get started, you will need a directory where all of your emulations can be stored e.g. `C:\Users\me\Documents\LuaEnvironments`. In this directory you will create a new directory for each environment to emulate.

```bash
📂 LuaEnvironments/
    ├── 📂 Environment1/
    │    ├── 📁 library/
    │    ├── 📜 config.lua
    │    └── 📜 plugin.lua
    └── 📂 Environment2/
         ├── 📁 library/
         └── 📜 config.lua
```

##### Definition Files
Your defintion files should have a [`@meta`](https://github.com/sumneko/lua-language-server/wiki/Annotations#meta) annotation to mark them as such. They can then be placed in the `library/` directory within the environment they help emulate.

##### Configuration File
The `config.lua` file is what lets you configure when the emulation should be recommended and what settings to apply.

```lua
-- config.lua

-- The name to use when suggesting this emulation. If omitted,
-- the name of the folder will be used
name = "Example Environment"

-- A list of words to look for in Lua files. If a match is
-- found, this environment will be recommended
words = {
    "example", -- exact match
    "testing%.%w+" -- wildcard match, matches testing.anyWord
}

-- A list of filenames to look for in the workspace. If a
-- match is found, this environment will be recommended
files = {
    "example%.lua", -- exact match
    "example/.*%.lua" -- wildcard match any Lua file in example/
}

-- configuration values to set/override in the user's local
-- config file when this emulation is applied
configs = {

    -- Set boolean/string/number value
    {
        key    = "Lua.runtime.version",
        action = "set",
        value  = "LuaJIT"
    },

    -- Add to array
    {
        key    = "Lua.diagnostics.globals",
        action = "add",
        value  = "exampleValue"
    },

    -- Add prop to object
    {
        key    = 'Lua.runtime.special',
        action = 'prop',
        prop   = 'include',
        value  = 'require',
    },
    {
        key    = 'Lua.runtime.builtin',
        action = 'prop',
        prop   = 'io',
        value  = 'disable',
    }
}

-- You can of course also execute Lua in here to make
-- things a little easier
local GLOBALS = { "Global, Global2, Global3" }
for _, name in ipairs(GLOBALS) do
    table.insert(configs, {
        key    = "Lua.diagnostics.globals",
        action = "add",
        value  = name
    })
end
```

See [settings](https://github.com/sumneko/lua-language-server/wiki/Settings) for more info on `configs`.

You can also include a [plugin](https://github.com/sumneko/lua-language-server/wiki/Plugins).

Have an environment emulation to share? [Post it in discussion #389](https://github.com/sumneko/lua-language-server/discussions/389).

## Bundling in a plugin extension
* _Disclaimer: This article was written by a [user](https://github.com/sumneko/lua-language-server/issues/417)._

An [extension](https://code.visualstudio.com/api/get-started/your-first-extension) that ships its own EmmyLua folder(s) can choose to automatically add this path.

1. Add an extension [dependency](https://code.visualstudio.com/api/references/extension-manifest) to your `package.json`
```json
    "extensionDependencies": [
        "sumneko.lua"
    ],
```

2.  Add the path to your folder(s) in the configuration.
```ts
function setExternalLibrary(folder: string, enable: boolean) {
	const extensionId = "publisher.name" // this id is case sensitive
	const extensionPath = vscode.extensions.getExtension(extensionId)?.extensionPath
	const folderPath = extensionPath+"\\"+folder
	const config = vscode.workspace.getConfiguration("Lua")
	const library: string[] | undefined = config.get("workspace.library")
	if (library && extensionPath) {
		// remove any older versions of our path e.g. "publisher.name-0.0.1"
		for (let i = library.length-1; i >= 0; i--) {
			const el = library[i]
			const isSelfExtension = el.indexOf(extensionId) > -1
			const isCurrentVersion = el.indexOf(extensionPath) > -1
			if (isSelfExtension && !isCurrentVersion) {
				library.splice(i, 1)
			}
		}
		const index = library.indexOf(folderPath)
		if (enable) {
			if (index == -1) {
				library.push(folderPath)
			}
		}
		else {
			if (index > -1) {
				library.splice(index, 1)
			}
		}
		config.update("workspace.library", library, true)
	}
}

setExternalLibrary("EmmyLua", true)
```
```json
    "Lua.workspace.library": [
        "c:\\Users\\UserName\\.vscode\\extensions\\publisher.name-0.0.2\\EmmyLua"
    ],
```
Note that when your extension is uninstalled this path will still remain in the configuration.
1. After restarting VS Code, the extension files will be removed from disk. However the EmmyLua files would still be already preloaded.
2. ~~Only on the second restart will the EmmyLua files not be preloaded since the extension is now gone.~~ The extension files will still be on disk. [https://github.com/Ketho/vscode-wow-api/issues/20](https://github.com/Ketho/vscode-wow-api/issues/20)

There is a [deactivate()](https://code.visualstudio.com/api/references/activation-events) event and an [uninstall hook](https://code.visualstudio.com/api/references/extension-manifest#extension-uninstall-hook) but it appears they [cannot edit](https://github.com/microsoft/vscode/issues/45474) the configuration.
