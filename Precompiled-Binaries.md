## Distributions

On many platforms, `lua-language-server` can be installed using a package manager. The following is a partial list:

### macOS

* Homebrew: `brew install lua-language-server`
* Macports: `sudo port install lua-language-server`

## Github

If your package manager does not offer `lua-language-server`, you can download the binaries compiled by Github CI. Note that you still need to clone the repository beforehand to obtain the necessary runtime scripts.

1. Open https://github.com/sumneko/lua-language-server/actions/workflows/build.yml
2. Open latest workflow
3. Downloads `Artifacts`
4. Binaries should be placed in `bin/<platform>/` under the repository root. If you are using VSCode, it should be placed in the corresponding location in the extension directory.
5. If you are on a `Linux` or `macOS` platform, you may need to add execution permissions before running, such as `chmod +x lua-language-server`

