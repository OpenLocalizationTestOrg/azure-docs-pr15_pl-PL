<properties
   pageTitle="Monitorowanie zależności aplikacji (ADM) w pakiecie zarządzania operacje (usługi OMS) | Microsoft Azure"
   description="Monitorowanie zależności aplikacji (ADM) to rozwiązanie usługi operacje zarządzania pakietu (OMS), które automatycznie wykrywa składniki aplikacji w systemach Windows i Linux i mapy komunikacji między usługami.  Ten artykuł zawiera informacje dotyczące wdrażania ADM w środowisku usługi oraz korzystania z niego w różnych scenariuszach."
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

# <a name="using-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Za pomocą aplikacji współzależności monitory usługi pakietu w operacji zarządzania (OMS)
![Ikona zarządzania alertu](media/operations-management-suite-application-dependency-monitor/icon.png) Monitorowanie zależności aplikacji (ADM) automatycznie wykrywa składniki aplikacji w systemach Windows i Linux i mapy komunikacji między usługami. Umożliwia wyświetlanie serwery, jak można je sobie wyobrazić — jako połączone systemy, które dostarczają usługi krytyczne.  Monitorowanie zależności aplikacji przedstawia połączenia między serwerami, procesy, i porty w dowolnej architekturze połączenia TCP z konfiguracji wymagane inne niż instalacji agenta.

W tym artykule opisano szczegóły monitorze zależności aplikacji.  Aby uzyskać informacje na temat konfigurowania ADM i rozpoczęcie korzystania zobacz [Konfigurowanie aplikacji współzależności monitory usługi pakietu w operacji zarządzania (OMS)](operations-management-suite-application-dependency-monitor-configure.md)

>[AZURE.NOTE]Monitorowanie zależności aplikacji jest obecnie Podgląd prywatne.  Można zażądać dostępu do podglądu prywatne ADM w [https://aka.ms/getadm](https://aka.ms/getadm).
>
>Podczas podglądu prywatne wszystkie konta usługi OMS mają nieograniczony dostęp do ADM.  Węzły ADM jest bezpłatna, ale dziennika analizy danych typu AdmComputer_CL i AdmProcess_CL będzie mierzony jak inne rozwiązanie.
>
>Po ADM wprowadza publicznej podglądu, będzie dostępna tylko dla klientów bezpłatnych i płatnych wgląd i analizy usługi OMS ceny plan.  Warstwa bezpłatnego konta będą ograniczone do 5 węzłów ADM.  Jeśli bierzesz udział w podglądzie prywatne i nie jest zarejestrowany w usługi OMS ceny Plan, gdy ADM wprowadza publicznej Podgląd ADM zostanie wyłączona w tym czasie. 


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Przypadków użycia: Wybierz swojego przetwarza pamiętać współzależności

### <a name="discovery"></a>Odnajdowanie
ADM automatycznie tworzy typowych mapę odwołania zależności między serwerami, procesów i 3 usług innych firm.  Zostanie odnaleziona i mapy wszystkie zależności TCP identyfikowanie niespodzianek połączenia zdalnego 3 systemów firmy zależą od i współzależności tradycyjnych ciemne obszary sieci, takich jak AD i serwera DNS.  ADM wykryje połączenia nie powiodło się, które usługi zarządzanych systemów próbujesz utworzyć, pomaga wskazuje potencjalne konfiguracji serwera, usługa dostawie oraz problemów z siecią.

### <a name="incident-management"></a>Zarządzanie zdarzeniami
ADM pomaga wyeliminować czynności izolacji problem pokazując, jak są połączone systemy i wpływu na innych elementach.  Oprócz zakończonego niepowodzeniem połączenia informacje o klientach połączonego pomocy identyfikowanie urządzenia do równoważenia obciążenia niepoprawnie skonfigurowane, zaskakująco lub zbyt duże obciążenie usługi krytyczne i rozpoczęcie klientów, takich jak maszyny Deweloper rozmawiać do systemów produkcji.  Zintegrowane przepływach pracy przy użyciu usługi OMS Zmienianie śledzenia umożliwia także sprawdzenia, czy zdarzenie zmiany na komputerze wewnętrznej lub usługa wyjaśniono przyczyny zdarzenia.

### <a name="migration-assurance"></a>Zapewnienie migracji
ADM umożliwia efektywnie planować, przyspieszenie i sprawdź poprawność Azure migracji zapewnienie, że nic się nie zostało i istnieją nie dostawie niespodzianek.  Można wyszukiwać wszystkich systemach wzajemnie, które należy przeprowadzić migrację ze sobą, oceny konfiguracji systemu i wydajność i określ, czy systemem nadal służy użytkowników lub kwalifikuje się do likwidować zamiast migracji.  Po zakończeniu Przenieś można sprawdzić na obciążenie klientów i tożsamość, aby sprawdzić połączenie systemów testowych i klientami.  Jeśli podsieci definicji planowania i zapory występują problemy, nie powiodło się połączenia w mapach ADM wskaże do systemów, które wymagają łączności.

### <a name="business-continuity"></a>Zapewnianie ciągłości
Jeśli używasz Odzyskiwanie witryny Azure i potrzebujesz pomocy definiowanie sekwencji odzyskiwania dla środowiska aplikacji ADM można automatycznie pokazano, jak systemy opierają się na siebie, aby upewnić się, że planu odzyskiwania jest niezawodne.  Wybieranie kluczowego serwera i przeglądania swoich klientów, można określić systemów zewnętrzną, które można odzyskać tylko po krytyczne serwera jest przywracane i jest dostępny.  I odwrotnie sprawdzając zależności wewnętrznej kluczowego serwera, można określić systemy, które można odzyskać przed systemu fokus zostanie przywrócony.

### <a name="patch-management"></a>Zarządzanie poprawkami
ADM usprawnia korzystanie z usługi OMS ocenę aktualizacji systemu, wskazujące, które innych zespołów i serwerów zależą od usługi, więc można powiadamiać ich z wyprzedzeniem przed rozpoczęciem systemów w dół dla poprawki.  ADM również usprawnia zarządzanie poprawkami w usługi OMS, pokazujące, czy usługi są dostępne i prawidłowo połączone po poprawkami i ponownie uruchomić. 


## <a name="mapping-overview"></a>Omówienie mapowania
Czynniki ADM gromadzenie informacji na temat wszystkich połączonych TCP procesów na serwerze, na którym jest zainstalowany, a także szczegółowe informacje o połączeń przychodzących i wychodzących dla każdego procesu.  Na liście komputera po lewej stronie rozwiązanie ADM, maszyn czynnikami ADM można wybrać wizualizacji współzależnościami dla zakresu zaznaczonego czasu.  Zależność komputera mapy fokusu na określonym komputerze i wyświetlanie wszystkich komputerów, na których są bezpośrednie klientów TCP lub serwerów tego komputera.

![Omówienie ADM](media/operations-management-suite-application-dependency-monitor/adm-overview.png)

Komputery można rozwinąć na mapie, aby pokazać uruchomione procesy z aktywnego połączenia sieciowego podczas wybrany zakres czasu.  Komputer zdalny z agentem ADM rozwinięty Aby wyświetlić szczegóły procesu są wyświetlane tylko procesy komunikowania się z komputerem fokus.  Liczba bez wykorzystania agentów komputerów frontonu połączenie do komputera fokus jest wskazywana po lewej stronie procesów, które łączą się.  Jeśli komputera fokus jest nawiązywania połączenia do komputera wewnętrznej bez agenta, że wewnętrznej jest reprezentowany przez węzeł w mapę, przedstawiającą jego adres IP protokołu IPv4, a węzeł można rozwinąć, aby wyświetlić poszczególne porty i usług, które komputera fokusu komunikuje się z.

Domyślnie mapy ADM wyświetlanie ostatnich 10 minut informacje o zależnościach.  Za pomocą kontrolek czasu w lewym górnym rogu, mapy mogą być wysyłane kwerendy dla zakresów czasu historycznego, nawet do szerokiego godziny, aby pokazać, jak zależności elementu w przeszłości, np. podczas zdarzenia lub przed wystąpieniem zmiany.    ADM dane są przechowywane przez 30 dni roboczych płatnego i przez 7 dni roboczych bezpłatne.

![Mapa komputera z wybranym komputerze właściwości](media/operations-management-suite-application-dependency-monitor/machine-map.png)

## <a name="failed-connections"></a>Połączenia nie powiodło się
Nie powiodło się połączenia są wyświetlane w mapy ADM dla procesów i komputerach z przedstawiający linii kreskowanej czerwony, jeśli na komputerze klienckim kończy się niepowodzeniem do osiągnięcia procesu lub port.  Nie powiodło się połączenia są zgłaszane z systemu z agentem wdrożonym ADM, jeśli systemu jest próby połączenia nie powiodło się.  ADM środków to obserwując sockets TCP, których nie powiodło się nawiązanie połączenia.  Może to być spowodowane jest zapora, błąd konfiguracji klienta i serwera lub zdalny są dostępne. 

![Połączenia nie powiodło się](media/operations-management-suite-application-dependency-monitor/failed-connections.png)

Opis połączenia nie powiodło się może pomóc w rozwiązywaniu problemów sprawdzania poprawności migracji i opis ogólnej architektury analizy zabezpieczeń.  Czasami nie powiodło się połączenia jest bezpieczna, ale często wskazuje, bezpośrednio do problemu, na przykład środowisku pracy awaryjnej nieoczekiwanie stają się niedostępne,.. warstwy aplikacji lub dwóch niemożności porozmawiać po migracji chmury.  Na powyższym obrazie usług IIS i WebSphere oba działają, ale nie można połączyć. 

## <a name="computer-and-process-properties"></a>Komputer i właściwości procesu
Podczas nawigowania po mapę ADM, można wybrać urządzenia i procesów, aby uzyskać dodatkowy kontekst o ich właściwości.  Maszyn zawierają informacje o nazwy DNS, adresy IP protokołu IPv4, pojemnością Procesora i pamięci, typ maszyn wirtualnych, wersja systemu operacyjnego, ponownie uruchomić ostatniego czasu i identyfikatory ich agentów usługi OMS i ADM.

Szczegóły procesu są zbierane od systemu operacyjnego metadanych dotyczących uruchamiania procesów, w tym nazwa procesu, opis procesu, nazwę użytkownika i domeny (w systemie Windows), nazwę firmy, nazwa produktu, wersję produktu, katalog roboczy, wiersza polecenia i czas rozpoczęcia procesu.

![Właściwości procesu](media/operations-management-suite-application-dependency-monitor/process-properties.png)

Panel Podsumowanie procesu udostępnia dodatkowe informacje dotyczące łączności tego procesu, łącznie z jego związane porty, połączeń przychodzących i wychodzących i nie połączenia. 

![Podsumowanie procesu](media/operations-management-suite-application-dependency-monitor/process-summary.png)

## <a name="oms-change-tracking-integration"></a>Integracja z programem śledzenia zmian za pomocą usługi OMS
Integracja ADM osoby z śledzenia zmian jest automatyczne po obu rozwiązań są włączone i skonfigurowane w obszarze roboczym usługi OMS.

Panel Podsumowanie komputera wskazuje, czy śledzenie zmian zdarzeń wystąpiły na wybranym komputerze podczas wybrany zakres czasu.

![Podsumowanie panelu komputerze](media/operations-management-suite-application-dependency-monitor/machine-summary.png)

Panel śledzenia zmian komputera zawiera listę wszystkich zmian, przy pierwszym ostatnio, razem z łączem do przechodzić do wyszukiwania dziennika, aby uzyskać dodatkowe informacje.
![Panel śledzenia zmian komputera](media/operations-management-suite-application-dependency-monitor/machine-change-tracking.png)

Oto rozwijania szczegółów widoku zdarzenia zmiany konfiguracji po wybraniu opcji **Pokaż w dzienniku analizy**.
![Zmiana konfiguracji](media/operations-management-suite-application-dependency-monitor/configuration-change-event.png)


## <a name="log-analytics-records"></a>Rekordy analizy dziennika
ADM na komputerze i przetwarzanie danych zapasów jest dostępna do [wyszukiwania](../log-analytics/log-analytics-log-searches.md) w dzienniku analizy.  To można stosować do scenariusze, w tym planowanie migracji, analiza przepustowości odnajdowanie i rozwiązywanie problemów z wydajnością ad hoc. 

Jeden rekord jest generowany na godzinę na każdym komputerze unikatowych i proces oprócz rekordy wygenerowane podczas procesu lub komputerze zaczyna się lub jest na następować do ADM.  Te rekordy mają właściwości w poniższych tabelach. 

Istnieją wewnętrznie generowane właściwości, które służy do identyfikowania unikatowych procesów i komputerów:

- PersistentKey_s jednoznacznie jest określona przez proces konfiguracji, np. wiersza polecenia i identyfikator użytkownika  Jest unikatowy dla danego komputera, ale należy powtórzyć na komputerach.
- ProcessId_s i ComputerId_s są globalnie unikatowe w modelu ADM.



### <a name="admcomputercl-records"></a>Rekordy AdmComputer_CL
Rekordy z typem **AdmComputer_CL** mają dane dotyczące serwerów czynnikami ADM.  Te rekordy mają właściwości w poniższej tabeli.  

| Właściwość | Opis |
|:--|:--|
| Typ | *AdmComputer_CL* |
| SourceSystem | *OpsManager* |
| ComputerName_s | Windows i Linux oraz nazwę komputera |
| CPUSpeed_d | Szybkość Procesora w MHz |
| DnsNames_s | Listę wszystkich nazw DNS na tym komputerze |
| IPv4s_s | Listę wszystkich adresów IP protokołu IPv4 używany przez komputer |
| IPv6s_s | Lista wszystkie adresy IPv6 używane przez ten komputer.  (ADM identyfikuje adresy IP protokołu IPv6, ale nie wykryje zależności protokołu IPv6.) |
| Is64Bit_b | PRAWDA lub FAŁSZ, w zależności od typu systemu operacyjnego |
| MachineId_s | Identyfikator GUID wewnętrznych unikatowe w obszarze roboczym usługi OMS  |
| OperatingSystemFamily_s | Windows i Linux oraz |
| OperatingSystemVersion_s | Długi ciąg wersja systemu operacyjnego |
| TimeGenerated | Data i godzina utworzenia rekordu. |
| TotalCPUs_d | Liczba rdzeni Procesora |
| TotalPhysicalMemory_d | Pojemność pamięci w Megabajtach |
| VirtualMachine_b | PRAWDA lub FAŁSZ, w zależności od tego, czy z systemem operacyjnym jest Gość maszyn wirtualnych |
| VirtualMachineID_g | Identyfikator maszyn wirtualnych funkcji Hyper-V |
| VirtualMachineName_g | Nazwa funkcji Hyper-V maszyn wirtualnych |
| VirtualMachineType_s | Hyperv, Vmware, Xen, Kvm, Ldom, Lpar, VirtualPC powoduje wykorzystanie dodatkowej |


### <a name="admprocesscl-type-records"></a>Typ AdmProcess_CL rekordów 
Rekordy z typem **AdmProcess_CL** mają dane dotyczące połączonych TCP procesów na serwerach czynnikami ADM.  Te rekordy mają właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| Typ | *AdmProcess_CL* |
| SourceSystem | *OpsManager* |
| CommandLine_s | Pełny wiersz polecenia procesu |
| CompanyName_s | Nazwa firmy (od środowiska Windows PE lub OBROTÓW Linux) |
| Description_s | Opis długi proces (od środowiska Windows PE lub OBROTÓW Linux) |
| FileVersion_s | Plik wykonywalny wersji (z systemem Windows PE, tylko w systemie Windows) |
| FirstPid_d | Identyfikator procesu systemu operacyjnego |
| InternalName_s | Nazwa wewnętrzna plik wykonywalny (ze środowiska Windows PE tylko w systemie Windows) |
| MachineId_s | Wewnętrzny identyfikator GUID unikatowych przez usługi OMS obszaru roboczego  |
| Name_s | Nazwa wykonywalny procesu |
| Path_s | Plik wykonywalny proces ścieżką systemu plików |
| PersistentKey_s | Wewnętrzny identyfikator GUID unikatowa w tym komputerze |
| PoolId_d | Wewnętrzny identyfikator dla agregacji procesów według wierszy podobne polecenia. |
| ProcessId_s | Wewnętrzny identyfikator GUID unikatowe w obszarze roboczym usługi OMS  |
| ProductName_s | Ciąg nazwy produktu (od środowiska Windows PE lub OBROTÓW Linux) |
| ProductVersion_s | Wersja produktu (od środowiska Windows PE lub OBROTÓW Linux) |
| StartTime_t | Czas rozpoczęcia procesu zegara komputera lokalnego |
| TimeGenerated | Data i godzina utworzenia rekordu. |
| UserDomain_s | Domeny właściciela procesu (tylko w systemie Windows) |
| UserName_s | Nazwa właściciela procesu (tylko w systemie Windows) |
| WorkingDirectory_s | Katalog roboczy procesu |


## <a name="sample-log-searches"></a>Przykładowy dziennik wyszukiwania

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Lista możliwości pamięci fizycznej wszystkich komputerów zarządzanych. 
Typ = AdmComputer_CL | Wybierz pozycję TotalPhysicalMemory_d, ComputerName_s | Dedup ComputerName_s

![Przykład zapytania ADM](media/operations-management-suite-application-dependency-monitor/adm-example-01.png)

### <a name="list-computer-name-dns-ip-and-os-version"></a>Nazwa komputera, wersja DNS, adresów IP i systemu operacyjnego.
Typ = AdmComputer_CL | Wybierz pozycję ComputerName_s, OperatingSystemVersion_s, DnsNames_s, IPv4s_s | dedup ComputerName_s

![Przykład zapytania ADM](media/operations-management-suite-application-dependency-monitor/adm-example-02.png)

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Znajdowanie wszystkich procesów za pomocą "sql" w wierszu polecenia
Typ = AdmProcess_CL CommandLine_s = \*sql\* | dedup ProcessId_s

![Przykład zapytania ADM](media/operations-management-suite-application-dependency-monitor/adm-example-03.png)

### <a name="after-viewing-event-data-for-given-process-use-its-machine-id-to-retrieve-the-computers-name"></a>Po wyświetleniu danych zdarzeń dla danego procesu, pobrać nazwy komputera za pomocą Identyfikatora komputera
Typ = "m!m-9bb187fa-e522-5f73-66d2-211164dc4e2b" AdmComputer_CL | Różne ComputerName_s

![Przykład zapytania ADM](media/operations-management-suite-application-dependency-monitor/adm-example-04.png)

### <a name="list-all-computers-running-sql"></a>Lista wszystkich komputerów z systemem SQL
Typ = w MachineId_s AdmComputer_CL {typ = AdmProcess_CL \*sql\* | Różne MachineId_s} | Różne ComputerName_s

![Przykład zapytania ADM](media/operations-management-suite-application-dependency-monitor/adm-example-05.png)

### <a name="list-of-all-unique-product-versions-of-curl-in-my-datacenter"></a>Lista wszystkich wersji produktu unikatowe zwinięcie w mojej centrum danych
Typ = AdmProcess_CL Name_s = zwinięcie | Różne ProductVersion_s

![Przykład zapytania ADM](media/operations-management-suite-application-dependency-monitor/adm-example-06.png)

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Tworzenie grupy komputerów wszystkich komputerów z systemem CentOS

![Przykład zapytania ADM](media/operations-management-suite-application-dependency-monitor/adm-example-07.png)



## <a name="diagnostic-and-usage-data"></a>Diagnostyka i użycia danych
Firma Microsoft automatycznie zbiera danych użycia i wydajności za pomocą korzystania z usługi Monitor zależności aplikacji. Firma Microsoft używa danych na udostępnianie i poprawy jakości, bezpieczeństwa i integralności usługi Monitor zależności aplikacji. Danych zawiera informacje o konfiguracji usługi oprogramowania, takiego jak wersja systemu operacyjnego i i zawiera również adres IP, nazwa DNS i nazwa stacji roboczej w celu zapewnienia dokładności i wydajną możliwości rozwiązywania problemów. Firma Microsoft nie gromadzi, nazwy, adresy i inne informacje o kontakcie.

Aby uzyskać więcej informacji dotyczących zbierania danych i zastosowania zobacz [Microsoft Online Services zasady zachowania poufności informacji](hhttps://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Następne kroki
- Dowiedz się więcej na temat [logowania wyszukiwania](../log-analytics/log-analytics-log-searches.md] in Log Analytics to retrieve data collected by Application Dependency Monitor.)
