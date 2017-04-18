<properties 
    pageTitle="Rozwiązywanie problemów i pytania dotyczące aplikacji wniosków" 
    description="Coś w Visual Studio aplikacji wniosków niejasne lub nie działa? Spróbuj tutaj." 
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
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="questions---application-insights-for-aspnet"></a>Pytania — wnioski aplikacji dla programu ASP.NET

## <a name="configuration-problems"></a>Problemy z konfiguracją

*Mam problem w górę moje:*

* [Aplikacja .NET](app-insights-asp-net-troubleshoot-no-data.md)
* [Monitorowanie aplikacji już uruchomiony](app-insights-monitor-performance-live-website-now.md#troubleshooting)
* [Diagnostyka Azure](app-insights-azure-diagnostics.md)
* [Aplikacja sieci web języka Java](app-insights-java-troubleshoot.md)
* [Innych platformach](app-insights-platforms.md)

*Otrzymuję żadnych danych z serwerem*

* [Konfigurowanie wyjątków zapory](app-insights-ip-addresses.md)
* [Konfigurowanie serwera programu ASP.NET](app-insights-monitor-performance-live-website-now.md)
* [Konfigurowanie serwera Java](app-insights-java-agent.md)


## <a name="can-i-use-application-insights-with-"></a>Czy mogę używać aplikacji wniosków z...?

[Zobacz platformy][platforms]


## <a name="is-it-free"></a>Jest to bezpłatne?

* Tak, w przypadku wybrania wolnych [ceny warstwy](app-insights-pricing.md). Zostanie wyświetlony większość funkcji i atrakcyjnych przydziału danych. 
* Musisz podać danych karty kredytowej, aby zarejestrować usługi Microsoft Azure, ale będą nie opłaty, chyba że korzystasz z innej opłaconej dla Azure usługi lub jawnie uaktualnienie do poziomu płatności.
* Aplikacji wysyła większej ilości danych niż miesięcznej kwoty dla poziomu bezpłatne, przestaje, rejestrowane. W takim przypadku możesz wybrać w celu uruchomienia opłaty lub poczekaj, aż przydziału zresetowaniem na koniec miesiąca.
* Podstawowe dane użycia i sesji nie podlega przydziału.
* Jest również bezpłatną wersję próbną 30 dni, w którym zostanie wyświetlony płatnej funkcje dostępne bezpłatnie.
* Każdemu zasobowi aplikacji zawiera osobne przydziału i ustaw jego cennik warstwa niezależnie od innych.

#### <a name="what-do-i-get-if-i-pay"></a>Co zrobić, jeśli zapłacić?

* Większe [miesięcznej kwoty danych](https://azure.microsoft.com/pricing/details/application-insights/).
* Opcja płatności nadmiarowych, aby kontynuować zbieranie danych miesięcznych przydział. Jeśli dane omówiono przydziału, jest naliczany na Mb.
* [Eksportowanie ciągły](app-insights-export-telemetry.md).


## <a name="q14"></a>Co wniosków aplikacji zmodyfikować należące do projektu?

Szczegóły zależy od typu projektu. Dla aplikacji sieci web:


+ Dodaje te pliki do projektu:

 + ApplicationInsights.config. 
 + AI.js


+ Instalacje te pakiety NuGet:

 -  *Interfejs API wniosków aplikacji* - podstawową interfejsu API

 -  *Interfejs API wniosków aplikacji dla aplikacji sieci Web* — służy do wysyłania telemetrycznego z serwera

 -  *Aplikacji wniosków API języka JavaScript aplikacji* — służy do wysyłania telemetrycznego od klienta

    Pakiety obejmują zestawy:

 - Microsoft.ApplicationInsights

 - Microsoft.ApplicationInsights.Platform

+ Wstawia elementy do:

 - Web.config

 - Packages.config

+ (Nowe projekty tylko — jeśli możesz [dodać aplikację wniosków w istniejącym projekcie][start], musisz zrobić to ręcznie.) Wstawia wstawki do kodu klienta i serwera zainicjować za pomocą identyfikatora wniosków aplikacji zasobów. Na przykład w aplikacji programu MVC kod zostanie wstawiona do strony wzorcowej Views/Shared/_Layout.cshtml


## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Jak uaktualnić ze starszych wersji SDK?

Dla zestawu SDK należy typ aplikacji, zobacz [informacje o wersji](app-insights-release-notes.md) . 



## <a name="update"></a>Jak można zmienić Azure zasobów projektu wysyła dane?

W Eksploratorze rozwiązań, kliknij prawym przyciskiem myszy `ApplicationInsights.config` i wybierz polecenie **Aktualizuj aplikacji wniosków**. Możesz wysłać dane do istniejącego lub nowego zasobu w Azure. Kreator aktualizacji zmienia klucz oprzyrządowania w ApplicationInsights.config, która określa miejsce, w którym serwer SDK wysyła dane. O ile nie wyłączysz "Aktualizuj wszystkie", zmieni się także klucz położenia stron sieci web.


#### <a name="data"></a>Jak długo trwa danych jest zachowywana w portalu Jest to bezpieczne?

Zapoznaj się [przechowywanie danych i prywatności][data].

## <a name="logging"></a>Rejestrowanie

#### <a name="post"></a>Jak wyświetlić dane wpisu w diagnostyczne wyszukiwania?

Firma Microsoft nie rejestrowanie danych wpisu automatycznie, ale można użyć połączenia TrackTrace: umieszczenia danych w parametrze wiadomości. Ma ograniczenie rozmiaru dłużej niż ograniczenia dotyczące właściwości ciągów, chociaż nie można filtrować go. 

## <a name="security"></a>Zabezpieczenia

#### <a name="is-my-data-secure-in-the-portal-how-long-is-it-retained"></a>Czy moje dane są bezpieczne w portalu? Jak długo trwa jest zachowywana?

Zobacz [danych przechowywania i prywatności][data].


## <a name="q17"></a>Włączone jest publikowane w aplikacji wniosków mają?

<table border="1">
<tr><th>Powinna być widoczna</th><th>Jak uzyskać</th><th>Dlaczego ma</th></tr>
<tr><td>Wykresy dostępności</td><td><a href="../app-insights-monitor-web-app-availability/">Testy sieci Web</a></td><td>Wiadomo, że działa aplikacji sieci web</td></tr>
<tr><td>Serwer aplikacji wydajności: czas odpowiedzi,...
</td><td><a href="../app-insights-asp-net/">Dodawanie aplikacji wniosków do projektu</a><br/>lub <br/><a href="../app-insights-monitor-performance-live-website-now/">Zainstaluj Monitor stanu AI na serwerze</a> (lub napisać własny kod w celu <a href="../app-insights-api-custom-events-metrics/#track-dependency">śledzenia współzależności</a>)</td><td>Wykrywanie problemy dotyczące wydajności</td></tr>
<tr><td>Zależność telemetrycznego</td><td><a href="../app-insights-monitor-performance-live-website-now/">Zainstaluj Monitor stanu AI na serwerze</a></td><td>Diagnozowanie problemów z bazy danych lub inne składniki zewnętrzne</td></tr>
<tr><td>Uzyskanie śledzenia stosu wyjątków</td><td><a href="../app-insights-search-diagnostic-logs/#exceptions">Wstawianie TrackException połączenia w kodzie</a> (ale niektóre zgłoszone automatycznie)</td><td>Wykrywanie i diagnozowanie wyjątków</td></tr>
<tr><td>Wyszukiwanie dziennika śledzenia</td><td><a href="../app-insights-search-diagnostic-logs/">Dodawanie karty Rejestrowanie</a></td><td>Diagnozowanie wyjątków, problemy dotyczące wydajności</td></tr>
<tr><td>Podstawy zastosowania klienta: liczba wyświetleń strony, sesje,...</td><td><a href="../app-insights-javascript/">Inicjator JavaScript na stronach sieci web</a></td><td>Analizy użycia</td></tr>
<tr><td>Niestandardowe wskaźniki klienta</td><td><a href="../app-insights-api-custom-events-metrics/">Śledzenie połączeń na stronach sieci web</a></td><td>Ulepszanie środowisko użytkownika</td></tr>
<tr><td>Niestandardowe metryki serwera</td><td><a href="../app-insights-api-custom-events-metrics/">Śledzenie połączeń w kod serwera</a></td><td>Funkcje analizy biznesowej</td></tr>
</table>


## <a name="automation"></a>Automatyzacji

Możesz [Pisanie skryptów programu PowerShell](app-insights-powershell.md) do tworzenia i aktualizowania wniosków aplikacji zasobów.

## <a name="more-answers"></a>Więcej odpowiedzi

* [Forum wniosków aplikacji](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)


<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md

 