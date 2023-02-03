The language server can be used to export documentation of your project as JSON and Markdown.

# Example

<details>
<summary>Example Markdown</summary>

# Computer

A computer or turtle wrapped as a peripheral

A computer will have the type `computer` while a turtle will have the type `turtle`

---
[Official Documentation](https://tweaked.cc/peripheral/computer.html)

## getID


```lua
function Computer.getID()
  -> ID: integer

```

Get the ID of the computer

@*return* `ID` — The ID of the computer

---

[Official Documentation](https://tweaked.cc/peripheral/computer.html#v:getID)

## getLabel


```lua
function Computer.getLabel()
  -> label: string|nil

```

Get the label of the computer

@*return* `label` — The computer's label or `nil` if it does not have one

---

[Official Documentation](https://tweaked.cc/peripheral/computer.html#v:getLabel)

## isOn


```lua
function Computer.isOn()
  -> isOn: boolean

```

Get whether the computer is on or not

@*return* `isOn` — If the computer is on

---

[Official Documentation](https://tweaked.cc/peripheral/computer.html#v:isOn)

## reboot


```lua
function Computer.reboot()

```

Reboot or turn on the computer

---

[Official Documentation](https://tweaked.cc/peripheral/computer.html#v:reboot)

## shutdown


```lua
function Computer.shutdown()

```

Shutdown the computer

---

[Official Documentation](https://tweaked.cc/peripheral/computer.html#v:shutdown)

## turnOn


```lua
function Computer.turnOn()

```

Turn the computer on

---

[Official Documentation](https://tweaked.cc/peripheral/computer.html#v:turnOn)

</details>

# Steps

1. Locate the `lua-language-server.exe` on your computer.
   - If you use VS Code, it can be found at `C:\Users\<USERNAME>\.vscode\extensions\sumneko.lua-XXXX\server\bin\lua-language-server.exe`.

2. From the `bin` directory, run `./lua-language-server.exe --doc"<PROJECT_PATH>"`.

3. Find `doc.json` and `doc.md` in [log location](https://github.com/LuaLS/lua-language-server/wiki/FAQ#where-can-i-find-the-log-file) (`sumneko.lua-XXXX/server/log`).
