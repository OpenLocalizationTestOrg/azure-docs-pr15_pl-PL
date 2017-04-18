<properties 
    pageTitle="Rozwiązywanie problemów z wniosków aplikacji w programie project web Java" 
    description="Podręcznik rozwiązywania problemów — monitorowanie żywo aplikacje Java z wniosków aplikacji." 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/01/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Rozwiązywanie problemów i pytań i odpowiedzi dla aplikacji wniosków dla języka Java

Pytania lub problemy z [Programu Visual Studio wniosków aplikacji w języku Java][java]? Oto kilka porad.


## <a name="build-errors"></a>Tworzenie błędów

*Zaćmienie podczas dodawania aplikacji SDK wniosków za pośrednictwem środowiska Maven lub Gradle otrzymuję kompilacji lub sumy kontrolnej błędów sprawdzania poprawności.*

* Jeśli zależność <version> elementu jest używanie wzorca z symboli wieloznacznych (np. (środowiska Maven) `<version>[1.0,)</version>` lub (Gradle) `version:'1.0.+'`), spróbuj zamiast tego określić określonej wersji, takich jak `1.0.2`. Zobacz [informacje o wersji](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) najnowszą wersję.

## <a name="no-data"></a>Brak danych 

*Został pomyślnie dodany wniosków aplikacji i uruchomiono aplikacji, ale można zobaczył danych w portalu.*

* Poczekaj chwilę i kliknij przycisk Odśwież. Wykresy odświeżanie się okresowo, ale można również odświeżyć ręcznie. Interwał odświeżania zależy od zakresu czasu dla wykresu.
* Sprawdź, czy masz klucz oprzyrządowania zdefiniowane w pliku ApplicationInsights.xml (w folderze zasobów w projekcie)
* Upewnij się, że jest nie `<DisableTelemetry>true</DisableTelemetry>` węzła w pliku xml.
* W zaporze może być konieczne Otwórz porty TCP 80 i 443 dla ruchu wychodzącego dc.services.visualstudio.com. Zobacz [pełną listę wyjątków zapory](app-insights-ip-addresses.md)
* W programie Microsoft Azure zacznij tablicy, spójrz na mapie stanu usługi. W przypadku niektórych oznaczeń alertów, poczekaj, aż one zwrócić do przycisku OK, a następnie zamknij i ponownie otwórz karta aplikacji usługi wniosków aplikacji.
* Włączanie rejestrowania w oknie konsoli IDE, dodając `<SDKLogger />` elementu w obszarze węzła głównego w pliku ApplicationInsights.xml (w folderze zasobów w projekcie) i sprawdź, czy wpisy nazwą [Błąd].
* Upewnij się, że właściwy plik ApplicationInsights.xml został pomyślnie załadowany przez Java SDK, sprawdzając konsoli wyjściowe komunikaty instrukcji "pliku konfiguracji pomyślnie znaleziono".
* Jeśli nie można odnaleźć pliku konfiguracji, zaznacz wiadomości dane wyjściowe, aby zobaczyć, gdzie są wyszukiwane pliku konfiguracji i upewnij się, że ApplicationInsights.xml znajduje się w jednej z tych lokalizacji wyszukiwania. Jako zasada możesz umieścić pliku konfiguracji u JARs SDK wniosków aplikacji. Na przykład: w Tomcat, oznacza to, czy folderu WEB-INF/Biblioteka.



#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Można używać do wyświetlania danych, ale zostało zatrzymane

* Sprawdzanie [stanu blogu](http://blogs.msdn.com/b/applicationinsights-status/).
* Masz trafisz miesięczny przydziału punktów danych Otwórz okno Ustawienia-przydziału i ceny, aby dowiedzieć się. Jeśli tak, możesz uaktualnić swój plan lub płatności dodatkowych. Zobacz [ceny schematu](https://azure.microsoft.com/pricing/details/application-insights/).

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Nie widzę wszystkich danych I jest oczekiwana

* Otwórz przydziałów i ceny karta i sprawdź, czy w operacji [pobierania](app-insights-sampling.md) . (przekazywanie 100% oznacza, że przy próbkowaniu nie ma w operacji). Usługa wniosków aplikacji można ustawić zaakceptować tylko część telemetrycznego, przychodzący z Twojej aplikacji. Pomaga to utrzymywania w ramach usługi miesięcznej kwoty telemetrycznego. 

## <a name="no-usage-data"></a>Nie danych dotyczących użycia

*Widzę dane dotyczące żądania, czasy odpowiedzi, ale nie widoku strony, przeglądarki lub dane użytkownika.*

Pomyślnym skonfigurowaniu aplikacji do wysłania telemetrycznego z serwera. Teraz, następnym krokiem jest [Ustawienie stron sieci web, aby wysłać telemetrycznego z przeglądarki sieci web][usage].

Alternatywnie Jeśli klient jest aplikacji [telefonu lub innego urządzenia][platforms], możesz wysłać telemetrycznego stamtąd. 

Użyj tego samego klucza oprzyrządowania skonfigurować telemetrycznego klienta i serwera. Dane zostaną wyświetlone w ten sam zasób wniosków aplikacji, a będziesz mieć możliwość przeniesionym wydarzenia z klienta i serwera.



## <a name="disabling-telemetry"></a>Wyłączanie telemetrycznego

*Jak wyłączyć zbioru telemetrycznego?*

W kodzie:

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);


**Lub** 

Aktualizowanie ApplicationInsights.xml (w folderze zasobów w projekcie). Dodaj następujący w obszarze węzeł główny:

    <DisableTelemetry>true</DisableTelemetry>

Przy użyciu metody XML, należy ponownie uruchomić aplikację, gdy zmienisz wartości.

## <a name="changing-the-target"></a>Zmienianie docelowej

*Jak można zmienić Azure zasobów projektu wysyła dane?*

* [Uzyskiwanie klucza oprzyrządowania nowego zasobu.][java]
* Jeśli zostanie dodany wniosków aplikacji do projektu przy użyciu narzędzi Azure dla programu Eclipse, kliknij prawym przyciskiem myszy projektu sieci web, zaznacz **Azure**, **Konfigurowanie wniosków aplikacji**i zmienianie klucza.
* W przeciwnym razie aktualizacji klucz w ApplicationInsights.xml w folderze zasobów w projekcie.


## <a name="the-azure-start-screen"></a>Ekran startowy Azure

*Jestem przeglądająca [Azure portal](https://portal.azure.com) Czy mapy Powiedz mi coś o aplikacji?*

Nie, jest wyświetlany prawidłowość serwerów Azure na świecie.

*Od początku Azure tablicy (ekran główny) jak znaleźć danych o aplikacji?*

Przy założeniu [Konfigurowanie aplikacji dla aplikacji wniosków][java], kliknij przycisk Przeglądaj, wybierz wniosków aplikacji i wybierz zasób aplikacji utworzoną dla aplikacji. Aby uzyskać szybciej w przyszłości, możesz przypiąć aplikacji do tablicy start.

## <a name="intranet-servers"></a>Serwery sieci intranet

*W mojej sieci intranet można monitorować serwer?*

Tak, pod warunkiem, że serwer można wysłać telemetrycznego do portalu wniosków aplikację za pośrednictwem publicznego Internetu. 

W zaporze może być konieczne Otwórz porty TCP 80 i 443 dla ruchu wychodzącego dc.services.visualstudio.com i f5.services.visualstudio.com.

## <a name="data-retention"></a>Przechowywanie danych 

*Jak długo trwa danych jest zachowywana w portalu Jest to bezpieczne?*

Zobacz [przechowywanie danych i prywatności][data].

## <a name="next-steps"></a>Następne kroki

*Skonfigurowano wniosków aplikacji dla aplikacji serwera Java. Co jeszcze można zrobić?*

* [Monitorowanie dostępność stron sieci web][availability]
* [Monitorowanie użycia strony sieci web][usage]
* [Śledzenie użycia i diagnozowanie problemów w aplikacji urządzenia][platforms]
* [Pisanie kodu do śledzenia zastosowania aplikacji][track]
* [Przechwytywanie dzienniki diagnostyczne][javalogs]


## <a name="get-help"></a>Uzyskiwanie pomocy

* [Przepełnienie stosu](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 