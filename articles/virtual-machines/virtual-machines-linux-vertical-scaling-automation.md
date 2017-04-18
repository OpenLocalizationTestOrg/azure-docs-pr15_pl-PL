<properties
    pageTitle="Pionowo skalowanie Azure maszyn wirtualnych przy użyciu automatyzacji Azure | Microsoft Azure"
    description="Jak pionowo przeskalować maszyny wirtualnej Linux w odpowiedzi na monitorowanie alertów przy użyciu automatyzacji Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/29/2016"
    ms.author="singhkay"/>

# <a name="vertically-scale-azure-virtual-machine-with-azure-automation"></a>Pionowo skalowanie Azure maszyn wirtualnych przy użyciu automatyzacji Azure

Skalowanie pionowe to proces zwiększania lub zmniejszania zasobów komputera w odpowiedzi na obciążenie pracą. Platformy Azure można to zrobić, zmieniając rozmiar maszyny wirtualnej. To może pomóc w następujących scenariuszach

- Jeśli nie jest często używany maszyny wirtualnej, możesz zmienić rozmiar go do mniejszy rozmiar, aby zmniejszyć koszty miesięczny
- Jeśli maszyna wirtualna jest wyświetlany ładowania Alokacja maksymalna, można oznaczającej większy rozmiar w celu zwiększenia jego pojemności

Konspekt z instrukcjami, aby osiągnąć ten cel jest jako poniżej

1. Konfigurowanie automatyzacji Azure, aby uzyskać dostęp do maszyn wirtualnych
2. Importowanie runbooks skali pionowej automatyzacji Azure do subskrypcji
3. Dodawanie webhook do swojego działań aranżacji
4. Dodawanie alertu na komputerze wirtualnych

> [AZURE.NOTE] Ze względu na rozmiar to pierwsza maszyna wirtualna rozmiarów, który może być skalowany, może być ograniczone z powodu dostępności innych rozmiarów w klastrze bieżącej maszyny wirtualnej zostanie wdrożony w. W runbooks opublikowanych automatyzacji używanych w tym artykule możemy zajmować sprawy i skalowanie tylko w poniżej pary rozmiar maszyn wirtualnych. Oznacza to, że maszyny wirtualnej Standard_D1v2 będzie nie nieoczekiwanie skalowania w górę do Standard_G5 lub aby Basic_A0.

>| Skalowanie pary rozmiarów maszyn wirtualnych |   |
|---|---|
|  Basic_A0 |  Basic_A4 |
|  Standard_A0 | Standard_A4 |
|  Standard_A5 | Standard_A7  |
|  Standard_A8 | Standard_A9  |
|  Standard_A10 |  Standard_A11 |
|  Standard_D1 |  Standard_D4 |
|  Standard_D11 | Standard_D14  |
|  Standard_DS1 |  Standard_DS4 |
|  Standard_DS11 | Standard_DS14  |
|  Standard_D1v2 |  Standard_D5v2 |
|  Standard_D11v2 |  Standard_D14v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Konfigurowanie automatyzacji Azure, aby uzyskać dostęp do maszyn wirtualnych

Najpierw należy wykonać jest utworzyć konto Azure automatyzacji, którym będzie przechowywana runbooks używana do skalowania wystąpienia Ustawianie skali maszyn wirtualnych. Ostatnio Usługa automatyzacji wprowadzić funkcję "Uruchom jako konta", co sprawia, że ustawienie w górę wystawcy usługi automatycznie uruchamiania runbooks w imieniu użytkownika bardzo proste. Więcej informacji o to w artykule poniżej:

* [Typ poświadczeń uwierzytelniania Runbooks konta Azure Uruchom jako](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Importowanie runbooks skali pionowej automatyzacji Azure do subskrypcji

Runbooks, niepotrzebne pionowo skalowania komputera wirtualnych zostały już opublikowane w galerii działań aranżacji automatyzacji Azure. Należy je zaimportować do subskrypcji. Możesz dowiedzieć się, jak zaimportować runbooks, czytając następujący artykuł.

* [Galerie działań aranżacji i moduł automatyzacji Azure](../automation/automation-runbook-gallery.md)

Na poniższej ilustracji przedstawiono runbooks, które muszą zostać zaimportowane

![Importowanie runbooks](./media/virtual-machines-vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Dodawanie webhook do swojego działań aranżacji

Po zaimportowaniu możesz runbooks, który musisz dodać webhook do działań aranżacji, może być uruchomiona przez alertu z maszyny wirtualnej. W tym miejscu można odczytywać szczegóły tworzenia webhook dla swojego działań aranżacji

* [Azure webhooks automatyzacji](../automation/automation-webhooks.md)

Upewnij się, że możesz skopiować webhook przed zamknięciem okna dialogowego webhook, jak będzie to w następnej sekcji.

## <a name="add-an-alert-to-your-virtual-machine"></a>Dodawanie alertu na komputerze wirtualnych

1. Wybierz pozycję Ustawienia maszyn wirtualnych
2. Wybierz pozycję "Reguły alertu"
3. Wybierz pozycję "Dodaj alert"
4. Wybierz pozycję metryczne uruchomienie alertu na
5. Wybierz warunek służący spełnione będzie powodować alert uruchomienie
6. Wybierz pozycję progu warunku w kroku 5. mają być spełnione
7. Wybierz okres pozwalającego sprawdzi Usługa monitorowania warunek i próg w kroki 5 i 6
8. Wklej webhook, który został skopiowany z poprzedniej sekcji.

![Dodawanie alertu dotyczącego maszyn wirtualnych 1](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-1.png)

![Dodawanie alertu dotyczącego maszyn wirtualnych 2](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-2.png)