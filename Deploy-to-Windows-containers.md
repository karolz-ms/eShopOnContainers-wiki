This is a draft page which will be evolving while our tests and dev regarding Windows Containers are completed.

> **CONTENT**

- [Supported platforms](#supported-platforms)
- [Set Docker to use Windows Container (Windows 10 only)](#set-docker-to-use-windows-container-windows-10-only)
- [The localhost loopback limitation in Windows Containers Docker hosts](#the-localhost-loopback-limitation-in-windows-containers-docker-hosts)
- [Deploy Windows Containers of eShopOnContainers](#deploy-windows-containers-of-eshoponcontainers)
  - [1. Compile the .NET application/services and build Docker images for Windows Containers](#1-compile-the-net-applicationservices-and-build-docker-images-for-windows-containers)
  - [2. Deploy/run the containers](#2-deployrun-the-containers)
- [Test/use the eShopOnContainers MVC app in a browser](#testuse-the-eshoponcontainers-mvc-app-in-a-browser)
- [RabbitMQ user and password](#rabbitmq-user-and-password)
- [Using custom login/password for RabbitMQ (if needed)](#using-custom-loginpassword-for-rabbitmq-if-needed)

## Supported platforms

**Windows 10** - Development Environment:

- Install **[Docker Community Edition](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description)** (Docker CE, formerly **Docker for Windows**)
- Support via forums/GitHub
- Can switch between Windows container development and Linux (in VM). There is no plan to drop either OS from Docker CE
- Designed for devs only. Not production

**Windows Server 2016** - Production Environment:

- Install **[Docker Enterprise Edition](https://store.docker.com/editions/enterprise/docker-ee-server-windows?tab=description)** (Docker EE)
- Designed to run apps in production
- Call Microsoft for support. If it's a Docker rather than Windows problem, they escalate to Docker and get it solved

Docker might provide per incident support system for Docker Community Edition, or provide a "EE Desktop" for developers, but it's their call to do so. Not Microsoft's.

## Set Docker to use Windows Container (Windows 10 only)

In Windows 10 you need to set Docker to use "Windows container" instead of Linux containers (in Windows Server 2016 Windows Containers are used by default). To do this, first you must have enabled container support in Windows 10. In "Turn Windows features on/off" select "Containers":

![](deploty-to-images/Deploy-to-Windows-containers/enable-windows-containers.png)

Then right click in the Docker icon on the notification bar and select the option "Switch to Windows Containers". If you don't see this option and see the option "Switch to Linux Containers" you're already using Windows Containers.

## The localhost loopback limitation in Windows Containers Docker hosts

Due to a default NAT limitation in current versions of Windows (see [https://blog.sixeyed.com/published-ports-on-windows-containers-dont-do-loopback/](https://blog.sixeyed.com/published-ports-on-windows-containers-dont-do-loopback/)) you can't access your containers using `localhost` from the host computer.
You have further information here, too: https://blogs.technet.microsoft.com/virtualization/2016/05/25/windows-nat-winnat-capabilities-and-limitations/

Although that [limitation has been removed beginning with Build 17025](https://blogs.technet.microsoft.com/networking/2017/11/06/available-to-windows-10-insiders-today-access-to-published-container-ports-via-localhost127-0-0-1/) (as of early 2018, still only available today to Windows Insiders, not public/stable release). With that version (Windows 10 Build 17025 or later), access to published container ports via “localhost”/127.0.0.1 is available.

Until you can use newer build of Windows 10 or Windows Server 2016, instead of localhost you can use either an IP address from the host's network card of (for example, let's suppose you have the 192.168.0.1 address) or you could also use the DockerNAT IP address, that is `10.0.75.1`. If you don't have that IP (`10.0.75.1`) shown when you get the info with `ipconfig`, you'll need to switch to Linux Containers so it creates that Docker NAT and then go back to Windows Containers (right click on Docker icon on the task bar).

If you use `start-windows-containers.ps1` to start the containers, as explained in the following section, that script will create environment variables with that IP for you, but if you directly use docker-compose, then you have to set the following environment variables:

Where you see `10.75.0.1` you could also use your network card IP discovered with `ipconfig`, or a production DNS name or IP if this were a production deployment.

* `ESHOP_EXTERNAL_DNS_NAME_OR_IP` to `10.75.0.1` 
* `ESHOP_AZURE_STORAGE_CATALOG_URL` to `http://10.0.75.1:5101/api/v1/catalog/items/[0]/pic/`
* `ESHOP_AZURE_STORAGE_MARKETING_URL` to `http://10.0.75.1:5110/api/v1/campaigns/[0]/pic/`

Note that the two last env-vars must be set only if you have not set them already because you were using Azure Storage for the images. If you are using azure storage for the images, you don't need to provide those URLs.

Once these variables are set you can run docker-compose to start the containers and navigate to `http://10.0.75.1:5100` to view the MVC Web app.

Using `start-windows-containers.ps1` is simpler as it'll create the env-vars for you.

## Deploy Windows Containers of eShopOnContainers

Since eShopOnContainers is using Docker Multi-Stage builds, the compilation of the .NET application bits is now performed by Docker itself right before building the Docker images.

Although you can create the Docker images when trying to run the containers, let's split it in two steps, so it is clearer.

### 1. Compile the .NET application/services and build Docker images for Windows Containers

In order compile the bits and build the Docker images, run:

```console
cd <root-folder-of--eshoponcontainers>
docker-compose -f docker-compose.yml -f docker-compose.windows.yml build
```

**Note** Be sure to pass both `-f` when building containers for windows containers!

### 2. Deploy/run the containers

The easiest way to run/start the Windows Containers of eShopOnContainers is by running this PowerShell script:

`start-windows-containers.ps1` 

You can find this script at /cli-windows/start-windows-containers.ps1

Otherwise, you could also run it directly with `docker-compose up` but then you'd be missing a few environment variables needed for Windows Containers. See the section below on the environment variables you will also need to configure.

Under the covers, in any case, the start-windows-containers.ps1 is running this command to deploy/run the containers:

```console
set ESHOP_OCELOT_VOLUME_SPEC=C:\app\configuration
docker-compose -f docker-compose.yml -f docker-compose.override.yml -f -f docker-compose.windows.yml -f docker-compose.override.windows.yml up
```

**IMPORTANT**: You need to include those files when running docker-compose up **and the `ESHOP_OCELOT_VOLUME_SPEC` environment variable must be set to `C:\app\configuration`**. Also you have to set the environment variables related to the localhost loopback limitation mentioned at the beginning of this post (if it applies to your environment).

Just for reference here are the docker compose files and what they do:

1. `docker-compose.yml`: Main compose file. Define all services for both Linux & Windows and set base images for Linux
2. `docker-compose.override.yml`: Main override file. Define all config for both Linux & Windows, with Linux-based defaults
3. `docker-compose.windows.yml`: Overrides some previous data (like images) for Windows containers
4. `docker-compose.override.windows.yml`: Adds specific windows-only configuration

## Test/use the eShopOnContainers MVC app in a browser

Open a browser and navigate to the following URL:

`http://10.0.75.1:5100`

## RabbitMQ user and password

For RabbitMQ we are using the [https://hub.docker.com/r/spring2/rabbitmq/](spring2/rabbitmq) image, which provides a ready RabbitMQ to use. This RabbitMQ is configured to accept AMQP connections from the user `admin:password`(this is different from the RabbitMQ Linux image which do not require any user/password when creating AMQP connections)

If you use `start-windows-containers.ps1` script to launch the containers or include the file `docker-compose.override.windows.yml` in the `docker-compose` command, then the containers will be configured to use this login/password, so everything will work.

## Using custom login/password for RabbitMQ (if needed)

**Note**: Read this only if you use any other RabbitMQ image (or server) that have its own user/password needed.

We support any user/password needed using the environment variables `ESHOP_SERVICE_BUS_USERNAME` and `ESHOP_SERVICE_BUS_PASSWORD`. These variables are used to set a username and password when connecting to RabbitMQ. So:

* In Linux these variables should be unset (or empty) **unless you're using any external RabbitMQ that requires any specific login/password**
* In Windows these variables should be set

To set this variables you have two options

1. Just set them on your shell 
2. Edit the `.env` file and add these variables

If you have set this images and you want to launch the containers you can use:

```powershell
.\cli-windows\start-windows-containers.ps1 -customEventBusLoginPassword $true
```

When passing the parameter `-customEventBusLoginPassword $true` to the script you are forcing to use the login/password set in the environment variables instead the default one (the one needed for spring2/rabbitmq). 

If you prefer to use `docker-compose` you can do it. Just call it without the `docker-compose.override.windows.yml` file:

```cmd
 set ESHOP_OCELOT_VOLUME_SPEC=C:\app\configuration
 docker-compose -f docker-compose.yml -f docker-compose.override.yml  -f docker-compose.windows.yml up
```
