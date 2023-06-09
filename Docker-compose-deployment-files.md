The root folder of the repo contains all docker-compose files (`docker-compose*.yml`). Here is a list of all of them and what's their purpose, for different deployment needs.

> **CONTENT**

- [Getting Started](#getting-started)
- [Run eShopOnContainers locally](#run-eshoponcontainers-locally)
- [Run eShopOnContainers on a remote docker host](#run-eshoponcontainers-on-a-remote-docker-host)
- [Run eShopOnContainers on Windows containers](#run-eshoponcontainers-on-windows-containers)
- [Run infrastructure containers](#run-infrastructure-containers)
- [Other files](#other-files)

## Getting Started

Make sure you have [installed](https://docs.docker.com/docker-for-windows/install/) and [configured](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Windows-setup#configure-docker) docker in your environment. After that, you can run the below commands from the **/src/** directory and get started with the `eShopOnContainers` immediately.

```powershell
docker compose build
docker compose up
```

You should be able to browse different components of the application by using the below URLs :

```powershell
Web Status : http://host.docker.internal:5107/
Web MVC :  http://host.docker.internal:5100/
Web SPA :  http://host.docker.internal:5104/
```

> Note: If you are running this application in macOS then use docker.for.mac.localhost as DNS name in .env file and the above URLs instead of host.docker.internal.

Below are the other avenues to setup `eShopOnContainers`.

## Run eShopOnContainers locally

* `docker-compose.yml`: This file contains **the definition of all images needed for running eShopOnContainers**.
* `docker-compose.override.yml`: This file contains the base configuration for all images of the previous file

Usually these two files are using together. The standard way to start eShopOnContainers from CLI is:

```console
docker compose -f docker-compose.yml -f docker-compose.override.yml
```

This will start eShopOnContainers with all containers running locally, and it is the default development environment.

## Run eShopOnContainers on a remote docker host

* `docker-compose.prod.yml`: This file is a replacement of the `docker-compose.override.yml` but contains some configurations more suitable for a "production" environment or when you need to run the services using an external docker host.

```console
docker compose -f docker-compose.yml -f docker-compose.prod.yml
```

When using this file the following environments variables must be set:

* `ESHOP_PROD_EXTERNAL_DNS_NAME_OR_IP` with the IP or DNS name of the docker host that runs the services (can use `localhost` if needed). 
* `ESHOP_AZURE_STORAGE_CATALOG` with the URL of the Azure Storage that will host the catalog images
* `ESHOP_AZURE_STORAGE_MARKETING` with the URL of the Azure Storage that will host the marketing campaign images

You might wonder why an external image resource (storage) is needed when using `docker-compose.prod.yml` instead of `docker-compose.override.yml`. Answer to this is related to a limitation of Docker Compose file format. This is how we set the environment configuration of Catalog microservice in `docker-compose.override.yml`:

```yml
PicBaseUrl=${ESHOP_AZURE_STORAGE_CATALOG:-http://localhost:5101/api/v1/catalog/items/[0]/pic/}
```

The `PicBaseUrl` variable is set to the value of `ESHOP_AZURE_STORAGE_CATALOG` if this variable is set to any value other than blank string. If not, the value is set to `http://localhost:5101/api/v1/catalog/items/[0]/pic/`. That works perfectly in a local environment where you run all your services in `localhost` and setting `ESHOP_AZURE_STORAGE_CATALOG` you can use or not Azure Storage for the images (if you don't use Azure Storage images are served locally by catalog servide). But when you run the services in a external docker host, specified in `ESHOP_PROD_EXTERNAL_DNS_NAME_OR_IP`, the configuration should be as follows:

```yml
PicBaseUrl=${ESHOP_AZURE_STORAGE_CATALOG:-http://${ESHOP_PROD_EXTERNAL_DNS_NAME_OR_IP}:5101/api/v1/catalog/items/[0]/pic/}
```

So, use `ESHOP_AZURE_STORAGE_CATALOG` if set, and if not use `http://${ESHOP_PROD_EXTERNAL_DNS_NAME_OR_IP}:5101/api/v1/catalog/items/[0]/pic/}`. Unfortunately seems that docker-compose do not substitute variables inside variables, so the value that `PicBaseUrl` gets if `ESHOP_AZURE_STORAGE_CATALOG` is not set is literally `http://${ESHOP_PROD_EXTERNAL_DNS_NAME_OR_IP}:5101/api/v1/catalog/items/[0]/pic/}` without any substitution.

## Run eShopOnContainers on Windows containers

All `docker-compose-windows*.yml` files have a 1:1 relationship with the same file without the `-windows` in its name. Those files are used to run Windows Containers instead of Linux Containers.

* `docker-compose-windows.yml`: Contains the definitions of all containers that are needed to run eShopOnContainers using windows containers (equivalent to `docker-compose.yml`).
* `docker-compose-windows.override.yml`: Contains the base configuration for all windows containers

**Note** We plan **to remove** the `docker-compose-windows.override.yml` file, because it is **exactly the same** as the `docker-compose.override.yml`. The reason of its existence is historical, but is no longer needed. You can use `docker-compose.override.yml` instead.

* `docker-compose-windows.prod.yml` is the equivalent of `docker-compose.prod.yml` for containers windows. As happens with `docker-compose-windows.override.yml` this file will be deleted in a near future, so you should use `docker-compose.prod.yml` instead.

## Run infrastructure containers

These files were intended to provide a fast way to start only "infrastructure" containers (SQL Server, Redis, etc). *This files are deprecated and will be deleted in a near future**:

* `docker-compose-external.override.yml`
* `docker-compose-external.yml`

If you want to start only certain containers use `docker compose -f ... -f ... up container1 contaner2 containerN` as specified in [compose doc](https://docs.docker.com/compose/reference/up/)

## Other files

* `docker-compose.nobuild.yml`: This file contains the definition of all images needed to run the eShopOnContainers. Contains **the same images that `docker-compose.yml`** but without any `build` instruction. If you use this file instead of `docker-compose.yml` when launching the project and you don't have the images built locally, **the images will be pulled from dockerhub**. This file is not intended for development usage, but for some CI/CD scenarios.
* `docker-compose.vs.debug.yml`: This file is used by Docker Tools of VS2017, and should not be used directly.
* `docker-compose.vs.release.yml`: This file is used by Docker Tools of VS2017, and should not be used directly.

**Note**: The reason why we need the `docker-compose.nobuild.yml` is that [docker-compose issue #3391](https://github.com/docker/compose/issues/3391). Once solved, parameter `--no-build` of docker-compose could be used safely in a CI/CD environments and the need for this file will disappear.
