# Containerization on Azure

## Comparison of Services

> This is the first paragraph of the document.

| Azure Service  | Second Header | Third Header |
| ------------   | ------------- | ------------ |
| Az App Service | Content Cell  | Content Cell |
| Az Function    | Content Cell  | Content Cell |

### [1] Azure App Service

[Azure App Service](https://azure.microsoft.com/nl-nl/services/app-service/) is one of the oldest services available on Azure. It offers the possibility of running a single application in a fully-managed environment. Microsoft has added the option of deploying a single Docker container to an App Service, making it one of the simplest options available when you want to run a single containerized application. Scaling options are available via the App Service Plan, which can scale out to multiple instances of the same App Service. Container images can be retrieved from ACR (Azure Container Registry), Docker Hub, or a private repository.

**Complexity & Ease of use:** Deploying a container to an App Service is very straightforward, and other than being able to create a Docker file, no further knowledge of container management is required.

**Capability:** An App Service provides nothing more than the ability to run a single container. Kubernetes services and other management options are not available.

**Cost:** App Services run on an App Service Plan, which provides dedicated, reserved computing resources. Pricing is based on the reserved resources, not on usage.

**When to use:** Long-running applications that require single containers (webapps, API’’s). Continuous workloads to optimally use monthly pricing. App Service is also a good choice for those just starting out with their first containerized application.

### [2] Azure Functions

Like the App Service, [Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/) are a well-established Azure offering, enabling running lightweight event-driven applications. It is possible to deploy Functions inside a container, using the Azure-Functions base-image. A container based on this image can be hosted on other container platforms (AKS for example), but can also be hosted inside an Azure Functions runtime, in which case it behaves like any other Function.

**Complexity & Ease of use:** Deployment is as easy as deploying an App Service. Running a Function in a container requires usage of the Azure-Functions base-image, which can complicate the creation of your custom image. As with an App Service, no further knowledge of container management is required.

**Capability:** Provides the ability to run a single container containing a Function.

**Cost:** Running a Functions container inside a Function runtime requires the use of an App Service Plan, making the costs comparable to an Azure App Service. It is not possible to use the usage-based Consumption Plan when using a container for a Function.
When to use: When developing Azure Functions that will probably be hosted on containers in other environments at a later time. As a container-based Functions runtime cannot be combined with the Consumption plan (pay-per-use), a key advantage of the Functions runtime is lost.

### [3] Azure Container instances (ACI)

The [Azure Container Instance (ACI)](https://azure.microsoft.com/nl-nl/services/container-instances/) is arguably the simplest way of getting a single container, or a container group (pod) up and running on Azure. Where the App Service and Functions runtime are focused on certain types of workloads, the Container Instance can be used for all kinds of different applications. The ACI is a lightweight container solution, allowing fast deployments and easy cleanup.

**Complexity & Ease of use:** Deployment to ACI is relatively simple. Using multiple containers in the same group requires some knowledge of ARM templates, and of container-container interactions.
**Capability:** ACI provides a very basic container runtime. This makes it very fast and cost-effective, but comes at the cost of absence of features like scaling and load-balancing.

**Cost:** ACI containers are billed by usage (CPU cores, memory), so they are the most cost-effective solution in scenarios where short-lived containers are created and removed often.

**When to use:** Processing of batch jobs, build jobs, automation tasks; short-duration, simple applications.

### [4] Azure Kubernetes Service (AKS)

With Kubernetes having become the default container management tooling in recent years, it is no surprise that Microsoft has added a Kubernetes-based offering to their portfolio of Container runtimes on Azure. The [Azure Kubernetes Service](https://azure.microsoft.com/en-us/services/kubernetes-service/) offers the complete Kubernetes experience, and adds easy integration with Azure Files and Disks, Log Analytics and Azure Active Directory. AKS provides a managed solution; you do not have to think about the underlying machines that your cluster is running on. However, the responsibility for administering your cluster lies with you in its entirety. Direct interaction with the Kubernetes API is both possible and required for cluster administration.

**Complexity & Ease of use:** Administering Kubernetes requires specialized knowledge, and many actions can only be performed via the command line interface. AKS provides the most freedom in configuration and usage, but this results in possibly the most intensive administrative workload of all the alternatives discussed in this article.

**Capability:** AKS provides a full Kubernetes cluster, enhanced by integrations with several other Azure services. Extensive networking options are also available.

**Cost:** For hosting a single container, AKS is a more expensive option compared to the previously discussed alternatives. When hosting more complex landscapes, cost can be managed better by using the features provided by Kubernetes (eg. scaling).

**When to use:** Complex landscapes requiring container orchestration, scaling, advanced configuration, or running specialized workloads.

### [5] Azure RedHat OpenShift (ARO)

RedHat OpenShift is easily the most feature-rich and extensive container platform available at this time. It is a complete platform offering container solutions, integration, security, and monitoring. OpenShift is available on multiple platforms, with the Azure version being named [Azure RedHat OpenShift or ARO](https://azure.microsoft.com/nl-nl/services/openshift/).

**Complexity & Ease of use:** With OpenShift, RedHat has aimed to create a more user-friendly experience when compared to working with Kubernetes.

**Capability:** OpenShift is the most capable and versatile platform discussed in this article. Its ability to easily integrate both on-premise and cloud resources, and connect IaaS virtual machines to PaaS containers make it uniquely suited for migration workloads

**Cost:** Due to the extra features provided on top of Kubernetes, and licensing costs, ARO is one of the more expensive options for hosting a single container. On an enterprise level, the management-, security-, and integration-features of ARO can provide significant reduction in administrative costs compared to other alternatives. RedHat also provides enterprise level support; though Microsoft provides technical support on all Azure offerings, they do not provide in-depth support for Kubernetes.

**When to use:** OpenShift provides a suite of various services and capabilities on top of Kubernetes ranging from security options to ci/cd services, while abstracting away the advanced configuration work. This makes OpenShift the best choice for large enterprise environments. OpenShift is also especially suited for cloud migrations and complex modernization journeys.

### [6] Azure Container Apps (ACA)

[Azure Container Apps](https://azure.microsoft.com/en-us/services/container-apps/) is the newest container runtime offered by Microsoft. It provides an environment using Kubernetes, enhanced with technologies like [Dapr](https://dapr.io/) and [KEDA](https://keda.sh/), but does not provide direct access to the Kubernetes API. Azure Container Apps is targeted specifically towards microservices-based applications, but can also be used for general-purpose applications. ACA can be seen as a PaaS layer on top of Kubernetes.

**Complexity & Ease of use:** ACA is a Microsoft-created package of Kubernetes with a selection of often-used additions. Microsoft takes care of the integration and correct operation of these additions, at the cost of some of the more advanced Kubernetes features and access to the Kubernetes API. This reduction of available features also reduces your administrative workload compared to AKS.

**Capability:** Features like scaling, integration of identity providers, ingress control and the addition of Dapr provide a remarkably complete environment for those looking for more than ACI offers, but are not willing to deal with managing Kubernetes. At the moment of writing, ACA only accepts Linux containers. This means it is not possible to host .NET Framework applications in ACA.

**Cost:** ACA is billed by usage. Idle containers are billed at a lower rate. This makes ACA a cost-effective option for applications with varying levels of load.

**When to use:** Microservices applications, processing jobs. ACA can be a good fit if the feature set of ACI is not adequate, but the full functionality of Kubernetes is not needed. ACA can also be a good fit if your knowledge of container orchestration is limited.

## Recap

> There are many ways to run containerized applications on [Azure](https://azure.microsoft.com/nl-nl/). Choosing the right environment for your containerized application can have a big impact on administrative burden, cost, and performance. It is therefore important to take a good look at the type of applications you are considering to containerize, and compare the pros and cons of these environments. With this article, we have tried to provide you with the basis to make an informed decision on selecting the Azure container environment that is right for you.