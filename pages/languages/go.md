---
permalink: /languages/go/
title: Go
parent: Languages
---
The purpose of this guide is to help familiarize yourself with the Go
ecosystem and set up a handful of tools to help create a seamless working
development environment. This guide will help you will learn how to manage Go
installations from source and manage project dependencies in isolation.

## Go installation

Binary packages for Go are available for Linux, OS X, and Windows platforms,
but it can be installed from source on even more platforms (even [Plan
9](https://mike-bland.com/2015/06/08/getting-go-on-plan-9.html)). The ["Getting
Started" page on golang.org](https://golang.org/doc/install) has the complete
instructions to install the software and configure the environment. On OS X,
it can also be installed using [Homebrew](http://brew.sh/).

## Managing versions using `gvm`

On a UNIX-like system (e.g. Linux and OS X), the recommended way to install
different Go versions is to eschew the earlier methods in favor of a tool
called [`gvm` (Go Version Manager)](https://github.com/moovweb/gvm). This tool
automates the installation process (from source by default, or using a binary
distribution by adding the `-B` switch) and makes it easy to switch between
multiple Go versions, each in their own [`GOPATH` workspace](#gopath).

## gvm installation and usage

While the `gvm` repository’s `README` has pretty clear and complete
instructions, the basics of installing and using `gvm` are as follows. First,
to install the `gvm` program:

```sh
$ bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
```

The latest Go version at the time of writing is 1.5.1; you can see the
currently available versions using:

```sh
$ gvm listall
```

To see the currently installed versions:

```sh
$ gvm list
```

Then to install a particular version from a binary distribution, where
`VERSION` is one of the identifiers from `gvm listall` (to install from
source, leave off the `-B` flag):

```sh
$ gvm install VERSION -B
```

To begin using this version and set it as the default whenever you open a new
shell:

```sh
$ gvm use VERSION --default
```

You can then check that Go was successfully installed by running:

```sh
$ go version
```

## Installing versions 1.5 and later

Starting with the 1.5 series, [Go must be compiled using an earlier version of
Go](https://docs.google.com/document/d/1OaatvGhEAq7VseQ9kkavxKNAfepWy2yhPUBs96FGV28/edit).
To install the latest Go version from the source distribution (since [the Go
1.5 binary install isn’t currently working on OS
X](https://github.com/moovweb/gvm/issues/152)), start by installing the
latest 1.4.x version (1.4.3 at the time of writing):

```sh
$ gvm install go1.4.3
$ gvm use go1.4.3
```

Until `gvm` v1.0.23 is released, which will integrate
[moovweb/gvm#154](https://github.com/moovweb/gvm/pull/154) (aka
[e34bbfc](https://github.com/moovweb/gvm/commit/e34bbfc017c22c194564a5f9cc3c83d0aa6c1b03)),
explicitly setting `GOROOT_BOOTSTRAP` is necessary when invoking `gvm
install`:

```sh
$ GOROOT_BOOTSTRAP=$GOROOT gvm install go1.5.1
$ gvm use go1.5.1 --default
```

## <a name="gopath"></a>Understanding workspaces and GOPATH

Much of the power and convenience of the Go programming environment comes from
its strongly-opinionated concept of workspace layout. The official [How to
Write Go Code](https://golang.org/doc/code.html) document contains an
excellent explanation and walkthrough, but the highlights are:

- The `GOPATH` environment variable identifies the root directory of all Go
  workspaces.
- The source code for packages is installed under `$GOPATH/src`.
- New workspaces should be created under `$GOPATH/src`.
- The convention for package paths under `$GOPATH/src` is
  `repository_server/user_or_org/package_or_project_name`.
  - e.g: `github.com/18F/hmacauth`
- This same package path convention is used to install packages using
  [`go get`](go-get) and to import packages into Go programs via the [`import
  statement`](https://golang.org/doc/effective_go.html#names).

## <a name="go-get"></a>Package management with go get

The `go get` command is the standard tool for installing Go packages and
projects into `$GOPATH`. It relies upon the conventions described above to
fetch and install open source packages. For example, using the real-world
[18F/hmacauth](https://github.com/18F/hmacauth/) package:

```sh
$ go get github.com/18F/hmacauth
```

will install the package under `$GOPATH/src/github.com/18F/hmacauth` by
performing a `git clone` on the original source repository. That is to say,
the package directory will contain a local Git repository that can be used for
local development. The package can then be imported into a Go program via:

```go
import "github.com/18F/hmacauth"
```

`go get` will also automatically scan the Go source files within the target
package or project and automatically install any other Go package
dependencies. A later section of this document will go into [Go dependency
management](#dependencies) in detail.

## go get and gvm

`gvm use VERSION` will set `GOPATH` to a version-specific directory, and will
automatically add `$GOPATH/bin` to your `PATH`. This means you may have to
install the same packages and project workspaces multiple times, one per Go
version, but it also helps isolate any issues that are Go version-dependent
rather than package or project version-dependent.

## Working on existing internal projects within `GOPATH`

This is all that is needed to clone the existing GitHub repository and begin
working, using the real world
[18F/hmacproxy](https://github.com/18F/hmacproxy/) project as an example
(notice that the `-t` flag also fetches test program dependencies):

```sh
$ go get -t github.com/18F/hmacproxy
$ cd $GOPATH/src/github.com/18F/hmacproxy
```

### Creating projects within `GOPATH`

When creating a new project, there are two paths you can go by; the choice is
up to you. Either way, the result will be a workspace directory within
`$GOPATH/src` that is a Git repository with its remote `origin` set to a
corresponding GitHub repository.

### Option 0: create a repository first

The easier path is to create a new, empty repository on GitHub and use `go
get` to fetch it into your workspace, e.g.:

```sh
$ go get github.com/18F/my_new_go_project
$ cd $GOPATH/src/github.com/18F/my_new_go_project
```

### Option 1: create the project first

If you want to create the project before creating the repository, create a
directory within `GOPATH` that matches the package path convention described
above. For example:

```sh
$ mkdir -p $GOPATH/src/github.com/18F/my_new_go_project
$ cd $GOPATH/src/github.com/18F/my_new_go_project
```

After adding some Go code, a `README` file, a `LICENSE` file, a `CONTRIBUTING`
file, and so on:

```sh
$ git init
$ git commit -m 'initial commit'
```

Then, after creating the corresponding GitHub repository:

```sh
$ git remote add origin git@github.com:18F/my_new_go_project
$ git push -u origin master
```

### Option 2: create a project-specific `GOPATH`

One way to ensure that each Go project has a specific, correctly-configured
set of dependencies is to set `GOPATH` to a unique value for each project.
While managing multiple `GOPATH`s manually can get hairy, the `gvm pkgset`
command enables you to create arbitrary `GOPATH` directories that you can
switch between at will.

With a project-specific `GOPATH` defined, proceed with Option 0 or Option 1 to
create/clone your project within this new `GOPATH`.

## Non-18F open source project workflow

When working on projects outside the organization, the first thing you need to
do is to fork the original project repository on GitHub (presuming, as we do
at 18F, that the code is on GitHub). However, __do not `go get` your fork of the
repository if your intent is to potentially contribute changes upstream!__
Even if the intent is to maintain your own fork without contributing changes
upstream, you may still wish to follow the instructions below, to potentially
save the work of having to update `import` paths within the Go source files,
since these paths are dependent on the `$GOPATH/src` installation convention.

Instead, since `go get` installs source packages and projects as cloned Git
repositories under `$GOPATH/src`, you can instead use the `git remote` command
to add your fork to this cloned repository as an additional remote repository.
For example, this is how to configure a clone of
[bitly/oauth2_proxy](https://github.com/bitly/oauth2_proxy/), a real world,
external, open source Go project to which 18F has contributed via the
[18F/oauth2_proxy](https://github.com/18F/oauth2_proxy/) fork (again adding
`-t` to fetch test dependencies as well):

```sh
$ go get -t github.com/bitly/oauth2_proxy
$ cd $GOPATH/src/github.com/bitly/oauth2_proxy
$ git remote add 18f git@github.com:18F/oauth2_proxy.git
```

For more details on how to work with a forked repository, see the [Working
with forked GitHub repositories section]({{ site.baseurl }}/github-forks/).

## <a name="dependencies"></a>Managing dependencies within a project

By default, `go get` will install into `$GOPATH/src` the latest change from
the source repository from either a branch or tag matching the go version, or
from the default branch. However, many developers will want to keep packages
at a known version (via a commit hash or tag label) for any particular release
of their own application projects.

Even when developing a package to be integrated into other applications,
stabilizing versions during development can help isolate causes when
investigating defects. Versioned dependencies for packages usually should not
be checked into source control, if possible, to allow for different
combinations of packages of different versions as determined by an
application.

### Third-Party Dependency Management

Several versioning tools have been prototyped, but the new [Go 1.5 vendor
directories experiment](https://golang.org/cmd/go/#hdr-Vendor_Directories)
aims to cultivate a standard directory structure for versioned dependencies
that is reminiscent of [`npm`’s `node_modules` directory
structure](https://docs.npmjs.com/files/folders). It requires the environment
variable setting `GO15VENDOREXPERIMENT=1`. Several dependency management tools
have already included support for this structure.

#### Glide

One of the more promising dependency management tools for Go 1.5 is
[Glide](https://github.com/Masterminds/glide). It requires that a project
contain a `glide.yaml` file (reminiscent of Ruby’s `Gemfile` and npm’s
`package.json`), and the glide tool itself automated many updates to this
file.

#### Godep

The [Godep](https://github.com/tools/godep) tool provides automated vendoring
support for Go versions before 1.5 based on a `Godeps/Godeps.json` file and a
`Godeps/_workspace` directory. The `Godeps/_workspace` directory can be added
to `.gitignore` to avoid checking package sources into your own repository.
Godep will also rewrite any package paths to match those in your
`Godeps/_workspace` directory.

Godep also supports the `GO15VENDOREXPERIEMENT` format, which means that your
Go 1.5-compatible project will not require that its import paths be rewritten.

#### Gpm

To specify which exact version of a package should be installed into your
`GOPATH`, you can use [`gpm`](https://github.com/pote/gpm) and create a
`Godeps` file mapping each package to the specific tag or revision you wish to
install. Running `gpm get` will then install these specific package versions
into your `GOPATH`. This may be best used when specifying a project-specific
`GOPATH`.

## Linting tools

Go has a built in command, `go fmt`, that will automatically update files to
conform to a standard formatting convention. It should always be run on any
project, early and often.

In addition, the [`golint`](https://github.com/golang/lint) tool will make
suggestions based on [Effective Go](https://golang.org/doc/effective_go.html)
and the [golang CodeReviewComments
wiki](https://golang.org/wiki/CodeReviewComments). It’s not required that all
programs remain 100% lint-free using this, but it’s a good idea to get as
close as possible.

If you want the kitchen sink,
[`gometalinter`](https://github.com/alecthomas/gometalinter) will run `go
fmt`, `golint`, and a host of other tools. It may be impossible to have your
program conform to all of the tools, but it might still provide a host of
useful suggestions that any one tool might not provide.

## Putting it all together

With `gvm` to manage Go versions, an understanding of how `GOPATH` and `go
get` work, and a dependency management system (if needed), you have everything
you need to begin writing go packages, tests, and programs. Always run `go
fmt` before checking code into source control, and optionally `golint` and
other linters.

## Additional reading

- [The moovweb/gvm GitHub repository](https://github.com/moovweb/gvm/)
- [7 Easy Steps to Install Go (Golang) on Ubuntu](http://www.hostingadvice.com/how-to/install-golang-on-ubuntu/)
- [Go 1.5 Bootstrap Plan](https://golang.org/s/go15bootstrap)
- [golang/go PackageManagementTools wiki](https://github.com/golang/go/wiki/PackageManagementTools)
- [Go 1.5 Vendor Experiment proposal](https://golang.org/s/go15vendor)
- [Glide: Vendor package management for Golang](https://github.com/Masterminds/glide)
- [Go 1.5’s vendor/experiment](https://medium.com/@freeformz/go-1-5-s-vendor-experiment-fd3e830f52c3)
- [The New Go 1.5 Vendor Handling](http://engineeredweb.com/blog/2015/go-1.5-vendor-handling/)
- [gb project-based build tool](https://getgb.io/)
- [Godep: go dependency management](https://github.com/tools/godep)
