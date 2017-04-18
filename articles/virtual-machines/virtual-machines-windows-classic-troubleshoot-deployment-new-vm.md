<properties
   pageTitle="Rozwiązywanie problemów z maszyn wirtualnych systemu Windows wdrożenia — klasyczny | Microsoft Azure"
   description="Rozwiązywanie problemów klasyczny wdrożenia utworzenie nowa maszyna wirtualna systemu Windows Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
  ms.service="virtual-machines-windows"
  ms.workload="na"
  ms.tgt_pltfrm="vm-windows"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/06/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Rozwiązywanie problemów klasyczny wdrożenia do tworzenia nowej maszyny wirtualnej Windows platformy Azure

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-selectors-include.md)]

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Dzienniki inspekcji zbierania

Aby rozpocząć, rozwiązywanie problemów, zbieranie dzienników inspekcji do identyfikowania błąd związany z problem.

W portalu Azure, kliknij przycisk **Przeglądaj** > **maszyn wirtualnych** > *komputera wirtualnych* > **Ustawienia** > **dzienników inspekcji**.

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** Jeśli jest system operacyjny Windows uogólniony i jest przekazane lub przechwycone za pomocą ustawień ogólnych, następnie nie można wszystkie błędy. Podobnie w przypadku systemu operacyjnego Windows specjalistyczne, jest przekazane lub przechwycone z ustawieniem specjalistyczne, a następnie będzie jakiekolwiek błędy.

**Przekazywanie błędów:**

**N<sup>1</sup>:** Jeśli system operacyjny Windows uogólniony, a przekazaniem jako specjalistyczne, zostanie wyświetlony obsługi administracyjnej błąd przekroczenia limitu czasu przy użyciu maszyn wirtualnych, które utknęły na ekranie pierwszego uruchomienia.

**N<sup>2</sup>:** W przypadku systemu operacyjnego Windows specjalistyczne i przekazaniem jako uogólniony, zostanie wyświetlony błąd błąd obsługi administracyjnej z maszyn wirtualnych, które utknęły na ekranie pierwszego uruchomienia, ponieważ nowa maszyna wirtualna działa z oryginalną nazwę komputera, nazwy użytkownika i hasła.

**Rozwiązanie:**

Aby rozwiązać obu tych błędów, Przekaż oryginalny wirtualny dysk twardy, dostępne lokalna, jak to samo ustawienie dla OS (uogólniony specjalistyczne). Aby przekazać uogólniony, pamiętaj, aby uruchomić narzędzie sysprep najpierw. Aby uzyskać więcej informacji, zobacz [Tworzenie i przekazywanie Windows serwera wirtualnego dysku twardego Azure](virtual-machines-windows-classic-createupload-vhd.md) .

**Rejestrowanie błędów:**

**N<sup>3</sup>:** Jeśli system operacyjny Windows uogólniony, a jest rejestrowany jako specjalistyczne, zostanie wyświetlony obsługi administracyjnej błąd przekroczenia limitu czasu ponieważ oryginalny maszyn wirtualnych nie jest używany jako zostanie oznaczona jako uogólniony.

**N<sup>4</sup>:** W przypadku systemu operacyjnego Windows specjalistyczne i jest rejestrowany, jak uogólniony, zostanie wyświetlony błąd błąd obsługi administracyjnej ponieważ nowa maszyna wirtualna działa z oryginalną nazwę komputera, nazwy użytkownika i hasła. Ponadto oryginalna maszyn wirtualnych nie jest używany, ponieważ jest oznaczona jako specjalnych.

**Rozwiązanie:**

Aby rozwiązać obu tych błędów, usuwania bieżącego obrazu z portalu i [ponownie go z bieżącym wirtualnych dysków twardych przechwycić](virtual-machines-windows-classic-capture-image.md) takie same ustawienia, jak dla systemu operacyjnego (uogólniony specjalistycznego).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Problem: Niestandardowe i Galeria-marketplace obrazu; Błąd alokacji
Ten błąd pojawia się w sytuacjach, gdy nowe żądanie maszyn wirtualnych jest wysyłana do klastrów, która nie ma wolnego miejsca, aby zezwalały na żądanie lub nie obsługuje rozmiar pamięci Wirtualnej żądanego. Nie jest możliwe mieszać różnych serii maszyny wirtualne w tym samym usługi w chmurze. Tak, jeśli chcesz utworzyć nowy maszyn wirtualnych o innym rozmiarze niż co może obsługiwać usługi w chmurze, żądanie obliczeń zakończy się niepowodzeniem.

W zależności od ograniczeń usługi w chmurze, który będzie używany do tworzenia nowych maszyn wirtualnych może wystąpić błąd spowodowane jedną z dwóch sytuacji.

**Powodować 1:** Usługa w chmurze zostanie przypięta do określonych klaster lub jest połączony z grupą koligacji i w związku z tym przypięta do określonych klaster zgodnie z projektem. Dlatego nowego zasobu obliczeń żądania w tej grupie koligacji są używane w tym samym klastrze miejsce, w którym znajdują się istniejących zasobów. Jednak tym samym klastrze mogą nie obsługuje żądany rozmiar pamięci Wirtualnej lub masz za mało dostępnego miejsca, uzyskując błąd alokacji. Jest to możliwe, czy nowe zasoby są tworzone za pośrednictwem usługi w chmurze nowego lub istniejącego usługi w chmurze.

**Rozwiązanie 1:**

- Tworzenie nowej usługi w chmurze i skojarzyć go z obszaru lub regionu wirtualnej sieci.
- Tworzenie nowych maszyn wirtualnych w nowej usługi w chmurze.
  Jeśli zostanie wyświetlony komunikat o błędzie podczas próby utworzenia nowego usługi w chmurze, spróbuj ponownie później lub zmienianie regionu dla usług w chmurze.

> [AZURE.IMPORTANT] Jeśli zostały próby utworzenia nowego maszyn wirtualnych w istniejącej usługi w chmurze, ale nie i utworzyć nowej usługi w chmurze dla Twojej nowej maszyn wirtualnych można konsolidować wszystkie VMs w tym samym usługi w chmurze. Aby to zrobić, Usuń maszyny wirtualne w istniejącej usługi w chmurze, a następnie ponownie przechwycić je z ich dysków w nowej usługi w chmurze. Jednak jest należy pamiętać o nowej usługi w chmurze że pod nową nazwą i VIP, więc należy zaktualizować ich, aby wszystkie zależności, które obecnie za pomocą tych informacji do istniejącej usługi w chmurze.

**Powodować 2:** Usługa w chmurze jest skojarzony z wirtualnych sieci, w której jest połączony z grupą koligacji tak zostanie przypięta do określonych klastrów, zgodnie z projektem. Żądania wszystkich nowych zasobów obliczeń w tej grupie koligacji w związku z tym są używane w tym samym klastrze miejsce, w którym znajdują się istniejących zasobów. Jednak tym samym klastrze mogą nie obsługuje żądany rozmiar pamięci Wirtualnej lub masz za mało dostępnego miejsca, uzyskując błąd alokacji. Jest to możliwe, czy nowe zasoby są tworzone za pośrednictwem usługi w chmurze nowego lub istniejącego usługi w chmurze.

**Rozdzielczość 2:**

- Tworzenie nowego regionalne wirtualnej sieci.
- Tworzenie nowych maszyn wirtualnych w nowej wirtualnej sieci.
- [Łączenie istniejącej sieci wirtualnej](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) nowe wirtualnej sieci. Zobacz więcej informacji na temat [regionalne wirtualnych sieci](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). Możesz też można [przeprowadzić migrację oparte na grupach koligacji wirtualnej sieci w celu regionalne wirtualnej sieci](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), a następnie utwórz nowy maszyn wirtualnych.

## <a name="next-steps"></a>Następne kroki
Jeśli po uruchomieniu przestał maszyn wirtualnych systemu Windows lub zmienić istniejące maszyn wirtualnych systemu Windows w Azure występują problemy, zobacz [Rozwiązywanie problemów klasyczny wdrożenia ponowne uruchomienie lub zmiana rozmiaru istniejące maszyny wirtualnej systemu Windows Azure](windows/classic/virtual-machines-windows-classic-restart-resize-error-troubleshooting.md).
