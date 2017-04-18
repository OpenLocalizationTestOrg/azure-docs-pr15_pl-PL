<properties 
    pageTitle="Przenoszenie zasobów do nowej grupy zasobów | Microsoft Azure" 
    description="Aby przenieść zasoby do nowej grupy zasobów lub innej subskrypcji za pomocą Menedżera zasobów Azure." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/21/2016" 
    ms.author="tomfitz"/>

# <a name="move-resources-to-new-resource-group-or-subscription"></a>Przenoszenie zasobów do nowej grupy zasobów lub innej subskrypcji

W tym temacie przedstawiono sposób przenoszenia zasobów do nowej subskrypcji lub nowej grupy zasobów w tej samej subskrypcji. Portalu, programu PowerShell, polecenie Azure lub interfejsu API usługi REST umożliwia przenoszenie zasobu. Operacje przenoszenia w tym temacie są dostępne bez pomocy od pomocy technicznej Azure.

Zazwyczaj możesz przejść zasobów, jeśli zdecydujesz, że:

- Do celów rozliczeń zasób ma są przechowywane w innej subskrypcji.
- Zasób nie jest już udostępnia samego cyklu życia jako zasoby, które były wcześniej zgrupowane z. Chcesz przenieść do nowej grupy zasobów, aby umożliwić zarządzanie tego zasobu niezależnie od innych zasobów.

Podczas przenoszenia zasobów, zarówno w grupie źródła, jak i w grupie docelowej są blokowane w trakcie operacji. Pisanie i usuwanie operacji są blokowane w grupach, aż do zakończenia Przenieś.

Nie można zmienić lokalizację zasobu. Przeniesienie zasobu tylko przeniesienie go do nowej grupy zasobów. Nowa grupa zasobów może być innej lokalizacji, ale nie zmienia lokalizację zasobu.

> [AZURE.NOTE] W tym artykule opisano sposób przenoszenia zasobów w istniejącej Azure kont wysyłania ofert. Jeśli chcesz zmienić konto Azure oferuje (na przykład uaktualnianie repartycyjny wstępnie płatności), pozostawiając do pracy z istniejących zasobów, zobacz [Przełączanie subskrypcji Azure na inną ofertę](billing-how-to-switch-azure-offer.md). 

## <a name="checklist-before-moving-resources"></a>Lista kontrolna przed przeniesieniem zasobów

Istnieje kilka ważnych czynności do wykonania przed przejściem zasobu. Sprawdzając następujące warunki, można uniknąć błędów.

1. Usługi, należy włączyć możliwość przenoszenia zasobów. Zobacz poniższą listą dowiedzieć się, które [usługi umożliwiają przenoszenie zasobów](#services-that-enable-move).
1. Subskrypcje źródłowa i docelowa musi istnieć w tej samej [dzierżawy usługi Active Directory](./active-directory/active-directory-howto-tenant.md). Aby przejść do nowej dzierżawy, zadzwoń do pomocy technicznej.
2. Subskrypcja miejsce docelowe musi być zarejestrowany dostawcy zasobów zasobu przenoszony. Jeśli nie otrzymujesz komunikat o błędzie z informacją, że **Subskrypcja nie jest zarejestrowany dla danego typu zasobu**. Ten problem mogą wystąpić podczas przenoszenia zasobu do nowej subskrypcji, ale ta nigdy nie został użyty subskrypcji z danym typem zasobu. Aby dowiedzieć się, jak sprawdzić stan rejestracji i rejestrować dostawców zasobów, zobacz [typy i dostawców zasobów](../resource-manager-supported-services.md#resource-providers-and-types).
4. Jeśli przenosisz aplikacji usługi aplikacji przejrzeniu [ograniczeń usługi aplikacji](#app-service-limitations).
4. Jeśli przenosisz zasobów związanych z usługami odzyskiwania przejrzeniu [ograniczenia usługi odzyskiwania](#recovery-services-limitations)
5. Jeśli przenosisz zasoby są wdrażane za pomocą modelu Klasyczny przejrzeniu [ograniczenia wdrażania klasyczny](#classic-deployment-limitations).

## <a name="when-to-call-support"></a>Kiedy Zadzwoń do pomocy technicznej

Możesz przenieść większość zasobów za pomocą Samoobsługowego operacje, w tym temacie. Używanie samoobsługowego operacje, aby:

- Przenosić zasoby Menedżera zasobów.
- Przenoszenie klasyczny zasobów według [ograniczenia klasyczny wdrożenia](#classic-deployment-limitations). 

Gdy trzeba Zadzwoń do pomocy technicznej:

- Przenoszenie zasobów do nowego konta Azure (i dzierżawy usługi Active Directory).
- Przenoszenie klasyczny zasobów, ale występują problemy z ograniczeniami.

## <a name="services-that-enable-move"></a>Usługi, które umożliwiają przenoszenie

Na obecnym są usług, które umożliwiają przechodzenie do nowej grupy zasobów i subskrypcji:

- Interfejs API zarządzania
- Aplikacje usługi aplikacji (aplikacje web apps) — zobacz [ograniczenia aplikacji usługi](#app-service-limitations)
- Automatyzacji
- Partii
- SIECI CDN
- Usług w chmurze — zobacz [ograniczenia dotyczące rozmieszczania klasyczny](#classic-deployment-limitations)
- Factory danych
- DNS
- DocumentDB
- Usługa HDInsight klastrów
- Koncentratory IoT
- Kluczowe magazynu 
- Usługi multimediów
- Zaangażowania urządzeń przenośnych
- Powiadomienie o koncentratory
- Wnioski operacyjne
- Redis pamięci podręcznej
- Harmonogram
- Wyszukiwanie
- Usługa Bus
- Miejsca do magazynowania
- Magazyn (klasyczny) — zobacz [ograniczenia dotyczące rozmieszczania klasyczny](#classic-deployment-limitations)
- Baza danych SQL server — bazy danych i serwera musi znajdować się w tej samej grupy zasobów. Po przeniesieniu programu SQL server, wszystkie swoje bazy danych również są przenoszone.
- Maszyn wirtualnych -, czy nie obsługuje Przechodzenie do nowej subskrypcji, gdy jego certyfikaty są przechowywane w magazynu klucza
- Maszyn wirtualnych (klasyczny) — zobacz [ograniczenia wdrażania klasyczny](#classic-deployment-limitations)
- Wirtualnych sieci

## <a name="services-that-do-not-enable-move"></a>Usługi, które nie umożliwiają przenoszenie

Są usługi, które obecnie nie umożliwiają przenoszenie zasobu:

- Brama aplikacji
- Wnioski aplikacji
- Trasa Express
- Magazynu usługi odzyskiwania - również nie przenieść skojarzone z magazynu usługi odzyskiwania zasoby obliczeń, sieci i miejsca do magazynowania, zobacz [ograniczenia usługi odzyskiwania](#recovery-services-limitations).
- Maszyn wirtualnych z certyfikatem przechowywanych w klucza magazynu
- Zestawy skali maszyn wirtualnych
- Wirtualnych sieci (klasyczny) — zobacz [ograniczenia wdrażania klasyczny](#classic-deployment-limitations)
- Brama VPN

## <a name="app-service-limitations"></a>Ograniczenia dotyczące usług aplikacji

Podczas pracy z aplikacjami usług aplikacji, nie można przenosić plan aplikacji usługi. Aby przenieść aplikacji usługi aplikacji, dostępne są następujące opcje:

- Przenoszenie plan usług aplikacji i inne zasoby aplikacji usługi w tej grupie zasobów do nowej grupy zasobów, która nie ma jeszcze zasoby aplikacji usługi. Wymaganie to oznacza, że należy przenieść nawet zasoby aplikacji usługi, które nie są skojarzone z planem usługi aplikacji. 
- Przenieś aplikacje do różnych grupach zasobów, ale zachować wszystkie plany aplikacji usługi w oryginalnej grupie zasobów.

Jeśli oryginalną grupę zasobów zawiera również zasób wniosków aplikacji, nie można przenieść tego zasobu, ponieważ wniosków aplikacji nie obsługuje obecnie operacji przenoszenia. Podczas przenoszenia aplikacji usługi aplikacje dotyczy zasobu wniosków aplikacji, operacji przenoszenia całej zakończy się niepowodzeniem. Jednak wniosków aplikacji i Planowanie aplikacji usługi nie muszą znajdować się w tej samej grupy zasobów jako aplikacja aplikacji do prawidłowego działania.

Na przykład, jeśli zawiera grupy zasobów:

- **w sieci Web** skojarzone z **planu a** i **aplikacji wniosków a**
- **b w sieci web** , który jest skojarzony z **planu b** i **aplikacji wniosków b**

Dostępne są następujące opcje:

- Przenoszenie **w sieci web**, **plan a**, **web-b**i **b planu**
- Przenoszenie **web a** i **b w sieci web**
- Przenoszenie **w sieci web**
- Przenoszenie **b w sieci web**

Wszystkie inne kombinacje obejmować przenoszenie typ zasobu, którego nie można przenieść (więcej informacji o aplikacji) lub pozostawiając typ zasobu, który nie może pozostać pod spód, podczas przenoszenia plan usług aplikacji (dowolny typ zasobu usługi aplikacji).

Jeśli aplikacji sieci web znajduje się w różnych grupach zasobów niż swój plan aplikacji usługi, ale chcesz przenieść zarówno do nowej grupy zasobów, należy wykonać czynności w dwóch krokach. Na przykład:

- **w sieci Web** znajduje się w **grupie sieci web**
- **Plan a** znajduje się w **grupie planu**
- Chcesz **w sieci web** i **plan-a** do znajdują się w **grupie połączone**

Aby osiągnąć ten przenoszenia, operacji dwóch oddzielnych Przenieś w następującej kolejności:

1. Przenoszenie **w sieci web** do **grupy planu**
2. Przenoszenie **w sieci web** i **plan w** grupie **połączone**.

Certyfikat usługi aplikacji można przenieść do nowej grupy zasobów lub subskrypcji bez żadnych problemów. Jednak jeśli aplikacji sieci web zawiera certyfikat SSL zakupionych zewnętrznie i przekazać do aplikacji, możesz usunąć certyfikat przed przeniesieniem aplikacji sieci web. Na przykład można wykonywać następujące czynności:

1. Usuwanie przekazanego certyfikat z aplikacji sieci web
2. Przenoszenie aplikacji sieci web
3. Przekazywanie certyfikatu do aplikacji sieci web

## <a name="recovery-services-limitations"></a>Ograniczenia usługi odzyskiwania

Przenoszenie, nie jest włączona dla miejscem do magazynowania, sieci, lub obliczyć zasobów użyte do skonfigurowania funkcji odzyskiwania Odzyskiwanie witryny Azure. 

Załóżmy na przykład, skonfigurowano replikacji komputery lokalnego na konto przestrzeni dyskowej (Storage1), a komputer chroniony pojawiły się po przełączeniu Azure jako maszyny wirtualnej (VM1) dołączone do wirtualnej sieci (Network1). Nie można przenosić dowolną z następujących zasobów Azure Storage1, VM1 i Network1 - grup zasobów w obrębie tej samej subskrypcji lub subskrypcji.

## <a name="classic-deployment-limitations"></a>Ograniczenia w klasycznym wdrażania

Opcje dotyczące przenoszenia zasobów wdrażane za pośrednictwem modelu Klasyczny różnią się w zależności od tego, czy chcesz przenieść zasobów w ramach subskrypcji lub do nowej subskrypcji. 

### <a name="same-subscription"></a>Samej subskrypcji

Przenoszenie zasobów z jednej grupy zasobów do innej grupy zasobów w obrębie tej samej subskrypcji, następującym ograniczeniom:

- Nie można przenieść wirtualnych sieci (klasyczny).
- Z usług w chmurze należy przenieść maszyn wirtualnych (klasyczny). 
- Usługa w chmurze można przenieść tylko w przypadku, gdy Przenieś zawiera wszystkie maszyn wirtualnych.
- Usługa w chmurze tylko jeden mogą być przenoszone w danej chwili.
- W danej chwili można przenosić tylko z jednym klientem miejsca do magazynowania (klasyczny).
- Nie można przenieść konta miejsca do magazynowania (klasyczny) w tej samej operacji maszyny wirtualnej lub usługi w chmurze.

Aby przenieść klasyczny zasobów do nowej grupy zasobów w obrębie tej samej subskrypcji, użyj operacji przenoszenia standardowego za pośrednictwem [portalu](#use-portal), [Azure programu PowerShell](#use-powershell), [Polecenie Azure](#use-azure-cli)lub [Interfejsu API usługi REST](#use-rest-api). Użyj operacji używasz na potrzeby przenoszenia zasobów Menedżera zasobów.

### <a name="new-subscription"></a>Nowej subskrypcji

Podczas przenoszenia zasobów do nowej subskrypcji, następującym ograniczeniom:

- W tej samej operacji należy przenieść wszystkie zasoby klasyczny w subskrypcji.
- Subskrypcja docelowej nie może zawierać inne zasoby klasyczny.
- Przenieś mogą być żądane tylko za pośrednictwem osobnych interfejsu API usługi REST dla przenosi klasyczny. Standardowe polecenia Przenieś Menedżera zasobów nie działają podczas przenoszenia klasyczny zasobów do nowej subskrypcji.

Aby przenieść klasyczny zasoby do nowej subskrypcji, należy użyć pozostałych operacji, które są specyficzne dla zasobów klasyczny. Wykonaj następujące czynności, aby przenieść klasyczny zasoby do nowej subskrypcji.

1. Sprawdzanie, jeśli subskrypcji źródła może uczestniczyć w Przenieś krzyżowe subskrypcji. Użyj następujących operacji:

         POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
    
     W treści wezwania obejmują:

         {
           "role": "source"
         }

     Odpowiedzi na operacja sprawdzania poprawności znajduje się w następującym formacie:

         {
           "status": "{status}",
           "reasons": [
             "reason1",
             "reason2"
           ]
         }

2. Sprawdzanie, jeśli subskrypcji elementu docelowego może uczestniczyć w Przenieś krzyżowe subskrypcji. Użyj następujących operacji:

         POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01

     W treści wezwania obejmują:

         {
           "role": "target"
         }

     Odpowiedź znajduje się w formacie poprawności subskrypcji źródła.

3. Jeśli oba subskrypcje przejdzie sprawdzanie poprawności, Przenieś wszystkie zasoby klasyczny z jedną subskrypcję, do innej subskrypcji następujące działania:

         POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01

    W treści wezwania obejmują:

         {
           "target": "/subscriptions/{target-subscription-id}"
         }

Operacja może działać kilka minut. 

## <a name="use-portal"></a>Za pomocą portalu

Aby przenieść zasoby do nowej grupy zasobów w tej **samej subskrypcji**, wybierz grupę zasobów zawierające te zasoby, a następnie wybierz przycisk **Przenieś** .

![Przenoszenie zasobów](./media/resource-group-move-resources/edit-rg-icon.png)

Lub, aby przenieść zasoby do **nowej subskrypcji**, wybierz grupę zasobów zawierające te zasoby, a następnie wybierz ikonę subskrypcji Edytuj.

![Przenoszenie zasobów](./media/resource-group-move-resources/change-subscription.png)

Wybierz zasoby, aby przenieść i grupa zasobów miejsca docelowego. Potwierdź, że musisz zaktualizować skrypty do tych zasobów i wybierz **przycisk OK**. Jeśli zaznaczono ikonę subskrypcji edycji w poprzednim kroku, musisz również zaznaczyć subskrypcji elementu docelowego.

![Wybieranie miejsca docelowego](./media/resource-group-move-resources/select-destination.png)

W **powiadomienia**można zobaczyć, czy jest uruchomiony operacji przenoszenia.

![Pokazywanie stanu przenoszenia](./media/resource-group-move-resources/show-status.png)

Po zakończeniu, użytkownik jest powiadamiany o wynik.

![Wyświetlanie wyników przenoszenia](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>Za pomocą programu PowerShell

Aby przenieść istniejących zasobów do innej grupy zasobów lub subskrypcji, użyj polecenia **Przenieś AzureRmResource** .

W pierwszym przykładzie pokazano sposób przenoszenia jeden zasób do nowej grupy zasobów.

    $resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId

W drugim przykładzie pokazano sposób przenoszenia wielu zasobów do nowej grupy zasobów.

    $webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
    $plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId

Aby przejść do nowej subskrypcji, Uwzględnij wartości parametru **DestinationSubscriptionId** .

Zostanie wyświetlony monit o potwierdzenie, że chcesz przenieść określonych zasobów.

    Confirm
    Are you sure you want to move these resources to the resource group
    '/subscriptions/{guid}/resourceGroups/newRG' the resources:

    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

## <a name="use-azure-cli"></a>Polecenie Azure za pomocą

Aby przenieść istniejących zasobów do innej grupy zasobów lub subskrypcji, użyj polecenia **Przenieś azure zasobów** . Należy podać identyfikatory zasobów zasobów, aby przenieść. Można uzyskać identyfikatory zasobów przy użyciu następującego polecenia:

    azure resource list -g sourceGroup --json

Która zwraca następujący format:

    [
      {
        "id": "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo",
        "name": "storagedemo",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "southcentralus",
        "tags": {},
        "kind": "Storage",
        "sku": {
          "name": "Standard_RAGRS",
          "tier": "Standard"
        }
      }
    ]

W poniższym przykładzie pokazano, jak przenieść konto miejsca do magazynowania do nowej grupy zasobów. W parametrze **-i** Opis Podaj firmy rozdzielaną średnikami listę identyfikator zasobu, aby przenieść.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
    
Zostanie wyświetlony monit o potwierdzenie przenoszenie określonego zasobu.

## <a name="use-rest-api"></a>Za pomocą interfejsu API usługi REST

Aby przenieść istniejących zasobów do innej grupy zasobów lub subskrypcji, uruchom polecenie:

    POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version} 

W treści wezwania Określ docelową grupę zasobów i zasoby, aby przenieść. Aby uzyskać więcej informacji o operacji pozostałych przenoszenia zobacz [Przenoszenie zasobów](https://msdn.microsoft.com/library/azure/mt218710.aspx).


## <a name="next-steps"></a>Następne kroki
- Aby uzyskać informacje dotyczące poleceń cmdlet środowiska PowerShell do zarządzania subskrypcji, zobacz [Używanie Azure przy użyciu Menedżera zasobów](powershell-azure-resource-manager.md).
- Aby dowiedzieć się o polecenie Azure poleceń związanych z zarządzaniem subskrypcji, zobacz [Używanie polecenie Azure przy użyciu Menedżera zasobów](xplat-cli-azure-resource-manager.md).
- Aby uzyskać informacje o portalu funkcje związane z zarządzaniem subskrypcji, zobacz [za pomocą portalu Azure do zarządzania zasobami](./azure-portal/resource-group-portal.md).
- Aby dowiedzieć się, stosując logiczną organizację do zasobów, zobacz [Używanie znaczników w celu organizowania zasobów](resource-group-using-tags.md).
