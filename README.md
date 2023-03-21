az login
 az account set --subscription c3d8fe47-5458-4f04-ab24-8dc3367ebf01
az group create --name funcprofiler-rg --location eastus
 az acr create --resource-group funcprofiler-rg --name funcprofilerregistry --sku Basic
az acr login --name funcprofilerregistry
docker login
Docker build -t funcprofilerimg .
docker tag funcprofilerimg funcprofilerregistry.azurecr.io/funcprofilerimg
 docker push funcprofilerregistry.azurecr.io/funcprofilerimg

az storage account create --name funcprofilerstg --location eastus --resource-group funcprofiler-rg --sku Standard_LRS

az functionapp plan create --resource-group funcprofiler-rg --name myPremiumPlan --location eastus --number-of-workers 1 --sku EP1 --is-linux
'az acr update -n funcprofilerregistry --admin-enabled true
az functionapp create --name funcprofilerdotnetlinux --storage-account funcprofilerstg --resource-group funcprofiler-rg --plan myPremiumPlan --deployment-container-image-name funcprofilerregistry.azurecr.io/funcprofilerimg:latest


# Profiling a .NET6 app running in a linux container with dotnet-trace, dotnet-dump, dotnet-counters, dotnet-gcdump and Visual Studio

This repository contains a demo application. It is a .NET 6 API with 3 endpoints:

- /blocking-threads endpoint.
- /high-cpu endpoint.
- /memory-leak endpoint.

Each endpoint contains a different performance issue.

This repository is used to demonstrate how to profile an app using  the .NET CLI diagnostic tools (dotnet-dump, dotnet-trace, dotnet-counters, dotnet-gcdump) and Visual Studio.   

More info about it on my blog post:
- https://www.mytechramblings.com/posts/profiling-a-net-app-with-dotnet-cli-diagnostic-tools/

