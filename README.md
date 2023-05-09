# askcos-deploy
[![askcos-base](https://img.shields.io/badge/-askcos--base-lightgray?style=flat-square)](https://github.com/ASKCOS/askcos-base)
[![askcos-data](https://img.shields.io/badge/-askcos--data-lightgray?style=flat-square)](https://github.com/ASKCOS/askcos-data)
[![askcos-core](https://img.shields.io/badge/-askcos--core-lightgray?style=flat-square)](https://github.com/ASKCOS/askcos-core)
[![askcos-site](https://img.shields.io/badge/-askcos--site-lightgray?style=flat-square)](https://github.com/ASKCOS/askcos-site)
[![askcos-deploy](https://img.shields.io/badge/-askcos--deploy-blue?style=flat-square)](https://github.com/ASKCOS/askcos-deploy)

Deployment scripts for the [`askcos-site`](https://github.com/ASKCOS/askcos-site) web application for the prediction of feasible synthetic routes towards a desired compound and associated tasks related to synthesis planning. Originally developed under the DARPA Make-It program and now being developed under the [MLPDS Consortium](http://mlpds.mit.edu).

## Getting Started

This package provides various deployment and management scripts for [`askcos-site`](https://github.com/ASKCOS/askcos-site).

Software requirements for deployment are Docker and Docker Compose. A basic configuration for Kubernetes is also provided.

The core functions needed for deployment are all contained in the `deploy.sh` script (or `deploy_k8.sh` for Kubernetes deployment). See `deploy.sh -h` for supported commands and options.

Additional information on deployment and releases can be found at [askcos.github.io](https://askcos.github.io).

## Building and Running Locally

1. Check out askcos-site, askcos-core, and askcos-deploy repos. Wait a second on askcos-data (see next step). All repos must be checked out into a single parent directory.
1. Build `askcos/askcos-data:dev` image (Note: it is probably possible to lightly rearrange some things to use the docker hub version of this image instead of building a `dev` version, but you'll need to download all the data either way).
    1. Ensure the `git-lfs` package is installed and set up _before_ cloning the askcos-dev repo.
    1. Clone the askcos-data repo. It may hang for some time with no output while the git-lfs hooks download several hundred megabytes of data.
    1. In the askcos-data directory, run `docker build -t askcos/askcos-data:dev .`
1. Build `askcos/askcos-core:dev` image by running `make build VERSION=dev` in the askcos-core directory.
    - This depends on the askcos-base image hosted on docker hub. This image doesn't appear do much besides set up a python environment and install various other third-party dependencies, so you likely won't need to modify anything about it necessitating a local version.
1. Build `askcos/askcos-site:dev` image by running `make build VERSION=dev` in the askcos-site directory.
    - This depends on the askcos-ketcher image hosted on docker hub. This is another image that seems to be just third-party dependencies that you likely don't need a modified local version of.
1. Run the deploy script.
    1. In the askcos-deploy directory, copy `.env.example` into a new file called `.env`, and change the new file to have `VERSION_NUMBER=dev`
    1. Run `./deploy.sh deploy-http -l -n -f docker-compose.dev.yml`
    1. App should now be available at http://localhost
    1. Code changes in askcos-site/askcos_site and askcos-core/askcos directories should be reflected on page refresh
