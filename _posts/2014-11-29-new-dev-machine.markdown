---
layout:     post
title:      "New Dev Machine"
date:       2014-12-07 21:00:00
categories: technical
---
Last week, I started to work with a brand new laptop: a Mac Book Pro running Yosemite.
In this post, I'm going to enumerate the minimun set of tools I need to start to write code in
a comfortable way. This setup is mainly focused on Ruby/Rails web projects.

Hopefully, next time I have to bootstrap a new dev machine, I'll be
able to follow this steps and save some time.

* Package manager: [Homebrew][homebrew]
* CVS (Control Version System): [Git][git]
* My VIM, Bash, Git configuration files: [dotfiles][mydotfiles], and its prerequisites
  * Symlink manager: [GNU stow][stow]
  * Ruby environment manager: [rbenv][env]
* Gems manager: Bundle: [bundler][bundle]
* Makes shims aware of bundle installation paths: [rbenv-bundle][rbundle]
* Homebrew system duplicates: [homebrew/dupes][dupes]
* Apple development tools: Xcode
* C development libraries: libxml2, libxslt, libiconv

And that's it, my laptop is ready to work.


[homebrew]: http://brew.sh
[git]: http://git-scm.com
[mydotfiles]: https://github.com/pvcarrera/dotfiles
[stow]: http://www.gnu.org/software/stow
[env]: https://github.com/sstephenson/rbenv
[rbundle]: https://github.com/carsomyr/rbenv-bundler
[bundle]: http://bundler.io
[dupes]: https://github.com/Homebrew/homebrew-dupes
