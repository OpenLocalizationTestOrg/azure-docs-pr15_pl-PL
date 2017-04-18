<properties
   pageTitle="Rozwiązywanie problemów z wdrażania maszyn wirtualnych Linux-Menedżera zasobów | Microsoft Azure"
   description="Rozwiązywanie problemów wdrożenia Menedżera zasobów w przypadku tworzenia nowej maszyny wirtualnej Linux platformy Azure"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue, azure-resource-manager"/>

<tags
  ms.service="virtual-machines-linux"
  ms.workload="na"
  ms.tgt_pltfrm="vm-linux"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/09/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Rozwiązywanie problemów wdrożenia Menedżera zasobów do tworzenia nowych maszyny wirtualnej Linux platformy Azure

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Dzienniki inspekcji zbierania

Aby rozpocząć, rozwiązywanie problemów, zbieranie dzienników inspekcji do identyfikowania błąd związany z problem. Poniższe łącza zawiera szczegółowe informacje dotyczące procesu do obserwowania.

[Rozwiązywanie problemów z wdrożeń grupa zasobów z Azure Portal](../resource-manager-troubleshoot-deployments-portal.md)

[Inspekcja operacji przy użyciu Menedżera zasobów](../resource-group-audit.md)

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** System operacyjny jest Linux uogólniony i jest przekazane lub przechwycone za pomocą ustawień ogólnych, następnie nie można wszystkie błędy. Podobnie jeśli jest system operacyjny Linux specjalne, jest przekazane lub przechwycone z ustawieniem specjalistyczne, a następnie będzie jakiekolwiek błędy.

**Przekazywanie błędów:**

**N<sup>1</sup>:** Jeśli system operacyjny jest Linux uogólniony i przekazaniu jako specjalistyczne, zostanie wyświetlony obsługi administracyjnej błąd przekroczenia limitu czasu ponieważ maszyn wirtualnych zatrzymuje się na etapie inicjowania obsługi administracyjnej.

**N<sup>2</sup>:** W przypadku specjalistyczne Linux i przekazaniem jako uogólniony system operacyjny zostanie wyświetlony błąd błąd obsługi administracyjnej ponieważ nowa maszyna wirtualna działa z oryginalną nazwę komputera, nazwy użytkownika i hasła.

**Rozwiązanie:**

Aby rozwiązać obu tych błędów, Przekaż oryginalny wirtualny dysk twardy, dostępne lokalna, jak to samo ustawienie dla OS (uogólniony specjalistycznego). Aby przekazać uogólniony, pamiętaj, aby uruchomić - deprovision najpierw.

**Rejestrowanie błędów:**

**N<sup>3</sup>:** Jeśli system operacyjny jest Linux uogólniony i jest rejestrowany jako specjalistyczne, zostanie wyświetlony obsługi administracyjnej błąd przekroczenia limitu czasu ponieważ oryginalny maszyn wirtualnych nie jest używany jako zostanie oznaczona jako uogólniony.

**N<sup>4</sup>:** W przypadku specjalistyczne Linux i jest rejestrowany, jak uogólniony system operacyjny zostanie wyświetlony błąd błąd obsługi administracyjnej ponieważ nowa maszyna wirtualna działa z oryginalną nazwę komputera, nazwy użytkownika i hasła. Ponadto oryginalna maszyn wirtualnych nie jest używany, ponieważ jest oznaczona jako specjalnych.

**Rozwiązanie:**

Aby rozwiązać obu tych błędów, usuwania bieżącego obrazu z portalu i [ponownie go z bieżącym wirtualnych dysków twardych przechwycić](virtual-machines-linux-capture-image.md) takie same ustawienia, jak dla systemu operacyjnego (uogólniony specjalistycznego).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Problem: Niestandardowe i Galeria-marketplace obrazu; Błąd alokacji
Ten błąd pojawia się w sytuacji, gdy nowe żądanie maszyn wirtualnych zostanie przypięta do klastrów, która nie obsługuje rozmiar pamięci Wirtualnej żądanego lub nie ma wolnego miejsca, aby zezwalały na żądanie.

**Powodować 1:** Klaster nie obsługuje żądany rozmiar pamięci Wirtualnej.

**Rozwiązanie 1:**

- Ponawianie żądania przy użyciu mniejszy rozmiar pamięci Wirtualnej.
- Jeśli nie można zmienić rozmiar wymagane maszyn wirtualnych:
  - Zatrzymanie wszystkich maszyny wirtualne w zestawie dostępności.
  Kliknij pozycję **grupy zasobów** > *grupy zasobów* > **zasobów** > *Ustawianie swojej dostępności* > **maszyn wirtualnych** > *komputera wirtualnych* > **zatrzymać**.
  - Po zatrzymać maszyny wirtualne, Utwórz nowy maszyn wirtualnych w odpowiednim rozmiarze.
  - Rozpoczynanie nowych maszyn wirtualnych najpierw i zaznacz wszystkie przestał maszyny wirtualne, a następnie kliknij przycisk **Start**.

**Powodować 2:** Klaster nie ma zwolnić zasoby.

**Rozdzielczość 2:**

- Ponawianie żądania w późniejszym czasie.
- Jeśli nowa maszyna wirtualna może być częścią zestawu różnych dostępności
  - Tworzenie nowych maszyn wirtualnych w różnych dostępność, ustawić (w tym samym regionie).
  - Dodawanie nowych maszyn wirtualnych do tej samej sieci wirtualnej.

## <a name="next-steps"></a>Następne kroki
Jeśli wystąpią problemy podczas uruchamiania przestał maszyny Linux lub zmienić istniejące maszyn wirtualnych Linux platformy Azure, zobacz [problemy wdrożenia Rozwiązywanie problemów z Menedżera zasobów z ponownie lub zmienianie rozmiaru istniejące maszyny wirtualnej Linux platformy Azure](virtual-machines-linux-restart-resize-error-troubleshooting.md).
