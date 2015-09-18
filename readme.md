# shellenv

This is a Package Control dependency that allows Sublime Text packages to get
a copy of the environmental variables for the current user as they would see
the in their terminal.

The reason this depenency exists is due to method with which Sublime Text
launches on Linux, but especially OS X. On Linux, Sublime Text may be launched
from a launcher like Unity, in which case `os.environ` may not contain the
correct environment. On OS X, Sublime Text is launched via the dock, which
never share's the user's shell environment.

## Version

1.3.0 - [changelog](changelog.md)

## API

This dependency exposes four functions for package developers to utilize:

 - `shellenv.get_env()`
 - `shellenv.get_path()`
 - `shellenv.get_user()`
 - `shellenv.get_user_login_shell()`

`get_env()` returns a 2-element tuple of:

 - [0] a unicode string of the path to the user's login shell
 - [1] a dict with unicode string keys and values of the environment variables

*If `for_subprocess=True` is passed to `get_env()`, and the user is running
Sublime Text 2, the result will be byte strings instead of unicode strings, as
required by the `subprocess` module.*

`get_path()` returns a 2-element tuple of:

 - [0] a unicode string of the path to the user's login shell
 - [1] a list of unicode strings of the directories in the `PATH`

`get_user()` returns a unicode string of the current user's username.

`get_user_login_shell()` returns a unicode string of the current user's login
shell.

## Usage

To have a package require `shellenv`, add a file named `dependencies.json` into
the root folder of your Sublime Text package, and add the following:

```json
{
    "*": {
        "*": [
            "shellenv"
        ]
    }
}
```

This indicates that for all operating systems (`*`), and all versions of
Sublime Text (nested `*`), require the `shellenv` dependency. You can also read
the
[official documentation about dependencies](https://packagecontrol.io/docs/dependencies).

## Example

The output of `get_env()` may be useful when launching a subprocess:

```python
import subprocess
import shellenv

_, env = shellenv.get_env(for_subprocess=True)
proc = subprocess.Popen(['executable', '-arg'], env=env)
```

## Development

Tests can be run using
[Package Coverage](https://packagecontrol.io/packages/Package%20Coverage).
