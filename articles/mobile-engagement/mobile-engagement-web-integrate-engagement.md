<properties
    pageTitle="Azure integracji SDK Web zaangażowania Mobile | Microsoft Azure"
    description="Najnowsze aktualizacje i procedury SDK Web zaangażowania Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="02/29/2016"
    ms.author="piyushjo" />

#<a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Integracja zaangażowania Mobile Azure w aplikacji sieci web

> [AZURE.SELECTOR]
- [Uniwersalny systemu Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Program Silverlight Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Procedury przedstawione w tym artykule opisano Najprostszym sposobem aktywowania analizy i monitorowania funkcji zaangażowania Mobile Azure w aplikacji sieci web.

Postępuj zgodnie z instrukcjami, aby aktywować raporty dziennika, które są potrzebne do obliczania statystyk wszystkich użytkowników, sesje, działania, awarii i technicals. Statystyki zależne od aplikacji, takich jak zdarzenia, błędów i zadania należy ręcznie uaktywnić raporty dziennika za pomocą interfejsu API zaangażowania Mobile Azure. Aby uzyskać więcej informacji Dowiedz się, [jak za pomocą zaawansowanych zaangażowania Mobile znakowania interfejsu API w aplikacji sieci web](mobile-engagement-web-use-engagement-api.md).

## <a name="introduction"></a>Wprowadzenie

[Pobierz Web Azure zaangażowania przenośnych SDK](http://aka.ms/P7b453).
SDK Web zaangażowania Mobile jest dołączona jako pojedynczy plik JavaScript azure-engagement.js, które mają zostać uwzględnione w każdej strony do witryny lub aplikacji sieci web.

> [AZURE.IMPORTANT] Przed uruchomieniem tego skryptu należy uruchomić skrypt lub kod wstawek, pisanym w celu skonfigurowania zaangażowania Mobile dla aplikacji.

## <a name="browser-compatibility"></a>Zgodność przeglądarek

SDK Web zaangażowania Mobile używa natywnych JSON kodowanie i dekodowanie oprócz żądania AJAX innej domeny (Polegaj na specyfikacji W3C CORS). Nie jest zgodna z następujących przeglądarek:

* Microsoft Edge 12 +
* Program Internet Explorer 10 lub jej nowszą
* Firefox 3.5 +
* Chrome 4
* Safari 6
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Konfigurowanie urządzeń przenośnych zaangażowania

Napisać skrypt, który tworzy globalny `azureEngagement` obiekt JavaScript, tak jak w poniższym przykładzie. Ponieważ witryny mogą mieć wielokrotności stron, w tym przykładzie założono, że ten skrypt znajduje się w każdej strony. W tym przykładzie ma nazwę obiektu JavaScript `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

`connectionString` Wartość dla aplikacji jest wyświetlana w portalu Azure.

> [AZURE.NOTE] `appVersionName`i `appVersionCode` są opcjonalne. Jednak zaleca się je skonfigurować tak, aby analizy można przetwarzać informacje o wersji.

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Dołączanie skryptów zaangażowania Mobile na stronach
Dodawanie skryptów zaangażowania Mobile do stron w jednym z następujących sposobów:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Lub to:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Alias

Po załadowaniu skryptu SDK Web zaangażowania Mobile tworzy alias **zaangażowania** , aby uzyskać dostęp do interfejsów API SDK. Za pomocą tego aliasu nie można określić konfigurację SDK. Ten alias jest używany jako odwołanie w dokumentacji.

Zwróć uwagę, że jeśli domyślny alias jest w konflikcie z innej zmiennej globalnej z Twojej strony, można zmienić go w konfiguracji następujący przed rozpoczęciem ładowania SDK Web zaangażowania Mobile:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Podstawowe raportowania

Podstawowe raportowania w zaangażowania Mobile obejmuje poziom sesji statystyki, takie jak statystyk dotyczących użytkowników, sesje, działania i awarii.

### <a name="session-tracking"></a>Śledzenie sesji

Do sesji zaangażowania Mobile jest podzielony na kolejność czynności, każda oznaczona nazwę.

Klasyczny witryny sieci Web zalecamy deklarować różne działania na każdej stronie witryny. Dla witryny sieci Web lub aplikacji sieci web w którym nie ulega zmianie bieżącej strony można śledzić aktywność skali mniejszy, takich jak na stronie.

W obu przypadkach, aby rozpocząć lub zmienianie bieżącej aktywności użytkownika, połączenie `engagement.agent.startActivity` funkcji. Na przykład:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

Serwer zaangażowania Mobile kończy się automatycznie, otwórz sesję w ciągu trzech minut po zamknięciu stronie aplikacji.

Ponadto można zakończyć sesję ręcznie, dzwoniąc `engagement.agent.endActivity`. Spowoduje to ustawienie bieżącej aktywności użytkownika w celu "Bezczynności."  Sesja zakończy później 10 sekund, chyba że nowe połączenie do `engagement.agent.startActivity` życiorysy sesji.

10-sekundowe opóźnienie można skonfigurować w obiekcie globalnej zaangażowania w następujący sposób:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [AZURE.NOTE] Nie można używać `engagement.agent.endActivity` w `onunload` zwrotnego, ponieważ nie można nawiązywać połączenia AJAX na tym etapie.

## <a name="advanced-reporting"></a>Zaawansowane raportowania

Opcjonalnie Jeśli chcesz zgłosić zdarzeń specyficzne dla aplikacji, błędów i zadania, należy korzystać z interfejsu API zaangażowania Mobile. Możesz uzyskać dostęp do interfejsu API zaangażowania Mobile za pośrednictwem `engagement.agent` obiektu.

Możesz uzyskać dostęp do wszystkich zaawansowanych funkcji w zaangażowania Mobile w interfejsie API zaangażowania Mobile. Interfejs API jest szczegółowe w artykule [jak za pomocą zaawansowanych zaangażowania Mobile znakowania interfejsu API w aplikacji sieci web](mobile-engagement-web-use-engagement-api.md).

## <a name="customize-the-urls-used-for-ajax-calls"></a>Dostosowywanie adresy URL używane na potrzeby połączeń AJAX

Możesz dostosować adresy URL używane SDK Web zaangażowania Mobile. Na przykład o ponowne zdefiniowanie adres URL dziennika (punkt końcowy SDK dla rejestrowania), można zmienić konfigurację w następujący sposób:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Jeśli adres URL funkcje zwracają ciąg, który zaczyna się od `/`, `//`, `http://`, lub `https://`, domyślnego schematu nie jest używane. Domyślnie `https://` schemat jest używana dla tych adresów URL. Jeśli chcesz dostosować domyślny schemat, zastępując konfiguracji, tak jak poniżej:

    window.azureEngagement = {
      ...
      urls: {
        ...      
        scheme: '//'
      }
    };
