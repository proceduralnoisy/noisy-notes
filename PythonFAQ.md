# Python FAQ

### Windows installation
The simplest (and apparently recommended) approach is to use the `Latest Python install manager` available on the [Python downloads](https://www.python.org/downloads/windows/) page.

Installation is straightforward: Run the described `winget` command to install the manager, then simply run `python` to install and run the latest non-prerelease version.

This also installs the `pymanager` and `py` commands to manage multiple Python installs (you can view installed versions with `py list` and install/uninstall specific versions via `pymanager install`/`pymanager uninstall`)

If you want to use a specific Python version, you can configure the default version as an environment variable or configuration options. See [here](https://docs.python.org/dev/using/windows.html#pymanager-config).

> **Issue** With the above instructions, python commands don't work in MINGW64 (command hangs).

### `pip` vs. `python -m pip`?
Favor `python -m pip ...`. 

[This answer](https://discuss.python.org/t/whats-the-difference-between-python-m-pip-args-and-pip-args/52154/2) provides some good reasoning. 
