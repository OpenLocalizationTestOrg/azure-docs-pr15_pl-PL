<properties
    pageTitle="Rozpoczynanie pracy z zaangażowania Mobile Azure dla aplikacji sieci Web | Microsoft Azure"
    description="Dowiedz się, jak zaangażowania Mobile Azure za pomocą analizy i wypychanych powiadomień dla aplikacji sieci Web."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="js"
    ms.topic="hero-article"
    ms.date="06/01/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Rozpoczynanie pracy z zaangażowania Mobile Azure dla aplikacji sieci Web

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

W tym temacie pokazano, jak za pomocą zaangażowania Mobile Azure opis do użycia aplikacji sieci Web.

Ten samouczek ma następujące wymagania:

+ Visual Studio 2015 lub dowolnego innego edytora
+ [Zestaw SDK w sieci Web](http://aka.ms/P7b453) 

Zestaw SDK sieci Web jest w podglądzie tylko obsługuje analizy w momencie i nie obsługuje jeszcze wysyłania powiadomień wypychanych przeglądarki lub w aplikacji. 

> [AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).

##<a name="setup-mobile-engagement-for-your-web-app"></a>Konfigurowanie urządzeń przenośnych zaangażowania dla aplikacji sieci Web

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych

Ten samouczek przedstawia "podstawowe integracji," której ustawiono minimalne wymagane na potrzeby zbierania danych.

Podstawowe przeglądania zostanie utworzony przy użyciu programu Visual Studio w celu zademonstrowania integracji, ale postępuj zgodnie z instrukcjami z dowolnej aplikacji sieci web utworzona poza Visual Studio również. 

###<a name="create-a-new-web-app"></a>Tworzenie nowej aplikacji sieci Web

Następujące czynności przyjęto założenie stosowania Visual Studio 2015, jeśli czynności są podobne we wcześniejszych wersjach programu Visual Studio. 

1. Uruchom program Visual Studio, a na ekranie **Strona główna** wybierz **Nowy projekt**.

2. W oknie podręcznym, zaznacz **Web** -> **Aplikacji sieci Web programu ASP.Net**. Wypełnianie w aplikacji **nazwę**, **lokalizację** i **nazwę rozwiązania**, a następnie kliknij **przycisk OK**.

3. W podręcznym **Wybierz szablon** wybierz **pusty** w obszarze **Szablony 4,5 ASP.Net** , a następnie kliknij **przycisk OK**. 

Teraz został utworzony nowy pusty projekt Web App w którym możemy integracja SDK Web zaangażowania Mobile Azure.

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Łączenie aplikacji z zaangażowania Mobile wewnętrznej bazy danych

1. Tworzenie nowego folderu o nazwie **kodu javascript** w rozwiązaniu i Dodaj plik Web SDK JS **azure engagement.js** do niego. 

2. Dodawanie nowego pliku o nazwie **main.js** w tym folderze javascript z następujący kod. Upewnij się zaktualizować parametry połączenia. To `azureEngagement` obiekt będzie używany Aby uzyskać dostęp do sieci Web SDK metod. 

        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };

    ![Program Visual Studio z plikami js][1]

##<a name="enable-real-time-monitoring"></a>Włączanie monitorowania w czasie rzeczywistym

Aby uruchomić przesyłanie danych i zapewnienie, że użytkownicy są aktywne, co najmniej jedno działanie, musisz wysłać do zaangażowania Mobile wewnętrznej bazy danych. Działanie w kontekście aplikacji sieci web jest to strona sieci web. 

1. Tworzenie nowej strony o nazwie **home.html** w rozwiązaniu i ustawiania jej jako strony początkowej dla aplikacji sieci web. 
2. Obejmuje dwa JavaScript, które dodano wcześniej na tej stronie, dodając następujące wewnątrz znacznika treści. 

        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>

3. Aktualizowanie znacznik treści połączenie EngagementAgent w `startActivity` metody
        
        <body onload="engagement.agent.startActivity('Home')">

4. Poniżej opisano, jak będą wyglądały usługi **home.html**
        
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

##<a name="connect-app-with-real-time-monitoring"></a>Łączenie aplikacji z monitorowania w czasie rzeczywistym

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

![][2]

##<a name="extend-analytics"></a>Rozszerzanie analizy

W tym miejscu wszystkie metody są obecnie dostępne z SDK sieci Web, używanego do analizy:

1. Działania i strony:

        engagement.agent.startActivity(name);
        engagement.agent.endActivity();

2. Zdarzenia
        
        engagement.agent.sendEvent(name, extras);

3. Błędy

        engagement.agent.sendError(name, extras);

4. Zadania

        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

