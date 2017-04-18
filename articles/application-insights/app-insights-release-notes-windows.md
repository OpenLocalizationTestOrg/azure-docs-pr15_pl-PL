<properties 
    pageTitle="Informacje o wersji wniosków aplikacji dla systemu Windows" 
    description="Najnowsze aktualizacje dla zestawu SDK ze Sklepu Windows." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/12/2016" 
    ms.author="joshweb"/>
 
# <a name="release-notes-for-application-insights-sdk-for-windows-phone-and-store"></a>Informacje o wersji dla aplikacji wniosków SDK dla Windows Phone i przechowywania

SDK wniosków aplikacji wysyła telemetrycznego o live aplikacji do [Wniosków aplikacji](https://azure.microsoft.com/services/application-insights/), gdzie można analizować jego zastosowania i wydajność.


## <a name="version-111"></a>Wersja 1.1.1

### <a name="windows-sdk"></a>SDK systemu Windows

- Naprawianie zawiesza się podczas awarii podczas korzystania z systemu Windows Phone Silverlight SDK. Ta zmiana dowolnego awarię, która później niż ~ 2 sekund, po połączenie WindowsAppInitialier.InitializeAsync(...) zostaną zachowywane do dysku i będą wysyłane następnego czasu uruchomieniu aplikacji. W przypadku awarii przed ~ 2 sekund po połączenie, będą ignorowane.  
- Ustawianie zależności NuGet do określonej wersji podstawowych i Microsoft.ApplicationInsights.PersistenceChannel (v1.2.3).   

### <a name="core-sdk"></a>Podstawowe SDK

- Podstawowe odbywa się w github. Przyszłych wersji zestawu SDK Core można znaleźć [w github](http://github.com/Microsoft/ApplicationInsights-dotnet/releases)

## <a name="version-11"></a>W wersji 1.1

### <a name="core-sdk"></a>Podstawowe SDK

- Zestaw SDK teraz wprowadza nowy typ telemetrycznego ```DependencyTelemetry``` zawierający informacje o zależnościach połączenia z aplikacji
- Nowa metoda ```TelemetryClient.TrackDependency``` umożliwia wysyłanie informacji o zależnościach połączeń z aplikacji

## <a name="version-100"></a>Wersja 1.0.0

### <a name="windows-app-sdk"></a>Zestaw SDK aplikacji systemu Windows

- Inicjowanie nowej aplikacji systemu Windows. Nowy `WindowsAppInitializer` klasy z `InitializeAsync()` metoda pozwala na inicjowanie inicjowanie zbioru SDK. Ta zmiana umożliwiają dokładniejszą kontrolę i ulepszenia dotyczące wydajności inicjowanie znaczną aplikacji na poprzedniej metoda ApplicationInsights.config.
- Nie będzie automatycznie ustawiono DeveloperMode. Aby zmienić zachowanie DeveloperMode musisz określić w kodzie.
- Pakiet NuGet nie jest już wstawia go ApplicationInsights.config. Zaleca się podczas ręcznego dodawania pakietu NuGet za pomocą nowych WindowsAppInitializer.
- Tylko odczytuje ApplicationInsights.config `<InstrumentationKey>`, wszystkie inne ustawienia są ignorowane w oknie dialogowym Preferencje ustawienia WindowsAppInitializer.
- Rynku Sklepu będą automatycznie zbierane przez SDK.
- Wiele poprawki, stabilność i ulepszenia wydajności.

### <a name="core-sdk"></a>Podstawowe SDK

- Plik ApplicationInsights.config jest już requiered. I nie dodane przez pakiet NuGet. Konfiguracja może w pełni określona w kodzie.
- Pakiet NuGet już nie spowoduje dodanie pliku elementów docelowych do rozwiązania. Spowoduje to usunięcie ustawienie automatycznego DeveloperMode podczas Kompilacja debugowania. DeveloperMode należy ręcznie skonfigurować w kodzie.

## <a name="version-017"></a>Wersja 0.17

### <a name="windows-app-sdk"></a>SDK aplikacji systemu Windows

- SDK aplikacji systemu Windows obsługuje teraz uniwersalny aplikacje systemu Windows utworzone przed systemu Windows 10 technical preview i z w PORÓWNANIU z RC 2015 r.

### <a name="core-sdk"></a>Podstawowe SDK

- Domyślnie TelemetryClient zainicjować InMemoryChannel.
- Nowy interfejs API dodane, `TelemetryClient.Flush()`. Ta metoda Dosunięty do wywoła natychmiastowej blokowania przekazywanie wszystkich telemetrycznego logowania do tego klienta. Dzięki temu ręcznego powodujące przekazywania przed Zamykanie procesu.
- Pakiet NuGet dodany docelowej .net 4,5. Ten obiekt docelowy ma nie zależności zewnętrznych (usunięto zależności BCL i źródła zdarzeń).

## <a name="version-016"></a>Wersja 0,16 

Podgląd 2015-04-28

- Zestaw SDK obsługuje teraz platformy docelowej DNX w celu umożliwienia, monitorowanie aplikacji [podstawowych .NET framework](http://www.dotnetfoundation.org/NETCore5) .
- Wystąpienia ```TelemetryClient``` wyłączenie buforowania klucza oprzyrządowania już. Jeśli klucz oprzyrządowania nie została ustawiona ```TelemetryClient``` jawnie ```InstrumentationKey``` zwróci wartość null. Rozwiązuje problem, ustawiając ```TelemetryConfiguration.Active.InstrumentationKey``` po niektórych telemetrycznego zostały już zebrane, moduły telemetrycznego, takie jak współzależność zbierających, web żądania zbierania i wydajności liczników modułów zbierających dane będzie używać nowego klucza oprzyrządowania.

## <a name="version-015"></a>Wersja 0,15

- Nowa właściwość ```Operation.SyntheticSource``` dostępnej teraz na ```TelemetryContext```. Teraz możesz oznaczanie wiadomości flagą telemetrycznego jako "nie ruch rzeczywistego użytkownika" i określ, jak ruch został wygenerowany. Jako przykład przez ustawienie tej właściwości można odróżnić ruch z usługi automatyzacji test z ruchu test ładowania.
- Logika kanału został przeniesiony do oddzielnych NuGet, o nazwie Microsoft.ApplicationInsights.PersistenceChannel. Kanału domyślnego nazywa się teraz InMemoryChannel
- Nowa metoda ```TelemetryClient.Flush``` umożliwia synchronicznie opróżnić telemetrycznego elementów z bufora

## <a name="version-013"></a>Wersja 0,13 cala

Nie wersji dla starszych wersji dostępne. 
