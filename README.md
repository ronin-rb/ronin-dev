# Ronin Dev

Ronin Dev allows a developer to easily clone and setup Ronin repositories.

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

