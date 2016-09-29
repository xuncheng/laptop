Laptop
======
[![Build Status](https://travis-ci.org/xuncheng/laptop.svg)](https://travis-ci.org/xuncheng/laptop)

Laptop is a script to set up an OS X computer for web development.

It can be run multiple times on the same machine safely.
It installs, upgrades, or skips packages
based on what is already installed on the machine.

Requirements
------------

* macOS Sierra (10.12)

Older versions may work but aren't regularly tested. Bug reports for older
versions are welcome.

Install
-------

Download, review, then execute the script:

```sh
curl --remote-name https://raw.githubusercontent.com/xuncheng/laptop/master/mac
sh mac 2>&1 | tee ~/laptop.log
```
The [script](https://github.com/xuncheng/laptop/blob/master/mac) itself is
available in this repo for you to review if you want to see what it does
and how it works.

Note that the script will ask you to enter your OS X password at various
points. This is the same password that you use to log in to your Mac.

**Once the script is done, make sure to quit and relaunch Terminal.**

Debugging
---------

Your last Laptop run will be saved to `~/laptop.log`. Read through it to see if
you can debug the issue yourself. If not, copy and paste the entire log into a
[new GitHub Issue](https://github.com/18F/laptop/issues/new) for us.

What it sets up
---------------

macOS tools:

* [Homebrew] for managing operating system libraries.
* [Homebrew Cask] for quickly installing Mac apps from the command line
* [Homebrew Services] so you can easily stop, start, and restart services

[Homebrew]: http://brew.sh/
[Homebrew Cask]: http://caskroom.io/
[Homebrew Services]: https://github.com/Homebrew/homebrew-services

Unix tools:

* [Exuberant Ctags] for indexing files for vim tab completion
* [Git] for version control
* [OpenSSL] for Transport Layer Security (TLS)
* [Selecta] A fuzzy text selector for files and anything else you need to select.
* [The Silver Searcher] for finding things in files
* [Tmux] for saving project state and switching between projects
* [Zsh] as your shell

[Exuberant Ctags]: http://ctags.sourceforge.net/
[Git]: https://git-scm.com/
[OpenSSL]: https://www.openssl.org/
[Selecta]: https://github.com/garybernhardt/selecta
[The Silver Searcher]: https://github.com/ggreer/the_silver_searcher
[Tmux]: http://tmux.github.io/
[Zsh]: http://www.zsh.org/

Image tools:

* [ImageMagick] for cropping and resizing images

Testing tools:

* [Qt] for headless JavaScript testing via Capybara Webkit

[Qt]: http://qt-project.org/

Programming languages and configuration:

* [Bundler] for managing Ruby libraries
* [NVM] for managing Node.js versions if you do not have [Node.js] already installed (Includes latest [Node.js] and [NPM], for running apps and installing JavaScript packages)
* [RVM] for managing Ruby versions (includes [Bundler] and the latest [Ruby])
* [Ruby] stable for writing general-purpose code

[Bundler]: http://bundler.io/
[ImageMagick]: http://www.imagemagick.org/
[NVM]: https://github.com/creationix/nvm
[Node.js]: http://nodejs.org/
[NPM]: https://www.npmjs.org/
[RVM]: https://github.com/rvm/rvm
[Ruby]: https://www.ruby-lang.org/en/

Databases:

* [Redis] for storing key-value data

[Redis]: http://redis.io/

It should take less than 15 minutes to install (depends on your machine and
internet connection).

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


Credits
-------

This laptop script is originally based on [thoughtbot's laptop] script.

[thoughtbot's laptop]: (https://github.com/thoughtbot/laptop)
