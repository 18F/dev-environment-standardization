---
permalink: /languages/
title: Languages
---

At 18F, our four primary development languages are:

- [Ruby](./ruby/)
- [Python](./python/)
- [Node.js (JavaScript)](./javascript/)
- [Go](./go/)

# Language version and package managers

The languages that we develop with at 18F, mainly Ruby, Python, JavaScript
with Node.js (which will be referred to as a language for the sake of
simplicity in this document), and Go, have very active development communities
that routinely improve the implementation of these languages and release new
versions of them.  Given the pace of change in software development,
maintaining a development environment that accounts for the required versions
of these languages for any given project can become problematic, especially
with multiple projects being developed on the same machine.

Furthermore, each of these languages support extremely vibrant communities
that develop third party packages utilizing the language.  Working with these
third party packages quickly becomes cumbersome and requires actively managing
them on a routine basis.  With multiple projects on the same machine, a
developer will likely need to use more than one version of a framework or
library for a given language.  However, only one version can be installed at a
time in the default location for these third party packages.

Thankfully several tools exists that can make these issues of maintaining
multiple language versions and managing project dependencies much simpler and
easier.  This allows a developer to preserve a clean and stable system while
allowing maximum flexibility for experimentation and development of multiple
projects at once.  This guide describes two tiers of solutions for managing
the languages themselves and for mananging their respective third party
packages.

# Language version managers

Most computers already have a version of the aforementioned languages
installed on them, but there are two fundamental problems with this:

- They are installed in system-level directories that have locked down
  permissions; it is dangerous to try and modify/update these installations
  directly as the host OS (Operating System) might be relying on them.
- They are several versions older from the current release of the language;
  new features and security updates are not available to the developer.

Instead of relying on the default installations provided by the system, a
developer should install the language explicitly.  Each of these languages can
be installed in user-controlled locations in order to allow a developer to
work with the latest version.  However, some projects might require an older
version and are not ready to be developed with the newer version of a given
language.  Sometimes the system-level dependencies are also a bit complicated
to deal with when trying to compile a language from the source itself.

Thankfully, several members in each of these respective development
communities have released a variety of tools to help manage installing and
managing multiple versions of these languages.  These tools are typically
called version managers and exist to make it easy to install multiple versions
of a given language and switch between them seamlessly.  Multiple options
exist for each of these languages.  While we recommend that a developer use
one, it is up to them to determine which one best suites their needs and work
style.  Please refer to the language specific guides we have for details about
some of the more prominent options available.

# Language package managers

Ruby, Python, Node.js (JavaScript), and Go all ship with tools that help
developers install, create, and maintain third-party packages for each
respective language:

- [Ruby](./ruby/):  `gem`
- [Python](./python/):  `pip`
- [NodeJS (JavaScript)](./javascript/):  `npm`
- [Go](./go/):  `go get`

Please refer to the language-specific guides for more details about each of
these tools.  Again, a developer must bear in mind that multiple projects might
rely on the same packages but different versions of them.  For example, one
project may be a Rails 3.x app, but another might be a Rails 4.x app.  In a
similar manner, a Python project may be developed on Django 1.4 but another is
using Django 1.8, and the developer is contributing to both of them.

Each language has a variety of solutions available to them to help solve this
problem.  In some cases, the version manager of a language can also serve as
the solution (`RVM` for Ruby comes to mind), and in others there are separate
tools available to help manage the project dependencies in a way that
preserves the stable state of your machine and isolates them (e.g.,
`virtualenv` for Python).  As with the language version managers we recommend
that a developer use one of these tools, but it is up to them to determine
which one(s) fit their workflow the best.
