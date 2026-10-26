Laptop
======

Laptop is a script to set up a macOS laptop for web and mobile development.

It can be run multiple times on the same machine safely.
It installs, upgrades, or skips packages based on what is already installed on the machine.

*NOTE*: Please take a moment to read *and customize* this script before running. The packages below are helpful suggestions, not a prescriptive list.

Requirements
------------

We support:

* macOS 26 (Tahoe) and later on Apple Silicon
* macOS Sequoia (15.x) on Apple Silicon and Intel

Older versions may work but aren't regularly tested.
Bug reports for older versions are welcome.

Install
-------

Make sure you have git installed. On macOS, just typing `git` in the terminal should prompt you to install the Command Line Tools, but if not, `xcode-select --install` should do the trick.

The script uses your existing Oh My Zsh install if you have one. If you don't, the Oh My Zsh installer replaces `~/.zshrc` with its template and saves your old file as `~/.zshrc.pre-oh-my-zsh`.

Download the script:

```sh
curl --remote-name https://raw.githubusercontent.com/CoProcure/laptop/main/mac
```

Review the script (avoid running scripts you haven't read!):

```sh
less mac
```

Execute the downloaded script:

```sh
sh mac 2>&1 | tee ~/laptop.log
```

Optionally, review the log:

```sh
less ~/laptop.log
```

The script modifies your terminal configuration (e.g. to add Homebrew and nvm to $PATH). If you want to pick up those changes without opening a new terminal, source your config file like so (substituting .bashrc if you're using bash).

```sh
source ~/.zshrc
```

Debugging
---------

Your last Laptop run will be saved to `~/laptop.log`.
Read through it to see if you can debug the issue yourself.
If not, copy the lines where the script failed into a
[new GitHub Issue](https://github.com/CoProcure/laptop/issues/new) for us.
Or, attach the whole log file as an attachment.

What it sets up
---------------

macOS tools:

* [Homebrew] for managing operating system libraries.

[Homebrew]: http://brew.sh/

Unix tools:

* [GitHub CLI] for working with pull requests from the terminal
* [htop] for viewing process information in the CLI
* [jq] for reading and transforming JSON
* [ripgrep] for finding things in files
* [tmux] for saving project state and switching between projects
* [Oh My Zsh] framework for Zsh, the default macOS shell

[GitHub CLI]: https://cli.github.com/
[htop]: https://htop.dev/
[jq]: https://jqlang.org/
[ripgrep]: https://github.com/BurntSushi/ripgrep
[tmux]: https://github.com/tmux/tmux
[Oh My Zsh]: https://ohmyz.sh/

Development tools:

* [AWS CLI] for interacting with the Amazon API

[AWS CLI]: https://aws.amazon.com/cli/

Programming languages, package managers, and configuration:

* [Node.js] 24, set as the default. This installer uses [nvm] to manage Node versions.
* [Bun] for installing JavaScript packages and running scripts.
* [uv] for managing Python versions and packages. It's installed with the standalone installer rather than Homebrew, because projects pin an exact uv version and Homebrew only ships the latest. Switch versions with `uv self update <version>`.
* [tfenv] for managing [Terraform] versions in the infrastructure repo.

[Node.js]: https://nodejs.org/
[Bun]: https://bun.sh/
[nvm]: https://github.com/nvm-sh/nvm
[tfenv]: https://github.com/tfutils/tfenv
[Terraform]: https://www.terraform.io/
[uv]: https://docs.astral.sh/uv/

Databases:

* [Postgres] 17 with [PostGIS] for storing relational data
* [Redis] for storing key-value data and running the Celery queue

[Postgres]: https://www.postgresql.org/
[PostGIS]: https://postgis.net/
[Redis]: https://redis.io/

Applications (optional, commented out by default):

* [Tailscale] for the VPN used to reach the production and staging databases and Django Admin
* [Google Chrome] as a browser option, and for testing
* [Slack] for team communication
* [iTerm2] for an improved terminal experience
* [Visual Studio Code] or [Cursor] as a text editor option
* [Sublime Text] as a text editor option
* [Sublime Merge] as a merge/diff tool and as a companion to Sublime Text
* [OrbStack] for running Docker containers, such as the Vespa system tests
* [Insomnia] for a convenient UI to experiment with API endpoints
* [Bitwarden] to store passwords/secrets and share them with teammates
* [TablePlus] for connecting to local and remote relational DBs

[Tailscale]: https://tailscale.com/
[Google Chrome]: https://www.google.com/chrome/
[Slack]: https://slack.com/
[iTerm2]: https://iterm2.com/
[Visual Studio Code]: https://code.visualstudio.com/
[Cursor]: https://cursor.com/
[Sublime Text]: https://www.sublimetext.com/
[Sublime Merge]: https://www.sublimemerge.com/
[OrbStack]: https://orbstack.dev/
[Insomnia]: https://insomnia.rest/
[Bitwarden]: https://bitwarden.com/
[TablePlus]: https://tableplus.com/

It should take less than 15 minutes to install (depends on your machine).

Customize in `~/.laptop.local`
------------------------------

Your `~/.laptop.local` is run at the end of the Laptop script.
Put your customizations there.
For example:

```sh
#!/bin/sh

brew bundle --file=- <<EOF
brew "go"
brew "ngrok"
cask "rectangle"
EOF

fancy_echo "Cleaning up old Homebrew formulae ..."
brew cleanup
```

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo` and `append_to_zshrc`
can be used in your `~/.laptop.local`.

Contributing
------------

Edit the `mac` file.
Document in the `README.md` file.
Follow shell style guidelines by using [ShellCheck].

```sh
brew install shellcheck
shellcheck mac
```

[ShellCheck]: https://www.shellcheck.net/

Fork Information
----------------

This repo is a fork of one originally put together by Thoughtbot, and has been customized to meet some of the needs of Pavilion engineers while retaining some opinionation about handy tools and common configurations.

The original Thoughtbot project has some features that have been excluded in this fork, and that project may also continue to recieve updates that are of interest to some developers. For more info, visit the original Thoughtbot repo at https://github.com/thoughtbot/laptop


License
-------

Laptop is © 2011 thoughtbot, inc.
It is free software,
and may be redistributed under the terms specified in the [LICENSE] file.

[LICENSE]: LICENSE

<!-- START /templates/footer.md -->
## About thoughtbot

![thoughtbot](https://thoughtbot.com/thoughtbot-logo-for-readmes.svg)

This repo is maintained and funded by thoughtbot, inc.
The names and logos for thoughtbot are trademarks of thoughtbot, inc.

We love open source software!
See [our other projects][community].
We are [available for hire][hire].

[community]: https://thoughtbot.com/community?utm_source=github
[hire]: https://thoughtbot.com/hire-us?utm_source=github

<!-- END /templates/footer.md -->