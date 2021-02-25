_Disclaimer: This article was written by [someone](https://github.com/sumneko/lua-language-server/issues/417) inexperienced with TypeScript and VS Code extensions._

[EmmyLua](https://github.com/EmmyLua) annotations can be manually added as an external library via the `Lua -> Workspace: Library` option or in `settings.json`
```json
    "Lua.workspace.library": {
        "D:\\SomePath\\EmmyLuaFolder": true
    },
```
To make this more user friendly, an extension that ships its own EmmyLua folder(s) can choose to automatically add this path.

* Add an extension [dependency](https://code.visualstudio.com/api/references/extension-manifest) to your `package.json`
```json
	"extensionDependencies": [
		"sumneko.lua"
	]
```
* Set the path to your EmmyLua folder in the configuration.
```ts
// get emmylua path
let extension = vscode.extensions.getExtension("publisher.name")
let path = extension?.extensionPath+"\\SomeEmmyLua"

// add it to the external libraries
let luaConfig = vscode.workspace.getConfiguration("Lua")
let library: any = luaConfig.get("workspace.library")
library[path] = true
luaConfig.update("workspace.library", library, true)
```
Note that when your extension is uninstalled this path will still remain in the configuration, including older versions e.g. `ketho.wow-api-0.1.0`, `...-0.0.2`
1. After restarting VS Code, the extension files will be removed from disk. However the EmmyLua files would still be already preloaded.
2. Only after the second restart will the EmmyLua files not be preloaded since the extension is now gone.

_(The writer doesn't know a way to properly cleanup the configuration entry on disable/uninstall.)_