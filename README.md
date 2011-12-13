# Ronin Dev

Ronin Dev allows a developer to easily checkout and setup Ronin repositories.
Ronin Dev supports the following repositories:

* [ronin-support](http://github.com/ronin-ruby/ronin-support)
* [ronin](http://github.com/ronin-ruby/ronin)
* [ronin-gen](http://github.com/ronin-ruby/ronin-gen)
* [ronin-web](http://github.com/ronin-ruby/ronin-web)
* [ronin-exploits](http://github.com/ronin-ruby/ronin-exploits)
* [ronin-scanners](http://github.com/ronin-ruby/ronin-scanners)

## Requirements

* [git](http://www.git-scm.com) >= 1.6.x.x
* [ruby](http://www.ruby-lang.org) >= 1.8.7
* [rake](http://rake.rubyforge.org/) >= 0.8.0

## Install

    git clone http://github.com/ronin-ruby/ronin-dev.git
    cd ronin-dev/
    rake install

Selectively install only the repositories you want to work on:

    rake install[ronin-scanners]

## Update

    rake update

Selectively update only the repositories you want to work on:

    rake update[ronin-scanners]

