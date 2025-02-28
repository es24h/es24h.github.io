---
layout: page
title: Running Elasticsearch
menubar: content_menu
show_sidebar: false
toc: true
---

## Installation options

Elasticsearch is a server-side component. It runs as a service on one or more servers or containers.

The easiest way to run Elasticsearch is on [Elastic Cloud](https://link.es24h.com/75ea), which uses the infrastructure of major cloud providers. As you create a deployment, Elastic Cloud offers you the choice to run on AWS, Google Cloud or Microsoft Azure. If you already have credits with a cloud provider, you can provision deployments through that cloud provider's marketplace.

If you don't want to run Elasticsearch on a public cloud provider, there are several options to run Elasticsearch on your own hardware. For Production workloads, [Elastic Cloud on Kubernetes](https://link.es24h.com/b64a) (ECK) is a good choice, especially if you are already using Kubernetes. Elastic also provides several non-Kubernetes [alternatives](https://link.es24h.com/54d8). There are binaries that you can run as a service, RPM or Debian packages to install Elasticsearch as a Linux service, and Docker container images.

For running a local development environment, Elastic provides several options. You can use a [`start-local`](https://link.es24h.com/caf8) script that runs a one-node Elasticsearch cluster and Kibana locally, you can download binaries for most platforms, or run on Docker.

## Start a development environment using Docker Compose

The easiest way to run a multi-node Elasticsearch cluster and Kibana is using Docker Compose.

1. Download, install, and start [Docker Desktop](https://link.es24h.com/c434).

1. Create a directory. In that directory, download these two files:
    * [`docker-compose.yml`](https://github.com/elastic/elasticsearch/blob/{{site.version_xy}}/docs/reference/setup/install/docker/docker-compose.yml)
    * [`.env`](https://github.com/elastic/elasticsearch/blob/{{site.version_xy}}/docs/reference/setup/install/ d docker/.env)  
  
    The `docker-compose.yml` file describes the containers that will be run. You don't need to edit this file. The `.env` file contains variables that you will edit next.

1. Edit the `.env` file and set values for these three variables:
    1. `ELASTIC_PASSWORD` - choose any password of at least 6 characters.
    1. `KIBANA_PASSWORD` - choose any password of at least 6 characters.
    1. `STACK_VERSION` - set to the desired version of Elasticsearch, for example: `{{site.version_xyz}}`.

1. Open a terminal, change into the directory containing the files, and run:  
```term
docker-compose up -d
```