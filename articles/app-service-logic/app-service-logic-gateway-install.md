<properties
   pageTitle="Aplikacje logiczny zainstalować bramę danych lokalnych | Microsoft Azure"
   description="Informacje na temat instalacji bramy danych lokalnych do użycia w aplikacji logika."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="install-the-on-premises-data-gateway-for-logic-apps"></a>Instalowanie bramy danych lokalnych logiki aplikacji dla

## <a name="installation-and-configuration"></a>Instalowanie i konfigurowanie

### <a name="prerequisites"></a>Wymagania wstępne

Minimum:

* 4,5 .NET framework
* 64-bitowej wersji systemu Windows 7 lub Windows Server 2008 R2 (lub nowszy)

Zalecane:

* Procesor 8 core
* 8 GB pamięci RAM
* 64-bitowej wersji systemu Windows 2012 R2 (lub nowszy)

Pokrewne zagadnienia:

* Nie można zainstalować bramę na kontrolerze domeny.
* Brama nie należy zainstalować na komputerze, takie komputera przenośnego, który może być wyłączona, asleep, lub nie połączenia z Internetem, ponieważ bramy nie może działać w tych sytuacjach. Ponadto w sieci bezprzewodowej dotknąć może wydajności bramy.

### <a name="install-a-gateway"></a>Instalowanie bramy

Możesz uzyskać [Instalatora tutaj bramy danych lokalnych](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).

Określanie **bramy danych lokalnych** jako tryb, zaloguj się przy użyciu swojej pracy lub szkolne konto i następnie skonfigurowanie Nowa brama lub przeprowadzić migrację, przywracanie lub przejąć istniejącej bramy.

* Aby skonfigurować bramę, wpisz **nazwę** dla niej i **klucza odzyskiwania**, a następnie kliknij lub naciśnij **Konfiguruj**.

    Określanie klucza odzyskiwania, który zawiera co najmniej ośmiu znaków i zachowaniu go w bezpiecznym miejscu. Musisz ten klucz, aby przeprowadzić migrację, przywracanie lub przejąć bramy.

* Aby przeprowadzić migrację, przywracanie lub przejąć istniejącej bramy, podać klucza odzyskiwania, który został określony podczas tworzenia bramy.

### <a name="restart-the-gateway"></a>Uruchom ponownie bramy

Brama jest uruchamiany jako usługa Windows i, podobnie jak w przypadku innych usług systemu Windows, można uruchamiać i zatrzymywać go na różne sposoby. Można na przykład otwórz wiersz polecenia z podwyższonym poziomem uprawnień na komputerze, na którym jest uruchomiona brama, a następnie uruchom jedno z następujących poleceń:

* Aby zatrzymać usługę, uruchom następujące polecenie:

    `net stop PBIEgwService`

* Aby uruchomić usługę, uruchom następujące polecenie:

    `net start PBIEgwService`

### <a name="configure-a-firewall-or-proxy"></a>Konfigurowanie zapory lub serwera proxy

Aby uzyskać informacje dotyczące zapewniające informacje o serwerze proxy dla Centrum zobacz [Konfigurowanie ustawień serwera proxy](https://powerbi.microsoft.com/en-us/documentation/powerbi-gateway-proxy/).

Można sprawdzić, czy usługi zapory lub serwera proxy, nie blokuje połączenia, uruchamiając następujące polecenie w wierszu polecenia programu PowerShell. Przeprowadź test łączności z programem Bus usługi Azure. Tym tylko sprawdza łączność sieciowa, a nie ma nic robić z usług w chmurze serwera lub bramy. Pomaga określić, czy komputer można do niej dostać z Internetem.

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

Wyniki powinna wyglądać podobnie do w tym przykładzie. Jeśli **TcpTestSucceeded** nie jest spełniony, mogła zostać zablokowana przez zaporę.

```
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Jeśli chcesz niepełna podstaw wartości **nazwa_komputera** oraz **portu** wymienione w obszarze [Konfiguruj porty](#configure-ports) w dalszej części tego tematu.

Zapora może również blokuje połączenia, które Bus usługi Azure powoduje centrach danych Azure. Jeśli jest to wielkość liter, warto przeprowadzić listy sprawdzonej (odblokuj) wszystkich adresów IP dla danego regionu dla tych centrach danych. Można uzyskać listę [adresów Azure IP tutaj](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="configure-ports"></a>Skonfiguruj porty

Brama tworzy połączenia wychodzące do Bus usługi Azure. Komunikuje na porty wyjściowe: port TCP 443 (ustawienie domyślne), 5671, 5672, 9350 do 9354. Bramy nie wymaga porty przychodzące.

Więcej informacji o [hybrydowych rozwiązań](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| NAZWY DOMEN | PORTY WYJŚCIOWE | OPIS |
| ----- | ------ | ------ |
| *. analysis.windows.net | 443 | PROTOKÓŁ HTTPS |
| *. login.windows.net | 443 | PROTOKÓŁ HTTPS |
| *. servicebus.windows.net |5671 5672 | Zaawansowane kolejkowanie Protocol (AMQP) |
| *. servicebus.windows.net | 443, 9350 9354 | Detektory na usługę Bus przekazywania przez TCP (wymaga 443 uzyskania tokenu kontrola dostępu) |
| *. frontend.clouddatahub.net | 443 | PROTOKÓŁ HTTPS |
| *. core.windows.net | 443 | PROTOKÓŁ HTTPS |
| Login.microsoftonline.com | 443 | PROTOKÓŁ HTTPS |
| *. msftncsi.com | 443 | Służy do sprawdzenia łączność z Internetem, czy brama jest nieosiągalny przez usługę Power BI. |

Jeśli potrzebujesz biały adresy IP zamiast domen, możesz pobrać i za pomocą [listy zakresy adresów IP centrum danych Azure firmy Microsoft](https://www.microsoft.com/download/details.aspx?id=41653). W niektórych przypadkach połączeń Bus usługi Azure zostaną wprowadzone z adresem IP zamiast w pełni kwalifikowanych nazw domen.

### <a name="sign-in-account"></a>Konta logowania

Użytkownicy będą Zaloguj się przy użyciu, albo służbowe konto służbowe. To jest konto organizacji. Jeśli w oferująca usługi Office 365, a nie dostarczania wiadomości e-mail praca rzeczywista, może wyglądać jeff@contoso.onmicrosoft.com. Konta w ramach usługi w chmurze, są przechowywane w dzierżawie w Azure Active Directory (AAD). W większości przypadków UPN konta AAD będą zgodne adres e-mail.

### <a name="windows-service-account"></a>Konto usługi systemu Windows

Brama danych lokalnych jest skonfigurowany do używania NT SERVICE\PBIEgwService dla poświadczenia logowania usługi systemu Windows. Domyślnie ma z prawej strony dziennika jako usługa. Jest to w kontekście komputera, na którym instalujesz bramy.

Nie jest to konto używane do łączenia źródeł danych lokalnych lub konta służbowego, z którym możesz zalogować się do usług w chmurze.

##<a name="frequently-asked-questions"></a>Często zadawane pytania

### <a name="general"></a>Ogólne

**Pytanie**: źródeł danych bramy obsługuje?<br/>
**Odpowiedzi**: od tego opisu programu SQL Server.

**Pytanie**: należy bramy dla źródeł danych w chmurze, takich jak platformy SQL Azure? <br/>
**Odpowiedzi**: nie. Brama łączy do lokalnych źródeł danych tylko.

**Pytanie**: co to jest rzeczywiste usługi systemu Windows o nazwie?<br/>
**Odpowiedzi**: W usługach bramy nosi nazwę Usługa bramy Power BI organizacji.

**Pytanie**: czy istnieją wszystkie połączenia przychodzące do bramy z chmury? <br/>
**Odpowiedzi**: nie. Brama używa połączenia wychodzące do Bus usługi Azure.

**Pytanie**: co zrobić, jeśli blokować połączenia wychodzące? Co trzeba otworzyć? <br/>
**Odpowiedzi**: Zobacz porty i hosts używane bramy.


**Pytanie**: czy bramy musi być zainstalowany na tym samym komputerze jako źródła danych? <br/>
**Odpowiedzi**: nie. Brama połączy się ze źródłem danych przy użyciu uzyskane informacje o połączeniu. Brama można traktować jako aplikacja klienta w ten sposób. Po prostu muszą mieć możliwość nawiązywanie połączenia z pola Nazwa serwera dostępnej podano.


**Pytanie**: co to jest czas oczekiwania na wykonywanie zapytań do źródła danych z bramy? Co to jest zalecane architektura? <br/>
**Odpowiedzi**: w celu zmniejszenia czasu oczekiwania w sieci, należy zainstalować bramę jak najbliżej źródła danych, jak to możliwe. Możesz zainstalować bramy w źródle danych rzeczywistych, ją zminimalizować opóźnienie wprowadzone. Należy rozważyć także centrach danych. Na przykład jeśli tej usługi jest za pomocą Centrum danych zachód Stanów Zjednoczonych i masz obsługiwany w maszyn wirtualnych Azure SQL Server, można także mieć maszyn wirtualnych Azure zachód w USA. To Minimalizuj czas oczekiwania i uniknięcia opłat za wyjściowego na maszyn wirtualnych Azure.


**Pytanie**: znajdują się wszystkie wymagania dotyczące przepustowości sieci? <br/>
**Odpowiedzi**: jest zalecane jest dobrym przepustowości dla danego połączenia sieciowego. Co środowiska jest inny, a ilości danych wysyłanych wpłynie na wyniki. Używanie ExpressRoute może pomóc w celu zagwarantowania poziomu przepustowość między lokalnego i centrami danych Azure.

Za pomocą aplikacji testu szybkości Azure narzędzia innej firmy by zmierzyć, co to jest przepustowość sieci.


**Pytanie**: można uruchamiać bramy usługi Windows przy użyciu konta usługi Azure Active Directory? <br/>
**Odpowiedzi**: nie. Usługa systemu Windows musi mieć prawidłowe konta systemu Windows. Domyślnie będzie działać na przeznaczony usługi NT SERVICE\PBIEgwService.


**Pytanie**: jak wyniki wysyłanych do chmury? <br/>
**Odpowiedzi**: jest to zrobić przez Bus usługi Azure. Aby uzyskać więcej informacji zobacz sposób działania.


**Pytanie**: gdzie są moje poświadczenia przechowywane? <br/>
**Odpowiedzi**: poświadczenia dla źródła danych są przechowywane zaszyfrowane w usłudze w chmurze brama. Poświadczenia są odszyfrowywane na bramy lokalnej.

### <a name="high-availabilitydisaster-recovery"></a>Wysoka dostępność i odzyskiwanie

**Pytanie**: czy jest planowane umożliwiających scenariusze wysokiej dostępności z bramą? <br/>
**Odpowiedzi**: jest to na mapy drogowej, ale nie mamy jeszcze osi czasu.


**Pytanie**: jakie opcje są dostępne dla awarii? <br/>
**Odpowiedzi**: klucz odzyskiwania umożliwia przywracanie i przenoszenie bramy. Po zainstalowaniu bramy, określ klucz odzyskiwania.


**Pytanie**: co to jest korzyści, jakie klucz odzyskiwania? <br/>
**Odpowiedzi**: go umożliwia migrowanie lub odzyskiwanie ustawień bramy za awarii.

### <a name="troubleshooting"></a>Rozwiązywanie problemów

**Pytanie**: miejsce, w którym są dzienniki bramy? <br/>
**Odpowiedzi**: Zobacz narzędzia w dalszej części tego tematu.


**Pytanie**: jak widać, co kwerendy są wysyłane do lokalnego źródła danych? <br/>
**Odpowiedzi**: możesz włączyć śledzenie kwerendy, obejmujące kwerend wysyłana. Pamiętaj go przywrócić oryginalną wartość po zakończeniu rozwiązywania problemów. Pozostawiając kwerendy włączone śledzenie spowoduje, że dzienniki być większa.

Możesz także przeglądać narzędzia, które ma źródła danych dla zapytania śledzenia. Na przykład można rozszerzonego zdarzenia lub SQL Profiler dla programu SQL Server i usług Analysis Services.

## <a name="how-the-gateway-works"></a>Jak działa bramy

Gdy interakcji użytkownika z element, który jest połączony z lokalnego źródła danych:

1. Usługa w chmurze tworzy kwerendę, wraz z zaszyfrowane poświadczenia dla źródła danych i wysyła zapytanie do kolejki bramy przetwarzania.
1. Usługa analizuje kwerendy i umieszcza żądania do Bus usługi Azure.
1. Brama danych lokalnych sprawdza Bus usługi Azure dla oczekujących żądań.
1. Brama otrzymuje kwerendę, odszyfrowuje poświadczenie za i łączy do źródeł danych przy użyciu tych poświadczeń.
1. Brama wysyła kwerendę w źródle danych do wykonania.
1. Powrót do bramy, a następnie na usługi w chmurze, wyniki są wysyłane ze źródła danych. Usługa następnie używa wyników.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="update-to-the-latest-version"></a>Aktualizowanie do najnowszej wersji

Wiele problemów może prezentować po bramy w wersji jest nieaktualny.  Jest ogólne zaleca się upewnić, że korzystasz z najnowszej wersji.  Jeśli jeszcze nie zaktualizowane bramy, miesiąc lub dłużej, warto Rozważ możliwość zainstalowania najnowszej wersji bramy i zobacz, jeśli można odtworzyć problem.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users-"></a>Błąd: Nie można dodać użytkownika do grupy. (Użytkownicy dzienników wydajności PBIEgwService-2147463168)

Ten błąd może zostać wyświetlony, jeśli próbujesz zainstalować bramę na kontrolerze domeny, która nie jest obsługiwana. Musisz wdrożyć bramy na komputerze, którego nie ma kontrolera domeny.

## <a name="tools"></a>Narzędzia

### <a name="collecting-logs-from-the-gateway-configurator"></a>Zbieranie dzienników od konfiguratora bramy

Może zbierać kilka dzienników bramy. Zawsze zaczynają się od dziennikach!

#### <a name="installer-logs"></a>Dzienniki Instalatora

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Dzienniki konfiguracji

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Dzienniki usługi bramy przedsiębiorstwa

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Dzienniki zdarzeń

Brama zarządzania danymi i PowerBIGateway Dzienniki znajdują się w obszarze **aplikacji i usług dzienniki**.

### <a name="fiddler-trace"></a>Śledzenie fiddler

[Fiddler](http://www.telerik.com/fiddler) to bezpłatne narzędzie z Telerik, który monitoruje ruch HTTP.  Można wyświetlać tła i czwartą dzięki usłudze Power BI usługi z komputera klienta. Może to wyświetlanie błędów i innych informacji pokrewnych.

## <a name="next-steps"></a>Następne kroki
- [Utworzenie połączenia lokalnego logiki aplikacji](app-service-logic-gateway-connection.md)
- [Funkcje integracji przedsiębiorstwa](app-service-logic-enterprise-integration-overview.md)
- [Logika aplikacje łączników](../connectors/apis-list.md)