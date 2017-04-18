<properties
   pageTitle="Narzędzia diagnostyczne uczestników i monitorowanie | Microsoft Azure"
   description="Ten artykuł zawiera opis narzędzia diagnostyczne i funkcje w runtime aktorów zaufanego tkaninie usługi, w tym wydarzeń i liczniki wydajności wysyłane przez nią monitorowania wydajności."
   services="service-fabric"
   documentationCenter=".net"
   authors="abhishekram"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/05/2016"
   ms.author="abhisram"/>

# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Narzędzia diagnostyczne i monitorowania wydajności zaufanego uczestników
Środowisko uruchomieniowe zaufanego aktorów emituje [liczniki wydajności](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx)i zdarzeń [źródła zdarzeń](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) . Te udostępnia spostrzeżeń jak działa środowisko uruchomieniowe i pomoc w rozwiązywaniu problemów oraz monitorowanie wydajności.

## <a name="eventsource-events"></a>Zdarzenia źródła zdarzeń
Nazwa dostawcy źródła zdarzeń Runtime zaufanego uczestników jest "Microsoft-ServiceFabric-aktorów". Wydarzenia z tego źródła zdarzenia pojawiają się w oknie [Zdarzenia diagnostyki](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) , gdy aplikacja aktor jest [debugowania w programie Visual Studio](service-fabric-debugging-your-application.md).

Przykłady narzędzi i technologii, które pomagają w zbierania i/lub wyświetlanie zdarzeń źródła zdarzeń są [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Diagnostyki Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) [Semantyczny rejestrowania](https://msdn.microsoft.com/library/dn774980.aspx)i [Biblioteki TraceEvent Microsoft](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Słowa kluczowe
Wszystkie zdarzenia, które należą do zaufanego źródła zdarzeń uczestników są skojarzone z jedną lub więcej słów kluczowych. Umożliwia filtrowanie zdarzeń, które były zbierane. Zdefiniowano następujące bitów słowo kluczowe.

|Bit|Opis|
|---|---|
|0x1|Zestaw ważne wydarzenia podsumowujące działania środowisko uruchomieniowe tkaninie aktorów.|
|0x2|Konfigurowanie zdarzenia, które opisują Aktor metody połączenia. Aby uzyskać więcej informacji zobacz [temat wprowadzający na uczestników](service-fabric-reliable-actors-introduction.md#actors).|
|0x4|Konfigurowanie zdarzenia związane z stan aktora. Aby uzyskać więcej informacji zobacz temat na [Zarządzanie stanem aktora](service-fabric-reliable-actors-state-management.md).|
|0x8|Konfigurowanie zdarzenia związane z oparte na włączanie współbieżności w aktora. Aby uzyskać więcej informacji zobacz temat na [współbieżności](service-fabric-reliable-actors-introduction.md#concurrency).|

## <a name="performance-counters"></a>Liczniki wydajności
Środowisko uruchomieniowe zaufanego aktorów określa następujące kategorie licznika wydajności.

|Kategoria|Opis|
|---|---|
|Usługa tkaninie Aktor|Liczniki specyficzne dla tkaninie usługi Azure uczestników, np. czas zapisywania stan Aktor|
|Metody Aktor tkaninie usługi|Liczniki specyficzne dla metody wykonywane przez uczestników tkaninie usługi, np. częstotliwość Aktor wywoływana jest metoda|

Każda z powyższych kategorii zawiera co najmniej jeden licznik.

Aplikacja [Monitor wydajności systemu Windows](https://technet.microsoft.com/library/cc749249.aspx) , która jest dostępna domyślnie w systemie operacyjnym Windows może służyć do zbierania i wyświetlania danych licznika wydajności. [Diagnostyka Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) jest inna opcja licznik zbieranie danych dotyczących wydajności i przekazania go do tabel Azure.

### <a name="performance-counter-instance-names"></a>Nazwy wystąpienia licznika wydajności
W klastrze zawierającym dużej liczby usług aktora lub partycje usługi Aktor uzyskuje dużej liczby wystąpień liczników wydajności aktora. Nazwy wystąpienia licznika wydajności może pomóc w identyfikacji określonej metody [partycją](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) i Aktor (jeśli dotyczy) czy wystąpienia licznika wydajności jest skojarzony.

#### <a name="service-fabric-actor-category"></a>Kategoria Aktor tkaninie usługi
Dla kategorii `Service Fabric Actor`, nazwy wystąpienia liczników są w następującym formacie:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* jest reprezentacja tekstowa identyfikator partition tkaninie usługi, którą jest skojarzony wystąpienia licznika wydajności. Identyfikator jest identyfikator GUID i jego reprezentacja ciąg jest generowany przez [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) metody specyfikatorem format "D".

*ActorRuntimeInternalID* jest reprezentacja tekstowa całkowitą 64-bitową, która jest generowany przez program obsługi aktorów tkaninie użytku wewnętrznego. To będzie obejmować nazwę wystąpienia licznika wydajności, aby zapewnić jego unikatowość i uniknąć konfliktów z innymi nazwy wystąpienia licznika wydajności. Użytkownicy nie należy próbować interpretować tej części nazwę wystąpienia licznika wydajności.

Oto przykład licznik nazwę wystąpienia licznika, który należy `Service Fabric Actor` kategorii:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

W powyższym przykładzie `2740af29-78aa-44bc-a20b-7e60fb783264` jest reprezentacja tekstowa identyfikator partition tkaninie usługi i `635650083799324046` jest generowany dla runtime jest wewnętrzny identyfikator 64-bitowej za pomocą.

#### <a name="service-fabric-actor-method-category"></a>Metody Aktor tkaninie usługi kategorii
Dla kategorii `Service Fabric Actor Method`, nazwy wystąpienia liczników są w następującym formacie:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName* jest nazwą metody aktora, którą jest skojarzony wystąpienia licznika wydajności. Format nazwy metody zależy, oparte na kilka reguł w środowisko uruchomieniowe tkaninie uczestników, który czytelności nazwy z ograniczeniami na maksymalna długość nazwy wystąpienia licznika wydajności w systemie Windows.

*ActorsRuntimeMethodId* jest reprezentacja tekstowa całkowitą 32-bitową, która jest generowany przez program obsługi aktorów tkaninie użytku wewnętrznego. To będzie obejmować nazwę wystąpienia licznika wydajności, aby zapewnić jego unikatowość i uniknąć konfliktów z innymi nazwy wystąpienia licznika wydajności. Użytkownicy nie należy próbować interpretować tej części nazwę wystąpienia licznika wydajności.

*ServiceFabricPartitionID* jest reprezentacja tekstowa identyfikator partition tkaninie usługi, którą jest skojarzony wystąpienia licznika wydajności. Identyfikator jest identyfikator GUID i jego reprezentacja ciąg jest generowany przez [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) metody specyfikatorem format "D".

*ActorRuntimeInternalID* jest reprezentacja tekstowa całkowitą 64-bitową, która jest generowany przez program obsługi aktorów tkaninie użytku wewnętrznego. To będzie obejmować nazwę wystąpienia licznika wydajności, aby zapewnić jego unikatowość i uniknąć konfliktów z innymi nazwy wystąpienia licznika wydajności. Użytkownicy nie należy próbować interpretować tej części nazwę wystąpienia licznika wydajności.

Oto przykład licznik nazwę wystąpienia licznika, który należy `Service Fabric Actor Method` kategorii:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

W powyższym przykładzie `ivoicemailboxactor.leavemessageasync` jest nazwą metody `2` jest identyfikator 32-bitowej wygenerowane do użytku wewnętrznego i runtime `89383d32-e57e-4a9b-a6ad-57c6792aa521` jest reprezentacja tekstowa identyfikator partition tkaninie usługi i `635650083804480486` jest za pomocą Identyfikatora 64-bitowej generowane dla runtime jest wewnętrzny.

## <a name="list-of-events-and-performance-counters"></a>Listy wydarzeń i liczników wydajności

### <a name="actor-method-events-and-performance-counters"></a>Aktor metody wydarzeń i liczników wydajności
Środowisko uruchomieniowe zaufanego aktorów emituje następujące zdarzenia związane z [metod aktora](service-fabric-reliable-actors-introduction.md#actors).

|Nazwa zdarzenia|Identyfikator zdarzenia|Poziom|Słowo kluczowe|Opis|
|---|---|---|---|---|
|ActorMethodStart|7|Pełne|0x2|Środowisko uruchomieniowe uczestników jest wywołania metody aktora.|
|ActorMethodStop|8|Pełne|0x2|Metody Aktor zakończył wykonywanie. Oznacza to, że zwrócił runtime asynchroniczne wywołanie metody aktora, a zadanie zwrócony przez metodę Aktor zostało zakończone.|
|ActorMethodThrewException|9|Ostrzeżenie|0x3|Wystąpił wyjątek podczas wykonywania metody Aktor podczas wykonywania asynchroniczne wywołanie metody aktora lub podczas wykonywania zadania zwracany przez metodę aktora. To zdarzenie oznacza jakiegoś błąd w kodzie aktora, który wymaga badania.|

Środowisko uruchomieniowe zaufanego aktorów publikuje następujące liczniki wydajności związane realizacją Aktor metod.

|Nazwa kategorii|Nazwa licznika|Opis|
|---|---|---|
|Metody Aktor tkaninie usługi|Wywołań na sekundę|Ile razy metody usługi aktor jest wywoływana na sekundę|
|Metody Aktor tkaninie usługi|Średnia milisekundy na wywołania|Czas wykonywania metody usługi Aktor (w milisekundach)|
|Metody Aktor tkaninie usługi|Wyjątki wygenerowany na sekundę|Liczba prób czy Aktor usługi metody zwrócił wyjątek na sekundę|

### <a name="concurrency-events-and-performance-counters"></a>Zdarzenia współbieżności i liczników wydajności
Środowisko uruchomieniowe zaufanego aktorów emituje następujące zdarzenia związane z [współbieżności](service-fabric-reliable-actors-introduction.md#concurrency).

|Nazwa zdarzenia|Identyfikator zdarzenia|Poziom|Słowo kluczowe|Opis|
|---|---|---|---|---|
|ActorMethodCallsWaitingForLock|12|Pełne|0x8|To zdarzenie jest zapisywany na początku każdej nowej Przekaż Aktor. Zawiera liczbę oczekujących połączeń aktora, oczekujących na uzyskanie blokady na aktora, wymusza oparte na włączanie współbieżności.|

Środowisko uruchomieniowe zaufanego aktorów publikuje następujące liczniki wydajności związane z współbieżności.

|Nazwa kategorii|Nazwa licznika|Opis|
|---|---|---|
|Usługa tkaninie Aktor|# Oczekiwanie na blokady Aktor połączeń Aktor|Liczba oczekujących połączeń Aktor oczekiwania na uzyskanie blokady na aktora, wymusza oparte na włączanie współbieżności|
|Usługa tkaninie Aktor|Poczekaj średnia milisekundy na blokady|Czas (w milisekundach) na uzyskanie blokady na aktora, wymusza oparte na włączanie współbieżności|
|Usługa tkaninie Aktor|Blokada Aktor średnia milisekund używana|Czas (w milisekundach), dla którego blokada na aktor jest używana|

### <a name="actor-state-management-events-and-performance-counters"></a>Liczniki wydajności i zdarzeń zarządzania stan Aktor
Środowisko uruchomieniowe zaufanego aktorów emituje następujące zdarzenia związane z [zarządzaniem stan aktora](service-fabric-reliable-actors-state-management.md).

|Nazwa zdarzenia|Identyfikator zdarzenia|Poziom|Słowo kluczowe|Opis|
|---|---|---|---|---|
|ActorSaveStateStart|10|Pełne|0x4|Środowisko uruchomieniowe uczestników jest zapisywanie stanu aktora.|
|ActorSaveStateStop|11|Pełne|0x4|Środowisko uruchomieniowe aktorów zakończył zapisywanie stanu aktora.|

Środowisko uruchomieniowe zaufanego aktorów publikuje następujące liczniki wydajności związanych z zarządzaniem stan aktora.

|Nazwa kategorii|Nazwa licznika|Opis|
|---|---|---|
|Usługa tkaninie Aktor|Stan operacji zapisywania średnia milisekundy na|Czas zapisywania stan Aktor (w milisekundach)|
|Usługa tkaninie Aktor|Średnia milisekundy na operacji stan ładowania|Czas trwania, aby załadować stan Aktor (w milisekundach)|

### <a name="events-related-to-actor-replicas"></a>Zdarzenia związane z repliki Aktor
Środowisko uruchomieniowe zaufanego aktorów emituje następujące zdarzenia związane z [repliki aktora](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-stateful-actors).

|Nazwa zdarzenia|Identyfikator zdarzenia|Poziom|Słowo kluczowe|Opis|
|---|---|---|---|---|
|ReplicaChangeRoleToPrimary|1|Przedstawienie informacji|0x1|Aktor replice zmienić rolę podstawowego. Oznacza to, że aktorów dla tego partition zostanie utworzony w tej replice.|
|ReplicaChangeRoleFromPrimary|2|Informacyjna|0x1|Aktor replice zmienić roli na inne niż podstawowe. Oznacza to, że aktorów dla tego partition już być tworzone w tej replice. Nie nowych wezwań będą dostarczane do uczestników już utworzony w tej replice. Uczestników zostaną usunięte po ukończeniu żądań w toku.|

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Aktor aktywacji i dezaktywacji wydarzeń i liczników wydajności
Środowisko uruchomieniowe zaufanego aktorów emituje następujące zdarzenia związane z [Aktor aktywacji i dezaktywacji](service-fabric-reliable-actors-lifecycle.md).

|Nazwa zdarzenia|Identyfikator zdarzenia|Poziom|Słowo kluczowe|Opis|
|---|---|---|---|---|
|ActorActivated|5|Przedstawienie informacji|0x1|Aktor został aktywowany.|
|ActorDeactivated|6|Przedstawienie informacji|0x1|Aktor został dezaktywowany.|

Środowisko uruchomieniowe niezawodne aktorów publikuje następujące liczniki wydajności związane z Aktor aktywacji i dezaktywacji.

|Nazwa kategorii|Nazwa licznika|Opis|
|---|---|---|
|Usługa tkaninie Aktor|Obliczanie średniej wartości milisekund OnActivateAsync|Czas wykonywania metody OnActivateAsync (w milisekundach)|

### <a name="actor-request-processing-performance-counters"></a>Przetwarzanie wezwania Aktor liczniki wydajności
Gdy klient wywołuje metodę za pośrednictwem obiektu serwera proxy aktora, wyników w wiadomości żądania wysyłane przez sieć z usługą aktora. Usługa przetwarza komunikat żądania i wysyła odpowiedź do klienta. Środowisko uruchomieniowe zaufanego aktorów publikuje następujące liczniki wydajności związane z przetwarzanie wezwania aktora.

|Nazwa kategorii|Nazwa licznika|Opis|
|---|---|---|
|Usługa tkaninie Aktor|# wysłanych żądań|Liczba żądań przetwarzanych w usłudze|
|Usługa tkaninie Aktor|Średnia milisekundy na żądanie|Czas (w milisekundach) przez usługę przetwarzania żądania|
|Usługa tkaninie Aktor|Średnia milisekundach deserializacji żądania|Czas (w milisekundach) zdeserializować wiadomości z wezwaniem Aktor odebraniu w punkcie usług|
|Usługa tkaninie Aktor|Średnia milisekundach szeregowania odpowiedzi|Czas (w milisekundach) szeregować Aktor odpowiedź w punkcie usług, przed wysłaniem odpowiedzi do klienta|

## <a name="next-steps"></a>Następne kroki
 - [Jak korzystać z zaufanego aktorów platformy tkaninie usługi](service-fabric-reliable-actors-platform.md)
 - [Aktor interfejsu API dokumentacji](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Przykładowy kod](https://github.com/Azure/servicefabric-samples)
