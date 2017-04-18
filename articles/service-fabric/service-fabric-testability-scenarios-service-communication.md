<properties
   pageTitle="Pola: Usługi komunikacji | Microsoft Azure"
   description="Komunikacja do usługi jest punkt krytyczne integracji aplikacji usługi tkaninie. W tym artykule omówiono zagadnienia projektowe i techniki testowania."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/06/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-testability-scenarios-service-communication"></a>Scenariusze pola tkaninie usługi: usługa komunikacji

Powierzchnia opartych na usługach architektura style w sposób naturalny na tkaninie usługi Azure i Microservices. W następujących typach architektury rozłożone składnikowa microservice aplikacji zwykle składają się z wielu usług, które muszą się ze sobą. Nawet najłatwiejszym przypadków zazwyczaj masz co najmniej usługi sieci web bezstanowe i usługi miejsca do magazynowania stanowe danych, które muszą się komunikować.

Komunikacji do usługi jest punkt integracji krytycznych aplikacji, ponieważ każda usługa udostępnia zdalnego interfejsu API z innymi usługami. Praca z zestawu ograniczenia interfejsu API zazwyczaj obejmuje we/wy wymaga niektórych care z dobrym ilość testowanie i zatwierdzanie.

Istnieje wiele zagadnienia, aby podczas granice te usługi są połączone ze sobą w Rozproszony system:

 - *Protokół transportowy*. Czy zamierzasz korzystać HTTP współdziałanie zwiększona lub binarne protokołu niestandardowego maksymalnej przepustowości?
 - *Obsługa błędów*. Jak są obsługiwane stałych i przejściowych błędy? Co się stanie, gdy usługa zostanie przeniesiony do innego węzła?
 - *Limity czasu i opóźnienie*. W aplikacjach rozwiązania jak będzie każdej warstwy, usługa obsługuje opóźnienie między kolejnymi nałożonymi na siebie i do użytkownika?

Niezwykle ważne zapewnienie elastyczność w aplikacji jest używany jeden z składniki komunikacji wbudowanej usługi dostarczony przez usługę tkaninie czy tworzenia własnych, testując interakcje między usługami.

## <a name="prepare-for-services-to-move"></a>Przygotowywanie do usług przenieść

Wystąpienia usług może poruszanie się w czasie. To jest szczególnie, gdy skonfigurowano metryki Załaduj do równoważenia dostosowanych niestandardowe optymalnego zasobów. Usługi tkaninie powoduje przejście do wystąpień usług maksymalizowanie ich dostępność nawet podczas uaktualniania, praca awaryjna, poza skalowanie i innych sytuacjach, które występują w okresie istnienia Rozproszony system.

Jak usług poruszanie się w grupie, klientów i innych usług należy przygotować się do obsługi dwa scenariusze, gdy zwróć się do usługi:

- W replice wystąpienie lub partycją usługi został przeniesiony od ostatniego obliczenia, które zawsze mówię do niego. Jest to normalny część świadczenia usługi i można się spodziewać nastąpić czasie trwania aplikacji.
- W replice wystąpienie lub partycją usługi jest w trakcie przenoszenia. Mimo że w tkaninie usługi bardzo szybko wystąpi awaryjne przeniesienie usługi z jednego węzła do innego, mogą wystąpić opóźnienie w udostępnianiu Jeżeli komunikacji część tej usługi jest powolne rozpocząć.

Obsługa scenariuszom bezpiecznie jest ważna w przypadku systemu gładkie pracy. W tym celu należy pamiętać, że:

- Każdej usługi, które mogą być połączone z ma *adres* , który wykrywa ona na (na przykład HTTP lub WebSockets). Po przeniesieniu wystąpienia usługi lub partycją, zmienia punktu końcowego adresu. (Powoduje przejście do innego węzła przy użyciu innego adresu IP.) Jeśli używasz składniki komunikacyjne wbudowane będą je obsługiwać ponownie rozpoznawania adresów usługi dla Ciebie.
- Czasami może występować tymczasowy wzrost opóźnienie usługi podczas uruchamiania wystąpienia usługi w górę jego odbiornika ponownie. To zależy od szybkiego usługę otwiera odbiornika po przeniesieniu wystąpienia usługi.
- Wszelkie istniejące połączenia trzeba zamknąć i otworzyć ponownie po otwarciu usługi w nowym węźle. Węzeł bezpieczne zamknięcie lub ponowne uruchomienie umożliwia czas prawidłowe zamknięcie istniejące połączenia.

### <a name="test-it-move-service-instances"></a>Należy je przetestować: przenoszenie wystąpień usługi

Za pomocą narzędzi pola tkaninie usługi, mogą tworzyć scenariusz test, aby przetestować tych sytuacji na różne sposoby:

1. Przenoszenie replice podstawowego akumulującej stan usługi.

    Dla dowolnej liczby powodów, dla których mogą być przenoszone podstawowego replice z partycją akumulującej stan usługi. Umożliwia docelowe podstawowego replice o określonym partition, aby zobaczyć, jak usług reagować na Przenieś w sposób bardzo kontroli.

    ```powershell

    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService

    ```

2. Zatrzymanie węzła.

    Po zatrzymaniu węzeł tkaninie usługi przeniesienie wszystkich wystąpień usługi lub partycje, które były w tym węźle na jedną z dostępnych węzłach w klastrze. Umożliwia testowanie sytuacji miejsce, w którym węzeł zostaną utracone z klaster i wszystkich wystąpień usług i repliki w tym węźle trzeba przenieść.

    Węzeł można zatrzymać za pomocą polecenia cmdlet programu PowerShell **Zatrzymaj ServiceFabricNode** :

    ```powershell

    PS > Restart-ServiceFabricNode -NodeName Node_1

    ```

## <a name="maintain-service-availability"></a>Obsługa dostępność usługi

Jako platformy tkaninie usługi służy do zapewnienia wysokiej dostępności usług. Jednak w sytuacjach skrajnych źródłowych problemy związane z infrastrukturą nadal mogą powodować niedostępności. Należy przetestować w następujących scenariuszach zbyt.

Za pomocą systemu kworum stanowe usług replikować stan wysokiej dostępności. Oznacza to, że kworum replik musi być dostępna do wykonywania operacji zapisu. Czasami, takich jak awarii sprzętu szerokie kworum replik nie mogą być dostępne. W tych przypadkach nie będzie mógł wykonywać operacje zapisu, ale nadal będzie mógł wykonywać operacje odczytu.

### <a name="test-it-write-operation-unavailability"></a>Należy je przetestować: pisanie niedostępności operacji

Przy użyciu narzędzia pola tkaninie usługi, można wprowadzić błędów, która powoduje utratę kworum jako test. Mimo że takiej sytuacji jest rzadkich, ważne jest klienci i usług, które są zależne od usługi stanowe są przygotowane sobie z sytuacjami miejsce, w którym nie można ich wprowadzać, Zapisz żądania do niego. Jest również ważne, że sama usługa stanowe zna tej możliwości i bezpiecznie można komunikować dotyczące obiektów wywołujących.

Utrata kworum może wywołać przy użyciu polecenia cmdlet programu PowerShell **Wywołaj ServiceFabricPartitionQuorumLoss** :

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

W tym przykładzie możemy ustaw `QuorumLossMode` do `QuorumReplicas` oznaczającą chcemy powodują utratę kworum bez konieczności w dół wszystkie repliki. W ten sposób operacji odczytu są nadal dostępne. Aby przetestować scenariusza, gdzie cały partycją jest niedostępna, można ustawić ten przełącznik `AllReplicas`.

## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej o akcje pola](service-fabric-testability-actions.md)

[Dowiedz się więcej o scenariusze pola](service-fabric-testability-scenarios.md)
