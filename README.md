# Git Hooks

[![Software License][ico-license]][link-license]

The simplest way to manage project, user, and global **[Git hooks](https://git-scm.com/docs/githooks)**.

## Install

Just download the `git-hooks` executable found in the root of this repository to a directory of your choice
and ensure that it is added to your PATH environment variable so `git-hooks` can be run.

~~~bash
curl -o /usr/local/bin/git-hooks https://raw.githubusercontent.com/slavcodev/git-hooks/master/git-hooks
chmod +x /usr/local/bin/git-hooks
~~~

Run `git hooks install` in a git project to tell it to use `git-hooks`
and `git hooks uninstall` at any time to revert to your previous state.
_(Check options of these commands to specify installation)._

## TL;DR

Git hooks are powerful and useful. They help to automate the developers routine,
such as testing, code linting, etc, but Git is limited to only one script per event.

There is where `git-hooks` comes into play.

The `git-hooks` utilizes the Git configs only. On installation, it configures `core.hooksPath`
to tell Git to run `git hooks trigger <hook-name>` command.

On trigger the hooks, `git-hooks` looks in git configs for list of hooks needed to execute.

For example for `pre-commit` hooks look like:
~~~
[hooks "pre-commit"]
  # Using path to directory, `git-hooks` will execute scripts which name ends to hook name,
  # or all executable files in sub-directory with name `<hook-name>.d`.
  all-in-dir="~/global-hooks"

  # You also can use pattern to run many scripts in directory.
  all-in-dir="~/global-hooks/pre-commit.d/*"
  files-by-extension="~/global-hooks/mixed/*.pre-commit"

  # Or you can specify the concrete script file.
  concrete-file="~/spell-checker/spell-checker.sh"

  # Unknown or not-executable files are ignored.
  invalid-file="~/foo.txt"

  # Relative path is working as well (see note below).
  relative-dir="project-hooks"
~~~

_A relative path is taken as relative to the directory where the hooks are run
([see more in documentation](https://git-scm.com/docs/githooks#_description))._

The two special sections in configs are used by `git-hooks` to look for common hooks for all events:
~~~
[hooks "pre-trigger"]
  # Trigger these hooks before specific hooks. 
  foo="~/global-hooks"
  
[hooks "post-trigger"]
  # Trigger these hooks after specific hooks. 
  foo="~/global-hooks"
~~~

Keep in mind, The config `core.hooksPath` overrides the Git config and it would not execute 
scripts from `.git/hooks` directory inside your project. If you have hooks in that directory,
you have to add it config, i.e.
~~~
[hooks "post-trigger"]
  default=".git/hooks"
~~~

_A additional note on git sub-commands:
When you type `git hooks` git actually looks for an executable called `git-hooks`.
This is done automatically, so although we're invoking `git hooks` in the examples in this doc,
you can also use `git-hooks` interchangeably._

## Locations

The `git-hooks` respects the config location supported by `git-config`
([see more in documentation](https://git-scm.com/docs/git-config)).

Example how ot install `git-hooks` globally:
~~~
git hooks install --global
~~~

## Documentation

For more details see `git-hooks` help:
~~~
git hooks --help
~~~

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) and [CODE OF CONDUCT](CODE_OF_CONDUCT.md) for more details.

## Credits

Thanks to [Benjamin Meyer](https://benjamin-meyer.blogspot.com/2010/06/managing-project-user-and-global-git.html)
for inspiration.

[ico-license]: https://img.shields.io/badge/License-BSD%202--Clause-blue.svg?style=for-the-badge
[link-license]: LICENSE
