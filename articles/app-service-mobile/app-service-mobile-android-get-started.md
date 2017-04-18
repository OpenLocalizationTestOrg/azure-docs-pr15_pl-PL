<properties
    pageTitle="Tworzenie aplikacji programu Android w aplikacji dla urządzeń przenośnych Azure aplikacji usługi | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby rozpocząć pracę z przy użyciu pomocą Azure aplikacji dla urządzeń przenośnych dla systemu Android rozwoju"
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

#<a name="create-an-android-app"></a>Tworzenie aplikacji programu Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Omówienie

Tym samouczku pokazano, jak dodawać usługi opartej na chmurze wewnętrznej bazy danych do Android aplikacji dla urządzeń przenośnych za pomocą aplikacji dla urządzeń przenośnych Azure wewnętrznej bazy danych.  Utworzysz nowy wewnętrznej bazy danych aplikacji dla urządzeń przenośnych jak prosta _Lista zadania_ Android aplikacji, w którym są przechowywane dane aplikacji platformy Azure.

Ten samouczek jest wymagane w przypadku wszystkich innych Android samouczki dotyczące funkcji aplikacji Mobile przy użyciu Azure aplikacji usługi.

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, są potrzebne następujące elementy:

* [Android narzędzi dla deweloperów](https://developer.android.com/sdk/index.html), który zawiera Android Studio zintegrowanego środowiska projektowego i najnowszych platformy systemu Android.
* Azure Mobile Android SDK, która odwołuje się automatycznie w ramach projektu Szybki Start do pobrania.
* [Aktywne konto Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-new-azure-mobile-app-backend"></a>Tworzenie nowej aplikacji dla urządzeń przenośnych Azure wewnętrznej bazie danych

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Konfigurowanie programu project server

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-android-app"></a>Pobierz i uruchom aplikację systemu Android

[AZURE.INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
