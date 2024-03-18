Laptop
======

Laptop is a script to set up a macOS laptop for web and mobile development.

It can be run multiple times on the same machine safely.
It installs, upgrades, or skips packages based on what is already installed on the machine.

*NOTE*: Please take a moment to read *and customize* this script before running. The packages below are helpful suggestions, not a prescriptive list.

Requirements
------------

We support:

* macOS Sonoma (14.x) on Apple Silicon and Intel
* macOS Ventura (13.x) on Apple Silicon and Intel
* macOS Monterey (12.x) on Apple Silicon and Intel

Older versions may work but aren't regularly tested.
Bug reports for older versions are welcome.

Install
-------

Make sure you have git installed. On macOS, just typing `git` in the terminal should prompt you to install the Command Line Tools, but if not, `xcode-select --install` should do the trick.

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

The script modifies your terminal configuration (e.g. to add NVM to $PATH). If you want to pick up those changes without opening a new terminal, source your config file like so (substituting .bashrc if you're using bash).

```sh
source ~/.zshrc
```

Debugging
---------

Your last Laptop run will be saved to `~/laptop.log`.
Read through it to see if you can debug the issue yourself.
If not, copy the lines where the script failed into a
[new GitHub Issue](https://github.com/thoughtbot/laptop/issues/new) for us.
Or, attach the whole log file as an attachment.

What it sets up
---------------

macOS tools:

* [Homebrew] for managing operating system libraries.

[Homebrew]: http://brew.sh/

Unix tools:

* [htop] for viewing process information in the CLI
* [OpenSSL] for Transport Layer Security (TLS)
* [The Silver Searcher] for finding things in files
* [tmux] for saving project state and switching between projects
* [Zsh] as your shell
* [Oh My Zsh] framework for Zsh

[htop]: https://htop.dev/
[OpenSSL]: https://www.openssl.org/
[The Silver Searcher]: https://github.com/ggreer/the_silver_searcher
[tmux]: http://tmux.github.io/
[Zsh]: http://www.zsh.org/
[Oh My Zsh]: https://ohmyz.sh/

Development tools:

* [AWS CLI] for interacting with the Amazon API
* [Sentry CLI] for interacting with the Sentry API

[AWS CLI]: https://aws.amazon.com/cli/
[Sentry CLI]: https://github.com/getsentry/sentry-cli

Programming languages, package managers, and configuration:

* [Node.js] and [npm], for running apps and installing JavaScript packages. This installer uses [nvm] to manage Node versions.
* [Ruby] stable for writing general-purpose code. This installer uses [rbenv] to manage Ruby versions.
* [Rosetta 2] for running tools that are not supported in Apple silicon processors

[Node.js]: http://nodejs.org/
[npm]: https://www.npmjs.org/
[nvm]: https://github.com/nvm-sh/nvm
[Ruby]: https://www.ruby-lang.org/en/
[rbenv]: https://github.com/rbenv/rbenv
[Rosetta 2]: https://developer.apple.com/documentation/apple-silicon/about-the-rosetta-translation-environment

Databases:

* [Postgres] for storing relational data
* [Redis] for storing key-value data

[Postgres]: http://www.postgresql.org/
[Redis]: http://redis.io/

Applications (optional, commented out by default):

* [Google Chrome] as a browser option, and for testing
* [Slack] for team communication
* [iTerm2] for an improved terminal experience
* [Sublime Text] as a text editor option
* [Sublime Merge] as a merge/diff tool and as a companion to Sublime Text
* [Visual Studio Code] as a text editor option
* [Insomnia] for a convenient UI to experiment with API endpoints
* [Flycut] to expand the depth/history of your clipboard
* [BitWarden] to store passwords/secrets and share them with teammates
* [Caffeine] to keep your computer from falling asleep when you don't want it to
* [TablePlus] for connecting to local and remote relational DBs

[Google Chrome]: https://www.google.com/chrome/
[Slack]: https://slack.com/
[iTerm2]: https://www.iterm2.com/
[Sublime Text]: https://www.sublimetext.com/
[Sublime Merge]: https://www.sublimemerge.com/
[Visual Studio Code]: https://code.visualstudio.com/
[Insomnia]: https://insomnia.rest/
[Flycut]: https://github.com/TermiT/flycut
[BitWarden]: https://bitwarden.com/
[Caffeine]: http://lightheadsw.com/caffeine/
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
brew "Caskroom/cask/dockertoolbox"
brew "go"
brew "ngrok"
brew "watch"
EOF

default_docker_machine() {
  docker-machine ls | grep -Fq "default"
}

if ! default_docker_machine; then
  docker-machine create --driver virtualbox default
fi

default_docker_machine_running() {
  default_docker_machine | grep -Fq "Running"
}

if ! default_docker_machine_running; then
  docker-machine start default
fi

fancy_echo "Cleaning up old Homebrew formulae ..."
brew cleanup
brew cask cleanup

if [ -r "$HOME/.rcrc" ]; then
  fancy_echo "Updating dotfiles ..."
  rcup
fi
```

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo` and
`gem_install_or_update`
can be used in your `~/.laptop.local`.

See the [wiki](https://github.com/thoughtbot/laptop/wiki)
for more customization examples.

Contributing
------------

Edit the `mac` file.
Document in the `README.md` file.
Follow shell style guidelines by using [ShellCheck] and [Syntastic].

```sh
brew install shellcheck
```

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