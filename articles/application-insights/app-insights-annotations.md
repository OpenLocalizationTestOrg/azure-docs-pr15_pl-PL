<properties
    pageTitle="Zwolnij adnotacje dla aplikacji wniosków | Microsoft Azure"
    description="Dodawanie wdrożenia lub utworzyć znaczniki w wykresach Eksploratora metryki w aplikacji wnioski."
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="awills"/>

# <a name="release-annotations-in-application-insights"></a>Zwolnij adnotacji w aplikacji wniosków

Zwolnij adnotacje Pokaż wykresy [Eksploratora metryki](app-insights-metrics-explorer.md) miejsce, w którym wdrożono nową kompilację. Ułatwiają ich zobaczyć, czy zmiany zostały wpływu na wydajność usługi aplikacji. Mogą być automatycznie tworzone przez [program Visual Studio Team Services tworzenie systemu](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs)i możesz też [utworzyć je z programu PowerShell](#create-annotations-from-powershell).

![Przykład adnotacji za pomocą widoczne korelacji z czasem odpowiedzi serwera](./media/app-insights-annotations/00.png)

Adnotacje wersji są funkcją kompilacji opartej na chmurze i zwolnij usługi programu Visual Studio Team Services. 

## <a name="install-the-annotations-extension-one-time"></a>Zainstaluj rozszerzenie adnotacje (raz)

Aby mieć możliwość tworzenia adnotacji wersji, musisz zainstalować jedno z wielu dostępnych rozszerzeń zespołu usługi witryny Marketplace programu Visual Studio.

1. Zaloguj się do projektu [Programu Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) .
2. Visual Studio rynku, [Uzyskiwanie rozszerzenia wersji adnotacje](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations)i dodać je do swojego konta usługi zespołu.

![AT od góry prawej strony sieci web usług zespołu, otwórz Marketplace. Wybierz wizualne usługi zespołu, a następnie w obszarze kompilacji i wersji, wybierz pozycję Zobacz więcej.](./media/app-insights-annotations/10.png)

Wystarczy w tym celu raz dla Twojego konta programu Visual Studio Team Services. Teraz można skonfigurować adnotacje wersji projektu na Twoim koncie. 

## <a name="get-an-api-key-from-application-insights"></a>Uzyskanie wniosków aplikacji klucz interfejsu API

Musisz to zrobić dla każdego wersji szablonu, który chcesz utworzyć adnotacje wersji.


1. Zaloguj się do [Portalu Microsoft Azure](https://portal.azure.com) i otwórz zasób aplikacji wniosków monitoruje aplikacji. (Lub [Utwórz go teraz](app-insights-overview.md), jeśli nie zostało to zrobione jeszcze.)
2. Otwórz **Program Access interfejsu API**i wykonać kopię **Identyfikator wniosków aplikacji**.

    ![W portal.azure.com otwórz zasób wniosków aplikacji i wybierz pozycję Ustawienia. Otwórz program Access interfejsu API. Skopiuj identyfikator aplikacji](./media/app-insights-annotations/20.png)

2. W osobnym oknie przeglądarki Otwórz lub Utwórz szablon wersja, która zarządza wdrożeń z programu Visual Studio Team Services. 

    Dodawanie zadania, a następnie z menu wybierz zadanie adnotacji wersji wniosków aplikacji.

    Wklej **Identyfikator aplikacji** kopiowane z karta interfejsu API programu Access.

    ![W usługach zespołu programu Visual Studio Otwórz wersji, wybierz definicję wersji i wybierz pozycję Edytuj. Kliknij przycisk Dodaj zadanie i wybierz aplikację wniosków wersji adnotacji. Wklej identyfikatorem aplikacji wnioski.](./media/app-insights-annotations/30.png)

3. Ustaw pole **APIKey** zmiennej `$(ApiKey)`.

4. W karta interfejsu API dostępu Utwórz nowy klucz interfejsu API i sporządzanie jego kopię.

    ![W karta interfejsu API programu Access w oknie Azure kliknij przycisk Utwórz klucz interfejsu API. Podaj komentarza, zaznacz adnotacje zapisu, a następnie kliknij generowanie klucza. Skopiuj nowy klucz.](./media/app-insights-annotations/40.png)

4. Otwórz kartę Konfiguracja wersji szablonu.

    Tworzenie definicji zmiennych `ApiKey`.

    Wklej swój klucz interfejsu API do definicji zmiennych ApiKey.

    ![W oknie usług zespołu wybierz kartę Konfiguracja, a następnie kliknij przycisk Dodaj zmiennych. Skonfiguruj nazwę, aby ApiKey i na wartość, Wklej klucz, który został wygenerowany.](./media/app-insights-annotations/50.png)


5. Na koniec **Zapisz** definicję wersji.

## <a name="create-annotations-from-powershell"></a>Tworzenie adnotacji z programu PowerShell

Można także tworzyć adnotacje z wszelkich procesów, który chcesz (bez użycia w PORÓWNANIU z Team System). 

Uzyskać [skrypt programu Powershell z GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

Używaj, tak jak poniżej:

    .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }

Uzyskiwanie `applicationId` i `apiKey` z zasób aplikacji wniosków: Otwórz ustawienia, interfejs API programu Access i kopiowanie identyfikator aplikacji. Następnie kliknij przycisk Utwórz klucz interfejsu API i skopiuj klucz. 

## <a name="release-annotations"></a>Zwolnij adnotacji

Teraz zawsze, gdy używasz wersji szablonu do wdrożenia nowej wersji adnotacji będą wysyłane do wniosków aplikacji. Adnotacje pojawi się na wykresach w Eksploratorze metryki.

Kliknij dowolny znacznik adnotacji, aby otworzyć szczegółowe informacje o wersji, łącznie z żądającego, Rozgałęzienie kontroli źródła, zwolnij definicji, środowiska i innych elementów.


![Kliknij dowolny znacznik adnotacji wersji.](./media/app-insights-annotations/60.png)
