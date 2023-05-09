This page covers the setup of your Windows development computer.

> **CONTENT**

- [Install Docker Desktop](#install-docker-desktop)
- [Configure Docker Desktop](#configure-docker-desktop)
  - [Memory and CPU](#memory-and-cpu)
- [Build and deploy eShopOnContainers](#build-and-deploy-eshoponcontainers)
  - [1. Create a folder for your repositories](#1-create-a-folder-for-your-repositories)
  - [2. Clone eShopOnContainer's GitHub repo](#2-clone-eshoponcontainers-github-repo)
  - [3. Build the application](#3-build-the-application)
  - [4. Deploy to the local Docker host](#4-deploy-to-the-local-docker-host)
- [Explore the application](#explore-the-application)
- [Optional - Use Visual Studio](#optional---use-visual-studio)
  - [Server side (Microservices and web applications) - Workloads](#server-side-microservices-and-web-applications---workloads)
  - [Mobile (Xamarin apps for iOS, Android and Windows UWP) - Workloads](#mobile-xamarin-apps-for-ios-android-and-windows-uwp---workloads)
  - [Stop Docker background tasks on project open](#stop-docker-background-tasks-on-project-open)
  - [Open eShopOnContainers solution in Visual Studio](#open-eshoponcontainers-solution-in-visual-studio)
  - [Build and run the application with F5 or Ctrl+F5](#build-and-run-the-application-with-f5-or-ctrlf5)
    - [Set docker-compose as the default StartUp project](#set-docker-compose-as-the-default-startup-project)
    - [But if someone wants to debug WebSPA project in Visual Studio, then follow the below instructions:](#but-if-someone-wants-to-debug-webspa-project-in-visual-studio-then-follow-the-below-instructions)
    - [Debug with several breakpoints across the multiple containers/projects](#debug-with-several-breakpoints-across-the-multiple-containersprojects)
  - [Issue with "Visual Studio Tools for Docker" and network proxies/firewalls](#issue-with-visual-studio-tools-for-docker-and-network-proxiesfirewalls)
- [Optional - Use Visual Studio Code](#optional---use-visual-studio-code)
- [Explore the code](#explore-the-code)
- [Additional resources](#additional-resources)

## Install Docker Desktop

You can install Docker Desktop following :

Install [Docker Desktop Of Windows](https://docs.docker.com/docker-for-windows/install/)

![](images/Deploy-to-Local-Kubernetes/docker-desktop.png)

>**NOTE** You can ensure your system meets the [System-requirements](https://github.com/dotnet-architecture/eShopOnContainers/wiki/System-requirements#windows)

## Configure Docker Desktop

The initial Docker for Desktop configuration is not suitable to run eShopOnContainers because the app uses a total of 25 Linux containers.

Even though the microservices are rather light, the application also runs SQL Server, Redis, MongoDb, RabbitMQ and Seq as separate containers. The SQL Server container has four databases (for different microservices) and takes an important amount of memory.

So it's important to have enough memory RAM and CPU to Docker.

>**IMPORTANT:** If you have installed Docker Desktop on a system that supports WSL 2, then WSL 2 will be enabled by default. It automatically uses the required CPU and memory resources while building and running containers. For more details refer : [Docker Desktop WSL 2 backend](https://docs.docker.com/docker-for-windows/wsl/)

## Build and deploy eShopOnContainers

At this point you should be able to run eShopOnContainers from the command line. To do that, you should:

### 1. Create a folder for your repositories

Go to a directory to clone the repo, something like `C:\Users\<username>\source` will be fine.

### 2. Clone [eShopOnContainer's GitHub repo](https://github.com/dotnet-architecture/eShopOnContainers)

```console
git clone https://github.com/dotnet-architecture/eShopOnContainers.git
```

**Note:** Remember that the active development is done in `dev` branch. To test the latest code, use this branch instead of `master`.

### 3. Build the application

```console
cd eShopOnContainers\src
docker compose build
```

While building the docker images, you should see something like the following image, and the process should take between 10 and 30 minutes to complete, depending on the system speed.

![](images/Windows-setup/building-eshoponcontainers.png)

The first time you run this command it'll take some more additional time as it needs to pull/download the dotnet/core/aspnet and SDK images, so it'll take its time.

### 4. Deploy to the local Docker host

```console
docker compose up
```

You should view something like this in the first seconds:

![](images/Windows-setup/running-1-eshoponcontainers.png)

After a few more seconds you should see something like this:

![](images/Windows-setup/running-2-eshoponcontainers.png)

At this point you should be able to navigate to <http://host.docker.internal:5107/> and see the WebStatus microservice:

![](images/Windows-setup/webstatus-eshoponcontainers.png)

When all microservices are up (green checks) you should be able to navigate to <http://host.docker.internal:5100/> and see the Home Page of eShopOnContainers:

![](images/Windows-setup/eshoponcontainers-homepage.png)

You should also be able to navigate to <http://host.docker.internal:5104/> and see the Home Page of eShopOnContainers on the SPA application:

![](images/Windows-setup/eshoponcontainers-spa-homepage.png)

## Explore the application

You can now [explore the application](Explore-the-application) or continue with the optional Visual Studio setup.

## Optional - Use Visual Studio

If you want to explore the code and debug the application to see it working, you have to install Visual Studio.

You have to install VS 2022 and you can install the latest release from https://visualstudio.microsoft.com/vs/.

**Make sure you have the latest .NET 6 SDK from <https://dotnet.microsoft.com/download/dotnet/6.0> installed.**

Upon running the installer, select the following workloads depending on the apps you intend to test or work with:

### Server side (Microservices and web applications) - Workloads

- ASP.NET and Web development
- Azure development (Optional) - It is optional but recommended in case you want to deploy to Docker hosts in Azure or use any other infrastructure in Azure.

![](images/Windows-setup/vs-2022-serverworkload.png)

### Mobile (Xamarin apps for iOS, Android and Windows UWP) - Workloads

If you also want to test/work with the eShopOnContainer model app based on Xamarin, you need to install the following additional workloads:

- Mobile development with .NET (Xamarin)
- Universal Windows Platform development
- .NET desktop development (Optional) - This is not required, but just in case you also want to make tests consuming the microservices from WPF or WinForms desktop apps

![](images/Windows-setup/vs-2022-mobileworkloads.png)

IMPORTANT: As mentioned above, make sure you are NOT installing Google's Android emulator with Intel HAXM hypervisor or you will run on an incompatibility and Hyper-V won't work in your machine, therefore, Docker for Windows won't work when trying to run the Linux host or any host with Hyper-V.

Make sure that you DO NOT select the highlighted options below with red arrows:

![](images/Windows-setup/xamarin-workload-options.png)

### Stop Docker background tasks on project open

VS runs some docker related tasks when opening a project with Docker support, to avoid these tasks from executing and slowing down your system, you might want to configure this options:

![](images/Windows-setup/configure-vs-docker-options.png)

### Open eShopOnContainers solution in Visual Studio

- If testing/working only with the server-side applications and services, open the solution: **eShopOnContainers-ServicesAndWebApps.sln** (Recommended for most cases testing the containers and web apps)

- If testing/working either with the server-side applications and services plus the Xamarin mobile apps, open the solution: **eShopOnContainers.sln**

Below you can see the full **eShopOnContainers-ServicesAndWebApps.sln** solution (server side) opened in Visual Studio 2022:

![](images/Windows-setup/vs-2022-eshoponcontainers-servicesandwebapps-solution.png)

Note how VS 2022 loads the `docker-compose.yml` files in a special node-tree so it uses that configuration to deploy/debug all the containers configured, at the same time into your Docker host.

### Build and run the application with F5 or Ctrl+F5

#### Set docker-compose as the default StartUp project

**IMPORTANT**: If the **"docker-compose" project** is not your "by default startup project", right click on the "docker-compose" node and select the "Set as Startup Project" menu option, as shown below:

![](images/Windows-setup/set-docker-node-as-default.png)

In Visual Studio before hitting F5, set the run configuration from the default 'Debug' to 'Release' for WebSpa as shown below:

![](images/Windows-setup/SetConfigurationToReleaseForWebSPA.png)

#### In case you find [issues](https://github.com/dotnet-architecture/eShopOnContainers/issues/1821) with WebSPA project while browsing the app using `http://<url>:5104/` after hitting F5 to debug in Visual Studio, then follow the below instructions:

1. At location `eShopOnContainers\src\Web\WebSPA`, update the Dockerfile in WebSPA project(`src/Web/WebSPA/Dockerfile`), insert the below piece of code between `WORKDIR /app`( Line 5) and `EXPOSE 80` (Line 6) 

```bash  
  RUN apt-get update
  RUN apt-get -y install curl gnupg
  RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -
  RUN apt-get -y install nodejs
  RUN npm install
  RUN npm -v
  ```

  ![](img/spa/docker-file-changes.png)

2. In `docker-compose.override.yml` file at location `eShopOnContainers\src`, under webspa.environment update the value of `ASPNETCORE_ENVIRONMENT` to `Development` as illustrated below:

```console
webspa:
  environment:
    - ASPNETCORE_ENVIRONMENT=Development
    - ASPNETCORE_URLS=http://0.0.0.0:80
    - IdentityUrl=http://${ESHOP_EXTERNAL_DNS_NAME_OR_IP}:5105
```	

At this point, after waiting sometime for the NuGet packages to be properly restored, you should be able to build the whole solution or even directly deploy/debug it into Docker by simple hitting F5 or pressing the debug "Play" button that now should be labeled as "Docker": 

![](images/Windows-setup/debug-F5-button.png)

VS 2022 should compile the .NET projects, then create the Docker images and finally deploy the containers in the Docker host (your by default Linux VM in Docker for Windows). 
Note that the first time you hit F5 it'll take more time, a few minutes at least, because in addition to compile your bits, it needs to pull/download the base images (SQL for Linux Docker Image, Redis Image, ASPNET image, etc.) and register them in the local image repo of your PC. The next time you hit F5 it'll be much faster.

Finally, because the docker-compose configuration project is configured to open the MVC application, it should open your by default browser and show the MVC application with data coming from the microservices/containers:

**NOTE: Depending on your system configuration, starting up all containers and applying database migrations might take some time. If you get a 503 error (service unavailable) in the browser, just give it some more time and refresh, after little while you should get to see the home page.**

![](images/Windows-setup/vs2017-f5-with-eshoponcontainers-web-mvc-in-browser.png)

Here's how the docker-compose configuration project is configured to open the MVC application:

![](images/Windows-setup/docker-compose-properties.png)

Finally, you can check out how the multiple containers are running in your Docker host by running the command **"docker ps"** like below:

![](images/Windows-setup/docker-ps.png)

You can see the 8 containers are running and what ports are being exposed, etc.

#### Debug with several breakpoints across the multiple containers/projects

Something very compelling and productive in VS 2022 is the capability to debug several breakpoints across the multiple containers/projects.
For instance, you could set a breakpoint in a controller within the MVC web app plus a second breakpoint in a second controller within the Catalog Web API microservice, then refresh the browser if you were still running the app or F5 again, and VS will be stopping within your microservices running in Docker as shown below! :)

Breakpoint at the MVC app running as Docker container in the Docker host:

![](images/Windows-setup/debugging-mvc-app.png)

Press F5 again...

Breakpoint at the Catalog microservice running as Docker container in the Docker host:

![](images/Windows-setup/debugging-webapi-microservice.png)

And that's it! Super simple! Visual Studio is handling all the complexities under the covers and you can directly do F5 and debug a multi-container application!

### Issue with "Visual Studio 2022 Tools for Docker" and network proxies/firewalls

After installing 2022 with docker support, if you cannot debug properly and you are trying from a corporate network behind a proxy, consider  the following issue and workarounds, until this issue is fixed in Visual Studio:

- https://github.com/dotnet-architecture/eShopOnContainers/issues/224#issuecomment-319462344

## Optional - Use Visual Studio Code

After installing VS code from <a href='https://code.visualstudio.com/'>Visual Studio Code</a> you can edit particular file or "open" the whole solution folder like in the following screenshots:

`Opening the Solution's folder`

![](images/Windows-setup/vs-code-1.png)

`Editing a .yml file`

![](images/Windows-setup/vs-code-2.png)

It is also recommended to install the C# extension and the Docker extension for VS Code:

![](images/Windows-setup/vs-code-3-extensions.png)

## Explore the code

You should be now ready to begin learning by [exploring the code](Explore-the-code) and debugging eShopOnContainers.

## Additional resources

- **Troubleshoot Visual Studio development with Docker (Networking)** \
  <https://docs.microsoft.com/en-us/visualstudio/containers/troubleshooting-docker-errorserrors-specific-to-networking-when-debugging-your-application>
  
- **[eShopOnContainers issue] Projects won't load in Visual Studio** \
  https://github.com/dotnet-architecture/eShopOnContainers/issues/1013#issuecomment-488664792
