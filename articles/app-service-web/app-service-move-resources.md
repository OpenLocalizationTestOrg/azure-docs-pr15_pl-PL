<properties
    pageTitle="Przenoszenie zasobów aplikacji sieci Web do innej grupy zasobów"
    description="W tym artykule opisano scenariusze, w których można przenosić aplikacji sieci Web i aplikacji usług z jednej grupy zasobów do innego."
    services="app-service"
    documentationCenter=""
    authors="ZainRizvi"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/04/2016"
    ms.author="zarizvi"/>
    
# <a name="supported-move-configurations"></a>Przenieś obsługiwanych konfiguracji

Możesz przenieść zasobów aplikacji sieci Web Azure za pomocą [Interfejsu Api usług ARM przenoszenie zasobów](../resource-group-move-resources.md).

Azure aplikacje sieci Web obsługuje obecnie w następujących scenariuszach Przenieś:

* Przenoszenie całą zawartość grupy zasobów (aplikacji sieci web, planów usług aplikacji i certyfikaty) do innej grupy zasobów 
    * Uwaga: Grupa zasobów miejsce docelowe nie może zawierać wszystkie zasoby Microsoft.Web w tym scenariuszu
* Przenoszenie aplikacji sieci web poszczególnych do różnych grupach zasobów, podczas nadal hostingu je w ich bieżącym planie usługi aplikacji (plan usług aplikacji pozostaje w grupie stary zasobów)
