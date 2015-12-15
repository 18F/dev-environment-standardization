---
permalink: /languages/ruby/
title: Ruby
parent: Languages
---

# Ruby Ecosystem Guide

This guide explains the basics of setting up a Ruby development environment
(installing Ruby and related helpful tools), so that you can run and modify
a Ruby-based project on your own computer. We wrote this for people inside
and outside 18F who want to run and contribute to projects we build, and who
aren't already familiar with working on Ruby projects. This guide is both for
people who write a lot of code and for people who don't write any code
(setting up this development environment doesn't require coding knowledge).

# Installing Ruby

If you use OS X or some Linux distributions (such as Ubuntu), your system
already has Ruby installed. The downside to this is that it is probably an
out-of-date version and it is installed in a system-level directory which you
should **not** modify as the host Operating System may rely on it for some of
its functionality.

You can ignore that default version and instead download and install Ruby
yourself in a user-controlled directory that you can modify at will.  There
are several ways you can download and install Ruby:

- Directly from the [Ruby website](https://www.ruby-lang.org/en/downloads/).
- On a Linux system, with the OS package manager (though this may not provide
  you with an entirely up-to-date version).
- On a recent OS X system, with [Homebrew](http://brew.sh/).
- With a Ruby version manager (both of these are popular, so pick the one that
  looks good to you):
  - [rbenv](https://github.com/sstephenson/rbenv)
  - [RVM](https://rvm.io/)

If you choose to use a version manager to install Ruby, this gives
you the ability to install and manage multiple versions of the language
easily.  This is especially useful if you work on projects that
have hard dependencies on the language version.  While not required, this is
the recommended method for 18F employees to install Ruby.

# Installing Ruby Gems

Ruby gems are third-party modules written in and for Ruby, and they're usually
found on [RubyGems](https://rubygems.org/). You can install and maintain them
using `gem`, which is automatically installed with Ruby as of version 1.9.  If
you're missing `gem` or it needs to be updated,
[follow the instructions on RubyGems](https://rubygems.org/pages/download).

`gem` can also install packages directly from several types of source control
repositories (such as Git) or from another location on your local machine.

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

You can also install multiple gems at once from a file, which is
usually named `Gemfile` by community convention (though if you're using
[Bundler](http://bundler.io/), the file must be named `Gemfile`):

`$ gem install -g Gemfile`

This will install the gems listed in the file.  For more instructions
and detailed usage information, see the
[`gem` User Guide](http://guides.rubygems.org/command-reference/).

# Managing Project Dependencies

With Ruby installed and working with Ruby gems under your belt, there is one
last thing to discuss:  per-project dependencies.  When you install a Ruby
gem, by default it will go into a directory called `gems`, which is a
couple of levels down from wherever Ruby is installed on your computer
(commonly referred to as the "global `gems` directory").  This can cause
problems though, because different projects may require different versions of
these gems (which cannot both be installed in the same place at the same
time). Also, you may not have the appropriate permissions for installing gems
there, such as if you're using a shared host.

A better approach is to keep project package dependencies isolated from each
other and from the global `gems` directory.  This enables you to install Ruby
gems in user-controlled directories, which prevent conflicts when changing
them from one project to another. The solution for this in the Ruby community
is a concept known as gemsets and/or using [Bundler](http://bundler.io/).

## Understanding Ruby Gemsets

Ruby gemsets are a way of managing per-project dependencies, first
[introduced by the version manager RVM](https://rvm.io/gemsets/basics). A
gemset is a specific directory that a collection of gems are installed in. You
define the gemset yourself and name it what you want, usually after the
project youa re working on.

This may remind you of a `Gemfile`, but the two are not the same. A `Gemfile`
lists all of the dependencies that a project relies on and can help lock those
dependencies at a specific version(s). A gemset is a specific directory
designated by the user to install a group of gems into, such as those found in
a `Gemfile`.

To create and manage gemsets, you must use a Ruby version manager,
either [RVM](https://rvm.io/) itself or
[rbenv](https://github.com/sstephenson/rbenv) with the
[rbenv-gemset](https://github.com/jf/rbenv-gemset) plugin.

## Using Bundler and the `bundle` Command

[Bundler](http://bundler.io/) is another tool in the Ruby community for
installing gem dependencies for a project.  It can be used by itself or in
conjunction with a Ruby version manager and gemsets.  To use Bundler, which is
a Ruby gem, you must install it first.  This is one of the instances where
installing a gem globally is helpful, because it is intended to be used
outside the context of any specific project even though it is being used to
manage the dependencies within a project:

`$ gem install bundler`

With Bundler installed you are able to utilize a project's `Gemfile` (or
define your own for your project).  The most common reason to use Bundler is
to install a project's dependencies:

`$ bundle install`

Or update them:

`$ bundle update`

If you find out you need to execute a project's scripts related to its gems
(such as if documentation advises this), you can use Bundler to do this.  For
example, running a Rails console:

`$ bundle exec rails c`

Performing Bundler commands while a gemset is active will lead to the
gems only being installed within that gemset.  However, if you choose not to
use gemsets you can still use Bundler to manage per-project
dependencies by specifying a directory when installing dependencies:

`$ bundle install --path=<specified path>`

This will install all gems and their dependencies into the location given, as
opposed to the global `gems` directory.

For more information on how to use Bundler, refer to its
[documentation](http://bundler.io/v1.10/man/bundle.1.html).

# Notable Exceptions for Global Ruby Gems

While most Ruby gems should be installed and managed within the context of a
gemset or a local project path relative to the project root, there are a
handful of gems that might need to be installed in the global `gems`
directory.  Bundler is one of these exceptions.

[Pry](http://pryrepl.org/) is another noteworthy exception. It's a rich REPL
(Read-Eval-Print Loop) for Ruby, useful for quick tests of ideas without
having to go into a specific project.

To install it, run the following command:

`$ gem install pry`

This will install Pry globally, and now you'll be able to run `pry`
without needing to activate a gemset or go into a specific project.

# Useful Ruby Development Tools

This is a short list of Ruby gems that may come in handy when working on a
Ruby project.  These packages aid in the development process by providing code
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

In addition, you may want to read the following bits of documentation around
testing with Ruby:

- [18F's Ruby Testing Cookbook](https://pages.18f.gov/testing-cookbook/ruby/)
- [Thoughtbot's Guide to Writing Unit Tests](https://robots.thoughtbot.com/back-to-basics-writing-unit-tests-first)

# Additional Reading

- [rbenv: A Simple, New Ruby Version Management Tool](http://www.rubyinside.com/rbenv-a-simple-new-ruby-version-management-tool-5302.html)
- [How To Install Ruby on Rails with rbenv on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rbenv-on-ubuntu-14-04)
- [Available plugins for Pry](https://github.com/pry/pry/wiki/Available-plugins)
- [rbenv installer](https://github.com/fesplugas/rbenv-installer)
- [ruby-build](https://github.com/rbenv/ruby-build)
- [A community-driven Ruby coding style guide](https://github.com/bbatsov/ruby-style-guide)
