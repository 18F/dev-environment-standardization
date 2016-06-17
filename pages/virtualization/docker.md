---
permalink: /virtualization/docker/
title: Docker
parent: Virtualization
---

### Why Docker?

From [What is Docker?][]:

> Docker allows you to package an application with all of its
> dependencies into a standardized unit for software development.

In short, it means that getting people set up with a development
environment can *potentially* be made dramatically easier. And because
the same tool can be used to deploy to production websites, it also
means more [dev/prod parity][].

The reality, at the time of this writing, is a little bit more nuanced.

For one thing, Docker itself has a learning curve, and it's still maturing
as a product. And until [Docker goes native][], for instance, Mac and
Windows users will have a bit of a harder time than Linux users.

Aside from that, 18F isn't yet deploying Docker containers in production,
so the dev/prod parity story also falls short at the moment.

However, despite these limitations, Docker can still make development a
lot easier; this will only improve as Docker matures, and (hopefully) as
the government adopts container-based infrastructure in production.

### Definitions and Terms

There are a lot of concepts and tools in the Docker ecosystem, and
understanding the relationship between them can be quite intimidating at
first. In this section, we'll introduce you to the bare minimum you'll
need to know to effectively use Docker at 18F.

**Containers** are the fundamental building block in the Docker ecosystem.
For the most part, you can think of a container as a very lightweight
virtual machine. Most web applications are actually a collection of
containers that are connected in some way. For more details, see
[What is Docker?][].

Another key component of Docker is the `docker` command-line tool. It's
actually a fairly low-level piece of software. You likely won't use it
day-to-day; think of it as "plumbing" that higher-level tools use to
do awesome things.

**Docker Compose** is one such tool. It's like the porcelain fixture that
sits atop the plumbing and makes everything a lot more user-friendly. It's
likely what you'll be using most often as a developer. For more details,
see the [Docker Compose][] product page.

**Docker Machine** is a tool that most folks won't need to know about once
[Docker goes native][]. Until then, though, if you're on Windows or OS X,
the key concept to keep in mind is that Docker Machine is responsible for
running a linux-based virtual machine behind-the-scenes on your computer.
Whenever you use `docker` or Docker Compose, your command is actually
proxied over to the VM, which does all the hard work. Also, when you
run a web server in a Docker container, it'll be exposed on this VM,
*not* your localhost, which makes figuring out its URL a bit inconvenient.
More on that later.

### Troubleshooting

Almost all troubleshooting of docker stems from Docker Machine. Again,
once [Docker goes native][], life will be much easier for OS X and
Windows users.

#### Connecting To A Container

As mentioned earlier, if you're on OS X or Windows, Docker is actually
running on a linux-based VM on your machine, which means that the app
you're developing is hosted on a container on that VM, *not* localhost.

To find the IP of that machine, run:

```sh
$ docker-machine ip default
```

So, if your app is exposed at port 8000, and the above command returns
`192.168.99.100`, you could visit your app by browsing to
`http://192.168.99.100:8000`.

#### Docker Daemon Connection Problems

The most common error is the following:

```
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```

Alternatively, you might see this:

```
ERROR: Couldn't connect to Docker daemon - you might need to run `docker-machine start default`.
```

To fix this issue, run the following in your terminal:

```sh
$ eval $(docker-machine env default)
```

Alternatively, you can run the **Docker Quickstart Terminal** app, which
should be discoverable through OS X Spotlight or the Windows Start Menu.
This will open a terminal session with your default Docker Machine started
and the proper environment variables present.

#### Network Timeout Problems

You might see an error like the following:

```
Network timed out while trying to connect to https://index.docker.io/v1/repositories/library/<some_image_name>/images. You may want to check your internet connection or if you are behind a proxy.
```

Most likely your docker host has got into a weird state. Run the following commands:

```sh
$ docker-machine restart default
$ eval "$(docker-machine env default)"
```

#### Other Problems

Try looking into Docker's [Troubleshooting guide][].

### Other Tools

* [DLite](https://github.com/nlf/dlite) has been recommended as
  a simpler, more convenient way to use Docker on OS X.

* [Kitematic](https://kitematic.com/) is a GUI client for docker
  that transparently takes care of some common problems that
  occur when using docker from the command-line.

[What is Docker?]: https://www.docker.com/what-docker
[dev/prod parity]: http://12factor.net/dev-prod-parity
[Docker goes native]: https://blog.docker.com/2016/03/docker-for-mac-windows-beta/
[Docker Compose]: https://www.docker.com/products/docker-compose
[Troubleshooting guide]: https://docs.docker.com/faqs/troubleshoot/
