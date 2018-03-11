# Sphinx-Server

Sphinx-Server allows you to build *Sphinx documentation* using a Docker
image based on Alpine (v3.7).

[![Build Status](https://travis-ci.org/dldl/sphinx-server.svg?branch=master)](https://travis-ci.org/dldl/sphinx-server)
[![Docker pulls](https://img.shields.io/docker/pulls/dldl/sphinx-server.svg)](https://hub.docker.com/r/dldl/sphinx-server/)
[![Docker layers](https://images.microbadger.com/badges/image/dldl/sphinx-server.svg)](https://microbadger.com/images/dldl/sphinx-server)
[![Docker automated build](https://img.shields.io/docker/automated/dldl/sphinx-server.svg)](https://hub.docker.com/r/dldl/sphinx-server/)

**Functionnalities:**

***Full version***
- *Sphinx documentation* served by a python server
- UML support with *PlantUML*
- `dot` support with *Graphviz*
- *Autobuild* with sphinx-autobuild
- HTTP *authentication*

***Basic version***
- *Sphinx documentation* served by a python server
- *Autobuild* with sphinx-autobuild
- HTTP *authentication*

**Limitations:**

- This image is not bundled with LaTeX but you can generate *.tex* files and
  compile them outside of the container
- It is not possible to customize ports when autobuild is enabled

## Installation

### With Docker Hub

Pull the image from Docker Hub using:

```sh
docker pull dldl/sphinx-server
```

If you only want to compile files without using the server, you can use the
following command (for example) at the root of your documentation:

```sh
docker run -t -v "$(pwd)":/web dldl/sphinx-server make html
```

### From source

Clone this repository on your computer and build the image using the following
command:

```sh
docker build -t dldl/sphinx-server .
```

## Usage

You may add a *.sphinx-server.yml* file at the root of your project
documentation if you want to use a custom configuration. You can see the default
*.sphinx-server.yml* in this repository that will be used if you don't add
yours.

### Container creation

**Without autobuild (production mode):**

If you want to enable HTTP authentication, just add a *.sphinx-server.yml* file
at the root of your project documentation and add a `credentials` section. You
also need to set `autobuild` to false.

Run the following command at the root of your documentation:

```sh
docker run -itd -v "$(pwd)":/web -p 8000:8000 --name sphinx-server dldl/sphinx-server
```

**With autobuild enabled:**

Autobuild is enabled by default. You may add folders and files to the `ignore` list.

Run the following command at the root of your documentation:

```sh
docker run -itd -v "$(pwd)":/web -u $(id -u):$(id -g) -p 35729:35729 -p 8000:8000 --name sphinx-server dldl/sphinx-server
```

The web server will be listening on port `8000`. You should not change it or autobuild
will no longer works. All the files in the current directory will be mount in the
container. A websocket will be listening on port `35729` to automatically
refresh the pages after a change.

### Interacting with the server

- To stop the server, use `docker stop sphinx-server`
- To start the server, use `docker start sphinx-server`
- To remove the server, use `docker rm -v sphinx-server`

You can use the following command to open a shell into the container:

```sh
docker exec -it sphinx-server /bin/sh
```

You can then run commands like `make html` to build the documentation.
