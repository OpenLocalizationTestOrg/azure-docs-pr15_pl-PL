<properties
    pageTitle="Tworzenie aplikacji programu iOS na telefon komórkowy Azure aplikacji usługi | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby rozpocząć pracę z przy użyciu pomocą Azure aplikacji dla urządzeń przenośnych dla systemu iOS rozwoju w celu C lub Swift"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

#<a name="create-an-ios-app"></a>Tworzenie aplikacji programu systemu iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Omówienie

Ten samouczek pokazano, jak dodać [Aplikacje Mobile Azure](app-service-mobile-value-prop.md), usługi w chmurze wewnętrznej bazy danych, do aplikacji w systemie iOS. Najpierw utworzymy nowych urządzeń przenośnych wewnętrznej bazie danych. Następnie użyjemy prostej aplikacji w systemie iOS _listy zadania_ do przechowywania danych w Azure.

Aby użyć tego samouczka, potrzebujesz Mac i [konto Azure](https://azure.microsoft.com/pricing/free-trial/)


## <a name="step-i-create-a-new-azure-mobile-app-backend"></a>Krok I: Tworzenie nowej aplikacji dla urządzeń przenośnych Azure wewnętrznej bazie danych

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="step-ii-configure-the-backend-project"></a>Krok II: Konfigurowanie projektu wewnętrznej bazy danych

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="step-iii-download-and-run-the-ios-app"></a>Krok III: Pobieranie i uruchamianie aplikacji w systemie iOS

[AZURE.INCLUDE [app-service-mobile-ios-run-app](../../includes/app-service-mobile-ios-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
