You could try [PreCompiled binaries](https://github.com/sumneko/lua-language-server/wiki/PreCompiled-Binaries) before building by yourself

1. Install [ninja](https://github.com/ninja-build/ninja/wiki/Pre-built-Ninja-packages)
2. Make sure you have C++17
3. Clone project
```shell
# clone project
git clone  --depth=1 https://github.com/sumneko/lua-language-server
cd lua-language-server
git submodule update --depth 1 --init --recursive
```
4. Build
+ `Windows`:
```shell
cd 3rd\luamake
compile\install.bat
cd ..\..
3rd\luamake\luamake.exe rebuild
```
+ `Linux`:
```shell
cd 3rd/luamake
./compile/install.sh
cd ../..
./3rd/luamake/luamake rebuild
```
+ `macOS`:
```shell
cd 3rd/luamake
./compile/install.sh
cd ../..
./3rd/luamake/luamake rebuild
```
5. Run (non-VSCode)
+ `Windows`:
```shell
.\bin\lua-language-server.exe
```
+ `Linux`:
```shell
./bin/lua-language-server
```
+ `macOS`:
```shell
./bin/lua-language-server
```

---

### Common issues

* Compile error `/usr/bin/ld: cannot find -lstdc++` or similar when running install.sh

  * You may need to install libstdc++. On Fedora linux or similar: `dnf install libstdc++-static`