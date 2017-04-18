<properties
    pageTitle="Pionowo skalowanie zestawy skali Azure maszyn wirtualnych | Microsoft Azure"
    description="Jak pionowo przeskalować maszyny wirtualnej w odpowiedzi na monitorowanie alertów przy użyciu automatyzacji Azure"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="madhana"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="guybo"/>

# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Ustawia pionowe autoscale ze skalą maszyn wirtualnych

Ten artykuł zawiera opis sposobu pionowo skalowania Azure [Zestawy skali maszyn wirtualnych](https://azure.microsoft.com/services/virtual-machine-scale-sets/) z lub bez reprovisioning. Skalowanie pionowe maszyny wirtualne, które nie są w zestawach skali, można znaleźć w [pionie skalowanie Azure maszyn wirtualnych przy użyciu automatyzacji Azure](../virtual-machines/virtual-machines-windows-vertical-scaling-automation.md).

Skalować w pionie, nazywane także _rozbudowy_ i _skali_, oznacza, że zwiększenie lub zmniejszenie rozmiarów maszyn wirtualnych (maszyn wirtualnych) w odpowiedzi na obciążenie pracą. Porównaj to z [Skalowanie w poziomie](./virtual-machine-scale-sets-autoscale-overview.md), nazywane też _skali się_ i _skali w_miejsce, w którym numer maszyny wirtualne został zmieniony w zależności od obciążenie pracą.

Reprovisioning oznacza, że usunięcie istniejącej maszyn wirtualnych i zamienić go na nową. Gdy zwiększyć lub zmniejszyć rozmiar maszyny wirtualne w zestawie skali maszyn wirtualnych, w niektórych przypadkach chcesz zmienić istniejące maszyny wirtualne i zachować dane, podczas gdy w innych przypadkach należy wdrożyć nowe maszyny wirtualne nowego rozmiaru. W tym dokumencie opisano obu przypadkach.

Skalowanie pionowe mogą być przydatne podczas:

- Usługa oparty na maszyn wirtualnych jest wykorzystywane w obszarze (na przykład w weekendy). Zmniejszanie rozmiaru maszyn wirtualnych można zmniejszyć koszty miesięczny.
- Zwiększanie rozmiaru maszyn wirtualnych radzenia sobie z większym żądanie bez tworzenia dodatkowych maszyny wirtualne.

Możesz skonfigurować skalowanie pionowe być wyzwalane na podstawie metryczne alerty na podstawie z zestawu maszyn wirtualnych Skala. Po aktywowaniu alert go uruchamiana webhook wyzwalaczy tego zestawu działań aranżacji, które można skalować usługi skali w górę lub w dół. Skalowanie pionowe można skonfigurować, wykonując następujące czynności:

1. Utwórz konto Azure automatyzacji z możliwością Uruchom jako.
2. Importowanie runbooks skali pionowej automatyzacji Azure dla zestawów skali maszyn wirtualnych do subskrypcji.
3. Dodawanie webhook do swojego działań aranżacji.
4. Dodawanie alertu do maszyn wirtualnych skali zestawu za pomocą powiadomień webhook.

> [AZURE.NOTE] Pionowa autoscaling może nastąpić w określonym zakresie rozmiarów maszyn wirtualnych. Porównanie specyfikacji każdego rozmiaru przed podjęciem decyzji o skalowanie od jednego do innego (większa niż liczba zawsze oznacza większy rozmiar pamięci Wirtualnej). Istnieje możliwość skalowania między następujące pary rozmiarów:

>| Skalowanie pary rozmiarów maszyn wirtualnych |   |
|---|---|
|  Standard_A0 | Standard_A11 |
|  Standard_D1 |  Standard_D14 |
|  Standard_DS1 |  Standard_DS14 |
|  Standard_D1v2 |  Standard_D15v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Utwórz konto Azure automatyzacji z możliwością Uruchom jako

Najpierw należy wykonać jest utworzyć konto Azure automatyzacji, którym będzie przechowywana runbooks używana do skalowania wystąpienia Ustawianie skali maszyn wirtualnych. Ostatnio [Automatyzacji Azure](https://azure.microsoft.com/services/automation/) wprowadzić funkcję "Uruchom jako konta", co sprawia, że ustawienie w górę wystawcy usługi automatycznie uruchamiania runbooks w imieniu użytkownika bardzo proste. Więcej informacji o to w artykule poniżej:

* [Typ poświadczeń uwierzytelniania Runbooks konta Azure Uruchom jako](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Importowanie runbooks skali pionowej automatyzacji Azure do subskrypcji

Runbooks potrzebne do pionowo skalowanie do zestawów skali maszyn wirtualnych zostały już opublikowane w galerii działań aranżacji automatyzacji Azure. Aby zaimportować je do subskrypcji wykonaj kroki opisane w tym artykule:

* [Galerie działań aranżacji i moduł automatyzacji Azure](../automation/automation-runbook-gallery.md)

Wybierz opcję Galerii Przeglądaj z Runbooks menu:

![Runbooks do zaimportowania][runbooks]

Wyświetlane są runbooks, które muszą zostać zaimportowane. Wybierz działań aranżacji, w zależności od tego, czy ma pionowe skalowania z lub bez reprovisioning:

![Galeria Runbooks][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Dodawanie webhook do swojego działań aranżacji

Po zaimportowaniu możesz runbooks, który musisz dodać webhook do działań aranżacji, może być uruchomiona przez alertu z zestawu skali maszyn wirtualnych. W tym artykule opisano szczegóły tworzenia webhook dla swojego działań aranżacji:

* [Azure webhooks automatyzacji](../automation/automation-webhooks.md)

> [AZURE.NOTE] Upewnij się, że możesz skopiować webhook identyfikatora URI przed zamknięciem okna dialogowego webhook, jak będzie to w następnej sekcji.

## <a name="add-an-alert-to-your-vm-scale-set"></a>Dodać alert, aby ustawić skali maszyn wirtualnych

Poniżej znajduje się skrypt programu PowerShell, w którym pokazano, jak dodać alert, aby ustawić skali maszyn wirtualnych. Zapoznaj się z artykułem następujące nazwy metryki uruchomienie alertu na: [Monitorowanie Azure autoscaling typowych metryki](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [AZURE.NOTE] Zalecane jest konfigurowanie okna czasu alertu, aby uniknąć powodujące skalowanie pionowe i wszystkie skojarzone przerwa w świadczeniu usługi, zbyt często. Należy rozważyć, czy okno co najmniej 20 – 30 minut lub więcej. Należy rozważyć, czy pozioma, skalowania, jeśli potrzeba uniknięcia zakłóceń.

Aby uzyskać więcej informacji na temat tworzenia alertów można znaleźć w następujących artykułach:

* [Azure próbki szybki start dla programu PowerShell monitora](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Azure próbki szybki start polecenie między platformami monitora](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano proste przykłady skalowania pionowego. Przy użyciu tych bloków konstrukcyjnych — konta automatyzacji, runbooks, webhooks, alerty — możesz połączyć szeroki zakres zdarzeń z zestawu niestandardowych akcji.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
