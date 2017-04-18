<properties
   pageTitle="Przykład strefy Zdemilitaryzowanej — tworzenie strefy Zdemilitaryzowanej ochrony aplikacji z zapory i NSGs | Microsoft Azure"
   description="Tworzenie strefy Zdemilitaryzowanej z zapory i sieci grup zabezpieczeń (NSG)"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor;sivae"/>

# <a name="example-2--build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs"></a>Przykład 2 — Tworzenie strefy Zdemilitaryzowanej ochrony aplikacji z zapory i NSGs

[Wróć do strony zabezpieczeń obramowanie najważniejsze wskazówki][HOME]

W tym przykładzie utworzy strefy Zdemilitaryzowanej z zapory, serwerów cztery systemu windows i grupy zabezpieczeń sieci. Również prowadzi przez wszystkich odpowiednich polecenia służące do udostępnienia szczegółowego zrozumienia każdego kroku. Istnieje także sekcji scenariusz ruch o podanie szczegółowych krok po kroku, jak ruch przechodzą przez warstw obrony w strefy Zdemilitaryzowanej. Na koniec w odwołaniach do sekcji jest pełny kod i instrukcje tworzenia tego środowiska, aby przetestować i poeksperymentować z różnych scenariuszach. 

![Ruch przychodzący strefy Zdemilitaryzowanej z NVA i NSG][1]

## <a name="environment-description"></a>Opis środowiska
W tym przykładzie jest subskrypcji, która zawiera następujące elementy:

- Dwa usług w chmurze: "FrontEnd001" i "BackEnd001"
- Wirtualna sieć "CorpNetwork" z dwóch podsieci: "FrontEnd" i "Wewnętrznej bazy danych"
- Z jednej grupy zabezpieczeń sieci, zastosowanego do obu podsieci
- Urządzenia wirtualnej sieci, w tym przykładzie zaporę NextGen Barracuda podłączony do podsieci serwera sieci Web
- Windows Server, reprezentująca serwer sieci web aplikacji ("IIS01")
- Serwery dwa okna, reprezentujących aplikacji ponownie zakończyć servers ("AppVM01", "AppVM02")
- Systemu Windows server, która reprezentuje serwera DNS ("DNS01")

>[AZURE.NOTE] Mimo że w tym przykładzie użyto zapory NextGen Barracuda, można używać wielu różnych urządzeń wirtualna sieć w tym przykładzie.

W poniższej sekcji odwołania jest skrypt programu PowerShell, który zostanie utworzona większość środowiska opisany powyżej. Tworzenie maszyny wirtualne i wirtualnych sieci jednak wykonywane są przez przykładowy skrypt nie zostały opisane szczegółowo w tym dokumencie.
 
Aby utworzyć środowisko:

  1.    Zapisywanie pliku xml konfiguracji sieci zawarte w sekcji odwołania (zaktualizowany przy użyciu nazwy, lokalizację i adresów IP adresy zgodnie z danego scenariusza)
  2.    Aktualizowanie zmienne użytkownika skrypt, aby odpowiadał środowiska, które skrypt ma być uruchamiane na (subskrypcje, nazwy usług itp.)
  3.    Wykonywanie skryptów w programie PowerShell

**Uwaga**: region oznaczony w skrypt programu PowerShell musi odpowiadać oznaczony w pliku xml konfiguracji sieci.

Po pomyślnym uruchomieniem skryptu może podjąć następujące kroki skryptu po:

1.  Konfigurowanie reguły zapory, to jest objęte do sekcji poniżej: reguły zapory.
2.  Opcjonalnie w sekcji Materiały referencyjne są dwa skrypty konfigurowania do serwera sieci web i serwera aplikacji przy użyciu aplikacji sieci web prosty, aby umożliwić testowanie za pomocą tej konfiguracji strefy Zdemilitaryzowanej.

W następnej sekcji wyjaśniono większość instrukcji skryptów względem grup zabezpieczeń sieci.

## <a name="network-security-groups-nsg"></a>Grupy zabezpieczeń sieci (NSG)
W tym przykładzie grupy NSG jest wbudowany i następnie załadować z regułami sześć. 

>[AZURE.TIP] Ogólnie mówiąc należy najpierw utworzyć określonych reguł "Zezwalaj", a następnie ostatni bardziej ogólne reguły "Odmów". Przydzielone priorytety ile wymagają tego warunki reguły są obliczane pierwszego. Gdy ruch znajduje się do określonej reguły, są obliczane w żadnych dalszych reguł. Reguły NSG można stosować w kierunku przychodzących i wychodzących (z punktu widzenia podsieci).

Deklaratywnie następujące reguły są tworzone dla ruchu przychodzącego:

1.  Ruch wewnętrzny DNS (port 53) jest dozwolona
2.  Ruch RDP (port 3389) z Internetu do dowolnego maszyn wirtualnych jest dozwolona
3.  Ruch HTTP (port 80) do NVA (zapora) z Internetu jest dozwolona
4.  Każdy ruch (wszystkie porty) z IIS01 do AppVM1 jest dozwolona
5.  Każdy ruch (wszystkie porty) z Internetu do całej odmowa VNet (zarówno podsieci)
6.  Odmowa każdy ruch (wszystkie porty) z podsieci serwera sieci Web do podsieci wewnętrznej bazy danych

Przy użyciu tych reguł związane z każdej podsieci, jeśli żądania HTTP przychodzące z Internetu na serwerze sieci web, obie reguły 3 (Zezwalaj) i 5 (odmowa) chcesz zastosować, ale ponieważ reguły 3 ma wyższy priorytet tylko będzie miało zastosowanie i reguły 5 nie będzie przychodzących do odtwarzania. Dlatego żądania HTTP będzie mieć możliwość zapory. Jeśli ten sam ruch została próby nawiązania połączenia z serwerem DNS01, będzie należy zastosować regułę 5 (Odmów) i dane czy nie można przekazać do serwera. Reguły 6 (Odmów) blokuje podsieci serwera sieci Web z rozmówcy podsieci wewnętrznej bazy danych (z wyjątkiem dozwolonego ruchu w regułach 1 i 4), to chroni sieci wewnętrznej bazy danych w przypadku kompromisów atakująca aplikacji sieci web na Frontend, tej czy mają ograniczony dostęp do sieci wewnętrznej bazy danych "chronionej" (tylko dla zasobów wyświetlane na serwerze AppVM01).

Istnieje domyślne reguły wychodzącej, która zezwala na ruch się z Internetem. W tym przykładzie mamy już zezwalania ruchu wychodzącego i nie modyfikowanie reguł ruchu wychodzącego. Do blokowania ruchu w zarówno wskazówek dotyczących dojazdu routingu zdefiniowane przez użytkownika jest wymagane, jest to omówione zostały w inny przykład, w którym można znaleźć w [głównym zabezpieczeń krawędzi dokumentu][HOME].

Powyższe zasady NSG opisanych są bardzo podobne do reguły NSG, na [przykład 1 - Tworzenie prostego strefy Zdemilitaryzowanej z NSGs][Example1]. Przejrzyj opis NSG w tym dokumencie szczegółowe przyjrzeć się każdej reguły NSG i jego atrybuty.

## <a name="firewall-rules"></a>Reguły zapory
Klient zarządzania będzie musi być zainstalowany na komputerze PC do zarządzania zapory i tworzenia konfiguracji potrzebne. Zobacz dostawcy dokumentacji z zapory (lub innych NVA) do zarządzania urządzeniem. Pozostała część tej sekcji opisując konfiguracja zapory, za pośrednictwem klienta zarządzania dostawców (to znaczy nie Azure portal lub programu PowerShell).

Instrukcje dotyczące pobierania klienta i nawiązywanie połączeń z Barracuda, w tym przykładzie użyto można znaleźć tutaj: [Barracuda NG administratora](https://techlib.barracuda.com/NG61/NGAdmin)

Na zaporę regułach przesyłania dalej muszą zostać utworzone. Ponieważ w tym przykładzie tylko kieruje ruch internetowy w powiązanych z zapory, a następnie na serwerze sieci web, jest wymagane przekazywanie tylko jedna reguła translatora adresów Sieciowych. W zaporze NextGen Barracuda, w tym przykładzie użyto reguła będzie reguły docelowego translatora adresów Sieciowych ("czasu letniego NAT") w celu przekazania ruch.

Aby utworzyć następującą regułę (lub Sprawdź istniejące zasady domyślne), rozpoczynając od pulpitu nawigacyjnego administratora NG Barracuda klienta, przejdź na kartę Konfiguracja w konfiguracji operacyjnych sekcji kliknij pozycję zestaw reguł. Siatki o nazwie "Główne reguł" zostaną wyświetlone istniejących reguł aktywne i nieaktywne na zapory. W prawym górnym rogu siatka jest małe, zielony "+" przycisku, kliknij tutaj, aby utworzyć nową regułę (Uwaga: zapory może być "zablokowana" w przypadku zmian, jeśli jest wyświetlany przycisk oznaczony "Zablokować" i nie można tworzyć lub edytować reguły, kliknij ten przycisk, aby "odblokować" zestaw reguł i Zezwalaj na edytowanie). Jeśli chcesz edytować istniejącą regułę, zaznacz tę regułę, kliknij prawym przyciskiem myszy i wybierz pozycję Edytuj regułę.

Utwórz nową regułę i podaj nazwę, na przykład "WebTraffic". 

Ikona reguły docelowego translatora adresów Sieciowych wygląda następująco: ![Ikona translatora adresów Sieciowych miejsce docelowe][2]

Reguła samej powinien wyglądać mniej więcej tak:

![Reguły zapory][3]

Tutaj dowolny ruch przychodzący adres trafienia zapory próby osiągnięcia HTTP (port 80 i 443 dla HTTPS) zostaną wysłane przez zaporę "Serwer 1 lokalny adres IP" interfejs i Przekierowanie do serwera sieci Web o adresie IP 10.0.1.5. Ponieważ ruch jest mieszczących się na porcie 80 i przechodzenie do serwera sieci web na porcie 80 bez zmian port został wymagane. Jednak z listy docelowej mogło być 10.0.1.5:8080 Jeśli słuchania serwera sieci Web na porcie 8080 więc tłumaczenia port wejściowy 80 zapory do ruchu przychodzącego portu 8080 na serwerze sieci web.

Metody połączenia należy również być oznaczony, reguły docelowego z Internetu, bardziej odpowiednia jest "Dynamiczne SNAT". 

Mimo że została utworzona reguła tylko jeden ważne jest jego priorytet jest ustawiona prawidłowo. Jeśli w siatce wszystkie reguły zapory to nowa reguła jest na dole (pod reguły "BLOCKALL") nigdy nie będą pochodzić do odtwarzania. Upewnij się, że nowa reguła dla ruchu w sieci web jest umieszczony nad reguły BLOCKALL.

Gdy zostanie utworzona reguła, musi być przypisany do zapory i następnie aktywowane, jeśli nie można to zrobić zmiany reguły nie zostały wprowadzone. W następnej sekcji opisano sposób wypychanych i aktywacji.

## <a name="rule-activation"></a>Aktywacja reguły
Z zestaw reguł modyfikować tak, aby dodać tę regułę można przekazać do zapory i aktywować zestaw reguł.

![Aktywacja reguły zapory][4]

W prawym górnym rogu klienta zarządzania są klastrze przycisków. Kliknij przycisk "Wyślij zmian", aby wysłać zmienione reguły zapory, a następnie kliknij przycisk "Aktywuj".

W aktywacji zestaw reguł zapory tej kompilacji środowiska przykład zostało wykonane. Opcjonalnie można uruchamiać skrypty Tworzenie wpisu w sekcji odwołania do dodania aplikacji do tego środowiska do testowania poniżej scenariusze ruch.

>[AZURE.IMPORTANT] Niezwykle ważne uznasz, że nie będzie naciśnięcie serwer sieci web bezpośrednio jest. Gdy przeglądarka żąda strony HTTP z FrontEnd001.CloudApp.Net, punkt końcowy HTTP (port 80) przekazuje ruch do zapory nie na serwerze sieci web. Zapora, zgodnie z regułą tworzona powyżej NAT żądające na serwerze sieci Web.

## <a name="traffic-scenarios"></a>Scenariusze ruchu

#### <a name="allowed-web-to-web-server-through-firewall"></a>(Dozwolone) Serwer sieci Web do sieci Web przez zaporę
1.  Strony internetowe użytkownika żądania HTTP z FrontEnd001.CloudApp.Net (Internet przeciwległych usługi w chmurze)
2.  Chmury usługi przebiegów ruch przez otwarte punktu końcowego na porcie 80 interfejsem lokalnym zapory z 10.0.1.4:80
3.  Przetwarzanie reguły przychodzące podsieci Frontend rozpoczynają się od:
  1.    NSG reguły 1 (DNS) nie zastosować, przejście do następnego reguły
  2.    2 reguły NSG (RDP) nie zastosować, przejście do następnego reguły
  3.    Stosowanie NSG zasady 3 (Internet do Zapora systemu Windows), ruch jest reguły dozwolonych, Zatrzymaj przetwarzanie
4.  Ruch trafienia wewnętrzny adres IP zapory (10.0.1.4)
5.  Reguły przesyłania dalej zapory Zobacz to jest ruch na porcie 80, przekierowuje je na serwerze sieci web IIS01
6.  IIS01 oczekuje na ruchu w sieci web, odbiera żądania i uruchamia, przetwarzania żądania
7.  IIS01 programu SQL Server na AppVM01 monituje o podanie informacji
8.  Nie reguł ruchu wychodzącego podsieci Frontend ruch jest dozwolony
9.  Przetwarzanie reguły przychodzące podsieci wewnętrznej bazy danych rozpoczynają się od:
  1.    NSG reguły 1 (DNS) nie zastosować, przejście do następnego reguły
  2.    2 reguły NSG (RDP) nie zastosować, przejście do następnego reguły
  3.    NSG zasady 3 (Internet zaporą) nie zastosować, przejście do następnego reguły
  4.    4 reguły NSG (IIS01 do AppVM01) zastosować ruch jest dozwolony, Zatrzymaj przetwarzanie reguł
10. AppVM01 otrzyma kwerendy SQL i odpowiedzi
11. Ponieważ istnieją żadne reguły wychodzącej podsieci wewnętrznej bazy danych odpowiedź jest dozwolona
12. Przetwarzanie reguły przychodzące podsieci Frontend rozpoczynają się od:
  1.    Istnieje reguła nie NSG, która dotyczy przychodzące ruchu z podsieci wewnętrznej bazy danych do podsieci serwera sieci Web, więc żaden z NSG zasady, stosowanie
  2.    Reguła systemowa domyślne zezwolenia na ruch między podsieci umożliwia ruch więc ruch jest dozwolony.
13. Serwer usług IIS odbiera odpowiedź SQL i kończy odpowiedź HTTP i wysyła do żądającego
14. Ponieważ jest to sesji translatora adresów Sieciowych przez zaporę, miejsce docelowe odpowiedzi (początkowo) jest dla zapory
15. Zapora odbiera odpowiedź na serwerze sieci Web i przekazuje je dalej do użytkownika Internet
16. Ponieważ istnieją żadne reguły wychodzącej podsieci Frontend odpowiedzi jest dozwolony, a użytkownik Internetu otrzymuje żądanej strony sieci web.

#### <a name="allowed-rdp-to-backend"></a>(Dozwolone) RDP do wewnętrznej bazy danych
1.  Administrator serwera w Internecie żądania sesja RDP AppVM01 na BackEnd001.CloudApp.Net:xxxxx, gdzie xxxxx jest numer portu losowo przypisany RDP do AppVM01 (przypisanego portu można znaleźć na Azure Portal lub za pośrednictwem programu PowerShell)
2.  Ponieważ zapory tylko nasłuchują na adres FrontEnd001.CloudApp.Net, nie należy zrobić z tym ruch
3.  Przetwarzanie reguły przychodzące podsieci wewnętrznej bazy danych rozpoczynają się od:
  1.    NSG reguły 1 (DNS) nie zastosować, przejście do następnego reguły
  2.    Zastosuj NSG reguły 2 (RDP), jest reguły dozwolonych, Zatrzymaj przetwarzanie
4.  Żadne reguły wychodzącej zasady domyślne i zwrotu ruch jest dozwolony
5.  Sesja RDP jest włączona
6.  AppVM01 monituje o podanie hasła nazwa użytkownika

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Dozwolone) Wyszukiwanie serwera DNS sieci Web dla serwera DNS
1.  Web Server, IIS01, wymagań w www.data.gov strumieniowego źródła danych, ale musi rozpoznać adresu.
2.  Konfiguracja sieciowa dla list VNet DNS01 (10.0.2.4 podsieci wewnętrznej bazy danych) jako podstawowy serwer DNS, IIS01 przesyła żądanie DNS DNS01
3.  Nie reguł ruchu wychodzącego podsieci Frontend ruch jest dozwolony
4.  Przetwarzanie reguły przychodzące podsieci wewnętrznej bazy danych rozpoczynają się od:
  1.     Zastosuj NSG reguły 1 (DNS), jest reguła dozwolonych, Zatrzymaj przetwarzanie
5.  Serwer DNS odbiera żądania
6.  Serwer DNS nie ma adres przechowywanych w pamięci podręcznej i z pytaniem główny serwer DNS w Internecie
7.  Nie reguły ruchu wychodzącego podsieci wewnętrznej bazy danych ruch jest dozwolony
8.  Serwer Internet DNS odpowiada, ponieważ w tej sesji zostało zainicjowane wewnętrznie, odpowiedź jest dozwolona
9.  Serwer DNS umieszcza odpowiedź w pamięci podręcznej i odpowiada żądania początkowego powrót do IIS01
10. Nie reguły ruchu wychodzącego podsieci wewnętrznej bazy danych ruch jest dozwolony
11. Przetwarzanie reguły przychodzące podsieci Frontend rozpoczynają się od:
  1.    Istnieje reguła nie NSG, która dotyczy przychodzące ruchu z podsieci wewnętrznej bazy danych do podsieci serwera sieci Web, więc żaden z NSG zasady, stosowanie
  2.    Reguła systemowa domyślne zezwolenia na ruch między podsieci umożliwi ruch, więc ruch jest dozwolony
12. IIS01 odbiera odpowiedź z DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(Dozwolone) Plik dostępu do serwera sieci Web na AppVM01
1.  IIS01 zapytanie dotyczące pliku w usłudze AppVM01
2.  Nie reguł ruchu wychodzącego podsieci Frontend ruch jest dozwolony
3.  Przetwarzanie reguły przychodzące podsieci wewnętrznej bazy danych rozpoczynają się od:
  1.    NSG reguły 1 (DNS) nie zastosować, przejście do następnego reguły
  2.    2 reguły NSG (RDP) nie zastosować, przejście do następnego reguły
  3.    NSG zasady 3 (Internet zaporą) nie zastosować, przejście do następnego reguły
  4.    4 reguły NSG (IIS01 do AppVM01) zastosować ruch jest dozwolony, Zatrzymaj przetwarzanie reguł
4.  AppVM01 odbiera żądanie i odpowiada pliku (przy założeniu, że dostępu)
5.  Ponieważ istnieją żadne reguły wychodzącej podsieci wewnętrznej bazy danych odpowiedź jest dozwolona
6.  Przetwarzanie reguły przychodzące podsieci Frontend rozpoczynają się od:
  1.    Istnieje reguła nie NSG, która dotyczy przychodzące ruchu z podsieci wewnętrznej bazy danych do podsieci serwera sieci Web, więc żaden z NSG zasady, stosowanie
  2.    Reguła systemowa domyślne zezwolenia na ruch między podsieci umożliwia ruch więc ruch jest dozwolony.
7.  Serwer usług IIS odbiera plik

#### <a name="denied-web-direct-to-web-server"></a>(Odmowa) Bezpośrednio do serwera sieci Web w sieci Web
Ponieważ serwer sieci Web, IIS01 i zapory znajdują się w tym samym usługi w chmurze ich udziału w ten sam publicznej przeciwległych adres IP. Dlatego ruch HTTP będzie przekierowywany do zapory. Gdy żądanie może być pomyślnie obsłużone, nie może przejść bezpośrednio do serwera sieci Web, przeszła, zgodnie z przeznaczeniem przez zaporę najpierw. Zobacz pierwszy scenariusz w tej sekcji przepływ ruchu.

#### <a name="denied-web-to-backend-server"></a>(Odmowa) Sieci Web do serwera wewnętrznej bazy danych
1.  Użytkownik Internetu próbuje uzyskać dostęp do plików na AppVM01 za pośrednictwem usługi BackEnd001.CloudApp.Net
2.  Ponieważ nie punkty końcowe są otwarte do udziału plików, to nie może przekazać usług w chmurze, a nie połączenia z serwerem
3.  Jeśli punkty końcowe były otwarte jakiegoś powodu, reguły NSG 5 (Internet do VNet) umożliwia zablokowanie ruch

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(Odmowa) Wyszukiwanie DNS sieci Web dla serwera DNS
1.  Użytkownik Internetu spróbuje wyszukać wewnętrznych rekordu DNS na DNS01 za pośrednictwem usługi BackEnd001.CloudApp.Net
2.  Ponieważ nie punkty końcowe są otwarte w systemie DNS, to nie może przekazać usług w chmurze, a nie połączenia z serwerem
3.  Jeśli punkty końcowe były otwarte jakiegoś powodu, reguły NSG 5 (Internet do VNet) umożliwia zablokowanie ruch (Uwaga: tej reguły 1 (DNS) nie ma zastosowania dwóch powodów, najpierw źródłowy adres jest internet, ta reguła dotyczy tylko lokalne VNet jako źródła, również jest Zezwalaj reguła, aby nigdy nie może go odmówić ruch)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(Odmowa) Web SQL dostępu przez zaporę
1.  Internet użytkownik zażąda danych SQL z FrontEnd001.CloudApp.Net (Internet przeciwległych usługi w chmurze)
2.  Ponieważ nie punkty końcowe są otwarty SQL, to nie będzie przebiegać usług w chmurze i nie osiągnięcia zapory
3.  Jeśli punkty końcowe były otwarte jakiegoś powodu, podsieci Frontend rozpoczyna się przetwarzanie reguły przychodzące:
  1.    NSG reguły 1 (DNS) nie zastosować, przejście do następnego reguły
  2.    2 reguły NSG (RDP) nie zastosować, przejście do następnego reguły
  3.    Stosowanie NSG reguła 2 (Internet do Zapora systemu Windows), ruch jest reguły dozwolonych, Zatrzymaj przetwarzanie
4.  Ruch trafienia wewnętrzny adres IP zapory (10.0.1.4)
5.  Zapory występują żadne reguły przesyłania dalej dla SQL i pomija ruchu

## <a name="conclusion"></a>Wnioski
Jest to stosunkowo bezpośrednio do przodu sposób ochrony aplikacji za pomocą zapory i izolowanie podsieci wewnętrznej z ruchu przychodzącego.

Więcej przykładów i omówienie granice zabezpieczeń sieci można znaleźć [tutaj][HOME].

## <a name="references"></a>Odwołania
### <a name="main-script-and-network-config"></a>Skrypt głównym i konfiguracji sieci
Pełna skrypt należy zapisać w pliku skryptu programu PowerShell. Zapisywanie konfiguracji sieci w pliku o nazwie "NetworkConf2.xml".
Zmodyfikuj zmienne zdefiniowane przez użytkownika, stosownie do potrzeb. Uruchom skrypt, a następnie postępuj zgodnie z instrukcjami instalacji reguły zapory powyżej.

#### <a name="full-script"></a>Pełna skryptu
Ten skrypt będzie, na podstawie zmiennych zdefiniowanych przez użytkownika:

1.  Nawiązywanie połączenia z subskrypcji usługi Azure
2.  Utwórz nowe konto miejsca do magazynowania
3.  Tworzenie nowego VNet i dwóch podsieci, zgodnie z definicją w pliku konfiguracji sieci
4.  Tworzenie 4 systemu windows server maszyny wirtualne
5.  Konfigurowanie NSG w tym:
  - Tworzenie NSG
  - Wypełnianie go za pomocą reguł
  - Wiązanie NSG do odpowiedniej podsieci

Ten skrypt programu PowerShell powinny być uruchamiane lokalnie na, że połączenie internetowe komputera lub serwera.

>[AZURE.IMPORTANT] Po uruchomieniu tego skryptu mogą istnieć ostrzeżenia i inne komunikaty informacyjne pop w programie PowerShell. Tylko komunikaty o błędach w kolorze czerwonym są przyczyny dotyczą.


    <# 
     .SYNOPSIS
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet (plus the NVA on the FrontEnd subnet)
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared
      
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.
    
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       myFirewall - 10.0.1.4
       IIS01      - 10.0.1.5
     
      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6
    
    #>
    
    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()
    
    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section
    
      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    
      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"
    
      # Service Details
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 1
        #       - The AppVM1 Server must be VM 2
        #       - The DNS server must be VM 4
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"
    
    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #
    
      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop
    
      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}
    
      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop
    
    # Validation
    $FatalError = $false
    
    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}
    
    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}
    
    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}
    
    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop
    
    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: Web traffic goes through the firewall, so we'll need to set up a HTTP endpoint.
                #       Also, the firewall will be redirecting web traffic to a new IP and Port in a
                #       forwarding rule, so the HTTP endpoint here will have the same public and local
                #       port and the firewall will do the NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) to $($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
            -Protocol *
        
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *
    
        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host


#### <a name="network-config-file"></a>Plik konfiguracyjny sieci
Zapisz ten plik xml z lokalizacją zaktualizowane i dodać łącze do tego pliku do zmiennej $NetworkConfigFile w skrypt powyżej.
    
    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Przykładowe skrypty aplikacji
Jeśli chcesz zainstalować przykładową aplikację dla tej i inne przykłady strefy Zdemilitaryzowanej jedną dostarczono na następujące łącze: [Przykładowy skrypt aplikacji][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Ruch przychodzący strefy Zdemilitaryzowanej z NSG"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Ikona translatora adresów Sieciowych miejsce docelowe"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Reguły zapory"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Aktywacja reguły zapory"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
