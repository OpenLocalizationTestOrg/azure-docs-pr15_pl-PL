<properties
    pageTitle="Co się stało z projektu WebJob (Visual Studio magazyn Azure połączenie usługi)? | Microsoft Azure"
    description="W tym artykule opisano, co się stało w projekcie Azure WebJob po nawiązywanie połączenia z kontem miejsca do magazynowania przy użyciu programu Visual Studio połączenia usług"
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

# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Co się stało z projektu WebJob (Visual Studio magazyn Azure połączenie usługi)?

## <a name="references-added"></a>Odwołania dodane

Pakiet NuGet miejsca do magazynowania Azure zostały dodane do lub zaktualizowane w projekcie programu Visual Studio.  
Ten pakiet zawiera następujące informacje .NET:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.ConfigurationManager**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **Dane systemowe**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Parametry połączenia dla magazyn Azure dodany
W pliku App.config projektu wpisy **AzureWebJobsStorage** i **AzureWebJobsDashboard** zostały zaktualizowane przy użyciu parametrów połączenia konta wybranego miejsca do magazynowania i legendy.

Aby uzyskać więcej informacji zobacz [Azure WebJobs dokumentacji zasoby](http://go.microsoft.com/fwlink/?linkid=390226).
