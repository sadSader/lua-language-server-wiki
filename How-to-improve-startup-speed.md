# 长话短说|Long story short
服务器的启动时间正比于你工作区中所有Lua文件的总大小，因此最有效的方式是使用设置 `workspace.ignoreDir` 来排除非必要的目录与文件。

--------------------------------

The startup time of the server is proportional to the total size of all Lua files in your workspace, so the most effective way is to set `workspace.ignoreDir` to exclude unnecessary directories and files.

# 加载时间分析|Loading time analysis
我使用了一个包含1500个Lua文件，总大小为20MB的真实项目进行了测试。
我的测试CPU为 `i7-9700K` ，项目文件放在HDD中，插件本身放在SSD中。

经过多次测试，20MB文件的 `加载工作区` 耗时约为10秒，即 `2MB/sec` 的加载速度。

1. 初始化服务器，与客户端握手并读取配置：0.5秒
2. 扫描工作区中的文件，找出所有Lua文件路径：0.25秒
3. `加载工作区` 开始，把Lua文件的内容读进内存：1.5秒
4. 把Lua文件的内容编译为语法树：6.5秒
5. 根据语法树收集文件中的全局变量等信息：1秒
6. 其他的时间花费：1秒
7. `加载工作区` 结束，可以使用了。

> 由于我进行了多次测试，而操作系统会在一定程度上缓存运算结果与文件读取，因此他们的测试表现会好于实际表现。

> 第2步扫描文件受到你工作区中总文件数量的影响（包括非Lua文件）。

> 实际工作时，第2步与第3步会同时进行，可以加快0.5秒左右。

根据测试，最耗时的部分为“将Lua文件编译为语法树”。这个时间符合预期，因此可能没有什么改进空间了。
正如之前所说的，最好的办法是通过设置排除掉无用的目录与文件，不要加载无用的`workspace.library`。

--------------------------------

I tested it with a real project containing 1500 Lua files with a total size of 20MB.
My test CPU is `i7-9700K`. The project file is placed in HDD and the server itself is placed in SSD.

After many tests, the `Loading workspace` of 20MB files takes about 10 seconds, that is, the loading speed is "2MB/sec".

1. Initialize the server, shake hands with the client and read the configuration: 0.5 sec
2. Scan the files in the workspace and find out all Lua file paths: 0.25 sec
3. Start `Loading workspace` and read the contents of Lua files into memory: 1.5 sec
4. Compile the contents of lua files into ASTs: 6.5 sec
5. Collect information such as global variables in the file according to the ASTs: 1 sec
6. Other: 1 sec
7. The `loading workspace` is finished, the server can work.

>Because I have test many times, the OS cached the computes and IOs, so the test performance will be better than the actual performance.

>Step 2 "Scan the files" are affected by the total number of files in your workspace (including non Lua files).

>In actual, step 2 and step 3 will be merged, which can save about 0.5 sec.

According to the test, the most time-consuming part is "Compile the contents of lua files into ASTs". This time is matching expectation, so there may be no room for improvement.

As mentioned earlier, the best way is to exclude unnecessary files and directories by settings and do not load unnecessary `workspace.ibrary`.

# 多进度条/单文件模式|Multi progress bar/Single file mode
在阅读接下来的内容之前，请先看一下 https://github.com/sumneko/lua-language-server/wiki/Multi-workspace-supports

当你以单文件模式启动服务器时，服务器只会创建 `<fallback>` Scope，所有的Lua文件都在此处理。

当你以工作区模式启动服务器时，服务器实际上会创建2个Scope，一个是你的工作区，另一个则是 `<fallback>` 。当你打开一个不属于工作区的Lua文件时，该文件会被放置到 `<fallback>` 中，以免该文件的全局变量污染你的工作区环境。

由于我的开发环境是VSCode，而VSCode可以简单的为不同的工作区进行不同的设置，因此我做出了以下假设：
* 用户会给工作区单独设置 `workspace.library`，因为Lua的能力主要由宿主提供，而不同的工作区一般会对应不同的宿主
* 用户不会给全局设置 `workspace.library` ，因为它的主要目的是快速的看一下某个环境未知/不需要环境的Lua代码。

但最近的用户反馈让我发现了一个问题，在非VSCode环境中用户不方便给每个工作区进行单独设置（虽然我也提供了 `.luarc.json` 的方式），很多人直接会在全局设置中设置 `workspace.library`，当 library 较大时导致了一些问题：
* 以单文件模式启动服务器时，启动速度会被 library 影响，导致你明明只是想快速的看一下某一段Lua代码，却需要等待漫长的加载时间。
* 以工作区模式启动服务器时，会看到多个进度条。如果 `<fallback>` 只需要加载基础的Lua API通常可以在0.5秒内完成（进度条有0.5秒的显示延迟），这使得你不会看到 `<fallback>` 的加载进度条。但 `<fallback>` 受到 library 影响后也需要加载很多文件，这导致你会看到多个进度条。不过这不会影响实际的加载速度，因为同一个文件只会被加载一次。

介于这个情况，我在考虑添加一个默认开启的设置，使得 `<fallback>` 可以忽略 `workspace.library` 。

-----------------------------------------------

Please take a look before reading the following content: https://github.com/sumneko/lua-language-server/wiki/Multi-workspace-supports

When you start the server in single file mode, the server will only create a `<fallback>` scope, and all Lua files will be processed here.

When you start the server in workspace mode, the server will actually create two scopes, one is your workspace, and the other is the `<fallback>'. When you open a Lua file that does not belong to the workspace, the file will be placed in `<fallback>` to prevent the global variables of the file from polluting your workspace environment.

Since my development environment is VSCode, and VSCode can simply set different settings for different workspaces, I make the following assumptions:
* The user will set the `workspace.library` separately for workspaces, because Lua's capabilities are mainly provided by the host, and different workspaces generally correspond to different hosts.
* The user will not set global `workspace.library`, because its main purpose is to quickly look at the Lua code of an unknown/unnecessary environment.

However, I found a problem in the recent user feedback. In the non VSCode client, it is not convenient for users to set each workspace separately (although I provided `.luarc.json`), many people directly set `workspace.library` in the global setting, which causes some problems when the library is very large:
* When you start the server in single file mode, the startup speed will be affected by the library, so you just want to take a quick look at Lua code, but you need to wait for a long loading time.
* When you start the server in workspace mode, you will see multiple progress bars. If `<fallback>` only needs to load the basic Lua API, it can usually be completed in 0.5 sec (the progress bar has a display delay of 0.5 sec), so you won't see the loading progress bar of `<fallback>` at all. But `<fallback>` also need to load many files after affecting by the library, which leads you to see multiple progress bars. Fortunately this will not affect the actual loading speed, because the same file will only be loaded once.

In this case, I'm considering adding a setting enabled by default, witch makes `<fallback>` ignore `workspace.library`.