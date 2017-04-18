<properties
   pageTitle="Konfigurowanie aplikacji współzależności Monitor (ADM) w pakiecie zarządzania operacje (usługi OMS) | Microsoft Azure"
   description="Monitorowanie zależności aplikacji (ADM) to rozwiązanie usługi operacje zarządzania pakietu (OMS), które automatycznie wykrywa składniki aplikacji w systemach Windows i Linux i mapy komunikacji między usługami.  Ten artykuł zawiera informacje dotyczące wdrażania ADM w środowisku oraz korzystania z niego w różnych scenariuszach."
   services="operations-management-suite"
   documentationCenter=""
   authors="daseidma"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/28/2016"
   ms.author="daseidma;bwren" />

# <a name="configuring-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Konfigurowanie aplikacji współzależności monitory usługi pakietu w operacji zarządzania (OMS)
![Ikona zarządzania alertu](media/operations-management-suite-application-dependency-monitor/icon.png) Monitorowanie zależności aplikacji (ADM) automatycznie wykrywa składniki aplikacji w systemach Windows i Linux i mapy komunikacji między usługami. Umożliwia wyświetlanie serwery, jak można je sobie wyobrazić — jako połączone systemy, które dostarczają usługi krytyczne.  Monitorowanie zależności aplikacji przedstawia połączenia między serwerami, procesy, i porty w dowolnej architekturze połączenia TCP z konfiguracji wymagane inne niż instalacji agenta.

W tym artykule opisano konfigurowanie Monitor zależności aplikacji i rozpoczęcie korzystania.  Aby uzyskać informacje na temat korzystania z ADM zobacz [Korzystanie z aplikacji współzależności monitora rozwiązanie w pakiecie operacje zarządzania usługi (OMS)](operations-management-suite-application-dependency-monitor.md)

>[AZURE.NOTE]Monitorowanie zależności aplikacji jest obecnie Podgląd prywatne.  Można zażądać dostępu do podglądu prywatne ADM w [https://aka.ms/getadm](https://aka.ms/getadm).
>
>Podczas podglądu prywatne wszystkie konta usługi OMS mają nieograniczony dostęp do ADM.  Węzły ADM jest bezpłatna, ale dziennika analizy danych typu AdmComputer_CL i AdmProcess_CL będzie mierzony jak inne rozwiązanie.
>
>Po ADM wprowadza publicznej podglądu, będzie dostępna tylko dla klientów bezpłatnych i płatnych wgląd i analizy usługi OMS ceny plan.  Warstwa bezpłatnego konta będą ograniczone do 5 węzłów ADM.  Jeśli bierzesz udział w podglądzie prywatne i nie jest zarejestrowany w usługi OMS ceny Plan, gdy ADM wprowadza publicznej Podgląd ADM zostanie wyłączona w tym czasie. 



## <a name="connected-sources"></a>Połączonych źródeł
W poniższej tabeli opisano połączonych źródeł, które są obsługiwane przez to rozwiązanie ADM.

| Połączone źródła | Obsługiwane | Opis |
|:--|:--|:--|
| [Czynniki systemu Windows](../log-analytics/log-analytics-windows-agents.md) | Tak | ADM analizuje i zbiera dane z komputerami agentów systemu Windows.  <br><br>Uwaga Oprócz agenta usługi OMS agenci Windows wymagają programu Microsoft Agent współzależności.  Zobacz [obsługiwane systemy operacyjne](#supported-operating-systems) , aby uzyskać pełną listę wersji systemu operacyjnego. |
| [Czynniki Linux](../log-analytics/log-analytics-linux-agents.md) | Tak | ADM analizuje i zbiera dane z komputerami agentów Linux.  <br><br>Uwaga Oprócz agenta usługi OMS agentów Linux wymagają programu Microsoft Agent zależności.  Zobacz [obsługiwane systemy operacyjne](#supported-operating-systems) , aby uzyskać pełną listę wersji systemu operacyjnego. |
| [Grupa Zarządzanie SCOM](../log-analytics/log-analytics-om-agents.md) | Tak | ADM analizuje i zbiera dane z systemu Windows i Linux oraz czynników w grupie Zarządzanie SCOM połączonego. <br><br>Bezpośrednie połączenie z komputera agenta SCOM usługi OMS jest wymagane. Dane są wysyłane bezpośrednio z przekazane w grupie Zarządzanie repozytorium usługi OMS.|
| [Konto Azure miejsca do magazynowania](../log-analytics/log-analytics-azure-storage.md) | Brak | ADM zbiera dane z komputerów agenta, dlatego żadne dane z zbieranie z magazynu Azure. |

Należy zauważyć, że ADM obsługuje tylko platformy 64-bitowej.

W systemie Windows, Microsoft monitorowania agenta (MMA) jest używany przez zarówno SCOM, jak i usługi OMS do zbierania i wysyłanie monitorowanie danych.  (Ten agent nosi nazwę agenta SCOM, agenta usługi OMS, MMA lub bezpośredni agenta, w zależności od kontekstu.)  SCOM i usługi OMS zawierają różne poza wersji pole MMA, ale tych wersji można każdy raport do SCOM, aby usługi OMS lub oba.  Na Linux, agenta usługi OMS dla Linux zbiera i wysyła monitorowanie danych usługi OMS.  Za pomocą ADM na serwerach czynnikami bezpośredni usługi OMS lub na serwerach, które zostały dołączone do usługi OMS za pośrednictwem grup zarządzania SCOM.  Do celów tej dokumentacji odnoszą się do wszystkich tych czynników — na Linux lub Windows, czy połączony g SCOM lub bezpośrednio do usługi OMS — jako "Agent usługi OMS", chyba że nazwę wdrażania agenta jest potrzebna kontekstu.

ADM agent nie przesyła wszystkie dane, a nie wymaga zmiany zapory lub porty.  Osoby ADM zawsze przesyłania danych przez agenta usługi OMS do usługi OMS, bezpośrednio lub za pośrednictwem usługi OMS bramy.

![Czynniki ADM](media/operations-management-suite-application-dependency-monitor/agents.png)

Jeśli jesteś klientem SCOM z grupą zarządzania połączony z usługi OMS:

- Jeśli usługi agentów SCOM uzyskać dostęp do Internetu, aby nawiązać połączenie usługi OMS, dodatkowa konfiguracja nie jest wymagane.  
- Usługi agentów SCOM nie ma dostępu usługi OMS w Internecie, należy skonfigurować bramę usługi OMS do pracy z SCOM.
  
Jeśli korzystasz z usługi OMS Agent bezpośredni, musisz skonfigurować agenta usługi OMS do łączenia się z usługi OMS lub Centrum usługi OMS.  Brama usługi OMS można pobrać z [https://www.microsoft.com/en-us/download/details.aspx?id=52666](https://www.microsoft.com/en-us/download/details.aspx?id=52666)


### <a name="avoiding-duplicate-data"></a>Można uniknąć zduplikowanych danych

Jeśli jesteś klientem SCOM, nie należy konfigurować usługi agentów SCOM można komunikować się bezpośrednio do usługi OMS lub dane są przedstawiane dwa razy.  W ADM spowoduje na komputerach znajdującego się na liście komputera dwa razy.

Konfiguracja usługi OMS powinny mieć w jednej z następujących lokalizacji:

- Panel pakiet administracyjny operacje konsoli SCOM dla komputerów zarządzanych
- Azure konfiguracji operacyjnych wniosków we właściwościach MMA

Za pomocą obu konfiguracjach z tym samym obszarem roboczym w każdej spowoduje Duplikowanie danych. Użycie zarówno z różnych obszarów roboczych może spowodować w konflikcie konfiguracji (jeden z rozwiązanie ADM włączony i bez), która może uniemożliwić ułożony do ADM całkowicie danych.  

Należy zauważyć, że nawet wtedy, gdy samej maszyny nie jest podany w konfiguracji usługi OMS konsoli SCOM, jeśli jest aktywny grupą wystąpień, takich jak "Grupy systemu Windows Server wystąpienia", może być nadal powoduje odbierania usługi OMS konfigurację komputera za pomocą SCOM.



## <a name="management-packs"></a>Pakiety zarządzania
Po aktywowaniu ADM w obszarze roboczym usługi OMS 300KB pakiet zarządzania są wysyłane do wszystkich Microsoft monitorowanie agentów w danym obszarze roboczym.  Korzystania z agentów SCOM w [połączonych grupy zarządzania](../log-analytics/log-analytics-om-agents.md)pakietu zarządzania ADM zostanie wdrożony z SCOM.  Jeśli agentów są podłączone bezpośrednio, CR będą dostarczane przez usługi OMS.

CR nosi nazwę Microsoft.IntelligencePacks.ApplicationDependencyMonitor*.  Zostanie zapisany *%Programfiles%\Microsoft monitorowania Agent\Agent\Health State\Management dodatki\*.  Źródła danych używanego przez pakiet administracyjny jest *% Program files%\Microsoft monitorowania Agent\Agent\Health usługi State\Resources\<AutoGeneratedID > \Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll*.



## <a name="configuration"></a>Konfiguracja
Oprócz systemu Windows i Linux oraz komputery mają agenta zainstalowany i połączony z usługi OMS, Instalatora agenta współzależności musi być pobrane z rozwiązanie ADM, a następnie jako główny lub administrator na wszystkich serwerach zarządzanych.  Po zainstalowaniu agenta ADM na serwerze raportów do usługi OMS ADM zależności mapy pojawi się w ciągu 10 minut.  Jeśli masz problemy, Wyślij wiadomość e-mail [oms-adm-support@microsoft.com](mailto:oms-adm-support@microsoft.com).


### <a name="migrating-from-bluestripe-factfinder"></a>Migrowanie z BlueStripe FactFinder
Monitorowanie zależności aplikacji będzie przedstawiana technologii BlueStripe do usługi OMS etapami. FactFinder nadal jest obsługiwane w przypadku istniejących klientów, ale nie jest już dostępna dla poszczególnych zakupów.  Ta wersja zapoznawcza agenta współzależności można komunikować się tylko z usługi OMS.  Jeśli jesteś bieżącego odbiorcy FactFinder, wskaż zestaw serwerów test ADM, które nie są zarządzane przez FactFinder. 

### <a name="download-the-dependency-agent"></a>Pobierz agenta współzależności
Oprócz agenta zarządzania firmy Microsoft (MMA) i agenta Linux usługi OMS, które zapewniają połączenia między komputerem a usługi OMS, wszystkich komputerach analizowane przez Monitor zależności aplikacji należy współzależności Agent jest zainstalowany.  W systemie Linux agenta usługi OMS Linux musi być zainstalowany przed agenta zależności. 

![Kafelek aplikacji Monitor współzależności](media/operations-management-suite-application-dependency-monitor/tile.png)

Aby można było pobrać agenta współzależności, kliknij przycisk **Skonfigurować rozwiązanie** kafelku **Monitor zależności aplikacji** , aby otworzyć karta **Agent zależności** .  Karta Agent współzależności zawiera łącza dla systemu Windows i Linux oraz czynników. Kliknij odpowiednie łącze, aby pobrać każdego agenta. Zobacz poniżej, aby uzyskać szczegółowe informacje na temat instalowania agenta z innych systemów.

### <a name="install-the-dependency-agent"></a>Instalowanie agenta współzależności

#### <a name="microsoft-windows"></a>Microsoft Windows
Aby zainstalować lub odinstalować agent są wymagane uprawnienia administratora.

Agent zależności jest zainstalowana na komputerach z systemem Windows z ADM-Agent-Windows.exe. Po uruchomieniu ten plik wykonywalny bez żadnych opcji, zostanie go uruchomić kreatora, który można wykonać, aby interakcyjnie instalacji.  

Zainstalować agenta zależność na poszczególnych komputerach systemu Windows, wykonaj następujące czynności.

1.  Upewnij się, że agent usługi OMS jest zainstalowany, zgodnie z instrukcjami zawartymi w przypadku komputerów Połącz bezpośrednio do usługi OMS.
2.  Pobierz program Windows agent i uruchom go, używając następującego polecenia.<br>*ADM-Agent-Windows.exe*
3.  Wykonaj kreatora, aby zainstalować agenta.
4.  Jeśli Agent zależności nie można uruchomić, sprawdź dzienniki, aby uzyskać szczegółowe informacje o błędzie. W przypadku czynników systemu Windows katalogu dziennika jest *C:\Program Files\BlueStripe\Collector\logs*. 

Zależność Agent dla systemu Windows można usunąć przez administratora za pomocą Panelu sterowania.


#### <a name="linux"></a>Linux
Instalowanie i Konfigurowanie agenta jest wymagane dostępu na poziomie administratora.

Agent współzależności został zainstalowany na komputerach Linux z ADM-Agent-Linux64.bin skrypt powłoki z samodzielnie się format binarny. Można uruchomić plik po lub dodać uprawnienia do pliku się wykonywania.
 
Wykonaj następujące czynności, zainstalować agenta zależność na każdym komputerze Linux.

1.  Sprawdź, czy agent usługi OMS jest zainstalowany, zgodnie z instrukcjami zawartymi w [Zbierz dane i zarządzać z komputerów Linux.  To aby musi być zainstalowana przed agenta współzależności Linux](https://technet.microsoft.com/library/mt622052.aspx).
2.  Zainstaluj agenta Linux oraz zależność jako główny przy użyciu następującego polecenia.<br>*Pokaż ADM-Agent-Linux64.bin*.
3.  Jeśli Agent zależności nie można uruchomić, sprawdź dzienniki, aby uzyskać szczegółowe informacje o błędzie. W przypadku czynników Linux katalogu dziennika jest */var/opt/microsoft/dependency-agent/log*.

### <a name="uninstalling-the-dependency-agent-on-linux"></a>Odinstalowanie agenta zależności w systemie Linux
Aby całkowicie odinstalować agenta współzależności z Linux, należy usunąć agent sam i serwera proxy, która jest instalowana automatycznie z agentem.  Po odinstalowaniu zarówno przy użyciu następującego polecenia pojedynczy.

    rpm -e dependency-agent dependency-agent-connector


### <a name="installing-from-a-command-line"></a>Instalowanie przy użyciu wiersza polecenia
Poprzedniej sekcji zawiera wskazówki dotyczące instalowania agenta monitora współzależności przy użyciu opcji domyślnych.  Poniżej sekcjach wskazówki dotyczące instalowania agenta z wiersza polecenia przy użyciu własnych opcji.

#### <a name="windows"></a>Systemu Windows
Opcje z poniższej tabeli służą do wykonywania instalacji z wiersza polecenia. Aby wyświetlić listę instalacji Flagi uruchom Instalatora z? Flaga w następujący sposób.

    ADM-Agent-Windows.exe /?

| Flagi | Opis |
|:--|:--|
| /S | Folderze sieciowym bez monitowania użytkownika. |

Pliki Agent współzależności systemu Windows są umieszczane w *C:\Program Files\BlueStripe\Collector* domyślnie.


#### <a name="linux"></a>Linux
Użyj opcji z poniższej tabeli, aby wykonać instalację. Aby wyświetlić listę instalacji uruchomienia flagi instalacja programów Pomoc flagi w następujący sposób.

    ADM-Agent-Linux64.bin -help

| Opis flagi
|:--|:--|
| -s | Folderze sieciowym bez monitowania użytkownika. |
| — Sprawdzanie | Sprawdza uprawnienia i systemu operacyjnego, ale nie można zainstalować agenta. |

Pliki agenta zależności są umieszczane w następujących katalogach.

| Pliki | Lokalizacja |
|:--|:--|
| Podstawowe pliki | /usr/lib/bluestripe-Collector |
| Pliki dziennika | /var/OPT/Microsoft/Dependency-Agent/log |
| Pliki konfiguracji | /etc/OPT/Microsoft/Dependency-Agent/config |
| Pliki wykonywalne usługi | /sbin/bluestripe-Collector<br>/sbin/bluestripe-Collector-Manager |
| Pliki magazynów binarny | /var/OPT/Microsoft/Dependency-Agent/Storage |



## <a name="troubleshooting"></a>Rozwiązywanie problemów
Jeśli występują problemy z Monitor zależności aplikacji, możesz gromadzenie informacji umożliwiających rozwiązywanie problemów z wielu składników, korzystając z następujących informacji.

### <a name="windows-agents"></a>Czynniki systemu Windows

#### <a name="microsoft-dependency-agent"></a>Agent Microsoft współzależności
Aby wygenerować dane dotyczące rozwiązywania problemów z agentem współzależności, Otwórz okno wiersza polecenia jako administrator, a następnie uruchom skrypt CollectBluestripeData.vbs przy użyciu następującego polecenia.  Możesz dodać flagę — pomoc, aby wyświetlić dodatkowe opcje.

    cd C:\Program Files\Bluestripe\Collector\scripts
    cscript CollectDependencyData.vbs

Obsługa pakiet danych zostanie zapisany w katalogu % USERPROFILE % dla bieżącego użytkownika.  Możesz użyć — plik <filename> opcję, aby zapisać go w innej lokalizacji.

#### <a name="microsoft-dependency-agent-management-pack-for-mma"></a>Pakiet administracyjny agenta firmy Microsoft zależności dla MMA
Pakiet zarządzania agenta współzależności uruchamia wewnątrz agenta zarządzania firmy Microsoft.  Odbieranych danych agenta zależności i przekazuje je dalej do usługi w chmurze ADM.
  
Upewnij się, że pakiet zarządzania są pobierane, wykonując następujące czynności.

1.  Znajdź plik o nazwie Microsoft.IntelligencePacks.ApplicationDependencyMonitor.mp C:\Program Files\Microsoft monitorowania Agent\Agent\Health State\Management dodatki.  
2.  Jeśli plik nie zostanie znaleziony i agent jest połączony z grupą zarządzania SCOM, sprawdź, czy zostały zaimportowane do SCOM, zaznaczając pole wyboru pakietów zarządzania w obszarze roboczym Administracja konsoli operacji.

CR ADM zapisuje zdarzenia w dzienniku zdarzeń systemu Windows menedżera operacji.  Dziennik można [przeszukiwać w usługi OMS](../log-analytics/log-analytics-log-searches.md) za pośrednictwem rozwiązania Dziennik systemu, miejsce, w którym można skonfigurować do przekazywania, które pliki dziennika.  Jeśli włączono zdarzeń debugowania, ich są zapisywane w dzienniku zdarzeń aplikacji ze źródłem zdarzenia *AdmProxy*.

#### <a name="microsoft-monitoring-agent"></a>Monitorowanie agenta firmy Microsoft
Zbieranie diagnostyczne śledzenia, Otwórz okno wiersza polecenia jako administrator, a następnie uruchom następujące polecenia: 

    cd \Program Files\Microsoft Monitoring Agent\Agent\Tools
    net stop healthservice 
    StartTracing.cmd ERR
    net start healthservice

Śledzenia są zapisywane w c:\Windows\Logs\OpsMgrTrace.  Możesz wyłączyć śledzenie z StopTracing.cmd.


### <a name="linux-agents"></a>Czynniki Linux

#### <a name="microsoft-dependency-agent"></a>Agent Microsoft współzależności
Generowanie dane dotyczące rozwiązywania problemów z agentem współzależności, zaloguj się przy użyciu konta z uprawnieniami sudo lub główny i uruchom następujące polecenie.  Możesz dodać flagę — pomoc, aby wyświetlić dodatkowe opcje.

    /usr/lib/bluestripe-collector/scripts/collect-dependency-agent-data.sh

Obsługa pakiet danych jest zapisywany w /var/opt/microsoft/dependency-agent/log (Jeśli główny) w obszarze Katalog instalacji agenta lub do katalogu macierzystego użytkownika skrypt (jeśli innego niż główny).  Możesz użyć — plik <filename> opcję, aby zapisać go w innej lokalizacji.

#### <a name="microsoft-dependency-agent-fluentd-plug-in-for-linux"></a>Wtyczki Linux Fluentd agenta firmy Microsoft współzależności
Wtyczka Fluentd agenta współzależności uruchamia wewnątrz usługi OMS agenta Linux.  Odbieranych danych agenta zależności i przekazuje je dalej do usługi w chmurze ADM.  

Dzienniki są zapisywane w następujących dwóch plików.

- /var/OPT/Microsoft/omsagent/log/omsagent.log
- /var/log/messages

#### <a name="oms-agent-for-linux"></a>Agent usługi OMS Linux
Zasób rozwiązywania problemów dla serwerów Linux nawiązywanie połączenia z usługi OMS można znaleźć tutaj: [https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md) 

Dzienniki agenta usługi OMS Linux znajdują się w */var/i/microsoft/omsagent/dziennika-*.  

Dzienniki dla omsconfig (konfiguracja agenta) znajdują się w */var/i/microsoft/omsconfig/dziennika-*.
 
Składniki OMI i SCX, które dostarczają dane dotyczące wydajności metryki w dzienniku znajdują się w */var i omi i zaloguj ponownie/* i */var/opt/microsoft/scx/log*.


## <a name="data-collection"></a>Zbieranie danych
Można się spodziewać każdego agenta przekazują około 25MB raz dziennie, w zależności od tego, jak złożone są usługi zależności systemu.  ADM współzależności dane są wysyłane przez każdego agenta co 15 sekund.  

ADM Agent zajmuje zwykle 0,1% pamięci i 0,1% Procesora systemu.


## <a name="supported-operating-systems"></a>Obsługiwane systemy operacyjne
W poniższych sekcjach wymieniono obsługiwane systemy operacyjne agenta zależności.   32-bitowej architektury nie są obsługiwane żadnego systemu operacyjnego.

### <a name="windows-server"></a>System Windows Server
- Windows Server 2012 R2
- System Windows Server 2012
- Windows Server 2008 R2 z dodatkiem SP1

### <a name="windows-desktop"></a>Pulpit systemu Windows
- Uwaga: Systemu Windows 10 nie jest jeszcze obsługiwane
- System Windows 8.1
- Windows 8
- Windows 7

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Czerwone funkcję Enterprise Linux, CentOS Linux i Linux Oracle (z jądro RHEL)
- Obsługiwane są tylko domyślne i SMP Linux jądra wersjach.
- Niestandardowe jądra udostępnia, takie jak rozszerzenia adresu fizycznego i Xen, nie jest obsługiwane dla dowolnego rozkładu Linux. Na przykład systemie wersji ciąg "2.6.16.21-0.8-xen" nie jest obsługiwane.
- Niestandardowe jądra, tym ponownych jądra standardowy, nie są obsługiwane.
- Centos Plus jądra nie jest obsługiwane.
- Oracle nierozerwalny jądra (UEK) jest objęta w innej sekcji poniżej.


#### <a name="red-hat-linux-7"></a>Czerwone funkcję Linux 7
| Wersja systemu operacyjnego | Wersja jądra |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |

#### <a name="red-hat-linux-6"></a>Czerwone funkcję Linux 6
| Wersja systemu operacyjnego | Wersja jądra |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6,7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Czerwone funkcję Linux 5
| Wersja systemu operacyjnego | Wersja jądra |
|:--|:--|
| 5.8 | 2.6.18-308 |
| 5,9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411 |

#### <a name="oracle-enterprise-linux-w-unbreakable-kernel-uek"></a>Oracle Enterprise Linux z nierozerwalny jądra (UEK)

#### <a name="oracle-linux-6"></a>Oracle Linux 6
| Wersja systemu operacyjnego | Wersja jądra
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| Wersja systemu operacyjnego | Wersja jądra
|:--|:--|
| 5.8 | Oracle 2.6.32-300 (UEK R1) |
| 5,9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11
| Wersja systemu operacyjnego | Wersja jądra
|:--|:--|
| 11 | 2.6.27 |
| 11 Z DODATKIEM SP1 | 2.6.32 |
| 11 Z DODATKIEM SP2 | 3.0.13 |
| 11 Z DODATKIEM SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10
| Wersja systemu operacyjnego | Wersja jądra
|:--|:--|
| 10 SP4 | 2.6.16.60 |

## <a name="diagnostic-and-usage-data"></a>Diagnostyka i użycia danych
Firma Microsoft automatycznie zbiera danych użycia i wydajności za pomocą korzystania z usługi Monitor zależności aplikacji. Firma Microsoft używa danych na udostępnianie i poprawy jakości, bezpieczeństwa i integralności usługi Monitor zależności aplikacji. Danych zawiera informacje o konfiguracji usługi oprogramowania, takiego jak wersja systemu operacyjnego i i zawiera również adres IP, nazwa DNS i nazwa stacji roboczej w celu zapewnienia dokładności i wydajną możliwości rozwiązywania problemów. Firma Microsoft nie gromadzi, nazwy, adresy i inne informacje o kontakcie.

Aby uzyskać więcej informacji dotyczących zbierania danych i zastosowania zobacz [Microsoft Online Services zasady zachowania poufności informacji](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Następne kroki
- Dowiedz się, jak [za pomocą aplikacji współzależności monitora](operations-management-suite-application-dependency-monitor.md) raz został wdrożony i skonfigurowany.
