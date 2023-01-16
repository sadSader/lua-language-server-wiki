# Overview

> Telemetry has been removed since `v3.6.5`, thank you for your positive feedback in the community!

Help improve the `Lua-language-server` by sending usage statistics and exceptions to the team.

If you don't want to send any of this information to the team, please follow the instructions below to disable these features.

# Collected data

If the ``Lua.telemetry.enable`` setting is configured to be ``true``, anonymous, non-identifying usage and stack traces from errors is sent to the development team, including:
* A random token, used to count the number of users online
* Extension version, e.g. `2.2.0`
* OS, e.g. `Windows 64`
* Client name, e.g. `Visual Studio Code 1.54.3`
* C++ runtime name, e.g. `msvc MSVC STL 202011L`
* C++ compiler name, e.g. `msvc MSVC 1928`
* Stack traces of `Lua-language-server`, relative paths are used, so it does not contain any user information

Find sending codes at [https://github.com/sumneko/lua-language-server/blob/master/script/service/telemetry.lua](https://github.com/sumneko/lua-language-server/blob/master/script/service/telemetry.lua)

# Disabling telemetry

To not send any usage data or error reports to the development team, set the ``Lua.telemetry.enable`` setting to ``false``. (To do so, search for the variable by name under File/Code->Preferences->Settings.)

# Opt-in policy

By default ``Lua.telemetry.enable`` is set to ``null``. With that setting, no data is sent to the development team, but UI will appear asking users to agree to enabling these settings. Only when a user explicitly opts-in will any data ever be sent.

If you choose to enable telemetry, I would be very grateful. In addition to improving the quality of the extension through error reports, more importantly, this will be reflected in the number of users, which will motivate me to continue developing this extension.

# Use of the collected data

The collected data is used to improve the `Lua-language-server`. The collected data is public, you can find it at [http://154.23.191.39/](http://154.23.191.39/)

Find collecting codes at [https://github.com/sumneko/lua-telemetry-server/tree/master/method](https://github.com/sumneko/lua-telemetry-server/tree/master/method)
