<properties
    pageTitle="Tworzenie aplikacji Cordova w aplikacji dla urządzeń przenośnych Azure aplikacji usługi | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby rozpocząć pracę z za pomocą aplikacji dla urządzeń przenośnych Azure pomocą rozwoju Apache Cordova"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""
    tags=""
    keywords="cordova, klienta przenośnego, javascript," />

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-an-apache-cordova-app"></a>Tworzenie Apache Cordova aplikacji

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Omówienie

Tym samouczku pokazano, jak dodawać usługi opartej na chmurze wewnętrznej bazy danych do aplikacji dla urządzeń przenośnych Apache Cordova za pomocą aplikacji dla urządzeń przenośnych Azure wewnętrznej bazy danych.  Utworzysz nowy wewnętrznej bazy danych aplikacji dla urządzeń przenośnych jak prosta _Lista zadania_ aplikacji Apache Cordova, w którym są przechowywane dane aplikacji platformy Azure.

Ten samouczek jest wymagane w przypadku wszystkich innych samouczków Apache Cordova o korzystaniu z funkcji aplikacji Mobile w Azure aplikacji usługi.

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, są potrzebne następujące elementy:

* Komputer z [programu Visual Studio społeczności 2015] lub nowszego.
* [Visual Studio Tools for Apache Cordova].
* [Aktywne konto Azure](https://azure.microsoft.com/pricing/free-trial/).

Możesz również pominąć Visual Studio i bezpośrednio za pomocą wiersza polecenia Apache Cordova.  Jest to przydatne w przypadku wykonywania samouczka na komputerze Mac.  Ten samouczek nie obejmuje kompilowania aplikacje klienckie Apache Cordova wiersza polecenia.

## <a name="create-a-new-azure-mobile-app-backend"></a>Tworzenie nowej aplikacji dla urządzeń przenośnych Azure wewnętrznej bazie danych

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Obejrzyj klip wideo przedstawiający podobne kroki](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>Konfigurowanie programu project server

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Pobierz i uruchom aplikację Apache Cordova

[AZURE.INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Następne kroki

Teraz, gdy wykonaniu tego samouczka szybki start przenieść do jednego z następujące samouczki:

* [Dodawanie uwierzytelniania] do swojego Apache Cordova aplikacji.
* [Dodawanie powiadomienia wypychane] do swojego Apache Cordova aplikacji.

Dowiedz się więcej na temat podstawowych pojęć usłudze Azure aplikacji.

* [Uwierzytelnianie]
* [Powiadomienia wypychane]

Dowiedz się, jak używać SDK.

* [Apache Cordova SDK]
* [Serwer programu ASP.NET SDK]
* [Serwer node.js SDK]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Społeczność programu Visual Studio 2015 r.]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Dodawanie uwierzytelniania]: app-service-mobile-cordova-get-started-users.md
[Dodawanie powiadomienia wypychane]: app-service-mobile-cordova-get-started-push.md
[Uwierzytelnianie]: app-service-mobile-auth.md
[Powiadomienia wypychane]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[Serwer programu ASP.NET SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Serwer node.js SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
