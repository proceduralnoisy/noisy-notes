# Python FAQ

## Python Installation

### Windows installation

#### Using Python Manager
You can use the 'Latest Python install manager' available on the [Python downloads](https://www.python.org/downloads/windows/) page.

Installation is straightforward: Run the described `winget` command to install the manager, then simply run `python` to install and run the latest non-prerelease version.

This also installs the `pymanager` and `py` commands to manage multiple Python installs (you can view installed versions with `py list` and install/uninstall specific versions via `pymanager install`/`pymanager uninstall`)

If you want to use a specific Python version, you can configure the default version as an environment variable or configuration options. See [here](https://docs.python.org/dev/using/windows.html#pymanager-config).

> **Issue** After upgrading to Windows 11, invoking `python` reverts to showing the Windows store for Python (`py` was still working). Had to remove/reinstall Python Manager to restore ability to run `python`.

## pipx Installation
Installation instructions are available [here](https://pipx.pypa.io/stable/installation/)

### Windows installation

## pipx

### Troubleshooting: pipx fails to find Python executable
If Python is removed and installed at a different location, pipx can fail with a message that references the prior location, e.g.
```
C:\Users\Work>pipx install conan
did not find executable at 'C:\Users\Work\AppData\Local\Programs\Python\Python313\python.exe': The system cannot find the file specified.
```

See [this question](https://stackoverflow.com/questions/70059862/pipx-install-path-change). Need to remove the `pipx` directory (which is separate from the Python `Scripts` directory). The folder location can be found by running `pipx environment` and looking for `PIPX_HOME`. Some common locations:
* **Windows:** `%USERPROFILE%/pipx`
* **Linux:** `%USERPROFILE%/pipx`


## Misc

### `pip` vs. `python -m pip`?
Favor `python -m pip ...`. 

[This answer](https://discuss.python.org/t/whats-the-difference-between-python-m-pip-args-and-pip-args/52154/2) provides some good reasoning. 
