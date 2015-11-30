---
permalink: /languages/
title: Languages
---

At 18F, our four primary development languages are:

- [Ruby](./ruby/)
- [Python](./python/)
- [Node.js (JavaScript)](./javascript/)
- [Go](./go/)

# Language Version and Package Managers

A handful of the languages that we develop with at 18F, mainly Ruby, Python,
JavaScript with Node.js (which will be referred to as a language for the sake
of simplicity in this document), and Go, have vibrant and active development
communities that routinely improve the implementation of these languages and
release new versions of them.  Given the pace of change in software
development, especially with things focused more toward the Internet,
maintaining a system that accounts for the required versions of these
languages for any given project becomes quite onerous and error prone.

Furthermore, each of these languages support extremely vibrant communities
that develop third party packages utilizing the language.  Managing these
packages quickly becomes quite messy as well if not actively managed by the
developer.  As soon as you are working with more than one project you might be
dealing with having to use different versions of a framework or library built
as an extension of the language for each project.

While none of this is terribly difficult, it is time consuming and can be
cumbersome to have to do deal with.  Thankfully there are tools that exist
that can make the issues of maintaining multiple language versions and
managing project dependencies much simpler and easier.  This allows a
developer to preserve a clean and working system while allowing maximum
flexibility for experimentation and development of multiple projects at once.

# Version Managers

Many times the computer you are using will already have a version of the
aforementioned languages already installed, however they are installed in
system-level directories that have locked down permissions.  It is rather
dangerous to try and modify/update these installations directly as the host OS
(Operating System) might be relying on them.  Red Hat Linux implementations in
particular are well known to depend on older Python 2.x installations (at
least older versions of Red Hat).

Each of these languages can be installed in other, user-controlled locations
with varying degrees of ease in order to allow a developer to work with the
latest and greatest version.  However, some projects might require an older
version and are not ready to be developed with the newer version of a given
language.  Sometimes the system-level dependencies are also a bit complicated
to deal with when trying to compile a language from source.

Thankfully, several members in each of these respective development
communities have released a variety of tools to help manage installing and
managing multiple versions of these languages.  These tools are typically
called Version Managers and exist to make it a breeze to install multiple
versions of a given language and switch between them seamlessly.  Multiple
options exist for each of these languages.  While we recommend that you use
one, it is up to you to determine which one best suites your needs and work
style.  Please refer to the language specific guides we have for details about
some of the more prominent options available.

# Package Managers

Ruby, Python, Node.js (JavaScript), and Go all ship with tools that help
developers install, create, and maintain third-party packages for each
respective language:

- [Ruby](./ruby/):  `gem`
- [Python](./python/):  `pip`
- [NodeJS (JavaScript)](./javascript/):  `npm`
- [Go](./go/):  `go get`

Please refer to the language-specific guides for more details about each of
these tools.  The main point here is that you should be using the package
managers to do anything related to third-party tools.  The tricky part with
this, however, comes again when dealing with multiple projects that rely on
the same packages but different versions of them.  For example, one project
may be a Rails 3.x app, but another might be a Rails 4.x app.  In a similar
manner, a Python project may be developed on Django 1.4 but another is using
Django 1.8, and you are contributing to both of them.

Each language has a variety of solutions available to them to help solve this
problem.  In some cases, the version manager of a language can also serve as
the solution (RVM for Ruby comes to mind), and in others there are separate
tools available to help manage the project dependencies in a way that
preserves the stable state of your machine and isolates them (e.g.,
`virtualenv` for Python).  Once again, the main point here is that you should
most certainly use one of these tools to help manage your project dependencies
and development environments, but which one is up to what works best for you.
