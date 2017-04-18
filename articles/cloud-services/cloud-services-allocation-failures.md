<properties
    pageTitle="Rozwiązywanie problemów z błąd alokacji usługi w chmurze | Microsoft Azure"
    description="Rozwiązywanie problemów z błąd alokacji wdrażanie usług w chmurze platformy Azure"
    services="azure-service-management, cloud-services"
    documentationCenter=""
    authors="simonxjx"
    manager="felixwu"
    editor=""
    tags="top-support-issue"/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="v-six"/>



# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Rozwiązywanie problemów z błąd alokacji wdrażanie usług w chmurze platformy Azure

## <a name="summary"></a>Podsumowanie
Gdy wdrażanie wystąpienia do usługi w chmurze lub Dodawanie nowej sieci web lub wystąpienia roli Pracownik, Microsoft Azure przydziela zasoby obliczeń. Od czasu do czasu mogą występować błędy, podczas wykonywania tych operacji, nawet przed osiągnięciem limitów Azure subskrypcji. W tym artykule opisano niektóre typowe błędy alokacji przyczyn i sugerowanych działań naprawczych możliwe. Informacje także mogą być przydatne podczas planowania wdrożenia usługi.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Tło — działania Alokacja
Serwery w Azure centrach danych są podzielone na klastrów. Nowe żądanie alokacji usługi cloud próbie w wielu klientów. Po wdrożeniu pierwsze wystąpienie usługi w chmurze (w tymczasowym lub produkcji), która usługa w chmurze otrzymuje przypięta do klastrów. W tym samym klastrze stanie wszelkie dodatkowe wdrożeniach dla usług w chmurze. W tym artykule firma Microsoft będzie odwołują się do tego, jak "przypięta do klastrów". Diagram 1 poniżej przedstawia sprawę normalny alokacji, w której jest podejmowana próba nawiązania w wielu klastrów; Diagramie 2 w przypadku przydział którą ma przypięta do klaster 2, ponieważ jest to miejsce, w którym znajduje się istniejących CS_1 usługi w chmurze.

![Diagram Alokacja](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Dlaczego się dzieje błąd alokacji
Gdy żądania przydzielenia zostanie przypięta do klastrów, to wyższy stopień nie można znaleźć bezpłatne zasoby, w związku z puli zasobów dostępnych jest ograniczona do klastrów. Ponadto jeśli żądania alokacji zostanie przypięta do klastrów, ale typ zasobu, żądanej nie jest obsługiwane przez ten klaster, żądania zakończy się niepowodzeniem nawet wtedy, gdy klaster ma bezpłatne zasobów. Diagram 3 poniżej przedstawia sprawę, w którym przypiętej alokacji kończy się niepowodzeniem, ponieważ klaster tylko candidate nie ma zwolnić zasoby. Diagram 4 przedstawia wielkość liter, gdzie przypiętej alokacji kończy się niepowodzeniem, ponieważ klaster candidate tylko nie obsługuje żądany rozmiar pamięci Wirtualnej, mimo że klaster ma zwolnić zasoby.

![Błąd przypiętej alokacji](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Błąd alokacji rozwiązywania problemów dla usług w chmurze
### <a name="error-message"></a>Komunikat o błędzie
Może zostać wyświetlony następujący komunikat o błędzie:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>Typowe problemy
Poniżej przedstawiono typowe scenariusze alokacji, powodujące żądania przydzielenia do być przypięta do jeden klaster.

- Wdrażanie przedział tymczasowego — Jeśli usługi w chmurze wdrożeniu w obu przedział, następnie usługi w chmurze całego zostanie przypięta do określonych klaster.  Oznacza to, że jeśli wdrożeniu już istnieje w przedział produkcji, następnie nowego wdrożenia tymczasowy tylko można przydzielić w tym samym klastrze jako przedział produkcji. Jeśli klaster zbliża wydajność, żądania może zakończyć się niepowodzeniem.

- Skalowanie — Dodawanie nowych wystąpień do istniejącej usługi w chmurze musisz przydzielić w tym samym klastrze.  Zwykle można przydzielić małe skalowania żądania, ale nie zawsze. Jeśli klaster zbliża wydajność, żądania może zakończyć się niepowodzeniem.

- Grupa koligacji — nowe wdrożenia do usługi w chmurze pustego można przypisać przez tkaninie w dowolnym klaster w danym regionie chyba że usługa w chmurze zostanie przypięta do grupy koligacji. Wdrożenia do tej samej grupy koligacji wypróbowywania na tym samym klastrze. Jeśli klaster zbliża wydajność, żądania może zakończyć się niepowodzeniem.

- Grupa koligacji vNet - starsze wirtualnych sieci zostały związane grupom koligacji zamiast regionów, a usługami w chmurze w tych wirtualnych sieci może być przypięta do klaster koligacji grupy. Wdrożeniach do tego typu wirtualną sieć wypróbowywania w klastrze przypięty. Jeśli klaster zbliża wydajność, żądania może zakończyć się niepowodzeniem.

## <a name="solutions"></a>Rozwiązania

1. Ponownie wdróż do nowej usługi w chmurze — tego rozwiązania jest prawdopodobnie najbardziej popularnych jak pozwala na platformie go wybrać z wszystkich klastrów w danym regionie.

    - Wdrażanie obciążenie pracą do nowej usługi w chmurze  

    - Aktualizowanie CNAME lub rekord, aby wskazywały ruch nowej usługi w chmurze

    - Po zero ruch ma starej witryny, możesz usunąć stare usługi w chmurze. To rozwiązanie powinny podlegać zero przestoje.

2. Usuwanie produkcji i tymczasowej gniazda — to rozwiązanie zachowuje nazwę istniejącego DNS, ale spowoduje, że przestoje do aplikacji.

    - Usuwanie produkcji i tymczasowej gniazda istniejące usługi w chmurze, aby usługa w chmurze jest pusta, a następnie

    - Tworzenie nowego wdrożenia w istniejącej usługi w chmurze. To spróbuje ponownie podział na wszystkich klastrów w regionie. Upewnij się, że usługa w chmurze nie jest powiązany z grupą koligacji.

3. Zastrzeżony adres IP - tego rozwiązania zachowuje swój adres IP istniejącego adresu, ale spowoduje, że przestoje do aplikacji.  

    - Tworzenie ReservedIP dla istniejącego wdrożenia przy użyciu programu Powershell

    ```
    New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
    ```

    - Wykonaj #2 miejsc położonych powyżej, pamiętając określić nowe ReservedIP w CSCFG usługi.

4. Usuwanie grupy koligacji dla nowych wdrożeń - zaleca się już koligacji grup. Wykonaj kroki 1 # powyżej wdrożenia nowej usługi w chmurze. Upewnij się, usługa w chmurze nie znajduje się w grupie koligacji.

5. Konwertowanie regionalne wirtualnej sieci — Zobacz, [jak przeprowadzić migrację z grup koligacji regionalne wirtualną siecią (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
