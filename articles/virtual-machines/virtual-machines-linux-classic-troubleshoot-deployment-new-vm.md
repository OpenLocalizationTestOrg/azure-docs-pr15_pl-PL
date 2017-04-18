<properties
   pageTitle="Rozwiązywanie problemów z maszyn wirtualnych Linux wdrożenia — klasyczny | Microsoft Azure"
   description="Rozwiązywanie problemów klasyczny wdrożenia w przypadku tworzenia nowej maszyny wirtualnej Linux platformy Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
  ms.service="virtual-machines-linux"
  ms.workload="na"
  ms.tgt_pltfrm="vm-linux"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/06/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Rozwiązywanie problemów klasyczny wdrożenia do tworzenia nowych maszyny wirtualnej Linux platformy Azure

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-selectors-include.md)]

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Dzienniki inspekcji zbierania

Aby rozpocząć, rozwiązywanie problemów, zbieranie dzienników inspekcji do identyfikowania błąd związany z problem.

W portalu Azure, kliknij przycisk **Przeglądaj** > **maszyn wirtualnych** > *komputera wirtualnych* > **Ustawienia** > **dzienników inspekcji**.

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** System operacyjny jest Linux uogólniony i jest przekazane lub przechwycone za pomocą ustawień ogólnych, następnie nie można jakiekolwiek błędy. Podobnie jeśli jest system operacyjny Linux specjalne, jest przekazane lub przechwycone z ustawieniem specjalistyczne, a następnie będzie jakiekolwiek błędy.

**Przekazywanie błędów:**

**N<sup>1</sup>:** Jeśli system operacyjny jest Linux uogólniony i przekazaniu jako specjalistyczne, zostanie wyświetlony obsługi administracyjnej błąd przekroczenia limitu czasu ponieważ maszyn wirtualnych zatrzymuje się na etapie inicjowania obsługi administracyjnej.

**N<sup>2</sup>:** W przypadku specjalistyczne Linux i przekazaniem jako uogólniony system operacyjny zostanie wyświetlony błąd błąd obsługi administracyjnej ponieważ nowa maszyna wirtualna działa z oryginalną nazwę komputera, nazwy użytkownika i hasła.

**Rozwiązanie:**

Aby rozwiązać obu tych błędów, Przekaż oryginalny wirtualny dysk twardy, dostępne lokalna, jak to samo ustawienie dla OS (uogólniony specjalistyczne). Aby przekazać uogólniony, pamiętaj, aby uruchomić - deprovision najpierw. Aby uzyskać więcej informacji, zobacz [Tworzenie i przekazywanie wirtualnego dysku twardego zawierający System operacyjny Linux](virtual-machines-linux-classic-create-upload-vhd.md) .

**Rejestrowanie błędów:**

**N<sup>3</sup>:** Jeśli system operacyjny jest Linux uogólniony i jest rejestrowany jako specjalistyczne, zostanie wyświetlony obsługi administracyjnej błąd przekroczenia limitu czasu ponieważ oryginalny maszyn wirtualnych nie jest używany jako zostanie oznaczona jako uogólniony.

**N<sup>4</sup>:** W przypadku specjalistyczne Linux i jest rejestrowany, jak uogólniony system operacyjny zostanie wyświetlony błąd błąd obsługi administracyjnej ponieważ nowa maszyna wirtualna działa z oryginalną nazwę komputera, nazwy użytkownika i hasła. Ponadto oryginalna maszyn wirtualnych nie jest używany, ponieważ jest oznaczona jako specjalnych.

**Rozwiązanie:**

Aby rozwiązać obu tych błędów, usuwania bieżącego obrazu z portalu i [ponownie go z bieżącym wirtualnych dysków twardych przechwycić](virtual-machines-linux-classic-capture-image.md) takie same ustawienia, jak dla systemu operacyjnego (uogólniony specjalistycznego).

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
Jeśli po uruchomieniu przestał maszyny Linux lub zmienić istniejące maszyn wirtualnych Linux platformy Azure występują problemy, zobacz [Rozwiązywanie problemów klasyczny wdrożenia ponowne uruchomienie lub zmiana rozmiaru istniejące maszyny wirtualnej Linux platformy Azure](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md).
