<properties
 pageTitle="Centrum IoT HA i DR | Microsoft Azure"
 description="W tym artykule opisano funkcje, które ułatwiają tworzenie wysokiej dostępności rozwiązań IoT z awarii możliwości odzyskiwania."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="02/03/2016"
 ms.author="elioda"/>

# <a name="iot-hub-high-availability-and-disaster-recovery"></a>Centrum IoT wysoka odzyskiwania dostępności i danych

Jako usługa Azure Centrum IoT zapewnia wysoką dostępność (HA) za pomocą zwolnień na poziomie Azure region, bez dodatkowych starań wymagane przez to rozwiązanie. Ponadto Azure udostępnia wiele funkcji, które ułatwiają tworzenie rozwiązań z zastosowanie (DR) funkcji lub dostępność krzyżowe region, w razie potrzeby. Należy zaprojektować i przygotować środowisko rozwiązań skorzystać z tych funkcji DR, jeśli chcesz podać globalnej, region krzyżowe wysoką dostępność dla urządzeń lub użytkowników. W artykule [Wskazówki techniczne ciągłości firm Azure](../resiliency/resiliency-technical-guidance.md) opisano wbudowanych funkcji platformy Azure ciągłości i DR. [Odzyskiwanie i wysoką dostępność dla aplikacji Azure][] papieru zawiera architektura wskazówki dotyczące strategii do osiągnięcia HA oraz oprogramowania DR Azure aplikacji.

## <a name="azure-iot-hub-dr"></a>Azure DR Centrum IoT
Oprócz HA wewnątrz obszaru Centrum IoT zawiera mechanizmy pracy awaryjnej awarii, które wymagają nie działania użytkownika. DR Centrum IoT własny została zainicjowana i cel czasu odzyskiwania (RTO), 2-26 godzin i następujących celów punktu odzyskiwania (RPOs).

| Funkcje | RPO |
| ------------- | --- |
| Dostępność usługi dla operacji rejestru i komunikacji | Utracie CName |
| Dane o tożsamości w rejestrze tożsamości urządzenia | 0-5 min utratą danych |
| Urządzenia w chmurze wiadomości | Wszystkich nieprzeczytanych wiadomości zostaną utracone |
| Operacje monitorowanie wiadomości | Wszystkich nieprzeczytanych wiadomości zostaną utracone |
| Wiadomości w chmurze do urządzenia | 0-5 min utratą danych |
| Kolejka opinii chmury do urządzenia | Wszystkich nieprzeczytanych wiadomości zostaną utracone |

## <a name="regional-failover-with-iot-hub"></a>Regionalne pracy awaryjnej z Centrum IoT

Wykonano traktowania topologii wdrożenia w rozwiązaniach IoT wykracza poza zakres tego artykułu, ale w celu szybkiej i odzyskiwanie firma Microsoft będzie należy rozważyć, czy model wdrażania *regionalnych pracy awaryjnej* .

W modelu regionalne pracy awaryjnej wewnętrznej rozwiązanie jest uruchomiony przede wszystkim w jednym miejscu centrum danych, ale dodatkowe Centrum IoT i wewnętrznej wdrożonych w innej lokalizacji centrum danych na potrzeby pracy awaryjnej w przypadku awarii cierpi Centrum IoT w podstawowego centrum danych lub jakiś sposób przerwaniu łączności z urządzenia podstawowego centrum danych. Urządzenia za pomocą punkt końcowy pomocnicza usługa przy każdym podstawowego bramy jest nieosiągalny. Z możliwością pracy awaryjnej krzyżowe region można zwiększyć dostępność rozwiązania poza wysoki poziom dostępności jednego regionu.

Na wysokim poziomie do wdrożenia modelu regionalne pracy awaryjnej z Centrum IoT będą potrzebne następujące elementy.

* **Pomocnicza Centrum IoT i urządzenia routingu logiczny**: w przypadku awarii usługi w danym regionie podstawowego, urządzenia musi rozpocząć nawiązywanie połączenia z danym regionie pomocniczą. Charakter pamiętać o stanie większości usług, są często administratorom rozwiązanie wyzwalanie między region awaryjnego procesu. Najlepszym sposobem na komunikowanie się nowy punkt końcowy dla urządzeń, zachowując kontrolę nad procesem, jest regularnego sprawdzi usługi *Konsjerż* dla aktywnego punktu końcowego. Usługa Konsjerż może być aplikacji sieci web proste, replikowane i przechowywane dostępne za pomocą techniki przekierowania DNS (na przykład za pomocą [Menedżera ruch Azure][]).
* **Replikacja rejestru tożsamości** — w celu można używać, IoT pomocniczym, Centrum musi zawierać wszystkich tożsamości urządzeń łączących się rozwiązanie. Rozwiązanie należy zachować replikowane geo kopie zapasowe tożsamości urządzenia i przekazać je do pomocniczego koncentratora IoT przed przełączeniem aktywnego punktu końcowego dla urządzeń. Funkcje eksportu tożsamości urządzenia Centrum IoT jest bardzo przydatne w tym kontekście. Aby uzyskać więcej informacji zobacz [Przewodnik dewelopera Centrum IoT - rejestru tożsamości][].
* **Scalanie logiczny** - podstawowy obszar stał się dostępny ponownie, wszystkie stan i dane, które zostały utworzone w witrynie pomocniczej należy przeprowadzić migrację z powrotem do obszaru podstawowego. Odnosi się to głównie do urządzenia tożsamości i aplikacji metadane, które musi zostać połączony z podstawowego koncentratora IoT i wszystkich innych sklepów specyficzne dla aplikacji, w obszarze podstawowy. Aby uprościć ten krok, zazwyczaj zaleca się używanie operacji idempotent. To minimalizuje efekty stronie nie tylko z ewentualnego przekazania spójne zdarzeń, ale z duplikaty i dostarczanie w nowym oknie kolejność zdarzeń. Ponadto logiki aplikacji powinny zostać zaprojektowane do tolerować potencjalne niespójności lub "nieco" poza Data stanu. To jest ze względu na dodatkowy czas potrzebny do "poprawianego" systemu na podstawie celów punktu odzyskiwania (RPO).

## <a name="next-steps"></a>Następne kroki

Wykonaj te łącza, aby dowiedzieć się więcej o Centrum IoT Azure:

- [Rozpoczynanie pracy z IoT koncentratory (samouczek)][lnk-get-started]
- [Co to jest Centrum IoT Azure?][]

[Odzyskiwanie i wysoką dostępność dla aplikacji Azure]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Menedżer ruchu]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Centrum deweloperów Guide - rejestru tożsamości]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[Co to jest Centrum IoT Azure?]: iot-hub-what-is-iot-hub.md
