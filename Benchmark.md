# Performance Benchmark

## Background
This benchmark tests the Lua language server with the Visual Studio Code extension client against a real project that contains 1500 Lua files with a total size of 20MB.

The CPU being used is an i7-9700K. The project file is located on a HDD and the server is on a SSD.

## Results
After many tests, the loading of the 20MB workspace takes about 10 seconds, meaning the loading speed is ~2MB/s.

|                                 Operation                                  | Time Elapsed |
| :------------------------------------------------------------------------: | :----------: |
| Initialize the server, shake hands with the client, read the configuration |     0.5s     |
|            Scan files in workspace and find all Lua file paths             |    0.25s     |
|              Start loading workspace and load Lua into memory              |     1.5s     |
|                   Convert Lua into abstract syntax trees                   |     6.5s     |
|                Collect information such as global variables                |      1s      |
|                                   Other                                    |      1s      |


> ℹ️ Note: Because the test was repeated multiple times, the OS cached some of the operations, so the above performance will be better than the actual performance.

> ℹ️ Note: In actuality, step 2 and 3 are merged, which can save about 0.5 sec.

## Conclusion
According to the test, the most time-consuming part is "Compile the contents of lua files into ASTs". This time is matching expectation, so there may be no room for improvement.

Step 2 is affected by the total number of files in your workspace (including non Lua files).

The best way to improve startup times is to [exclude unnecessary files and directories](https://github.com/sumneko/lua-language-server/wiki/Settings#workspaceignoredir) and to not load unnecessary [libraries](https://github.com/sumneko/lua-language-server/wiki/Libraries).
