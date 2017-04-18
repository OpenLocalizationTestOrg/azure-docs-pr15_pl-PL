<properties 
    pageTitle="Wprowadzenie do aplikacji usługi w systemie Linux | Microsoft Azure" 
    description="Informacje na temat aplikacji usługi w systemie Linux." 
    keywords="Usługa Azure aplikacji, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="introduction-to-app-service-on-linux"></a>Wprowadzenie do aplikacji usługi w systemie Linux
Usługa aplikacji w systemie Linux jest obecnie w podglądzie publicznej i obsługuje uruchomione aplikacje sieci web graficznych w systemie Linux. 

## <a name="overview"></a>Omówienie ##
Klienci umożliwia aplikacji usługi w systemie Linux aplikacji sieci web hosta macierzyste na Linux oraz dla stosy obsługiwanej aplikacji. W poniższej sekcji funkcje lista stosy aplikacji obecnie obsługiwane.

## <a name="features"></a>Funkcje ##
Usługa aplikacji w systemie Linux obsługuje obecnie następujące stosy aplikacji

- Node.js
- PHP

Wdrażania ich za pomocą aplikacji

- FTP.
- Lokalne cyfra.
- GitHub lub BitBucket.

Aplikacja skalowania


- Klientów można skalować ich aplikacji sieci web i rozwijaniu zmieniając warstwy w ich Planowanie aplikacji usługi. 
- Klientów można skalować ich aplikacji się i uruchomić ich aplikacji u wielu wystąpień w obrębie ich SKU.

Dla Kudu niektóre podstawowe funkcje będą działać

- Środowisko.
- Wdrożenia.
- Podstawowe konsoli.

## <a name="limitations"></a>Ograniczenia ##

Portal Azure zarządzania będzie tylko wyświetlanie obecnie obsługiwane funkcje dla aplikacji usługi w systemie Linux i ukrycie pozostałych. Jako nasz zespół Włączanie większej liczby funkcji firma Microsoft będzie Zachowaj to zastanowienie się nad zadaniem do portalu zarządzania. Niektóre funkcje, takie jak integracji VNET i AAD / uwierzytelniania innych firm lub Kudu witryny rozszerzenia obecnie działa. Ale uzyskując tych działa firma Microsoft zaktualizuje naszą dokumentację i blogu o zmianach.

W tym podglądzie publicznej jest obecnie dostępny tylko w następujących regionach

-   US Zachód.
-   Europa Zachodnia.
-   Kraje Azji Zachód.

Aplikacji sieci Web na Linux jest obsługiwane tylko w dedykowanej plany usługi aplikacji i nie ma warstwa wolny lub udostępnione. Plany usługi aplikacji dla zwykłego i Linux oraz aplikacje sieci web są także wzajemnie wykluczających się, więc nie można utworzyć aplikację sieci web Linux w planie usługi aplikacji innych niż Linux.

W grupie zasobów, który nie zawiera aplikacje sieci web nie Linux w tym samym regionie należy utworzyć aplikacji sieci Web na Linux.

Z powodu braku nakładające się odtwarzanie aplikacji sieci web klienci spodziewać, że masz ponowne uruchomienie małych przestoje w przypadku aplikacji sieci web. 

## <a name="next-steps"></a>Następne kroki ##

Wykonaj poniższe łącza, aby rozpocząć pracę z usługą aplikacji w systemie Linux. Opublikuj [nasze forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview)w witrynie pytań i problemów.

* [Tworzenie aplikacji sieci Web w aplikacji usługi w systemie Linux](./app-service-linux-how-to-create-a-web-app.md)
* [Przy użyciu konfiguracji PM2 dla Node.js w aplikacjach sieci Web w systemie Linux](./app-service-linux-using-nodejs-pm2.md)

