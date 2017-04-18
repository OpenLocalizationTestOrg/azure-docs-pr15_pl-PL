<properties
   pageTitle="Zaawansowane Użycie usług zaufanego | Microsoft Azure"
   description="Informacje na temat zaawansowanych funkcji usługi tkaninie zaufanego obsługę elastycznie usług."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="advanced-usage-of-the-reliable-services-programming-model"></a>Zaawansowane zastosowania zaufanego modelu programowania usług
Azure tkaninie usługi upraszcza pisania usług oraz zarządzania nimi niezawodnego bezstanowe i stanowe. Ten przewodnik będzie porozmawiać na temat zaawansowanych zaimka zaufania usługi, aby uzyskać więcej kontrolę i elastyczność przez usługi. Przed czytania tego przewodnika, zapoznaj się z [Zaufanego modelu programowania usług](service-fabric-reliable-services-introduction.md).

Usługi zarówno stanowe i bezpaństwowców mieć dwóch punktów wejścia podstawowego dla kodu użytkownika:

 - `RunAsync`jest punktem wejścia ogólnego zastosowania kodu usługi.
 - `CreateServiceReplicaListeners`i `CreateServiceInstanceListeners` jest otwierania komunikacji detektory żądania klienta.
 
W przypadku większości usług te punkty wejścia dwa są wystarczające. Czasami podczas większą kontrolę nad świadczenia usługi jest wymagane, zdarzenia dodatkowe czasu trwania są dostępne.

## <a name="stateless-service-instance-lifecycle"></a>Usługa Stateless wystąpienie cyklu

Cykl życia stateless usługi jest bardzo proste. Bezstanowa usługi można tylko być otwarty, zamknięte lub zostało przerwane. `RunAsync`w usłudze stateless jest wykonywane po wystąpienie usługi jest otwarty, a zostało anulowane w przypadku zamknięcia lub przerwane wystąpienie usługi. 

Mimo że `RunAsync` powinny być wystarczające w niemal wszystkich przypadkach, otwieranie, zamykanie i zdarzeń przerwanie w usłudze stateless są również dostępne:

- `Task OnOpenAsync(IStatelessServicePartition, CancellationToken)`
  OnOpenAsync nosi nazwę podczas wystąpienia stateless usługi ma być używany. Rozszerzony serwis Inicjowanie zadania można wykonywać w tej chwili.

- `Task OnCloseAsync(CancellationToken)`
  OnCloseAsync nosi nazwę podczas wystąpienia usługi stateless ma bezpiecznie zamknięty. Może to występować podczas kod usługi jest uaktualniany, wystąpienie usługi został przeniesiony z powodu równoważenia obciążenia lub przejściowych błędów zostanie wykryta. OnCloseAsync może służyć do bezpiecznie zamknąć wszystkie zasoby, Zatrzymaj przetwarzanie w tle, Zakończ zapisywanie stanu zewnętrznych lub zamknij istniejące połączenia.

- `void OnAbort()`
  OnAbort nosi nazwę podczas wystąpienia stateless usługi jest wymusza zamykana. Zazwyczaj jest to polecenie, gdy błąd trwały zostanie wykryta w węźle, lub gdy tkaninie usługi nie mogą zarządzać niezawodne cyklu życia wystąpienie usługi ze względu na błędy wewnętrzne.

## <a name="stateful-service-replica-lifecycle"></a>Cykl życia replice stanowe usługi

Cykl życia replice akumulującej stan usługi jest bardziej złożony niż wystąpienie stateless usługi. Ponadto otwieranie, zamykanie i przerwać zdarzenia, replice stanowe usług podlega zmianom roli podczas jego użytkowania. Zmiana replice stanowe usługi roli, `OnChangeRoleAsync` wyzwolenia zdarzenia:

- `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`
  OnChangeRoleAsync nosi nazwę podczas replice usługi stanowe zmiany roli, na przykład podstawowy i pomocniczy. Podstawowy repliki podano stan zapisu (są dozwolone tworzenie i zapisywanie zaufanego zbiorów). Pomocnicza repliki podano Stan odczytu (tylko może czytać z istniejących zbiorów zaufanego). Większość pracy w usłudze stanowe odbywa się w replice podstawowego. Pomocnicza repliki mogą wykonywać sprawdzanie poprawności tylko do odczytu, Generowanie raportu, wyszukiwania danych lub innych zadań tylko do odczytu.

W usługą stanowe podstawowego replice ma dostęp do zapisu do stanu i dlatego jest na ogół gdy usługa działa praca rzeczywista. `RunAsync` Metody akumulującej stan usługi jest wykonywane tylko wtedy, gdy w replice akumulującej stan usługi jest dokumentem głównym. `RunAsync` Metody anulowaniu zmiany od podstawowy, a także podczas zdarzeń Zamknij i przerwanie roli replice podstawowego. 

Za pomocą `OnChangeRoleAsync` zdarzenia umożliwia wykonywanie pracy w zależności od replice roli oraz odpowiedzi na zmiany roli.

Stanowe Usługa udostępnia również tego samego cztery zdarzenia cyklu życia stateless usług, z same znaczenie i przypadków użycia:

- `Task OnOpenAsync(IStatefulServicePartition, CancellationToken)`
- `Task OnCloseAsync(CancellationToken)`
- `void OnAbort()`



## <a name="next-steps"></a>Następne kroki
Dla bardziej zaawansowanych tematów związanych z tkaninie usługi zobacz następujące artykuły:

- [Konfigurowanie usług zaufanego stanowe](service-fabric-reliable-services-configuration.md)

- [Wprowadzenie kondycji usługi tkaninie](service-fabric-health-introduction.md)

- [Przy użyciu raportów dotyczących kondycji systemu do rozwiązywania problemów](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

- [Konfigurowanie usług z usługi tkaninie klaster Menedżera zasobów](service-fabric-cluster-resource-manager-configure-services.md)
