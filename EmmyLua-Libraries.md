## External libraries
EmmyLua [annotations](https://github.com/sumneko/lua-language-server/wiki/EmmyLua-Annotations) can be manually added as an external library via the `Lua -> Workspace: Library` option or in `settings.json`
```json
	"Lua.workspace.library": [
		"d:\\SomePath\\EmmyLuaFolder"
	],
```
![](https://user-images.githubusercontent.com/1073877/115629918-7f3f0d00-a303-11eb-954f-134cb646c030.png)

## Plugin extension
* _Disclaimer: This article was written by [someone](https://github.com/sumneko/lua-language-server/issues/417) inexperienced with TypeScript and VS Code extensions._
* Built-in libraries are also planned to be supported for commonly used environments, see [#409](https://github.com/sumneko/lua-language-server/issues/409).

An [extension](https://code.visualstudio.com/api/get-started/your-first-extension) that ships its own EmmyLua folder(s) can choose to automatically add this path.

1. Add an extension [dependency](https://code.visualstudio.com/api/references/extension-manifest) to your `package.json`
```json
	"extensionDependencies": [
		"sumneko.lua"
	],
```

2.  Add the path to your EmmyLua folder in the configuration.
```ts
function setExternalLibrary(enable: boolean) {
	let name = "publisher.name" // your extension id
	// get emmylua path
	let extension = vscode.extensions.getExtension(name)
	let path = extension?.extensionPath+"\\EmmyLuaFolder"
	// get configuration
	let luaConfig = vscode.workspace.getConfiguration("Lua")
	let config: string[] | undefined = luaConfig.get("workspace.library")
	if (config) {
		// remove any older versions of our path e.g. "publisher.name-0.0.1"
		for (let i = config.length-1; i >= 0; i--) {
			const el = config[i]
			if (el.indexOf(name) > -1 && el.indexOf(path) == -1) {
				config.splice(i, 1)
			}
		}
		// add or remove path
		const index = config.indexOf(path)
		if (enable) {
			if (index == -1) {
				config.push(path)
			}
		}
		else {
			if (index > -1) {
				config.splice(index, 1)
			}
		}
		luaConfig.update("workspace.library", config, true)
	}
}

setExternalLibrary(true)
```
Note that when your extension is uninstalled this path will still remain in the configuration.
1. After restarting VS Code, the extension files will be removed from disk. However the EmmyLua files would still be already preloaded.
2. Only on the second restart will the EmmyLua files not be preloaded since the extension is now gone.

_(The writer doesn't know a way to properly remove the configuration entry on disable/uninstall.)_