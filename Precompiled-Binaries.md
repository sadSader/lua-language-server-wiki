## Distributions

On many platforms, `lua-language-server` can be installed using a package manager. The following is a partial list:

### macOS

* Homebrew: `brew install lua-language-server`
* Macports: `sudo port install lua-language-server`

## Github

### Releases

You can download release builds from https://github.com/sumneko/lua-language-server/releases. Simply download and extract the archive for your platform and call `bin/lua-language-server`.

Note that you can't simply create a symbolic link to the binary in one of the directories on your `$PATH`, since `lua-language-server` expects to find the scripts in a fixed location relative to the directory it is run from. Instead, create a wrapper script with the contents, e.g.,
```bash
#!/bin/bash
exec "<path-to-directory>/bin/lua-language-server" "$@"
```

### Prereleases

You can also download the binaries for prerelease commits from the [Actions](https://github.com/sumneko/lua-language-server/actions) page. Simply click on the commit you are interested in and download the corresponding archive.
