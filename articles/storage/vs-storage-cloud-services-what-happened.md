<properties
    pageTitle="Co się stało z chmury usługi projektu? | Microsoft Azure | Usługi połączone programu Visual Studio"
    description="W tym artykule opisano, co się dzieje w chmurze projektu usług po łączenie się z kontem Azure miejsca do magazynowania przy użyciu programu Visual Studio połączenia usług"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Co się stało z projekcie cloud services (usługi połączone Visual Studio magazyn Azure)?

## <a name="references-added"></a>Odwołania dodane

Pakiet NuGet miejsca do magazynowania Azure został dodany do projektu programu Visual Studio.  
Ten pakiet zawiera następujące informacje .NET:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.Configuration**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **Dane systemowe**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Parametry połączenia dla magazyn Azure dodany
Elementy zostały utworzone przy użyciu parametrów połączenia i klucz konta wybranego miejsca do magazynowania. Zmiany wprowadzono następujące pliki:

- **ServiceDefinition.csdef**
- **ServiceConfiguration.Cloud.cscfg**
- **ServiceConfiguration.Local.cscfg**
