# Containerization on Azure

## Comparison of Services

> This is the first paragraph of the document.

| Azure Service  | Second Header | Third Header |
| ------------   | ------------- | ------------ |
| Az App Service | Content Cell  | Content Cell |
| Az Function    | Content Cell  | Content Cell |

### Azure App Service

[https://azure.microsoft.com/nl-nl/services/app-service/](Azure App Service) is one of the oldest services available on Azure. It offers the possibility of running a single application in a fully-managed environment. Microsoft has added the option of deploying a single Docker container to an App Service, making it one of the simplest options available when you want to run a single containerized application. Scaling options are available via the App Service Plan, which can scale out to multiple instances of the same App Service. Container images can be retrieved from ACR (Azure Container Registry), Docker Hub, or a private repository.