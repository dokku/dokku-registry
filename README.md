# dokku-registry [![Build Status](https://travis-ci.org/dokku/dokku-registry.svg?branch=master)](https://travis-ci.org/dokku/dokku-registry)

Manages interaction with remote Docker Registries.

## requirements

- dokku 0.19.0+
- docker 1.14.x

## installation

> WARNING: This plugin is likely to change in functionality and break workflows, so please do not use this unless you are absolutely sure you're okay with that.

```shell
dokku plugin:install https://github.com/dokku/dokku-registry.git registry
```

## commands

```shell
registry:cleanup <app>                        # Cleans up old local images
registry:create-repository <app>              # Creates an image repository on the remote server
registry:login <server> <username> <password> # Logs into a docker registry
registry:report <app> [<flag>]                # Shows the full report for an app
registry:pull <app> <tag>                     # Pull an image from a docker registry
registry:push <app> <tag>                     # Push an image to a docker registry
registry:set <app> <key> [<value>]            # Set or clear a key
```

## usage

To enable automatic pushing to a remote registry, you will need to first login to that registry:

```shell
dokku registry:login hub.docker.com username password
```

You also need to set the registry for each app you desire to integrate with:

```shell
dokku registry:set node-js-app server hub.docker.com
```

The default image repository is `dokku/APP`. This may be changed via the following `registry:set` call:

```shell
dokku registry:set node-js-app image-repo dokku/node-js-app
```

Once set, this plugin will:

- on `post-release`, create a tagged image with an auto-incrementing tag number
- push the new tag to the remote registry (creating the repository if necessary)
- delete the new tag locally

Application deletion *will not* clean up remote repositories. Please keep this in mind and adjust your workflow for application deletion accordingly.

### creating repositories

> This currently only affects ECR registries

The `registry:create-repository` command can be used to create repositories for a configured app.

```shell
dokku registry:create-repository node-js-app
```

### settable options

The following options may be set via `registry:set`:

- `image-repo` (optional, default `dokku/APP`): The image repository to use for pushes. This will default to `dokku/APP`, where `APP` is the app name.
- `server` (required): The remote registry to push to.

Calling `dokku registry:set APP KEY` with no value will result in unsetting that key.
