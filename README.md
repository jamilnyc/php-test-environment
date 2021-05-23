# PHP Test Environment

## Overview

This repository contains a basic Docker image of a PHP environment for testing PHP scripts or to use for PHP code tests during the interview process.

## Setup

### Repository

Clone the repository to your desired location.

```bash
git clone https://github.com/jamilnyc/php-test-environment.git
```

### Customize

Edit the `docker/Dockerfile` and `docker/docker-compose.yml` files to suit your needs. The files are already setup with a basic PHP environment with XDebug configured, but if you need additional libraries installed or files configured, feel free to add them.

If you're working on a full-fledged web app with a framework and Composer dependencies, you'll want to uncomment and configure the sections in the `Dockerfile` related to setting up Apache and installing composer libraries.

### Build Image

The following command builds the `interview-test` Docker image with the tag `latest`.

```bash
# From the project root directory
docker build --no-cache --rm --tag=interview-test:latest  docker/
```

### Run Container

The following command spins up the container. Note that it uses the `docker compose` subcommand instead of `docker-compose`.

```bash
docker compose -f docker/docker-compose.yml up -d
```

You should now have a container running called `interview-container`, which you can verify with the following.

```bash
docker container ls
```

### Configure Debugger

These instructions are specific to PHPStorm, but the high level concepts may apply to other IDEs.

#### Connect to Docker Server

* Open `PHPStorm > Preferences`
* Open `Build, Execution, Deployment > Docker`
* Add a Docker Server if one does not exist with the `+` sign
* Confirm you see _Connection Successful_ at the bottom
* Click the `Apply` button

#### Configure PHP Interpreter

* Open `PHPStorm > Preferences`
* Open `PHP`
* Click the three dots `...` next to CLI Interpreter
* Click the `+` icon to add an interpreter "From Docker, Vagrant, VM, WSL, Remote ..."
  * Select the Docker _Server_ created in the previous section
  * Point the _Configuration file_ to `docker-compose.yml`
  * Select the _Service_ defined in `docker-compose.yml` (default: `app-test`)
* For _Lifecycle_ select "Connect to existing container"
* Under _General_ you can use the refresh button to confirm the PHP and XDebug versions

#### Debug Settings

* Open `PHPStorm > Preferences`
* Open `PHP > Debug`
* Set the _Debug port_ to `9000` (set in the `Dockerfile`)
* Enable/disable "Break at first line in PHP scripts" as desired

#### Listen for Connections

* Enable listening for connections in PHPStorm
  * Click the icon that looks like a phone

## Troubleshooting

* On a Mac, if you have Screen Time limitations set, it may prevent the container from building and cause errors like the one below
  * See [this Q&A](https://superuser.com/a/1649205/174595)

```
File has unexpected size (137678 != 138126). Mirror sync in progress?
```
