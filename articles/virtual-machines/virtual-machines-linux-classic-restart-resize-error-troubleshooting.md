<properties
   pageTitle="Maszyn wirtualnych ponownie lub zmienianie rozmiaru problemów | Microsoft Azure"
   description="Rozwiązywanie problemów klasyczny wdrażania z ponownie lub zmienianie rozmiaru istniejące maszyny wirtualnej Linux platformy Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="required"
   ms.date="09/20/2016"
   ms.devlang="na"
   ms.author="delhan"/>

# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Rozwiązywanie problemów klasyczny wdrażania z ponownie lub zmienianie rozmiaru istniejące maszyny wirtualnej Linux platformy Azure

> [AZURE.SELECTOR]
- [Klasyczny](../articles/virtual-machines/virtual-machines-linux-classic-restart-resize-error-troubleshooting.md)
- [Menedżer zasobów](../articles/virtual-machines/virtual-machines-linux-restart-resize-error-troubleshooting.md)

Podczas próby Uruchom program przestał Azure maszyn wirtualnych (maszyn wirtualnych) lub zmienianie rozmiaru istniejących maszyn wirtualnych Azure typowych błędów, które wystąpić jest błąd alokacji. Ten błąd wyników, gdy klaster lub region nie ma dostępnych zasobów lub nie obsługuje żądany rozmiar pamięci Wirtualnej.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Dzienniki inspekcji zbierania

Aby rozpocząć, rozwiązywanie problemów, zbieranie dzienników inspekcji do identyfikowania błąd związany z problem.

W portalu Azure, kliknij przycisk **Przeglądaj** > **maszyn wirtualnych** > _komputera wirtualnych Linux_ > **Ustawienia** > **dzienników inspekcji**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problem: Błąd podczas uruchamiania przestał maszyn wirtualnych

Spróbuj uruchomić program przestał maszyn wirtualnych, ale awaria alokacji.

### <a name="cause"></a>Przyczyna

Żądania zatrzymania maszyn wirtualnych musi podjąć próbę w klastrze oryginalnego, który obsługuje usługa w chmurze. Jednak klaster nie ma wolnego miejsca dostępnego w celu spełnienia żądania.

### <a name="resolution"></a>Rozdzielczość

* Tworzenie nowej usługi w chmurze i skojarzyć z albo regionie lub wirtualnej sieci oparte na region, ale nie grupy koligacji.

* Usuwanie przestał maszyn wirtualnych.

* Ponownie utwórz maszyn wirtualnych w nowej usługi w chmurze przy użyciu dysków.

* Uruchom ponownie utworzony maszyn wirtualnych.

Jeśli zostanie wyświetlony komunikat o błędzie podczas próby utworzenia nowego usługi w chmurze, spróbuj ponownie później lub zmienianie regionu dla usług w chmurze.

> [AZURE.IMPORTANT] Nowej usługi w chmurze będzie miała pod nową nazwą i VIP, dlatego należy zmienić te informacje dla wszystkich zależności korzystać z tych informacji do istniejącej usługi w chmurze.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problem: Błąd podczas zmieniania rozmiaru istniejących maszyn wirtualnych

Podczas próby zmiany rozmiaru istniejących maszyn wirtualnych, ale awaria alokacji.

### <a name="cause"></a>Przyczyna

Żądanie tak, aby zmienić rozmiar maszyn wirtualnych musi podjąć próbę w klastrze oryginalnego, który obsługuje usługa w chmurze. Jednak klaster nie obsługuje żądany rozmiar pamięci Wirtualnej.

### <a name="resolution"></a>Rozdzielczość

Zmniejsz żądany rozmiar pamięci Wirtualnej, a następnie ponów żądanie zmiany rozmiaru.

* Kliknij przycisk **Przeglądaj wszystkie** > **maszyn wirtualnych (klasyczny)** > _komputera wirtualnych_ > **Ustawienia** > **rozmiar**. Aby uzyskać szczegółowe instrukcje zobacz [Zmienianie rozmiaru maszyny wirtualnej](https://msdn.microsoft.com/library/dn168976.aspx).

Jeśli nie jest możliwe zmniejszyć rozmiar pamięci Wirtualnej, wykonaj następujące czynności:

  * Tworzenie nowej usługi w chmurze, zapewnianie jest nie jest połączony z grupą koligacji i nie zostanie skojarzone z wirtualnych sieci, w której jest połączony z grupą koligacji.

  * Tworzenie nowego, o większym rozmiarze maszyn wirtualnych w nim.

Można skonsolidować wszystkich VMs w tym samym usługi w chmurze. Jeśli istniejące usługi w chmurze jest skojarzony z wirtualnej sieci oparte na region, można nawiązać nowej usługi w chmurze istniejących wirtualnej sieci.

Jeśli istniejące usługi w chmurze nie jest skojarzone z wirtualnej sieci oparte na region, następnie należy usunąć maszyny wirtualne w istniejącej usługi w chmurze i utwórz je ponownie w nowej usługi w chmurze z ich dysków. Jednak jest należy pamiętać o nowej usługi w chmurze że pod nową nazwą i VIP, więc należy zaktualizować ich, aby wszystkie zależności, które obecnie za pomocą tych informacji do istniejącej usługi w chmurze.

## <a name="next-steps"></a>Następne kroki

Jeśli występują problemy podczas tworzenia nowego maszyny Linux w Azure, zobacz [Rozwiązywanie problemów wdrożenia do tworzenia nowych maszyny wirtualnej Linux Azure](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
