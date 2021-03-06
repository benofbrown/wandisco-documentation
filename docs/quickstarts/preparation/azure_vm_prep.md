---
id: azure_vm_prep
title: Preparing an Azure Linux VM for a Fusion installation
sidebar_label: Azure VM preparation
---

This quickstart helps you prepare an Azure Linux VM suitable for a Fusion installation using docker. It walks you through:

* Installation of utilities.
* Verification of available storage for docker images.

**This is not required if you have used our [Azure VM creation](https://wandisco.github.io/wandisco-documentation/docs/quickstarts/preparation/azure_vm_creation) guide, as all utilities and disk requirements will have been included.**

## Prerequisites

[//]: <Issues with running out of disk space because of docker images filling up the root partition (see DAP-134). As such, we suggest adding a data disk for storage.>

* Azure VM created and started.
  * Instructions are provided for UbuntuLTS 18.04.
  * A minimum of 24GB available storage for the `/var/lib/docker` directory.
  * Root access on server (this is normally available by default). All the commands given here should be run as **root** user.
* Access to your company's VPN or similar if required.

## Preparation

### Install utilities

1. Log in to the VM via a terminal session.

2. Run the command below to install Git.

   `apt-get update && apt install -y git`

3. Run the commands below to install [Docker](https://docs.docker.com/install/) (v19.03.5 or higher).

   `apt install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common`

   `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -`

   `add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

   `apt-get update && apt install -y docker-ce docker-ce-cli containerd.io`

4. Start the Docker service and verify that it is correctly installed.

   `systemctl start docker`

   `docker run hello-world` - This will print an informational message and exit if docker is running correctly.

   `systemctl enable docker` - This will enable docker to start up automatically on server reboot.

5. Install [Docker Compose for Linux](https://docs.docker.com/compose/install/#install-compose) (v1.25.0 or higher) by running the commands below.

   `curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

   `chmod +x /usr/local/bin/docker-compose`

   `ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose`

6. Verify that Docker Compose is correctly installed.

   `docker-compose --version`

   _Example output_
   ```json
   docker-compose version 1.25.0, build 0a186604
   ```

### Verify storage for docker images

Verify that there is at least 24GB of disk space available in the `/var/lib/docker` directory.

`df -h /var/lib/docker`

_Example output_

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        31G    3G   28G  10% /
```
