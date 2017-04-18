<properties
   pageTitle="Różnice między usługami w chmurze i tkaninie usługi | Microsoft Azure"
   description="Omówienie migracji aplikacjom tkaninie usługi z usług w chmurze."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Informacje o różnicach między usługami w chmurze i tkaninie usługi przed migracją aplikacji.
Microsoft Azure usługi jest platformy aplikacji chmury generacji dla wysoce skalowalna, wysoce niezawodne aplikacje rozproszone. Podaj wiele nowych funkcji dla opakowania, wdrażania, uaktualnianie i zarządzanie aplikacjami rozłożone chmury. 

Jest to Przewodnik wprowadzający migrację aplikacjom tkaninie usługi z usług w chmurze. Jego omówiono przede wszystkim architektura i projektowanie różnice między usługami w chmurze i tkaninie usługi.
 
## <a name="applications-and-infrastructure"></a>Infrastruktura i aplikacje

Podstawowa różnica między usługami w chmurze i tkaninie usługi jest powiązanie maszyny wirtualne, obciążenia i aplikacji. Obciążenie pracą w tym miejscu jest definiowana jako kod, którą piszesz do wykonywania określonych zadań lub świadczenia usługi.
 
 - **Usług w chmurze jest dotyczące rozmieszczania aplikacji jako maszyny wirtualne.** Kod, jaki jest ściśle związane wystąpieniu maszyn wirtualnych, na przykład w sieci Web lub roli pracownika. Aby wdrożyć obciążenie pracą w usług w chmurze jest wdrożenie wystąpienia maszyn wirtualnych, uruchamianych obciążenie pracą. Jest bez spacji, aplikacji i maszyny wirtualne i dlatego jest brak formalne definicji aplikacji. Aplikacja można traktować jako zestaw wystąpień sieci Web lub Rola pracownika w ramach wdrożenia usług w chmurze lub całego wdrożenia usług w chmurze. W tym przykładzie aplikacja jest wyświetlana jako zestaw wystąpień roli.
 
![Aplikacje usług w chmurze i topologii][1]

 - **Usługa jest temat wdrażania aplikacji do istniejącego maszyny wirtualne lub komputerów z systemem tkaninie usługi w systemach Windows i Linux.** Usługi, którą piszesz są całkowicie rozłączone z infrastruktury źródłowych, które jest z dala od komputera usunięte przez platformę aplikacji tkaninie usługi, aby wdrożyć aplikację wielu środowiskach. Obciążenie pracą w tkaninie usługi nosi nazwę "Usługa", a co najmniej jednej usługi są zgrupowane w aplikacji formalnego zdefiniowane uruchamianej na platformie aplikacji usługi tkaninie. Wiele aplikacji może zostać rozmieszczony na jednym klastrze tkaninie usługi.
 
![I topologii aplikacji usługi tkaninie][2]
 
Usługa sam jest warstwa platformy aplikacji, która działa w systemach Windows i Linux, usług w chmurze jest system wdrożenie zarządzanych Azure maszyny wirtualne z obciążeń pracą dołączony.
Model aplikacji usługi tkaninie ma wiele zalet:

 - Czas szybkiego rozmieszczania. Tworzenie wystąpień maszyn wirtualnych może zająć dużo czasu. Na tkaninie usługi maszyny wirtualne są wdrażać tylko po do utworzenia klastrze obsługującego platformy aplikacji usługi tkaninie. Od tego momentu pakietów aplikacji można wdrożyć z klastrem bardzo szybko.
 - Obsługa wysokiej gęstości. W usług w chmurze maszyny roli Pracownik obsługuje jeden obciążenie pracą. Na tkaninie usługi aplikacji są oddzielone od maszyny wirtualne uruchamiane, co oznacza, że można wdrażać dużej liczby aplikacjom niewielka liczba maszyny wirtualne, które można zmniejszyć całkowity koszt w przypadku dużych wdrożeń.
 - Tkaninie usług, które platformy można uruchomić dowolne miejsce zawierające występują serwera systemu Windows i Linux oraz maszyn, czy jest Azure lub lokalnego. Platformy zapewnia warstwę abstrakcji przez infrastrukturę źródłowych, aplikacja może zostać uruchomiony w różnych środowiskach. 
 - Zarządzanie aplikacjami rozłożone. Usługa jest platformę, że nie tylko obsługującą aplikacje rozproszone, ale również ułatwia zarządzanie jej cyklu życia niezależnie od hostingu maszyn wirtualnych lub krótki czas ważności komputera.

## <a name="application-architecture"></a>Architektura aplikacji

Architektura aplikacji usług w chmurze zazwyczaj zawiera wiele zależności usług zewnętrznych, takich jak Bus usługi, Azure tabeli i magazyn obiektów Blob SQL, Redis i inne osoby do zarządzania stanu i danych aplikacji i komunikacji między sieci Web i ról pracownik we wdrożeniu usług w chmurze. Przykładem wykonania aplikacji usług w chmurze może wyglądać następująco:  

![Architektura usług w chmurze][9]

Aplikacje usług tkaninie także możliwość korzystania z tym samym usług zewnętrznych w aplikacji pełną. W tym przykładzie Architektura usług w chmurze najłatwiejszym ścieżkę migracji z usług w chmurze do usługi tkaninie ma zamienić tylko wdrożenia usług w chmurze z aplikacją usługi tkaninie samo zachowanie ogólną architekturę. Sieci Web i ról pracownika można przenieść do usług stateless tkaninie usługi ze zmianami minimalnego kodu.

![Usługa architektury po migracji prostych][10]

Na tym etapie system powinien nadal działa tak samo jak przed. Korzystając z funkcji stanowe tkaninie usługi, stan zewnętrznych sklepy można internalized, jak stateful usług w stosownych przypadkach. Jest to bardziej skomplikowane niż prosty migracji sieci Web i ról pracownika do usług stateless tkaninie usługi, jak wymaga zapisywania usługi niestandardowe dostarczających podobne funkcje do aplikacji usług zewnętrznych, jak poprzednio. Korzyści wynikające w ten sposób: 

 - Usunięcie zależności zewnętrznych 
 - Widać wdrożenia, zarządzanie i uaktualnienia modeli. 
 
Przykład wyniku architektura internalizing tych usług może wyglądać tak:

![Usługa architektury po pełnej migracji][11]

## <a name="communication-and-workflow"></a>Komunikacji i przepływu pracy

Większość aplikacji usługi w chmurze składa się z więcej niż jeden poziom. Podobnie aplikacji usługi tkaninie składa się z więcej niż jedną usługę (zwykle wiele usług). Są dwa typowe modele komunikacji bezpośredniej komunikacji i za pośrednictwem trwałe pamięci zewnętrznej.

### <a name="direct-communication"></a>Bezpośrednie komunikacji

Dzięki bezpośredniego połączenia poziomów można komunikować się bezpośrednio za pośrednictwem punktu końcowego ujawnionego przez każdą. W środowiskach stateless takich jak usług w chmurze, to oznacza wybranie wystąpienie rolę maszyn wirtualnych albo losowo lub okrężnego załadować saldo i nawiązywanie połączeń z punktu końcowego bezpośrednio.

![Usług w chmurze bezpośredni komunikacji][5]

 Bezpośredniego połączenia jest wspólny wzór komunikacji w tkaninie usługi. Kluczowe różnica między tkaninie usługi i usług w chmurze jest tego w usług w chmurze możesz połączyć się Głosowa, natomiast tkaninie usługi umożliwia nawiązanie połączenia z usługą. Jest to ważne rozróżnienie kilka powodów:

 - Usług w tkaninie usługa nie są powiązane z maszyny wirtualne obsługujące usługi może poruszanie się w klastrze i w rzeczywistości oczekuje do poruszania się po różnych powodów: zasobów równoważenia, pracy awaryjnej uaktualnienia aplikacji i infrastruktury i położenie lub załadowania ograniczeń. Oznacza to, że adres wystąpieniem usługi można zmienić w dowolnym momencie. 
 - Maszyn wirtualnych w tkaninie usługi może zawierać wiele usług, każda z unikatowymi punktów końcowych.

Usługa tkaninie zawiera mechanizm wykrywania usługi o nazwie Usługa nazewnictwa, które mogą być używane w celu rozwiązania adresy punktu końcowego usługi. 

![Usługa tkaninie bezpośredniej komunikacji][6]

### <a name="queues"></a>Kolejki

Wspólnego mechanizmu komunikacji między poziomów w środowiskach stateless, takich jak usług w chmurze jest użycie kolejkę zewnętrzna trwale przechowywanie zadań Praca z jednej warstwy. Typowy scenariusz jest warstwa wysyłające zadania do kolejki Azure lub usługi Bus miejsce, w którym wystąpienia roli pracownika można usuwania z kolejki i przetwarzania zadań.

![Komunikacji kolejki usług w chmurze][7]

Ten sam model komunikacji można użyć w tkaninie usługi. Może to być przydatne podczas migracji istniejącej aplikacji usług w chmurze do usługi tkaninie. 

![Usługa tkaninie bezpośredniej komunikacji][8]
 
## <a name="next-steps"></a>Następne kroki

Najłatwiejszym ścieżkę migracji z usług w chmurze do usługi tkaninie jest zamienić tylko wdrożenia usług w chmurze z aplikacją tkaninie usługę pozostawiania ogólną architekturę aplikacji mniej więcej tak samo. Następujący artykuł zawiera przewodnik ułatwiające konwertowanie rolę Pracownik lub sieci Web do usługi stateless tkaninie usługi.

 - [Prosta migracji: konwertowanie rolę Pracownik lub sieci Web do usługi stateless tkaninie usługi](./service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png
