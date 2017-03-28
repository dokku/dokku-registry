# dokku-registry [![Build Status](https://travis-ci.org/josegonzalez/dokku-registry.svg?branch=master)](https://travis-ci.org/josegonzalez/dokku-registry)

Manages interaction with remote Docker Registries.

## requirements

- dokku 0.7.0+
- docker 1.12.x

## installation

```shell
dokku plugin:install https://github.com/josegonzalez/dokku-registry.git registry
```

## commands

```shell
registry:login <server> <username> <password> # Logs into a docker registry
registry:report <app> [<flag>]                # Shows the full report for an app
registry:pull <app> <tag>                     # Pull an image from a docker registry
registry:push <app> <tag>                     # Push an image to a docker registry
registry:set <app> <key> [<value>]            # Set or clear a key
```

## usage

To enable automatic pushing to a remote registry, you'll need to first login to that registry:

```shell
dokku registry:login REGISTRY_SERVER REGISTRY_USERNAME REGISTRY_PASSWORD
```

You also need to set the registry for each app you desire to integrate with:

```shell
dokku registry:set-server APP_NAME DOCKER_REGISTRY
```

Once set, this plugin will:

- on `post-release`, create a tagged image with an auto-incrementing tag number
- push the new tag to the remote registry
- delete the new tag locally

Application deletion *will not* clean up remote repositories. Please keep this in mind and adjust your workflow for application deletion accordingly.
