<properties
    pageTitle="Co się stało z projektu programu ASP.NET 5 (Visual Studio połączony usług) | Magazyn Microsoft Azure"
    description="W tym artykule opisano, co się dzieje, gdy połączenie z kontem Azure miejsca do magazynowania w projekcie programu Visual Studio ASP.NET 5 przy użyciu programu Visual Studio połączenia usług"
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

# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Co się stało z projektu programu ASP.NET 5 (Visual Studio magazyn Azure połączony services)?

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

Ponadto pakiet NuGet **Microsoft.Framework.Configuration.Json** został dodany.

## <a name="connection-string-for-azure-storage-added"></a>Parametry połączenia dla magazyn Azure dodany
W pliku config.json projektu element został utworzony przy użyciu parametrów połączenia i klucz konta wybranego miejsca do magazynowania.

Aby uzyskać więcej informacji zobacz [ASP.NET 5](http://www.asp.net/vnext).
