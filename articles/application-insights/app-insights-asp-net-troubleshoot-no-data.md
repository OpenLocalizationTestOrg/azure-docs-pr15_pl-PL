<properties 
    pageTitle="Rozwiązywanie problemów z nie danych — wnioski aplikacji dla środowiska .NET" 
    description="Nie są wyświetlane dane w Visual Studio aplikacji wniosków? Spróbuj tutaj." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Rozwiązywanie problemów z nie danych — wnioski aplikacji dla środowiska .NET

## <a name="some-of-my-telemetry-is-missing"></a>Brakuje części Moje telemetrycznego

*W aplikacji wniosków jest widoczny tylko część zdarzenia generowane przez aplikacji.*

* Jeśli spójne widzisz samej ułamek, jest prawdopodobnie z powodu adaptacyjne [próbki](app-insights-sampling.md). Aby to sprawdzić, otwórz wyszukiwania (od karta Przegląd) i spójrz na wystąpienie żądanie lub innego zdarzenia. W dolnej sekcji właściwości kliknij pozycję "..." Aby uzyskać pełny właściwości szczegółów. Jeśli żądanie liczba > 1, a następnie przy próbkowaniu znajduje się w operacji. 
* W przeciwnym razie jest możliwe, że możesz już naciśnięcie [ograniczyć szybkość danych](app-insights-pricing.md#limits-summary) dla planu cennik. Limity te są stosowane na minutę.

## <a name="no-data-from-my-server"></a>Nie danych z serwerem

*Po zainstalowaniu aplikacji na serwerze sieci web, a nie widać dowolnego telemetrycznego z niego. Na moim komputerze deweloperów zadziałało przycisk OK.*

* Prawdopodobnie problem zapory. [Ustawianie wyjątki zapory dla aplikacji wniosków wysyłanie danych](app-insights-ip-addresses.md).

*Mam [zainstalowane Monitor stanu](app-insights-monitor-performance-live-website-now.md) na serwerze sieci web, monitorowanie istniejące aplikacje. Nie ma żadnych wyników.*

* Zobacz [Rozwiązywanie problemów Monitor stanu](app-insights-monitor-performance-live-website-now.md#troubleshooting). 


## <a name="q01"></a>Opcja Brak "Dodawanie wniosków aplikacji" w programie Visual Studio

*Podczas tworzenia nowego projektu w programie Visual Studio, lub gdy I kliknij prawym przyciskiem myszy istniejący projekt w Eksploratorze rozwiązań, nie jest wyświetlane wszystkie opcje wniosków aplikacji.*

+ Nie we wszystkich typach projekt .NET są obsługiwane za pomocą narzędzi. Projekty sieci Web i WCF są obsługiwane. Dla innych typów projektów, takich jak aplikacji komputerowych lub usługi można [ręcznie dodać SDK wniosków aplikacji do projektu](app-insights-windows-desktop.md).
+ Upewnij się, że masz [Visual Studio 2013 aktualizacji 3 lub nowszej](http://go.microsoft.com/fwlink/?LinkId=397827). Nadejdzie preinstalowanymi narzędziami wniosków aplikacji.
+ Wybierz pozycję **Narzędzia**, **rozszerzenia i aktualizacje** i sprawdź zainstalowanego i włączonego **Narzędzia wniosków aplikacji** . Jeśli tak, kliknij pozycję **aktualizacji** , aby sprawdzić, czy jest aktualizacja.
+ Otwórz okno dialogowe Nowy projekt i wybierz aplikację sieci Web programu ASP.NET. Jeśli opcja wniosków aplikacji jest widoczna, są zainstalowane narzędzia. Jeśli nie, spróbuj odinstalować i ponowne zainstalowanie narzędzia wniosków aplikacji.


## <a name="q02"></a>Dodawanie aplikacji wniosków nie powiodło się

*Podczas tworzenia nowego projektu sieci web lub podczas próby dodania aplikacji wniosków w istniejącym projekcie, widzę komunikat o błędzie.*

Prawdopodobne przyczyny:

* Komunikowanie się z portalem wniosków aplikacji nie powiodło się; lub
* Występuje problem z kontem usługi Azure;
* Masz tylko [do odczytu subskrypcji lub grupie miejsce, w którym chcesz utworzyć nowy zasób](app-insights-resources-roles-access-control.md).

Poprawka:

+ Sprawdź dostępne poświadczeń logowania dla konta po prawej Azure. 
+ W przeglądarce Sprawdź, czy masz dostęp do [portalu Azure](https://portal.azure.com). Otwórz stronę ustawienia i sprawdzić, czy jest ograniczeń.
+ [Dodawanie aplikacji wniosków do istniejącego projektu](app-insights-asp-net.md): W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projektu i wybierz pozycję "Dodaj wniosków aplikacji".
+ Jeśli nadal nie działa, należy wykonać [procedury ręcznego](app-insights-windows-services.md) dodanie zasobu w portalu, a następnie dodaj zestawu SDK do projektu. 

## <a name="emptykey"></a>Pojawia się błąd "klucz oprzyrządowania nie może być puste"

Wygląda na wystąpił problem podczas zostały instalowania aplikacji wniosków lub być może karta Rejestrowanie.

W Eksploratorze rozwiązań, kliknij prawym przyciskiem myszy `ApplicationInsights.config` i wybierz pozycję **Konfigurowanie wniosków aplikacji**. Zostanie wyświetlony okno dialogowe umożliwiające logowanie się do Azure i Utwórz zasób wniosków aplikacji lub ponowne użycie istniejącej.


##<a name="NuGetBuild"></a>"Pakiety NuGet brakuje" na serwerze kompilacji

*Tworzy wszystko OK, gdy jestem I debugowania na moim komputerze rozwoju, ale otrzymuję błąd NuGet na serwerze kompilacji.*

Zobacz [Przywracanie pakietu NuGet](http://docs.nuget.org/Consume/Package-Restore) i [Automatyczne przywracanie pakietu](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Brakujące polecenia menu w celu otwarcia aplikacji wniosków z programu Visual Studio

*Po I kliknij prawym przyciskiem myszy projekcie Eksplorator rozwiązań, nie ma żadnych poleceń aplikacji wniosków lub nie ma polecenia Otwórz wniosków aplikacji.*

Prawdopodobne przyczyny:

* Jeśli utworzona ręcznie zasobów wniosków aplikacji lub jeśli projekt jest typu, który nie jest obsługiwane za pomocą narzędzi wniosków aplikacji.
* Narzędzia wniosków aplikacji są wyłączone w programu Visual Studio.
* Programu Visual Studio jest starsza niż 2013 aktualizacji 3.

Poprawka:

* Upewnij się, że Twoja wersja programu Visual Studio jest aktualizacja 2013 3 lub nowszej.
* Wybierz pozycję **Narzędzia**, **rozszerzenia i aktualizacje** i sprawdź zainstalowanego i włączonego **Narzędzia wniosków aplikacji** . Jeśli tak, kliknij pozycję **aktualizacji** , aby sprawdzić, czy jest aktualizacja.
* Kliknij prawym przyciskiem myszy projektu w Eksploratorze rozwiązań. Polecenie **Konfigurowanie wniosków aplikacji**jest widoczne go używać nawiązać projektu zasobów w usłudze wniosków aplikacji.


W przeciwnym razie usługi typ projektu nie jest obsługiwana bezpośrednio za pomocą narzędzi wniosków aplikacji. Aby zobaczyć telemetrycznego, zaloguj się do [portalu Azure](https://portal.azure.com), wybierz wniosków aplikacji na pasku nawigacyjnym po lewej stronie, a wybierz pozycję aplikacja.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>"Odmowa dostępu" przy otwieraniu wniosków aplikacji z programu Visual Studio

*Polecenie menu "Otwórz aplikację wniosków" powoduje przejście do portalu Azure, ale pojawia się błąd "odmowa dostępu".*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

Microsoft logowania ostatnio używana w domyślnej przeglądarce nie ma dostępu do [tego zasobu, który został utworzony podczas dodawania aplikacji wniosków dla tej aplikacji](app-insights-asp-net.md). Istnieją dwie prawdopodobne przyczyny: 

* Masz więcej niż jedno konto Microsoft — może pracy i osobistego konta Microsoft? Zaloguj się do ostatnio używana w domyślnej przeglądarce dotyczyło konta innego niż ten, który ma dostęp do [dodawania aplikacji wniosków do projektu](app-insights-asp-net.md). 

 * Poprawka: Kliknij swoją nazwę na górnego prawego okna przeglądarki, a następnie zaloguj. Następnie zaloguj się do konta, które ma dostęp. Na pasku nawigacyjnym po lewej stronie kliknij wniosków aplikacji i wybierz aplikację.

* Wnioski aplikacji innej osobie dodani do projektu, a ich zapomniano o umożliwiają [dostęp do grup zasobów](app-insights-resources-roles-access-control.md) , w której został utworzony. 

 * Poprawka: Jeśli wykorzystane konto organizacji mogą dodawać możesz zespołowi; lub ich można udziel indywidualnego dostępu do grupy zasobów.



## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>"Trwały" nie można odnaleźć przy otwieraniu wniosków aplikacji z programu Visual Studio

*Polecenie menu "Otwórz aplikację wniosków" powoduje przejście do portalu Azure, ale pojawia się błąd "nie można odnaleźć elementów zawartości".*

Prawdopodobne przyczyny:

* Usunięto zasób wniosków aplikacji dla danej aplikacji; lub
* Klucz oprzyrządowania został ustawiony lub zmienione w ApplicationInsights.config edytując je bezpośrednio, bez aktualizowania pliku projektu. 

Klucz oprzyrządowania w miejsce, w którym jest wysyłana telemetrycznego kontrolki ApplicationInsights.config. Wiersz w pliku programu project Określa, który zasób jest otwierany, gdy w programie Visual Studio za pomocą polecenia. 

Poprawka:

* W oknie Eksplorator rozwiązań projektu kliknij prawym przyciskiem myszy i wybierz pozycję wniosków aplikacji, konfigurowanie wniosków aplikacji. W oknie dialogowym albo możesz wysłać telemetrycznego do istniejącego zasobu, lub Utwórz nowy. Lub:
* Bezpośrednio otworzyć tego zasobu. Zaloguj się do [portalu Azure](https://portal.azure.com), kliknij pozycję wniosków aplikacji na pasku nawigacji po lewej stronie, a następnie wybierz aplikację.



## <a name="where-do-i-find-my-telemetry"></a>Gdzie znaleźć mojej telemetrycznego?

*Zalogowaniu się do [portalu Microsoft Azure](https://portal.azure.com)i wyświetlane Azure głównym pulpitu nawigacyjnego. Dlatego gdzie mogę znaleźć moich danych wniosków aplikacji?*

* Na pasku nawigacyjnym po lewej stronie kliknij wniosków aplikacji, a następnie nazwę aplikacji. Jeśli posiadasz wszystkich projektów, musisz [dodać lub skonfigurować wniosków aplikacji w projekcie w sieci web](app-insights-asp-net.md).

    Zobaczysz Niektóre wykresy podsumowań. Kliknięcie przez ich tak, aby wyświetlić więcej szczegółów.

* W programie Visual Studio gdy debugowanie aplikacji, kliknij przycisk wniosków aplikacji.


## <a name="q03"></a>Nie danych serwera (lub Brak danych we wszystkich)

*Uruchomiono Moja aplikacja i następnie otworzyć usługę wniosków aplikacji platformy Microsoft Azure, ale wszystkie wykresy wyświetlić "Dowiedz się, jak zbierać..." lub "Nie skonfigurowano."* Lub, *tylko dane widoku strony i użytkownika, ale bez danych serwera.*

+ Uruchamianie aplikacji w trybie debugowania w programie Visual Studio (F5). Za pomocą aplikacji w taki sposób, aby wygenerować niektórych telemetrycznego. Sprawdź, czy jest widoczny zdarzenia zarejestrowane w oknie dane wyjściowe programu Visual Studio. 

    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)

+ Otwórz [Diagnostyki wyszukiwania](app-insights-diagnostic-search.md)w portalu wniosków aplikacji. Dane zazwyczaj są wyświetlane poniżej najpierw.
+ Kliknij przycisk Odśwież. Karta odświeża się okresowo, ale można też wykonać je ręcznie. Interwał odświeżania jest dłużej większych zakresów czasu.
+ Zaznacz pole wyboru Uwzględnij klawiszy oprzyrządowania. Na głównym karta dla aplikacji w portalu wniosków aplikacji w **podstawowe informacje dotyczące** listy rozwijanej Sprawdź **oprzyrządowania klucza**. Następnie w projekcie w programie Visual Studio, otwórz ApplicationInsights.config i znajdowanie `<instrumentationkey>`. Sprawdź, czy te dwa klucze są równe. Jeśli nie jest:
 + W portalu kliknij wniosków aplikacji, a dla zasobu aplikacji z prawym klawiszem; lub
 + W Eksploratorze rozwiązań programu Visual Studio kliknij prawym przyciskiem myszy projektu i wybierz pozycję wniosków aplikacji, Konfiguruj. Resetuj aplikację, aby wysłać telemetrycznego do prawej zasobów.
 + Jeśli nie możesz znaleźć pasujące klawiszy, sprawdź, używanym tych samych poświadczeń logowania w programie Visual Studio, jak w do portalu.

    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
    
+ W [głównym pulpitu nawigacyjnego platformy Microsoft Azure](https://portal.azure.com)Spójrz na mapę kondycja usługi. W przypadku niektórych oznaczeń alertów, poczekaj, aż one zwrócić do przycisku OK, a następnie zamknij i ponownie otwórz karta aplikacji usługi wniosków aplikacji.
+ Sprawdź też [blog poświęcony stanu](http://blogs.msdn.com/b/applicationinsights-status/).
+ Czy pisania kodu dla [SDK po stronie serwera](app-insights-api-custom-events-metrics.md) , który może zostać zmieniony klucz oprzyrządowania w `TelemetryClient` wystąpienia lub `TelemetryContext`? Lub czy zapisać [konfiguracji filtru lub próbki](app-insights-api-filtering-sampling.md) , która może filtrować się zbyt dużo?
+ Jeśli edytujesz ApplicationInsights.config, uważnie sprawdź konfigurację [TelemetryInitializers i TelemetryProcessors](app-insights-api-filtering-sampling.md). Typ o nazwie niepoprawnie lub parametr mogą powodować SDK wysyłanie nie danych.

## <a name="q04"></a>Brak danych dotyczących użycia wyświetleń stron, przeglądarek,

*Widzę danych na wykresach czas odpowiedzi serwera i wezwań na serwerze, ale bez danych podczas ładowania widoku strony lub w przeglądarce lub użycie karty.*

Dane pochodzą z skryptów na stronach sieci web. 

+ Po dodaniu aplikacji wniosków do istniejącego projektu sieci web, [należy ręcznie dodać skryptów](app-insights-javascript.md).
+ Upewnij się, że w trybie zgodności w programie Internet Explorer nie są wyświetlane witryny.
+ Sprawdź, czy dane są przesyłane do za pomocą funkcji debugowania w przeglądarce (F12 w niektórych przeglądarkach, następnie wybierz sieć) `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Brak danych zależności lub wyjątku

Zobacz [telemetrycznego zależności](app-insights-asp-net-dependencies.md) i [telemetrycznego wyjątek](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>Nie dane dotyczące wydajności

Dane dotyczące wydajności (CPU, Wy i tak dalej) jest dostępna dla [usług sieci web języka Java](app-insights-java-collectd.md), [aplikacje komputerowe systemu Windows](app-insights-windows-desktop.md), [aplikacji i usług w przypadku zainstalowania monitor stanu web usług IIS](app-insights-monitor-performance-live-website-now.md)i [Usług w chmurze Azure](app-insights-azure.md). można je znaleźć w obszarze Ustawienia serwerów.

Nie jest dostępna dla Azure witryn sieci Web.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>Bez danych (server), ponieważ po opublikowaniu serwerem aplikacji

+ Sprawdzanie faktycznie skopiowany przez firmę Microsoft. ApplicationInsights dll na serwerze, razem z Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll
+ W zaporze może być konieczne [otwarcie niektórych portów TCP](app-insights-ip-addresses.md#data-access-api).
+ Jeśli masz Użyj serwera proxy, aby wysłać poza siecią firmową, ustawianie [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) w Web.config
+ Windows Server 2008: Upewnij się, masz zainstalowane następujące aktualizacje: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523) [KB2600217](https://support.microsoft.com/kb/2600217).


## <a name="i-used-to-see-data-but-it-has-stopped"></a>Można używać do wyświetlania danych, ale zostało zatrzymane

* Sprawdzanie [stanu blogu](http://blogs.msdn.com/b/applicationinsights-status/).
* Masz trafisz miesięczny przydziału punktów danych Otwórz ustawienia-przydziału i ceny, aby dowiedzieć się. Jeśli tak, możesz uaktualnić swój plan lub płatności dodatkowych. Zobacz [ceny schematu](https://azure.microsoft.com/pricing/details/application-insights/).


## <a name="i-dont-see-all-the-data-im-expecting"></a>Nie widzę wszystkich danych I jest oczekiwana

Jeśli aplikacja wysyła wiele danych i korzystania z SDK wniosków aplikacji dla programu ASP.NET wersji 2.0.0-beta3 lub nowszym, funkcja [adaptacyjne przy próbkowaniu](app-insights-sampling.md) może działać i Wyślij tylko procent swojego telemetrycznego. 

Można ją wyłączyć, ale nie jest to zalecane. Przy próbkowaniu została zaprojektowana tak, aby poprawnie telemetrycznego pokrewne są przesyłane w celach diagnostycznych. 

## <a name="wrong-geographical-data-in-user-telemetry"></a>Nieprawidłowe dane geograficzne w telemetrycznego użytkownika

Miasto, region i wymiary kraju pochodzą z adresów IP i nie są zawsze dokładności.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Wyjątku "nie można odnaleźć metody" w programie usług w chmurze Azure

Czy utworzyć dla .NET 4.6? 4.6 nie jest automatycznie obsługiwana w roli usług w chmurze Azure. [Instalowanie 4.6 na każdej roli](../cloud-services/cloud-services-dotnet-install-dotnet.md) przed uruchomieniem aplikacji.

## <a name="still-not-working"></a>Nadal nie działa...

* [Forum wniosków aplikacji](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

