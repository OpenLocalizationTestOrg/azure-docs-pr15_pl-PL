<properties
 pageTitle="Tworzenie węzła głównego HPC Pack w maszyn wirtualnych Azure | Microsoft Azure"
 description="Dowiedz się, jak utworzyć Microsoft HPC Pack węzła głównego w maszyn wirtualnych Azure za pomocą Azure portal i model wdrożenia Menedżera zasobów."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/17/2016"
 ms.author="danlep"/>

# <a name="create-the-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Tworzenie węzła głównego klastrze HPC Pack w maszyn wirtualnych Azure z obrazem Marketplace


Umożliwia utworzenie węzła głównego klastrze HPC [Microsoft HPC Pack maszyn wirtualnych obraz](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) z usługi Azure Marketplace i Azure portal. Ten obraz HPC maszyn wirtualnych z dodatkiem Service Pack jest oparty na Centrum Windows Server 2012 R2 danych z preinstalowanym HPC Pack 2012 R2 aktualizacji 3. Użyj tego węzła głównego dowodu koncepcji wdrażania pakietu HPC platformy Azure. Następnie można dodać węzły obliczeń do klastrów, aby uruchomić obciążenia HPC.



>[AZURE.TIP]Aby wdrożyć klastrze HPC Pack pełną platformy Azure, zawierająca węzła głównego i węzły obliczeń, zalecamy metoda automatycznego. Opcje obejmują [skrypt wdrożenia HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) i szablonu Menedżera zasobów [klastrze HPC Pack dla obciążenia systemu Windows](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/) . Zobacz [Opcje klaster HPC Pack w Azure](virtual-machines-windows-hpcpack-cluster-options.md) dla dodatkowych szablonów. 


## <a name="planning-considerations"></a>Zagadnienia związane z planowaniem

Jak pokazano na poniższym rysunku, należy wdrożyć węzła głównego HPC Pack w domenie usługi Active Directory w Azure wirtualnej sieci.

![HPC Pack węzła głównego][headnode]

* **Domeny usługi active Directory** - HPC Pack węzła głównego muszą być dołączone do domeny usługi Active Directory platformy Azure przed rozpoczęciem usług HPC maszyn wirtualnych. Przedstawione w tym artykule, dowód wdrożenia koncepcja można awansować maszyn wirtualnych, możesz utworzyć dla węzła głównego jako kontrolera domeny przed rozpoczęciem usług HPC. Innym rozwiązaniem jest wdrożenie kontrolera domeny oddzielnych i las platformy Azure, do którego możesz dołączyć węzła głównego maszyn wirtualnych.

* **Azure wirtualną sieć** — po wdrażanie węzła głównego za pomocą Menedżera zasobów modelu wdrożenia, możesz określić lub utworzyć Azure wirtualną sieć. Możesz użyć wirtualnej sieci, jeśli chcesz dołączyć do węzła głównego do istniejącej domeny usługi Active Directory. Ponadto potrzebny go później, aby dodać węzeł obliczeń maszyny wirtualne z klastrem.

    
## <a name="steps-to-create-the-head-node"></a>Procedura tworzenia węzła głównego

Poniżej przedstawiono czynności wysokiego poziomu za pomocą portalu Azure Tworzenie maszyn wirtualnych Azure dla węzła głównego HPC Pack przy użyciu modelu wdrożenia Menedżera zasobów. 


1. Jeśli chcesz utworzyć nowy las usługi Active Directory platformy Azure kontrolera domeny osobnych maszyny wirtualne, jedną z opcji jest użycie [szablonu Menedżera zasobów](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/). Prosta dowód wdrożenia pojęcia jest poprawnie Pomiń ten krok i skonfiguruj węzła głównego maszyn wirtualnych sam jako kontrolera domeny. Ta opcja jest opisane w dalszej części tego artykułu.
    
2. Na [HPC Pack 2012 R2 na stronie Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) w Azure Marketplace kliknij przycisk **Utwórz maszyn wirtualnych**. 

3. W portalu na stronie **HPC Pack 2012 R2 w systemie Windows Server 2012 R2** wybierz model wdrożenia **Menedżera zasobów** , a następnie kliknij przycisk **Utwórz**.

    ![Obraz HPC Pack][marketplace]

4. Konfigurowanie ustawień i Tworzenie maszyn wirtualnych za pomocą portalu. Jeśli jesteś nowym użytkownikiem Azure, wykonaj samouczek [tworzenia maszyny wirtualnej systemu Windows w portalu Azure](virtual-machines-windows-hero-tutorial.md). Dla dowód wdrożenia pojęcia zazwyczaj można zaakceptować domyślną lub zalecanych ustawień.

    >[AZURE.NOTE]Jeśli chcesz dołączyć do węzła głównego do istniejącej domeny usługi Active Directory platformy Azure, upewnij się, że określić wirtualnej sieci dla tej domeny podczas tworzenia maszyn wirtualnych.
       
4. Po utworzenia maszyn wirtualnych i maszyn wirtualnych jest uruchomiony, [Nawiązywanie połączenia z maszyn wirtualnych](virtual-machines-windows-connect-logon.md) przez pulpit zdalny. 

5. Dołączanie do istniejącego domenami maszyn wirtualnych, lub Utwórz domenami na maszyn wirtualnych się.

    * Jeśli utworzono maszyn wirtualnych w Azure wirtualnej sieci z domenami istniejące połączenia maszyn wirtualnych las przy użyciu standardowych narzędzi Menedżer serwera lub środowiska Windows PowerShell. Następnie uruchom ponownie.

    * Jeśli utworzono maszyn wirtualnych w nowych wirtualnej sieci (bez istniejących domenami), promowanie maszyn wirtualnych jako kontrolera domeny. Wykonaj czynności standardowy, aby zainstalować i skonfigurować roli usług domenowych Active Directory na węzła głównego. Aby uzyskać szczegółowe instrukcje zobacz [Instalowanie nowego systemu Windows Server 2012 las usługi Active Directory](https://technet.microsoft.com/library/jj574166.aspx).

5. Po maszyn wirtualnych jest uruchomiony i jest dołączony do las usługi Active Directory, uruchom usługi HPC Pack w następujący sposób:

    . Podłącz do węzła głównego maszyn wirtualnych przy użyciu konta domeny, które jest członkiem lokalnej grupy Administratorzy. Na przykład za pomocą konta administratora, które możesz skonfigurować podczas tworzenia węzła głównego maszyn wirtualnych.

    b. Dla domyślnej konfiguracji węzła głównego Uruchom program Windows PowerShell jako administrator i wpisz następujące polecenie:

    ```
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```

    Może potrwać kilka minut na uruchomienie usług HPC Pack.

    Opcje konfiguracji dodatkowe węzła głównego, wpisz `get-help HPCHNPrepare.ps1`.


## <a name="next-steps"></a>Następne kroki

* Teraz można pracować z węzeł głowy klaster HPC Pack. Na przykład uruchom Menedżera klaster HPC i pełna [Lista zadań do wykonania wdrożenia](https://technet.microsoft.com/library/jj884141.aspx).
* Jeśli chcesz zwiększyć klaster obliczyć pojemności na żądanie, Dodaj [węzły Azure serii](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md) w usłudze w chmurze. 

* Spróbuj uruchomić test obciążenie pracą w klastrze. Na przykład zobacz HPC Pack [Przewodnik wprowadzenie](https://technet.microsoft.com/library/jj884144).

<!--Image references-->
[headnode]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/marketplace.png
