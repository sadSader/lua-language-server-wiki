## External libraries
EmmyLua [annotations](https://github.com/sumneko/lua-language-server/wiki/EmmyLua-Annotations) and other source code can be manually added as external libraries via the `Lua -> Workspace: Library` option or in `settings.json`
```json
    "Lua.workspace.library": [
        "d:\\SomePath\\EmmyLuaFolder"
    ],
```
![](https://user-images.githubusercontent.com/1073877/115629918-7f3f0d00-a303-11eb-954f-134cb646c030.png)

## Third party directories

Third party directories are somewhat similar to workspace libraries, but aside from just providing meta definitions, you can define:
* When to suggest the third party library
* Changes to apply to the [server configuration](https://github.com/sumneko/lua-language-server/wiki/Setting)
* [Plugins](https://github.com/sumneko/lua-language-server/wiki/Plugin)

![image](https://user-images.githubusercontent.com/79615454/160298856-ec66a65b-448a-4eff-b650-060c02616e71.png)

You can find more information on the motivation of this in [#409](https://github.com/sumneko/lua-language-server/issues/409). Built-in libraries are also supported for commonly used environments, such as OpenResty.

To add a custom third party directory, supply the path in `Lua.workspace.userThirdParty`:
```json
    "Lua.workspace.userThirdParty": [
        "d:\\SomePath\\ThirdPartyFolder",
        "./meta/MyThirdParty"
    ],
```

Note: The language server will search for content in **sub-directories** of the path supplied to `Lua.workspace.userThirdParty`, meaning that your `userThirdParty` should look like that:
> * MyThirdParty/
>   * Library1/
>     * library/
>     * config.lua
>   * Library2/
>     * library/
>     * config.lua
>     * plugin.lua

You can find examples for third party libraries [here](https://github.com/sumneko/lua-language-server/tree/master/meta/3rd).

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
2. ~~Only on the second restart will the EmmyLua files not be preloaded since the extension is now gone.~~ The extension files will still be on disk. https://github.com/Ketho/vscode-wow-api/issues/20

There is a [deactivate()](https://code.visualstudio.com/api/references/activation-events) event and an [uninstall hook](https://code.visualstudio.com/api/references/extension-manifest#extension-uninstall-hook) but it appears they [cannot edit](https://github.com/microsoft/vscode/issues/45474) the configuration.
