---
permalink: /github-forks/
title: Working with forked GitHub repositories
---
When working on projects originating outside the organization, the first thing
you need to do is to fork the original project repository on GitHub
(presuming, as we do at 18F, that the code is on GitHub).

### Note for Go projects

__Do not `go get` your fork of the repository if your intent is to potentially
contribute changes upstream!__ Even if the intent is to maintain your own fork
without contributing changes upstream, you may still wish to [follow the
instructions below](#clone-original) for cloning the original repository, to
potentially save the work of having to update `import` paths within the Go
source files, since these paths are dependent on the `$GOPATH/src`
installation convention.

## <a name="clone-original"></a>Cloning the original repository

This is how you can clone the original repository and configure it to push
your changes to your forked repository, using
[jekyll/jekyll](https://github.com/jekyll/jekyll/) as an example:

```sh
$ git clone https://github.com/jekyll/jekyll.git
$ cd jekyll
$ git remote add 18f git@github.com:18F/jekyll.git
```

To view the result:

```sh
$ git remote -v
18f     git@github.com:18F/jekyll.git (fetch)
18f     git@github.com:18F/jekyll.git (push)
origin  https://github.com/jekyll/jekyll (fetch)
origin  https://github.com/jekyll/jekyll (push)
```

This way, you can pull changes from the original repository by running `git
fetch` or `git pull` on branches from the `origin` remote, while you push your
changes to `18f`.

You may wish to instead rename `origin` to `upstream`, and set `origin` as the
label for your forked repository, to match the scheme described in the [Cloning
the forked repository section](#clone-fork). This is left as an exercise for
the reader.

## <a name="clone-fork"></a>Cloning the forked repository

This is how you can clone the forked repository and set up the original
repository to pull in upstream changes, again using
[jekyll/jekyll](https://github.com/jekyll/jekyll/) as an example:

```sh
$ git clone git@github.com:18F/jekyll.git
$ cd jekyll
$ git remote add upstream https://github.com/jekyll/jekyll.git
```

To view the result:

```sh
$ git remote -v
origin    git@github.com:18F/jekyll.git (fetch)
origin    git@github.com:18F/jekyll.git (push)
upstream  https://github.com/jekyll/jekyll (fetch)
upstream  https://github.com/jekyll/jekyll (push)
```

This way, you can pull changes from the original repository by running `git
fetch` or `git pull` on branches from the `upstream` remote, while you push
your changes to `origin`.

## Preparing changes to contribute to the original repository

Now, to begin implementing a feature, check out a branch based on the original
repository (usually `origin/master` if you cloned the original, or
`upstream/master` if you’ve cloned your fork):

```sh
$ git checkout -b my-new-feature
```

Once you’ve made your changes, written tests for them, and are satisfied with
the result, you can then push your new feature branch to your fork of the
repository (18F/jekyll in this case; replace `18f` with `origin` if you cloned
your fork instead of the original repository):

```sh
$ git push -u 18f my-new-feature
```

For subsequent changes to the branch, you can just use `git push`. When you’re
ready, you can now visit your forked repository on GitHub and [create a pull
request](https://help.github.com/articles/creating-a-pull-request/) to propose
that the changes to your fork be integrated into the original repository.

