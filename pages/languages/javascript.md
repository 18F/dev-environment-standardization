---
permalink: /languages/javascript/
title: JavaScript
parent: Languages
---

# JavaScript Ecosystem Guide

The purpose of this guide is to help familiarize yourself with the JavaScript ecosystem and set up a handful of tools to help create a seamless working development environment. In particular, we’ll be using a tool called `nvm` to make sure we’v, e got the right versions of `node` and `npm` installed for a project.

## Node Installation

Node is super easy to install and you can do so in a number of ways. The fine folks behind the project have put a bunch of time and energy into ensuring cross platform compatibility and it totally shows.

You can install it through the binaries on nodejs.org, through `brew` on Mac OS X, or compiling it from source but the preferred method is through a tool called `nvm`. `nvm` stands for Node Version Manager. It will enable us to easily install and use different versions of Node (including the relatively short lived iojs fork).

## nvm Installation

### Installing on Mac OS X

You can find the homepage for the project at https://github.com/creationix/nvm and [installation instructions](https://github.com/creationix/nvm#install-script) there. Overall, it is a really easy tool to set up, and can generally done with copying and pasting the following line of code into your terminal:
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.29.0/install.sh | bash

Once you have `nvm` installed, you can use it by just typing `nvm` on your command line. Generally, the first thing you’ll want to do is install a version of node. Let’s say your project calls for node v0.12.x (generally you can find this information under `engines` in the `package.json` at the root of the project).

You can just issue the command `$ nvm install 0.12` which will install the correct binary for node. You can see all of the node versions you have installed with `nvm ls`.

To use any of the versions of node you have installed, you just need to use the `nvm use` command. For example, if we wanted to use the v0.12 we just installed, we can simply say `nvm use 0.12` and the version will be switched for us. You can confirm that it has been by asking node for its version like `node --version`.

You can install any version of node this way. To see all the options you can use `nvm ls-remote`.

Woah, that’s a lot of nodes.

### Installing on Windows

For installing on a Windows machine, we recommend using [nvm-windows](https://github.com/coreybutler/nvm-windows).

## npm Installation

Now, all versions of node you’ll install come prepackaged with `npm`. `npm` is the defacto package manager for JavaScript, but the team behind it makes it a point to be clear that it doesn’t stand for Node Package Manager.

It really doesn’t matter what it stands for, it is just a pretty great tool and a cornerstone of the JavaScript ecosystem.

Take a look and see which version of `npm` you currently have installed. You can do this with a similar `--version` flag, like `npm --version`.

Now let’s say that you wanted to have a different version of `npm` installed for the version of node you’re using. `npm` is updatable through `npm`, so if you wanted version 2.11.3 of `npm` you would just say `npm install -g npm@2.11.3`. Check your `npm` version after running this to make sure it worked.
