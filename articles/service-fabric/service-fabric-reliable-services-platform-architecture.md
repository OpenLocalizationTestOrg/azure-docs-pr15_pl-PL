<properties
   pageTitle="Architektura niezawodne usługi | Microsoft Azure"
   description="Omówienie architektura niezawodne usługi dla usług stanowe i bezpaństwowców"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/30/2016"
   ms.author="alanwar"/>

# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Architektura usługi stanowe i bezpaństwowców niezawodne

Azure tkaninie niezawodne usługi może być stanowe lub bezpaństwowca. Każdego typu usług uruchamiany w określonym architektury. W tym artykule opisano następujące architektury.
Zobacz [Omówienie niezawodne usługi](service-fabric-reliable-services-introduction.md) Aby uzyskać więcej informacji o różnicach między usługami stanowe i bezpaństwowców.

## <a name="stateful-reliable-services"></a>Stanowe zaufania usługi

### <a name="architecture-of-a-stateful-service"></a>Architektura stanowe usługi
![Diagram architektury stanowe usługi](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>Stanowe niezawodne usługi

Stanowe usługi zaufanego mogą pochodzić od klasy StatefulService albo StatefulServiceBase. Obu tych podstawowych klas są dostarczane przez usługę tkaninie. Oferują różne poziomy pomocy technicznej i abstrakcji usługi stanowe komunikowanie się z usługi tkaninie — i pełnić rolę usługi w klastrze tkaninie usługi.

StatefulService pochodzi z StatefulServiceBase. StatefulServiceBase oferuje usługi większa elastyczność, ale wymaga więcej opis wewnętrzne materiału usługi.
Aby uzyskać więcej informacji na temat zapisywania usług za pomocą klasy StatefulService i StatefulServiceBase, zobacz [Omówienie niezawodne usługi](service-fabric-reliable-services-introduction.md) i [niezawodne usługi Zaawansowane zastosowania](service-fabric-reliable-services-advanced-usage.md) .

Obie klasy podstawowe zarządzanie istnienia i roli implementacji usługi. Implementacji usługi mogą zastąpić wirtualnych metody albo klasy podstawowej, jeśli zadań do wykonania w tych miejscach cyklu realizacji usługi — zawiera implementacji usługi lub jeśli chce się utworzyć obiekt odbiornika komunikacji. Należy zauważyć, że chociaż implementacji usługi może wprowadzić własne obiekt listener komunikacji Uwidacznianie ICommunicationListener, na diagramie powyżej, odbiornika komunikacji jest wykonywane przez usługę tkaninie — jak implementacji usługi używa odbiornik komunikacji jest wykonywane przez usługę tkaninie.

Usługi zaufanego stanowe używa Menedżer stanu zaufanego skorzystać z zaufanego zbiorów. Kolekcje zaufanego są struktur lokalnych danych, które są bardziej dostępne dla usługi —, były zawsze dostępne, bez względu na to praca awaryjna usługi. Każdego typu niezawodne zbioru jest zaimplementowana przez dostawcę niezawodne stan.
Aby uzyskać więcej informacji na zbiory zaufanego zobacz [Omówienie zaufanego zbiorów](service-fabric-reliable-services-reliable-collections.md).

### <a name="reliable-state-manager-and-state-providers"></a>Menedżer stanu zaufanego i stan dostawców

Menedżer stanu zaufanego jest obiekt, który zarządza zaufanego stan dostawców. Ma funkcja tworzenie, usuwanie, wyliczanie i zapewnienia trwałych i wysokiej dostępności dostawców zaufanego stan. Wystąpienia dostawcy zaufanego stan reprezentuje wystąpienie struktury danych trwałe i wysokiej dostępności, takich jak słownik lub kolejki.

Każdy dostawca niezawodne stan udostępnia interfejs, który jest używany przez usługę stanowe współdziałanie z dostawcą niezawodne stan. Na przykład IReliableDictionary jest używana do interfejsu z zaufanego słownik, podczas gdy IReliableQueue jest używany do interfejsu z zaufanego kolejki. Wszystkich dostawców zaufanego stan zaimplementować interfejsu IReliableState.

Menedżer stanu zaufanego ma interfejsu o nazwie IReliableStateManager, który umożliwia dostęp do niej z usługą stanowe. Interfejsy dostawcom zaufanego stan są zwracane przez IReliableStateManager.

Menedżer stanu zaufanego korzysta z wtyczki architektury, tak, aby nowe typy kolekcji zaufanego można podłączyć dynamiczne.

Po wprowadzeniu wysokiej wydajności, wersji magazynu różnicy utworzono zaufanego słownika i zaufanego kolejki.

### <a name="transactional-replicator"></a>Replikator transakcji

Składnik Replikator transakcji jest odpowiedzialny za zapewnia, że stan usługi (oznacza to, że stan w Menedżer stanu zaufanego i niezawodnego zbiorów) jest spójna przez wszystkich replik uruchomiona usługa. Zapewnia również, że stan jest umieszczany w dzienniku. Interfejsy Menedżer stanu zaufanego z transakcji Replikator za pośrednictwem mechanizmu prywatne.

Transakcji Replikator komunikuje protokół sieciowy się stan z innymi replikami wystąpienia usługi, aby uniknąć wszystkich replik informacje o stanie aktualne.

Transakcji Replikator używa dziennika do innej witryny informacje o stanie, dzięki czemu informacje o stanie przeżyje proces lub węzeł ulega awarii. Interfejs w dzienniku jest za pośrednictwem mechanizmu prywatne.

### <a name="log"></a>Log

Składnik dziennika zapewnia Magazyn trwały wysokiej wydajności, który może być zoptymalizowany pod kątem pisania wirujący lub półprzewodnikowe dysków.  Projekt dziennik dotyczy wygenerowanie (to znaczy dyski) lokalną węzłów, które jest uruchomiona usługa stanowe. Dzięki temu opóźnienia niskiej i wysokiej wydajności w porównaniu z Magazyn zdalny trwałych, który znajduje się węzeł.

Składnik dziennika używa wielu plików dziennika. Istnieje węzeł na poziomie udostępnionego pliku dziennika, który wszystkie repliki za pomocą go udostępnia opóźnienie najniższych i najwyższych przepustowość do przechowywania danych o stanie. Domyślnie dziennik udostępniony jest umieszczany w katalogu pracy węzeł tkaninie usługi, ale również może być skonfigurowany do umieszczenia w innej lokalizacji, najlepiej na dysku zarezerwowane dla udostępnionego dziennika. Każdej replice usługi ma również dedykowane plik i dedykowane dziennika jest umieszczany w katalogu pracy usługi. Istnieje mechanizm skonfigurować dedykowanego dziennika mają być umieszczone w innym miejscu.

Dziennik udostępniony jest obszarem przejściowych replice informacji o stanie, gdy plik dziennika dedykowane jest przeznaczenia miejsce, w którym jest zachowywane. W tym projekcie informacje o stanie jest najpierw zapisywane do udostępnionego pliku dziennika, a następnie lazily przeniesione do pliku dziennika dedykowane w tle. W ten sposób zapisu w dzienniku udostępnionego woli opóźnienie najniższych i najwyższych przepustowość, dzięki czemu usługę, aby przyspieszyć postępu.

Odczytuje i zapisuje w dzienniku udostępnionego są wykonywane za pośrednictwem Jo bezpośrednio do przydzielonych wstępnie miejsca na dysku dla pliku udostępnionego dziennika. Aby umożliwić optymalne wykorzystanie miejsca na dysku za pomocą dzienników dedykowane, plik dziennika dedykowane jest tworzona jako plikami NTFS. Zauważ, że będzie overprovisioning miejsca na dysku i system operacyjny zostaną wyświetlone pliki dziennika dedykowane przy użyciu znacznie więcej miejsca na dysku niż w rzeczywistości jest używana.

Oprócz interfejs minimalnego tryb użytkownika w dzienniku dziennik jest zapisywany jako sterownik trybu jądra. Działając jako sterownik trybu jądra dziennik zapewnia najwyższą wydajność do wszystkich usług, które z niej korzystać.

Aby uzyskać więcej informacji o konfigurowaniu dziennika zobacz [Konfigurowanie stanowe niezawodne usługi](service-fabric-reliable-services-configuration.md).

## <a name="stateless-reliable-service"></a>Bezstanowa niezawodne usługi

### <a name="architecture-of-a-stateless-service"></a>Architektura stateless usługi
![Diagram architektury stateless usługi](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Bezstanowa niezawodne usługi

Implementacji usługi Stateless pochodzi od klasy StatelessService lub StatelessServiceBase. Klasa StatelessServiceBase umożliwia bardziej elastyczne niż klasa StatelessService.
Czas użytkowania i roli usługi zarządzać obu klas podstawowych.

Implementacji usługi mogą zastąpić metod wirtualnych albo klasy podstawowej, w przypadku usługi zadań do wykonania w tych miejscach świadczenia usługi — lub jeśli chce utworzyć obiekt odbiornika komunikacji. Należy zauważyć, że mimo że usługa może wprowadzić własny obiekt listener komunikacji Uwidacznianie ICommunicationListener, na diagramie powyżej, odbiornika komunikacji jest wykonywane przez tkaninie usługi zgodnie z wprowadzonym przez usługę tkaninie detektor komunikacji korzysta z tej implementacji usługi.

Aby uzyskać więcej informacji na temat zapisywania usług przy użyciu klas StatelessService i StatelessServiceBase, zobacz [Omówienie niezawodne usługi](service-fabric-reliable-services-introduction.md) i [niezawodne usługi Zaawansowane zastosowania](service-fabric-reliable-services-advanced-usage.md) .

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat usługi tkaninie zobacz:

[Omówienie usługi zaufanego](service-fabric-reliable-services-introduction.md)

[Szybki start](service-fabric-reliable-services-quick-start.md)

[Omówienie zaufanego zbiory](service-fabric-reliable-services-reliable-collections.md)

[Niezawodne usługi Zaawansowane zastosowania](service-fabric-reliable-services-advanced-usage.md)

[Konfiguracja usługi zaufanego](service-fabric-reliable-services-configuration.md)  
