<properties 
   pageTitle="Monitorowanie porównanie produktów firmy Microsoft | Microsoft Azure"
   description="Pakiet Microsoft operacje zarządzania usługi (OMS) jest podstawą rozwiązanie do zarządzania IT, który ułatwia zarządzanie i ochrona sieci lokalnej i w chmurze infrastruktury chmury firmy Microsoft.  W tym artykule identyfikuje różnych usługach objęte usługi OMS i znajdują się łącza do szczegółowych zawartości."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="microsoft-monitoring-product-comparison"></a>Monitorowanie porównanie produktów firmy Microsoft

Ten artykuł zawiera porównanie ich architektura, logiczny sposób ich monitorowanie zasobów i sposobu ich wykonywania analiz danych, które pobierają System Center Operations Manager (SCOM) i analizy dziennika usługi pakietu w operacji zarządzania (OMS).  To zapewniają podstawowe opis różnic między i względne zalet.  

## <a name="basic-architecture"></a>Architektura podstawowe
### <a name="system-center-operations-manager"></a>System Center Operations Manager

Wszystkie składniki SCOM są zainstalowane w centrum danych.  [Czynniki są zainstalowane](http://technet.microsoft.com/library/hh551142.aspx) na komputerach systemu Windows i Linux, które są zarządzane przez SCOM.  Czynniki nawiązać [Serwerów zarządzania](https://technet.microsoft.com/library/hh301922.aspx) komunikować się z magazynu SCOM bazy danych oraz danych.  Czynniki oparte na uwierzytelnianie domeny, aby połączyć się z serwerami zarządzania.  Osoby spoza domeny zaufanej uwierzytelniania certyfikatów lub połączyć się z [Serwerem bramy](https://technet.microsoft.com/library/hh212823.aspx).

SCOM wymaga dwóch baz danych programu SQL, jedną dla danych operacyjnych i innego magazynu danych do obsługi raportów i analizy danych.  [Raportowanie serwera](https://technet.microsoft.com/library/hh298611.aspx) uruchamia SQL Reporting Services do raporty dotyczące danych z magazynu danych. 

SCOM można monitorować zasobów chmurze za pomocą pakietów zarządzania dotyczące produktów, takich jak [Azure](https://www.microsoft.com/download/details.aspx?id=38414), [Office 365](https://www.microsoft.com/download/details.aspx?id=43708)i [AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html).  Te pakiety narzędzi zarządzania za pomocą jednej lub kilku agentów lokalnych jako serwery proxy dla odnajdowania zasobów w chmurze i uruchomione przepływy pracy do pomiaru ich wydajności i dostępności.  Serwer proxy czynników również są używane do [urządzeń sieciowych monitor](https://technet.microsoft.com/library/hh212935.aspx) i inne zasoby zewnętrzne.

Konsola operacji jest aplikacji systemu Windows, który łączy do jednego z serwerów zarządzania i umożliwia administratorowi wyświetlanie i analizowanie danych zebranych i skonfigurować środowisko SCOM.  Konsola oparte na sieci web można obsługiwać na serwerze usług IIS, a także analizy danych za pomocą przeglądarki sieci.

![Architektura SCOM](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Analizy dziennika

Większość składników usługi OMS znajdują się w chmurze Azure, aby można było wdrożyć i zarządzać nim z minimalnymi kosztów i administracyjne nakładu.  Wszystkie dane zbierane przez analizy dziennika są przechowywane w repozytorium usługi OMS.

Analizy dziennika może zbierać dane z jednej z trzech źródeł:

- Fizycznych i maszyn wirtualnych z systemem Windows i [Microsoft monitorowania agenta (MMA)](https://technet.microsoft.com/library/mt484108.aspx) lub Linux i [Agent pakietu zarządzania operacje Linux](https://technet.microsoft.com/library/mt622052.aspx).  Tych urządzeń może być lokalnego lub maszyn wirtualnych w Azure lub innym chmury.
- Konto Azure miejsca do magazynowania z [Diagnostyki Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) danych zebranych przez pracownika Azure roli, ról w sieci web lub maszyn wirtualnych.
- [Połączenie z grupą zarządzania SCOM](https://technet.microsoft.com/library/mt484104.aspx).  W tej konfiguracji agentów komunikowania się z serwerami zarządzania SCOM, które dostarczają dane do bazy danych SCOM miejsce, w którym następnie będą dostarczane do magazynu danych usługi OMS.
Administratorzy analizowanie danych zebranych i konfigurowanie analizy dziennika z portalem usługi OMS, który jest obsługiwany w Azure i można uzyskać do nich dostęp za pomocą dowolnej przeglądarki.  Aplikacje mobilne, aby uzyskać dostęp do tych danych są dostępne dla platform standardowy.

![Architektura analizy dziennika](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>Integrowanie SCOM i analizy dziennika

Gdy SCOM jest używana jako źródła danych dla analizy dziennika mogą korzystać z funkcji oba produkty we wdrożeniu hybrydowym monitorowanie środowiska.  Możesz skonfigurować istniejących czynników SCOM za pomocą konsoli operacje do zarządzania przez usługi OMS, oprócz nadal pakietów zarządzania z SCOM.  
Dane w grupie Zarządzanie połączonego SCOM będą dostarczane do analizy dziennika przy użyciu jednego z czterech metod:

- Zdarzenia i dane dotyczące wydajności są zbierane przez agenta i dostarczona do SCOM.  Serwerów zarządzania w SCOM następnie dostarczać dane do analizy dziennika.
- Niektóre zdarzenia, na przykład dzienniki programu IIS i zdarzeń zabezpieczeń nadal mają być dostarczane bezpośrednio do analizy dziennika agenta.
- Niektóre rozwiązania będzie wydać dodatkowego oprogramowania do agenta lub wymagają zainstalowania oprogramowania do zbierania dodatkowych danych.  Zazwyczaj te dane będą wysyłane bezpośrednio do analizy dziennika.
- Niektóre rozwiązania będzie zbierać dane bezpośrednio od serwery zarządzania SCOM, które nie pochodzi z agentem.  Na przykład [rozwiązanie do zarządzania alertami](https://technet.microsoft.com/library/mt484092.aspx) zbiera alerty z SCOM po ich utworzeniu.

## <a name="monitoring-logic"></a>Monitorowanie logika
SCOM i dziennika analizy pracy z podobne dane zebrane czynnikami, ale podstawowych różnią się w sposób określania i realizacji ich logiki do zbierania danych i sposobu ich przeanalizować dane, które pobierają.

### <a name="operations-manager"></a>Programu Operations Manager
Monitorowanie logiki SCOM jest zaimplementowana w [pakiety zarządzania](https://technet.microsoft.com/library/hh457558.aspx) zawierające logiczny za odkrycie składniki monitorowanie, pomiaru kondycji tych elementów, a do zbierania danych do analizy.  Monitorowanie danych może być tak proste jak gromadzenie licznik wydarzenie lub wydajności, lub można użyć złożonych logika zaimplementowana w skrypt.  Pakietów zarządzania, które obejmować monitorowanie wykonania są dostępne dla różnych [firmy Microsoft](http://go.microsoft.com/fwlink/?LinkId=82105) i aplikacje innych firm poza sieci i urządzeń.  Można [tworzyć własne pakiety zarządzania](http://aka.ms/mpauthor) dla niestandardowe aplikacje.

Pakiety zarządzania zawiera wiele przepływów pracy, że każdy z nich wykonuje kilka różnych funkcji monitorowania, takiej jak próbki licznika wydajności, sprawdzanie stanu usługi lub uruchamianie skryptu.  Każdego przepływu pracy jest uruchamiany niezależnie i definiuje własny wyniki, takie jak bazę danych, która jest zapisywana do i czy Generowanie alertu. 

Szczegóły przepływu pracy, takie jak częstotliwość są uruchamiane progu, gdzie uznają komunikat o błędzie i ważności alertu, który generują można zastąpić.  Możesz także podać dodatkowe funkcje, dodając własne przepływy pracy.

![Zastępuje](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Pakiety zarządzania są zainstalowane w bazie danych programu Operations Manager i automatycznie przekazywana do czynników przez serwery zarządzania.  Każdego agenta automatycznie pobierze pakiety zarządzania i ładowanie przepływy pracy dotyczącym zainstalowanych aplikacji.  Powrót do serwera zarządzania w celu wstawienia magazynu bazy danych oraz danych SCOM jest dostarczana danych zebranych przez agenta.  Konsola operacje umożliwia wyświetlanie i analizowanie danych za pomocą niestandardowych widoków, pulpitów nawigacyjnych i raportów dostępnych w pakiecie zarządzania.

Rozkład pakietów zarządzania przedstawiono na poniższym diagramie.

![Zarządzanie przepływem pack](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Analizy dziennika
#### <a name="event-and-performance-collection"></a>Zbieranie wydajności i zdarzeń
Dziennik analizy zbiera liczniki wydajności i zdarzeń z systemów agenta korzystanie ze źródeł takich jak dziennika zdarzeń systemu Windows, dzienniki programu IIS i Syslog.  Można zdefiniować kryteria, dla których dane są zbierane za pośrednictwem portalu analizy dziennika, a następnie utworzenie kwerend dziennika do analizowania danych zebranych.  Zestaw kryteriów standardowe zdefiniowano podczas tworzenia obszaru roboczego usługi OMS, można określić dodatkowe dane dla określonej aplikacji. 

![Definiowanie dzienniki zdarzeń w dzienniku analizy](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

SCOM ma wiele szczegółowe przepływów pracy, które zwykle określić kryteria danych i akcję, która powinna być wykonywana w odpowiedzi, analizy dziennika ma bardziej ogólne kryteria dla zbioru danych.  Dziennik kwerend i rozwiązań zapewniają więcej wybranych kryteriów do analizowania i działającą na określonych danych w chmurze, gdy zostały one zebrane.

#### <a name="solutions"></a>Rozwiązania
Rozwiązania zapewniają dodatkowe logiki do zbierania danych i analizy.  Możesz wybrać rozwiązań, aby dodać do swojej subskrypcji usługi OMS z galerii rozwiązań.

![Galeria rozwiązań](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

Rozwiązania przede wszystkim Uruchom w chmurze, dostarczając analizy wydarzeń i liczniki wydajności zebrane w repozytorium usługi OMS.  Mogą również określić dodatkowe dane mają zostać pobrane, które można analizować z kwerendami dziennika lub przy użyciu interfejsu użytkownika dodatkowe dostarczony przez rozwiązanie na pulpicie nawigacyjnym usługi OMS. 

Na przykład [Śledzenie zmian rozwiązanie](https://technet.microsoft.com/library/mt484099.aspx) wykrywa zmiany konfiguracji w systemach agenta i zapisuje zdarzenia do repozytorium usługi OMS, które mogą być analizowane za pomocą kilka widoków graficzne, służące do podsumowywania wykrył zmiany.  Czy Przechodzenie do szczegółów w widoku podsumowania do dziennika kwerend, które wyświetlają szczegółowych danych zebranych przez rozwiązanie.


Gdy wybierzesz rozwiązania, które możesz dodać do swojej subskrypcji, obecnie nie zawierają możliwość tworzenia własnych rozwiązań.  Możesz wybrać wydarzeń i liczników wydajności do gromadzenia i tworzyć niestandardowe widoki oparte na zapytań dziennika.

Na poniższym diagramie podsumowania monitorowania logika dotycząca analizy dziennika.

![Przepływ rozwiązanie analizy dziennika](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Monitorowanie kondycji
### <a name="operations-manager"></a>Programu Operations Manager
SCOM można modelować różne części aplikacji i udostępniać w czasie rzeczywistym kondycja dla każdej z nich.  Dzięki temu można nie tylko wyświetlanie wykrywania błędów i wydajności w czasie, ale również do sprawdzania poprawności rzeczywisty kondycji systemu lub aplikacji i poszczególnych części w dowolnym momencie.  Ponieważ siebie okresy jest dostępna aplikacja aparat kondycji w SCOM obsługuje także dotyczącą poziomu usług umów (SLA) analizowanie i raport o dostępności aplikacji w czasie.

Na przykład poniższa ilustracja przedstawia w czasie rzeczywistym kondycję monitorowane przez SCOM aparatów baz danych SQL.  Kondycja każdego z baz danych dla jednej z aparatów baz danych przedstawiono na dole połowa widoku.

![Wyświetlanie stanu](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

Poniżej przedstawiono Eksploratora kondycji dla jednej z aparatów baz danych przy użyciu monitorów, które są używane do określania ogólnej sprawności.  Tych monitorów są zdefiniowane w pakiecie zarządzania dla programu SQL i uruchamiana wszystkie aparaty bazy danych SQL wykryte przez SCOM.

![Eksplorator kondycji](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Składniki w wielu systemach można łączyć kondycję aplikacji rozproszonej.  Może to być szczególnie przydatne w przypadku aplikacji branżowych, obejmujące wiele składników rozłożone.  Możesz utworzyć model środki kondycję każdego składnika tego zestawienia do dostępność dla aplikacji.

Usługa Active Directory jest przykładem jeden pakiet zarządzania zawierającego model, aby przeanalizować składnikom rozłożone.  Przykładowy diagram poniżej pokazano zdrowia ogólnej środowiska i relacje między lasami, domenami i kontrolerami domen.  Każdy z tych składników obejmuje składniki podrzędne i wiele monitorów, podobnie jak w powyższym przykładzie SQL.

![Widok diagramu SCOM](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Analizy dziennika
Usługi OMS nie zawiera typowe aparat aplikacjom modelu lub zmierzyć kondycję w czasie rzeczywistym.  Rozwiązania dla poszczególnych może oceny ogólnej kondycji usługi określonego na podstawie danych zebranych, a może instalacji niestandardowej logiki agenta w celu przeprowadzenia analizy w czasie rzeczywistym.  Ponieważ rozwiązań działa w chmurze z dostępem do repozytorium usługi OMS, są często udostępniają dokładniejszej analizy nie są zwykle wykonywane przez pakietów zarządzania. 

Na przykład [Ocena AD i ocena SQL rozwiązań](https://technet.microsoft.com/library/mt484102.aspx) analizowanie danych zebranych i podaj ocen dla różnych aspektów środowiska.  Zawiera zalecenia dotyczące ulepszenia wprowadzone w celu poprawienia dostępności i wydajności środowiska.

![Rozwiązanie ocena AD](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Analiza danych
SCOM, jak i analizy dziennika zapewniają różne funkcje do analizowania danych zebranych.  SCOM zawiera widoki i pulpitów nawigacyjnych w konsoli operacje do analizy danych ostatnio używane w różnych formatów i raporty do prezentowania danych z magazynu danych w formie tabelarycznej.  Analizy dziennika zapewnia pełną dziennika języka kwerend oraz interfejs analizowania danych w repozytorium usługi OMS.  Gdy SCOM jest używany jako źródła danych dla analizy dziennika, repozytorium zawiera danych zebranych przez SCOM, więc narzędzia analizy dziennika może służyć do analizowania danych z obu systemów.

### <a name="operations-manager"></a>Programu Operations Manager

#### <a name="views"></a>Widoki
Widoki w konsoli operacje pozwalają wyświetlać różne typy danych zebranych przez SCOM w różnych formatach, zwykle tabelaryczny dla zdarzenia, alerty i dane o stanie i liniowe wykresy dla dane dotyczące wydajności.  Widoki wykonywania minimalnymi analizy lub konsolidacji danych, ale można filtrować według określonych kryteriów. 

![Widoki](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Pakiety zarządzania zwykle dostarcza wiele widoków obsługi aplikacji lub systemu, którą monitoruje.  Może to widokami stanu dla różnych obiektów, które zostanie odnaleziona pakiet zarządzania, alert widoków dla wykryte błędy i wydajności liczników.

Widoki są szczególnie odpowiednie do analizy bieżący stan środowiska, w tym Otwórz alertów i stan kondycji systemów monitorowania i obiektów.  Możesz przejść do szczegółowego zdarzeń lub dane dotyczące wydajności pomocniczych alert określonego celu diagnozowanie jej przyczyny. Podobnie możesz wyświetlić wydajności i kondycji różne części aplikacji do oszacowania jego bieżącego stanu.

#### <a name="dashboards"></a>Pulpity nawigacyjne
Pulpity nawigacyjne w konsoli operacje przede wszystkim działać z tymi samymi danymi, zgodnie z widoków, ale oferują większe możliwości dostosowywania i mogą zawierać bardziej rozbudowane wizualizacji.  Zestaw standardowych pulpity nawigacyjne są dostępne, że można łatwo dostosować do własnych celów.  Umożliwia także widżetu programu PowerShell, której mogą być wyświetlane dane zwrócone przez kwerendę programu PowerShell.

![Pulpit nawigacyjny](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

Deweloperzy mają możliwość dodawania składników niestandardowych do pulpitów nawigacyjnych, które są umieszczane w ich pakietów zarządzania.  Te mogą być bardzo specjalistyczne do określonej aplikacji, takiej jak pulpit nawigacyjny w pakiecie zarządzania SQL, jak pokazano poniżej.  Tego pulpitu nawigacyjnego można także jako szablonu niestandardowego wersji.

![Pulpit nawigacyjny SQL](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Raporty
Raporty w SCOM analizowanie danych z magazynu danych w formie tabelarycznej.  Mogą być drukowane i termin automatyczną dostawy w innych formatach, takich jak PDF, CSV i Word.  Raporty pracować z danymi z magazynu danych, dzięki czemu są one przeznaczone przede wszystkim dla analizy tendencji długoterminową.

Pakiety zarządzania zwykle zapewni raportów niestandardowych dla określonej aplikacji.  Możesz również wybrać z biblioteki ogólnego raportów, które można dostosować do własnych aplikacji lub do wykonywania analizy ad hoc.

Oto przykładowy raport wydajności z danych zebranych przez pakiet zarządzania usługi Active Directory.

![Raport](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Analizy dziennika
Analizy dziennika ma [języka kwerend](https://technet.microsoft.com/library/mt484120.aspx) , który służy do przeprowadzania analizy poprzez danych z wielu aplikacji bez konieczności Tworzenie niestandardowego widoku lub raportu.  Ponieważ usługi OMS jest zaimplementowana w chmurze, wydajności kwerend i analiza danych nie są objęte ograniczeń sprzętu i szybko można analizować kwerend w tym miliony rekordów. 

Kwerendy w dzienniku analizy są także podstawa innych funkcji.  Można zapisać zapytania, Eksportuj wyniki do programu Excel lub automatycznego uruchamiania w regularnych interwałach i generowanie alertu, jeśli wyniki są zgodne z kryteriami.  

![Przepływ kwerendy dziennika](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

Poniżej przedstawiono przykładowy kwerendy analizy dziennika.  W tym przykładzie wszystkie zdarzenia z "wprowadzenie" w nazwie są zwracane oraz pogrupowane według zdarzenia identyfikator.  Użytkownik po prostu zawiera kwerendy i analizy dziennika dynamicznie generuje interfejs użytkownika, aby przeprowadzić analizę.  Wybranie elementów z listy może zwrócić dane szczegółowe zdarzenia.

![Kwerenda dziennika](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Oprócz dostarczania analizy ad hoc, zapytania w analizy dziennika można zapisać do użytku w przyszłości i również dodany do pulpitu [nawigacyjnego usługi OMS](http://technet.microsoft.com/library/mt484090.aspx) , jak pokazano w poniższym przykładzie.

![Pulpit nawigacyjny usługi OMS](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Następne kroki

- Wdrażanie [System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx).
- Załóż [Analizy dziennika](https://azure.microsoft.com/documentation/services/log-analytics).  
