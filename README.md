# dokku-registry [![Build Status](https://travis-ci.org/josegonzalez/dokku-registry.svg?branch=master)](https://travis-ci.org/josegonzalez/dokku-registry)

Allows you to interact with a remote Docker Registry.

## requirements

- dokku 0.6.5+
- docker 1.10.x

## installation

```shell
dokku plugin:install https://github.com/josegonzalez/dokku-registry.git  registry
```

## usage

```
Usage: dokku registry[:COMMAND] <app> [<tag>] [<registry>]

Run registry-related commands on an application.

Additional commands:
    registry <app>                                       Shows the registry status for an application
    registry:login <server> <username> <password>        Logs into a docker registry
    registry:pull <app> <tag>                            Pull an image from a docker registry
    registry:push <app> <tag>                            Push an image to a docker registry
    registry:set-image <app> <IMAGE>                     Set the image name for an app
    registry:set-registry <app> <registry>               Set the registry for an app
    registry:set-username <app> <username>               Set the username for an app
    registry:tag-latest-local <app>                      Shows latest local tag version
    registry:unset-image <app> <IMAGE>                   Unsets the image name for an app
    registry:unset-registry <app>                        Unsets the registry for an app
    registry:unset-username <app>                        Unsets the username for an app
```

### Setup

To enable automatic pushing to a remote registry, you'll need to first login to that registry:

```shell
dokku registry:login REGISTRY_SERVER REGISTRY_USERNAME REGISTRY_PASSWORD
```

You also need to set the registry for each app you desire to integrate with:

```shell
dokku registry:set-registry APP_NAME DOCKER_REGISTRY
```

Once set, this plugin will:

- on `post-release`, create a tagged image with an auto-incrementing tag number
- push the new tag to the remote registry
- delete the new tag locally

Application deletion *will not* clean up remote repositories. Please keep this in mind and adjust your workflow for application deletion accordingly.
