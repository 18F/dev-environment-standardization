---
permalink: /languages/javascript/
title: JavaScript
parent: Languages
---

# JavaScript Ecosystem Guide

This guide explains how to set up basic tools for working with JavaScript projects on your computer, specifically for working with [Node.js](https://nodejs.org/en/) (also called Node) and [npm](https://www.npmjs.com/), so that you can run and modify 18F projects that use these tools. (People who use JavaScript have built many useful tools on top of JavaScript, and this guide explains the few that we use a lot at 18F; this doesn't cover the entire JavaScript ecosystem.)

We wrote this guide for people inside and outside 18F who want to set up their own development environments for contributing to our projects — or reusing and adapting these projects for their own purposes! Following these setup instructions doesn't require knowing how to code.

Note: this guide talks about both `npm` and `nvm` — one letter different, but different tools.

## Installing Node

There are multiple ways to install Node, including through the binary downloads on [nodejs.org](https://nodejs.org/en/), through [Homebrew](http://brew.sh/) (`brew`) on OS X, or compiling it from [source](https://nodejs.org/en/download/). We've found that the most useful way is to first install a tool called `nvm` (Node Version Manager), since `nvm` helps you install and use multiple versions of Node.

## Installing nvm

To install [`nvm`](https://github.com/creationix/nvm), follow [its installation instructions](https://github.com/creationix/nvm#node-version-manager-) — running the install script should work fine.

Once you've installed `nvm`, you can use it by typing `nvm` on your command line. Now you can install a version of node. Let’s say your project calls for node v0.12.x (generally you can find this information under `engines` in the `package.json` at the root of the project).

You can issue the command `nvm install 0.12` which will install the correct binary for node. You can see all of the node versions you have installed with `nvm ls`.

To use any of the versions of node you have installed, use the `nvm use` command. For example, if you wanted to use the v0.12 you just installed, you can say `nvm use 0.12` and the version will be switched for you. You can confirm this by asking node for its version: `node --version`.

You can install any version of node this way. To see all the options, use `nvm ls-remote`.

Whoa, that’s a lot of nodes.

## Installing npm

When you install Node, it comes with `npm`. `npm` is the de facto package manager for JavaScript. (If you're wondering, the team behind `npm` says it doesn't stand for Node Package Manager. It doesn’t matter what it stands for, it is just a useful tool and part of the JavaScript ecosystem.)

Take a look and see which version of `npm` you currently have installed. You can do this with a similar `--version` flag, like `npm --version`.

Now let’s say that you want to have a different version of `npm` installed for the version of node you’re using. `npm` is updatable through `npm`, so if you wanted version 2.11.3 of `npm` you would say `npm install -g npm@2.11.3`. Check your `npm` version after running this to make sure it worked.
