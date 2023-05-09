## Deploying to a "production" environment

_IMPORTANT: This instructions section is in early draft state because the current version of eShopOnContainers has been tested mostly on plain Docker engine and just smoke tests on some orchestrators like AKS and Kubernetes._

However, since there are a few folks testing it in "production" environments, out of the dev PC machine (VS2019+Docker) but in a prod. environment like Azure or a regular Docker Host, here's some important information to consider, in addition to the [CLI setup procedure detailed for Windows](Windows-setup).

The "by default configuration" in the `docker-compose.override.yml` file is set with specific config so it makes it very straightforward to test the solution in a Windows PC with Visual Studio or the CLI, almost just F5 after the first configuration. But, for instance, it is using the "**host.docker.internal**" DNS name set up by default by all "Docker for Windows" installations, so it can be used by the Identity Container for the login page when being redirected from the client apps **without you having to change any specific external IP**, etc.

However, when deploying eShopOnContainers to other environments like a real Docker Host, or simply if you want to access the apps from remote applications, there are some settings for the Identity Service that need to be changed by using the config specified at a "PRODUCTION" docker-compose file:

  docker-compose.**prod**.yml  (**for "production environments"**).

That file is using the environment variables provided by the "**.env**" file, which basically has the local name and the "External" IP or DNS name to be used by the remote client apps.

## Steps

It's just a couple of basic steps to deploy to a simple test "production" environment.

### 1. Configure a valid "external" IP address

Basically, you'd need to change the IP (or DNS name) at the .env file with the IP or DNS name you have for your Docker host or orchestrator cluster. If it is a local machine using Docker for Windows, then, it'll be your real WiFi or Ethernet card IP:
<https://github.com/dotnet/eShopOnContainers/blob/master/.env>

The IP below should be swapped and use your real IP or DNS name, like 192.168.88.248 or your DNS name, etc. if testing from remote browsers or mobile devices.

ESHOP_EXTERNAL_DNS_NAME_OR_IP=localhost
ESHOP_PROD_EXTERNAL_DNS_NAME_OR_IP=10.121.122.92

### 2. Include docker-compose "prod" .yml files

For deploying with docker-compose, instead of doing a regular “docker-compose up” do:

```console
docker compose -f docker-compose.yml -f docker-compose.prod.yml up
```

So it uses the docker-compose.**prod**.yml file which uses the EXTERNAL IP or DNS name.
<https://github.com/dotnet/eShopOnContainers/blob/master/docker-compose.prod.yml>

## Additional resources

- [Windows setup](Windows-setup)
- [Docker-compose files](Docker-compose-deployment-files)
- [Using Azure resource](Using-Azure-resources)
