<properties
   pageTitle="Maszyn wirtualnych ponownie lub zmienianie rozmiaru problemów | Microsoft Azure"
   description="Rozwiązywanie problemów wdrożenia Menedżera zasobów z ponownie lub zmienianie rozmiaru istniejące maszyny wirtualnej systemu Windows Azure"
   services="virtual-machines-windows, azure-resource-manager"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-windows"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-windows"
   ms.devlang="na"
   ms.workload="required"
   ms.date="09/09/2016"
   ms.author="delhan"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a>Rozwiązywanie problemów wdrożenia Menedżera zasobów z ponownie lub zmienianie rozmiaru istniejące maszyny wirtualnej systemu Windows Azure

Podczas próby Uruchom program przestał Azure maszyn wirtualnych (maszyn wirtualnych) lub zmienianie rozmiaru istniejących maszyn wirtualnych Azure typowych błędów, które wystąpić jest błąd alokacji. Ten błąd wyników, gdy klaster lub region nie ma dostępnych zasobów lub nie obsługuje żądany rozmiar pamięci Wirtualnej.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Dzienniki inspekcji zbierania

Aby rozpocząć, rozwiązywanie problemów, zbieranie dzienników inspekcji do identyfikowania błąd związany z problem. Poniższe łącza zawiera szczegółowe informacje dotyczące procesu:

[Rozwiązywanie problemów z wdrożeń grupa zasobów z Azure Portal](../resource-manager-troubleshoot-deployments-portal.md)

[Inspekcja operacji przy użyciu Menedżera zasobów](../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problem: Błąd podczas uruchamiania przestał maszyn wirtualnych

Spróbuj uruchomić program przestał maszyn wirtualnych, ale awaria alokacji.

### <a name="cause"></a>Przyczyna

Żądania zatrzymania maszyn wirtualnych musi podjąć próbę w klastrze oryginalnego, który obsługuje usługa w chmurze. Jednak klaster nie ma wolnego miejsca dostępnego w celu spełnienia żądania.

### <a name="resolution"></a>Rozdzielczość

*   Zatrzymaj wszystkie maszyny wirtualne w zestawie dostępność i ponownie uruchom poszczególnych maszyn wirtualnych.

  1. Kliknij pozycję **grupy zasobów** > _grupy zasobów_ > **zasobów** > _Ustawianie swojej dostępności_ > **maszyn wirtualnych** > _komputera wirtualnych_ > **zatrzymać**.

  2. Po zatrzymać maszyny wirtualne, zaznacz wszystkie przestał maszyny wirtualne, a następnie kliknij przycisk Start.

*   Ponawianie żądania ponownego uruchomienia w późniejszym czasie.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problem: Błąd podczas zmieniania rozmiaru istniejących maszyn wirtualnych

Podczas próby zmiany rozmiaru istniejących maszyn wirtualnych, ale awaria alokacji.

### <a name="cause"></a>Przyczyna

Żądanie tak, aby zmienić rozmiar maszyn wirtualnych musi podjąć próbę w klastrze oryginalnego, który obsługuje usługa w chmurze. Jednak klaster nie obsługuje żądany rozmiar pamięci Wirtualnej.

### <a name="resolution"></a>Rozdzielczość

* Ponawianie żądania przy użyciu mniejszy rozmiar pamięci Wirtualnej.

* Jeśli nie można zmienić rozmiar wymagane maszyn wirtualnych:

  1. Zatrzymanie wszystkich maszyny wirtualne w zestawie dostępności.

    * Kliknij pozycję **grupy zasobów** > _grupy zasobów_ > **zasobów** > _Ustawianie swojej dostępności_ > **maszyn wirtualnych** > _komputera wirtualnych_ > **zatrzymać**.

  2. Po zatrzymać maszyny wirtualne, Zmień rozmiar odpowiedniej maszyn wirtualnych o większym rozmiarze.
  3. Wybierz pozycję po zmianie rozmiaru maszyn wirtualnych i kliknij przycisk **Start**, a następnie zacznij wszystkich przestał maszyny wirtualne.

## <a name="next-steps"></a>Następne kroki

Jeśli występują problemy podczas tworzenia nowych maszyn wirtualnych systemu Windows w Azure, zobacz [Rozwiązywanie problemów wdrożenia do tworzenia nowych maszyny wirtualnej systemu Windows Azure](../virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md).
