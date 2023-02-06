> **CONTENT**
- [2023-02-03 - Release 6.0.0](#2023-02-03---release-600)
  - [Highlights](#highlights)
  - [Details](#details)
    - [Changes](#changes)
    - [Bug Fixes](#bug-fixes)
  - [Contributions](#contributions)
- [2021-11-08 - Release 5.0.0](#2021-11-08---release-500)
  - [Highlights](#highlights)
  - [Details](#details)
    - [Changes](#changes)
    - [Bug Fixes](#bug-fixes)
  - [Contributions](#contributions)
- [2021-01-21 - Release 3.1.1](#2021-01-21---release-311)
  - [Highlights](#highlights-1)
  - [Details](#details-1)
    - [Changes](#changes-1)
    - [Bug Fixes](#bug-fixes-1)
  - [Contributions](#contributions-1)
- [2020-12-14 - Release 3.1.0](#2020-12-14---release-310)
  - [Highlights](#highlights-2)
  - [Details](#details-2)
    - [Changes](#changes-2)
    - [Bug Fixes](#bug-fixes-2)
  - [Contributions](#contributions-2)
- [2019-11-26 - Release 3.0.0](#2019-11-26---release-300)
  - [Highlights - 3.0.0](#highlights---300)
  - [Details - 3.0.0](#details---300)
    - [New folder structure](#new-folder-structure)
    - [Retired deployment scenarios](#retired-deployment-scenarios)

## 2023-02-03 - Release 6.0.0

### Highlights

1. Updates the code to .NET 6.0

### Details

  #### Changes

  - Updates the code to .NET 6.0
  - Updates the dotnet sdk tag in Dockerfiles
  - Updated `Envoy` version to 1.22.0
  - Created `GlobalUsings.cs` and migrate using statements to these files for all projects
  - Removes language preview version from csproj
  - Update credit card `Expiration` date
  - `WebSPA` project related changes:
  - Updates packages in `package-lock.json` file in WebSPA project.
  - Updates following `Readme` files to incorporate latest instructions:
    - `README.md` file.
    - `branch-guide.md` file.
    - `CONTRIBUTING.md`
  - Contains following updates in `eShopOnContainers` wiki:
    - Updates the System-requirements page.
    - Docker-compose-deployment-files
    - Updates the steps for Windows-setup
    - Updates to the docker version images and VS 2022 installation images.
    - Updates to Deploy to Local Kubernetes page
    - Updates the section to describe how to run WebSPA project via Visual Studio 2022 in debug mode.
    - Updated the broken links
    
  #### Bug Fixes

  - Fixes ingress yaml indentation
  - Fixes for bundleconfig json
  - Fixes a few typos in fields name, documentation, and refactoring.


  ### Contributions

  We’d like to acknowledge and thank the following community members for their valuable contributions!

  @marranica, @Ich1goSan, @onurkanbakirci



## 2021-11-08 - Release 5.0.0

### Highlights

1. Updates the code to .NET 5.0

### Details

  #### Changes
  
  - Updates the code to .NET 5.0
  - Includes `C# 9.0` features.
  - Removes unused using from different classes.
  - Removes mobile project from `eShopOnContainers` to [eshop-mobile-client](https://github.com/dotnet-architecture/eshop-mobile-client)
  - Renames `GracePeriodManagerTask` in `GracePeriodManagerService`
  - Updates `CardType` and `Enumeration` class.
  - Updates the dotnet sdk tag in Dockerfiles
  - Migrates `Newtonsoft.Json` to `System.Text.Json`
  - `WebSPA` project related changes:
    - Updates the `WebSPA` UI theme.
    - Updates `WebSPA` Angular Version to `11.2`
    - Updates the `Catalog Item` specific images.
    - Updates packages in `package-lock.json` file in WebSPA project.
  - `GitHub` Actions specific changes:
    - Includes first version of GitHub actions as part of CI/CD pipelines
    - Updates Job steps to accomodate latest action packages.
    - Refactors GitHub Actions workflows to use composite.
  - Updates Azure Service Bus ARM Templates in `deploy/azure/az/servicebus/sbusdeploy.json `
  - Updates the Azure Service Bus SDK version and changes the namespace from `Microsoft.Azure.ServiceBus` to `Azure.Messaging.ServiceBus`
  - Uses the `Microsoft.AspNetCore.DataProtection.StackExchangeRedis` package instead of `Microsoft.AspNetCore.DataProtection.Redis`
  - Downgrades `Microsoft.AspNetCore.Hosting.Abstractions` to `2.2.0`
  - Updates following `Readme` files to incorporate latest instructions:
    - `Main.md` file.
    - `branch-guide.md` file.
    - `CONTRIBUTING.md` file.
  - Contains following updates in `eShopOnContainers` wiki:
    - Updates the System-requirements page.
    - Docker-compose-deployment-files
    - Updates the images Visual-Studio-2017-environment
    - Updates the steps for Windows-setup
    - Updates the steps for Mac-setup
    - Moves the Xamarin-setup specific content to 
    - Architecture
    - gRPC
    - Using-HealthChecks
    - Azure-Key-Vault
    - Unit-and-integration-testing
    - Updates `Frequent-errors` page.
    - Includes Github Actions related documentations.
    
  #### Bug Fixes

  - Fixes `SameSite` cookie policy.
  - Fixes `ContentPage.ToolbarItems` in a `TabbedPage`
  - Fixes persistency for `ISubscriptionClient`
  - Fixes `ordering-signalrhub` workflow badge image
  - Fixes `WebSPA` pager display.
  - Fixes `WebSPA` catalog filter.
  - Fixes `mobileshoppingagg` address in mobileshopping `envoy.yaml`
  - Fixes serialization in `EventBusServiceBus.cs` class.
  - Fixes to use existing cosumerChannel in `EventRabbitMQ` queue.
  - Fixes few typos in fields name, documentation and scripts.
  - Fixes `deploy-all.sh` deployment script file

  ### Contributions

  We’d like to acknowledge and thank the following community members for their valuable contributions!

  @dsrodenas, @vishipayyallore, @borjasanes, @mvelosop, @hetal-kapadia, @william-keller, @Sreenivas-Kalluru, @alecola, @f1nzer, @Marusyk, @n-stefan, @mohamed-seada-1994, @colindembovsky, @sanderobdeijn, @deckerbd, @michaelgregson, @kaypee90, @alan0428a, @GitHubPang, @ryanceleslie, @oliviergaumond, @zedy-wj


## 2021-01-21 - Release 3.1.1

The latest [3.1.1 release](https://github.com/dotnet-architecture/eShopOnContainers/releases/tag/3.1.1) contains multiple changes and bug fixes:

### Highlights

1. Refactors code for few of the code modules
2. Fixes few important bugs

### Details

  #### Changes
    
  - Removed unused using and refactored spacing in many classes.
  - Removes `dotnet.myget.org` NuGet package feed dependency from the `NuGet.config` file.
  - Removes unnecessary  await from `Ordering.BackgroundTasks`
  - Updates packages in `package-lock.json` file in WebSPA project. 

  #### Bug Fixes
    
  - Fixes total decimal place and drop-down menu hover issue in Web MVC app. 
  - Fixes Ordering Functional Test case.

### Contributions

We’d like to acknowledge and thank the following community members for their valuable contributions !

@vishipayyallore, @william-keller, @hetal-kapadia , @InstanceFactory


## 2020-12-14 - Release 3.1.0

The latest [3.1.0 release](https://github.com/dotnet-architecture/eShopOnContainers/releases/tag/3.1.0) contains multiple changes and bug fixes:

### Highlights

1. Updates to .NET Core 3.1.0
2. Refactors code for few of the code modules
3. Fixes few important bugs

### Details

  #### Changes

  - Removes unused using from different classes.
  - Updates Readme with relevant information.  
  - Updates app manifest to support helm 3.x+ and Kubernetes version 1.16.x+
  - Updates different npm package versions in WebSPA
  - Changes docker host DNS default value to `host.docker.internal`
  - Changes `OpenIdConnect` string literal to `OpenIdConnectDefaults.AuthenticationScheme`
  - Changes ReadAllBytes to ReadAllBytesAsync in PicController
  - Updates Solution file 

  #### Bug Fixes

  - Fixes SignalR 401 Unauthorized error.
  - Fixes different `typo` in the main `Readme` file.
  - Fixes Firewall specific rule check in the script.
  - Fixes disposing of direct instantiated objects in calalog service
  - Fixes typo in `Readme` and `appsetting.json` file. 
  - Fixes unit test cases
  - Fixes parameter error in multiarch job
  - Fixes WebSPA build error after updating sha hashes in `packages-lock.json`
  - Fixes missing claimsType for load testing
  - Fixes PurchaseUrl port in WebSPA `appsettings.json`
  - Fixes spelling mistake in code comment.
  - Fixes k8s manifest deployment error `invalid type for io.k8s.api.core.v1.ConfigMap.data` from macOS environment.

### Contributions

We’d like to acknowledge and thank the following community members for their valuable contributions !

@vishipayyallore, @markharwood101, @hfz-r, @smholvoet, @InstanceFactory, @EdmondShtogu, @nsedoud, @H3RSKO, @MajidAliKhanQuaid, @fjvela, @jeremiahflaga, @zakaria-c, @wojciechrak , @anjoy8, @m-knet, @n-stefan, @synercoder , @Rosenberg96

## 2019-11-26 - Release 3.0.0

The latest [3.0.0 release](https://github.com/dotnet-architecture/eShopOnContainers/releases/tag/3.0.0) contains a **LOT** of changes and new features:

### Highlights - 3.0.0

1. Update to .NET Core 3.0
2. Use of gRPC for microservice-to-microservice communication
3. Use of Envoy Proxy for BFF
4. Repo cleanup
5. Initial Service Mesh support
6. Revise deployment scenarios

### Details - 3.0.0

- Migrate solution from ASP.NET Core 2.2 to 3.0 and update all projects to use the latest .NET Core 3.0 templates.

- Implement the new .NET Core 3.0 WorkerService in Ordering.API and other background processes.

- Improve Ordering.API
  - Group order items
  - apply discounts from Marketing.API

- Handle two deployment scenarios
  - Basic deployment, better for learning:
    - [Visual Studio (F5 experience)](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Windows-setup#optional---use-visual-studio)
    - [Docker compose on windows](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Windows-setup)
    - [Docker compose on macOS](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Mac-setup)
    - [Local Kubernetes](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Deploy-to-Local-Kubernetes)

  - Advanced deployment, complex but more real-life:
    - [Deploy to AKS with a Service Mesh for resiliency](https://github.com/dotnet-architecture/eShopOnContainers/wiki/Deploy-to-Azure-Kubernetes-Service-(AKS))
      - Sidecar implementation with Envoy/Linkerd
      - Improved API Gateway and resilience
      - gRPC for microservice-to-microservice communications
      - Dev Spaces support (only for containers that don't use gRPC)

#### New folder structure

The repo also has a new, simpler, folder structure, as shown in the following image:

![](images/Release-Notes/new-folder-structure.png)

In the above image you can see that the first folder level contains, basically:

- **build**: Scripts for building Docker images.
- **deploy**: Scripts for deployment.
- **src**: All source projects, including tests.
  - **ApiGateways**: Envoy configuration and Aggregators source code.
  - **BuildBlocks**: Common components used by several projects.
  - **Mobile**: Mobile apps projects.
  - **Services**: Backend for all services. Including unit and functional tests for some projects.
    - Basket
    - Catalog
    - Identity
    - Location
    - Marketing
    - Ordering
    - Payment
    - Webhooks
  - **Tests**: General functional application tests.
  - **test-results**: Test results
  - **Web**: Web applications

#### Retired deployment scenarios

- Service Fabric & Service Fabric Mesh.
- Kubernetes using YAML (only Helm charts are supported)
- CLI scripts for build and push (docker-compose and docker multi-stage are used)
