1. Install [ninja](https://github.com/ninja-build/ninja/wiki/Pre-built-Ninja-packages)
2. Make sure you have C++17
3. Clone project
```shell
# clone project
git clone https://github.com/sumneko/lua-language-server
cd lua-language-server
git submodule update --init --recursive
```
4. Build
+ `Windows`:
```shell
cd 3rd\luamake
tools\ninja.exe -f ninja\msvc.ninja
cd ..\..
3rd\luamake\luamake.exe rebuild
```
+ `Linux`:
```shell
cd 3rd/luamake
ninja -f ninja/linux.ninja
cd ../..
./3rd/luamake/luamake rebuild
```
+ `macOS`:
```shell
cd 3rd/luamake
ninja -f ninja/macos.ninja
cd ../..
./3rd/luamake/luamake rebuild
```
5. Run
+ `Windows`:
```shell
bin\Windows\lua-language-server.exe -E main.lua
```
+ `Linux`:
```shell
./bin/Linux/lua-language-server -E ./main.lua
```
+ `macOS`:
```shell
./bin/macOS/lua-language-server -E ./main.lua
```