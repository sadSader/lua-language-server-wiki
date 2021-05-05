## External libraries
EmmyLua [annotations](https://github.com/sumneko/lua-language-server/wiki/EmmyLua-Annotations) and other source code can be manually added as external libraries via the `Lua -> Workspace: Library` option or in `settings.json`
```json
	"Lua.workspace.library": [
		"d:\\SomePath\\EmmyLuaFolder"
	],
```
![](https://user-images.githubusercontent.com/1073877/115629918-7f3f0d00-a303-11eb-954f-134cb646c030.png)

## Plugin extension
* _Disclaimer: This article was written by a [user](https://github.com/sumneko/lua-language-server/issues/417)._
* Built-in libraries are also planned to be supported for commonly used environments, see [#409](https://github.com/sumneko/lua-language-server/issues/409).

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
	const extensionId = "publisher.name"
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
2. Only on the second restart will the EmmyLua files not be preloaded since the extension is now gone.

_(The writer doesn't know a way to properly remove the configuration entry on disable/uninstall.)_