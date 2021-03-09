_Disclaimer: This article was written by [someone](https://github.com/sumneko/lua-language-server/issues/417) inexperienced with TypeScript and VS Code extensions._

[EmmyLua](https://github.com/EmmyLua) annotations can be manually added as an external library via the `Lua -> Workspace: Library` [option](https://github.com/sumneko/vscode-lua/blob/v1.18.0/setting/schema.json#L1096-L1103) or in `settings.json`
```json
    "Lua.workspace.library": [
        "d:\\SomePath\\EmmyLuaFolder"
    ],
```
To make this more user friendly, an extension that ships its own EmmyLua folder(s) can choose to automatically add this path.

* Add an extension [dependency](https://code.visualstudio.com/api/references/extension-manifest) to your `package.json`. Note: This seems to be broken since VS Code 1.54.1 ([#444](https://github.com/sumneko/lua-language-server/issues/444))
```json
	"extensionDependencies": [
		"sumneko.lua"
	]
```
* Set the path to your EmmyLua folder in the configuration.
```ts
function setExternalLibrary(enable: boolean) {
	// get emmylua path
	let extension = vscode.extensions.getExtension("publisher.name")
	let path = extension?.extensionPath+"\\EmmyLuaFolder"
	// add it to the external libraries
	let luaConfig = vscode.workspace.getConfiguration("Lua")
	let library: string[] | undefined = luaConfig.get("workspace.library")
	// check if sumneko.lua is loaded
	if (library) {
		const index = library.indexOf(path)
		if (enable) {
			if (index == -1)
				library.push(path)
		}
		else {
			if (index > -1)
				library.splice(index, 1)
		}
		luaConfig.update("workspace.library", library, true)
	}
}

setExternalLibrary(true)
```
Note that when your extension is uninstalled this path will still remain in the configuration, including older versions e.g. `ketho.wow-api-0.0.1`, `...-0.0.2`
1. After restarting VS Code, the extension files will be removed from disk. However the EmmyLua files would still be already preloaded.
2. Only after the second restart will the EmmyLua files not be preloaded since the extension is now gone.

_(The writer doesn't know a way to properly cleanup the configuration entry on disable/uninstall.)_