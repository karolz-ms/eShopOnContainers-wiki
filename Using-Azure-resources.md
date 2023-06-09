This page contains details about configuring eShopOnContainers to use Azure resources.

See the page [Deploying Azure resources](Deploying-Azure-resources) For information about deploying the resources needed.

- [Azure Redis Cache service](#azure-redis-cache-service)
- [Azure Service Bus service](#azure-service-bus-service)
- [Azure Storage Account service](#azure-storage-account-service)
  - [Check status of Azure Storage Account with Health Checks](#check-status-of-azure-storage-account-with-health-checks)
- [Azure SQL Database](#azure-sql-database)
- [Azure Cosmos DB](#azure-cosmos-db)
- [Azure Functions](#azure-functions)

**Note**: It is very important to disable any ESHOP_AZURE variables from .env file when the local storage or container services is set. Remember you can disable any variable from .env file putting '#' character before the variable declaration and you can run and test separately any Azure services.

With the steps explained in the next section, you will be able to run the application with Azure Redis Cache instead of the container of Redis service.

# Azure Redis Cache service

To enable the Redis Cache of Azure in eShop it is necessary to have previously configured the Azure Redis service through ARM file or manually through Azure portal. You can use the [ARM files](deploy/az/redis/readme.md) already created in eShop. Once the Redis Cache service is created, it is necessary to get the Primary connection string from information service in the Azure portal and modify the port value from 6380 to 6379 and the ssl value from True to False to establish a without ssl connection with the cache server. This Primary connection must be declared on .env file located in the solution root folder with `ESHOP_AZURE_REDIS_BASKET_DB` variable name.

For example:
>ESHOP_AZURE_REDIS_BASKET_DB=yourredisservice.redis.cache.windows.net:6379,password=yourredisservicepassword,ssl=False,abortConnect=False

With the steps explained in the next section, you will be able to run the application with Azure Service Bus instead of the container of RabbitMQ service.

# Azure Service Bus service

To enable the service bus of Azure in eShop solution it is necessary having created previously the service bus service through ARM file or manually through Azure portal. You can use the [ARM files](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/deploy/azure/az/servicebus/readme.md) already created in eShop. Finally, it is necessary to get the Shared access policy named "Root" (if you generated the service through ARM file) from eshop_event_bus topic. This policy must be declared on .env file located in the solution root folder with `ESHOP_AZURE_SERVICE_BUS` name.

For example:
>ESHOP_AZURE_SERVICE_BUS=Endpoint=sb://yourservicebusservice.servicebus.windows.net/;SharedAccessKeyName=Root;SharedAccessKey=yourtopicpolicykey=;EntityPath=eshop_event_bus

Once the service bus service is created, it is necessary to set to true the "AzureServiceBusEnabled" environment variable from the `appsettings.json` file on Catalog.API, Ordering.API, Basket.API, Payment.API, GracePeriodManager, Marketing.API and Locations.API.

With the steps explained in the next section, you will be able to run the application with Azure Storage Account instead of the local container storage.

# Azure Storage Account service

To enable Azure storage of Azure in the eShopOnContainers solution it is necessary having created previously the storage service through ARM file or manually through the Azure portal. You can use the ARM files find under **deploy/az/storage** folder already created in eShop. Once the storage account is created, it is very important to create a new container(blob kind) and upload the solution catalog pics files before to continue. Later, it is necessary to set to true the "AzureStorageEnabled" environment variable from `appsettings.json` in `Catalog.API`, `docker-compose.override.yml` and `docker-compose.prod.yml`. Finally, it is necessary to get the container endpoint url from information service in the Azure portal. This url must be declared on .env file located in the solution root folder with `ESHOP_AZURE_STORAGE_CATALOG` for the Catalog.API content.

Do not forget to put a slash character '/' at the end of the URL.

For example:
>ESHOP_AZURE_STORAGE_CATALOG=https://yourcatalogstorageaccountservice.blob.core.windows.net/yourcontainername/

To use Azure Storage, you can refer to the instructions answered in the [comment](https://github.com/dotnet-architecture/eShopOnContainers/issues/2047#issuecomment-1437084043) for one of the issues.

## Check the status of the Azure Storage Account with Health Checks

It is possible to add a status check for the Azure Storage Account inside the Catalog Web Status. In case the status check is enabled, for the Catalog section in the WebStatus page, Azure Storage will be checked as one of the dependencies for theses APIs. To enable this check add the account name and key to the .env file for your account.

For example:
>ESHOP_AZURE_STORAGE_CATALOG_NAME=storageaccountname
>ESHOP_AZURE_STORAGE_CATALOG_KEY=storageaccountkey

With the steps explained in the next section, you will be able to run the application with Azure SQL Database instead of local storage.

# Azure SQL Database

To enable Azure SQL Database in eShop is required to have a Azure SQL with the databases for Ordering.API, Identity.API, and Catalaog.API. You can use the [ARM files](deploy/az/sql/readme.md) already created in this project or do it manually. Once the databases are created, it is necessary to get the connection string for each service and set the corresponding variable in the .env file.

For example:
>ESHOP_AZURE_CATALOG_DB=catalogazureconnectionstring
>ESHOP_AZURE_IDENTITY_DB=identityazureconnectionstring
>ESHOP_AZURE_ORDERING_DB=orderingazureconnectionstring
>ESHOP_AZURE_MARKETING_DB=marketingazureconnectionstring

With the steps explained in the next section, you will be able to run the application with Azure Cosmos DB Database instead of local storage.

# Azure Cosmos DB

To enable Azure Cosmos DB in eShop is required to have the connection string. If you do not have an Azure Cosmos DB created you can use the ARM files under **deploy/az/cosmos** folder available in eShop or do it manually. Once the connection string is availabe it is necessary to add it in the .env file in the `ESHOP_AZURE_COSMOSDB`variable.

For example:
>ESHOP_AZURE_COSMOSDB=cosmosconnectionstring

# Azure Functions

To enable the Azure Functions in eShop you can add the URI where the functions have been deployed. You can use the ARM files under **deploy/az/azurefunctions** to create the resources in Azure. Once created and available, it is necessary to add to the .env file the `ESHOP_AZUREFUNC_CAMPAIGN_DETAILS_URI` variable.

For example:
 >ESHOP_AZUREFUNC_CAMPAIGN_DETAILS_URI=https://marketing-functions.azurewebsites.net/api/MarketingDetailsHttpTrigger?code=AzureFunctioncode
 
See Azure Functions deployment Files and Readme for more details [ARM files](deploy/az/azurefunctions/readme.md)
