# Ronin Dev

Ronin Dev allows a developer to easily clone and setup Ronin repositories.
Ronin Dev supports the following repositories:

* [ronin-support](http://github.com/ronin-ruby/ronin-support)
* [ronin](http://github.com/ronin-ruby/ronin)
* [ronin-gen](http://github.com/ronin-ruby/ronin-gen)
* [ronin-asm](http://github.com/ronin-ruby/ronin-asm)
* [ronin-web](http://github.com/ronin-ruby/ronin-web)
* [ronin-metasploit](http://github.com/ronin-ruby/ronin-metasploit)
* [ronin-exploits](http://github.com/ronin-ruby/ronin-exploits)
* [ronin-fuzz](http://github.com/ronin-ruby/ronin-fuzz)
* [ronin-scanners](http://github.com/ronin-ruby/ronin-scanners)
* [ronin-dorks](http://github.com/ronin-ruby/ronin-dorks)
* [ronin-sql](http://github.com/ronin-ruby/ronin-sql)
* [ronin-php](http://github.com/ronin-ruby/ronin-php)
* [ronin-wiki](http://github.com/ronin-ruby/ronin-wiki)

## Requirements

* [git](http://www.git-scm.com) >= 1.6.x.x
* [ruby](http://www.ruby-lang.org) >= 1.8.7
* [thor](http://rubygems.org/gems/thor) >= 0.14.3

## Install

    git clone http://github.com/ronin-ruby/ronin-dev.git
    
## Clone

    cd ronin-dev/
    thor git:clone

Selectively clone only the repositories you want to work on:

    thor git:clone ronin-scanners ronin-web

## Bundle

    thor bundle:install

## Update

    thor git:pull
    thor bundle:update

