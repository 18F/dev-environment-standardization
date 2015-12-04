---
permalink: /languages/ruby/
title: Ruby
parent: Languages
---

# Ruby Ecosystem Guide

This guide explains the basics of setting up Ruby and related helpful tools,
so that you can run and modify a Ruby-based project on your own computer. We
wrote this for people inside and outside 18F who want to run and contribute to
projects we build, and who aren't already familiar with working on Ruby
projects. This guide is both for people who write a lot of code and for people
who don't write any code (setting up this development environment doesn't
require coding knowledge).

# Installing Ruby

If you use OS X or some Linux distributions (such as Ubuntu), your system
already has Ruby installed. The downside to this is that it is probably an
out-of-date version and it is installed in a system-level directory which you
should **not** modify as the host Operating System may rely on it for some of
its functionality.

You can ignore that default version and instead download and install Ruby
yourself in a user-controlled directory that you can modify at will.  There
are several ways you can download and install Ruby:

- Directly from the Ruby web site:  https://www.ruby-lang.org/en/downloads/
- On a Linux system, with the OS package manager (though this may not provide you with an entirely up-to-date version).
- On a recent OSX system, with [Homebrew](http://brew.sh/).
- With a Ruby version manager:
  - [rbenv](https://github.com/sstephenson/rbenv)
  - [RVM](https://rvm.io/)

If you choose to go with a version manager for installing Ruby this also buys
you the ability to install and manage multiple versions of the language
easily.  This is especially useful should you need to work on projects that
have hard dependencies on the language version.  While not required, this is
the recommended method for 18F employees to install Ruby.

# Installing Ruby Gems

Ruby gems are third-party modules written in and for Ruby and are usually
found within [RubyGems](https://rubygems.org/).  There is a tool to use to
help install and maintain them called `gem`, which is automatically installed
with Ruby as of version 1.9.  If you find that you are missing `gem` or feel
it needs to be updated,
[follow the instructions on RubyGems](https://rubygems.org/pages/download).

In addition to installing packages from RubyGems, `gem` can also install
packages directly from several types of source control repositories (e.g.,
Git) or even from another location on your local machine.

# Using `gem`

`gem` is a simple way to install a Ruby gem.  For instance, if you want to
install Rails, you can do this:

`$ gem install rails`

This will download and install the latest version of Rails package from
RubyGems.  You can also install a specific version:

`$ gem install rails==4.1.14`

To update a gem to its latest version, use this command:

`$ gem update rails`

To uninstall a gem, run this command:

`$ gem uninstall rails`

Lastly, you can also install multiple gem at once from a file, which is
usually named `Gemfile` by community convention (though if you're using
[Bundler](http://bundler.io/), the file must be named `Gemfile`):

`$ gem install -g Gemfile`

This will install whatever gems are listed in the file.  For more instructions
and detailed usage information, please refer to the
[`gem` User Guide](http://guides.rubygems.org/command-reference/).

# Managing Project Dependencies

With Ruby installed and working with Ruby gems under your belt, there is one
last thing to discuss:  per-project dependencies.  When you install a Ruby
gem, by default it will go into a directory called `gems`, which lives a
couple of levels down from wherever Ruby is installed on your computer
(commonly referred to as the "global `gems` directory").  This is undesirable,
however, as it does not account for the fact that different projects will
likely require different versions of these gems (which cannot both be
installed in the same place at the same time) and you may also not have the
appropriate permissions for installing the gems in the first place, such as on
a shared host.

Instead, a better approach is to be able to keep project package dependencies
isolated from each other and from the global `gems` directory.  This enables
you to install Ruby gems in user-controlled directories and prevent conflicts
when changing them from one project to another. The solution for this in the
Ruby community is a concept known as gemsets and/or using
[Bundler](http://bundler.io/).

## Understanding Ruby Gemsets

The idea behind a Ruby gemset was first
[introduced with RVM](https://rvm.io/gemsets/basics) as a means of managing
per-project dependencies.  What this means is that you could be working on
multiple projects at once on your computer, but gem dependencies of each
project are kept in isolation from one another and from the global `gem`
directory itself.  This solves the issue of one project requiring version X
of a particular gem and another project requiring version Y of that same gem,
but not being able to install both versions together in the global `gem`
directory without any potential conflicts.  This also solves the issue of not
being able to install Ruby gems due to lack of administrative permissions,
such as on a shared host.

In order to take advantage of gemsets you must use a Ruby version manager,
either [RVM](https://rvm.io/) itself or
[rbenv](https://github.com/sstephenson/rbenv) with the
[rbenv-gemset](https://github.com/jf/rbenv-gemset) plugin.  Both of these
tools will allow you to create and manage gemsets.

## Using Bundler and the `bundle` Command

[Bundler](http://bundler.io/) is another tool that has very widespread
adoption within the Ruby community and is used predominantly to install gem
dependencies for a project.  It can be used by itself or in conjunction with a
Ruby version manager and gemsets.  In order to use Bundler, which itself is a
Ruby gem, you must install it first.  This is one of the rare instances where
installing a gem globally is acceptable, because it is intended to be used
outside the context of any specific project even though it is being used to
manage the dependencies within a project:

`$ gem install bundler`

With Bundler installed you are able to make full use of a project's `Gemfile`
(or define your own for your project).  Generally speaking, the most common
tasks you will perform are to install a project's dependencies:

`$ bundle install`

Or update them:

`$ bundle update`

On some occasions, however, you might also want to ensure you are executing
that project's specific scripts related to its gems, in which case you can
leverage Bundler to do so.  For example, running a Rails console:

`$ bundle exec rails c`

Performing all of these commands while a gemset is active will lead to the
gems only being installed within that gemset.  However, if you choose not to
use gemsets you can still use Bundler itself to manage per-project
dependencies by specifying a directory when installing dependencies:

`$ bundle install --path=<specified path>`

This will install all gems and their dependencies into the location given as
opposed to the global `gems` directory.

For more information on how to use Bundler, please refer to its
[documentation](http://bundler.io/v1.10/man/bundle.1.html).

# Notable Exceptions for Global Ruby gems

While most Ruby gems should be installed and managed within the context of a
gemset or a local project path relative to the project root, there may be a
small handful of gems that might need to be installed in the global `gems`
directory.  Bundler is one such exception certainly.

Pry is another noteworthy exception as it is a much richer REPL
(Read-Eval-Print Loop) to use when working with Ruby, and if you're just
testing something quick it is a bit of a pain to have to get into a specific
project.  You can read more about Pry here:  http://pryrepl.org/

To install it, simply run the following command:

`$ gem install pry`

This will install Pry globally and now you'll be able to just run `pry`
without needing to activate a gemset or go into a specific project to
immediately get into this powerful Ruby REPL.

# Useful Ruby Development Tools

This is a short list of Ruby gems that may come in handy when working on a
Ruby project.  The packages aid in the development process by providing code
coverage analysis, advanced debugging capabilities, more robust testing
support, and code style/formatting checks.

For more in-depth information regarding Ruby development, please check out the
[18F Development Guide](https://pages.18f.gov/development-guide/).

## Code Coverage

- [simplecov](https://github.com/colszowka/simplecov)

## Debugging

- [Plugins for Pry](https://github.com/pry/pry/wiki/Available-plugins)
  (requires Pry)

## Linters

- [RuboCop](https://github.com/bbatsov/rubocop)
- [ruby-lint](http://code.yorickpeterse.com/ruby-lint/latest/)

## Testing

- [RSpec](http://rspec.info/)
- [Cucumber](https://cucumber.io/)
- [Capybara](http://jnicklas.github.io/capybara/)
- [Factory Girl](https://github.com/thoughtbot/factory_girl)
- [Webmock](https://github.com/bblimke/webmock)
- [Timecop](https://github.com/travisjeffery/timecop)

In addition, you may want to peruse the following bits of documentation around
testing with Ruby:

- [18F's Ruby Testing Cookbook](https://pages.18f.gov/testing-cookbook/ruby/)
- [Thoughtbot's Guide to Writing Unit Tests](https://robots.thoughtbot.com/back-to-basics-writing-unit-tests-first)

# Additional Reading

- http://www.rubyinside.com/rbenv-a-simple-new-ruby-version-management-tool-5302.html
- https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rbenv-on-ubuntu-14-04
- https://github.com/pry/pry/wiki/Available-plugins
- http://bundler.io/
- http://pryrepl.org/
- https://rubygems.org/
- https://github.com/sstephenson/rbenv/blob/master/README.md
- https://github.com/fesplugas/rbenv-installer/blob/master/README.md
- https://github.com/sstephenson/ruby-build/blob/master/README.md
- https://github.com/jf/rbenv-gemset/blob/master/README.mkd
- https://github.com/bbatsov/ruby-style-guide

