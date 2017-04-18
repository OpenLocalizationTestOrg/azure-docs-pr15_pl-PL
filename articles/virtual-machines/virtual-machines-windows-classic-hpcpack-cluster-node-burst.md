<properties
 pageTitle="Dodawanie serii węzły z klastrem HPC Pack | Microsoft Azure"
 description="Dowiedz się, jak rozwiń klaster HPC Pack w Azure na żądanie, dodając wystąpień roli pracownika w usłudze w chmurze"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>Dodawanie węzłów na żądanie "serii" z klastrem HPC Pack platformy Azure



Jeśli skonfigurujesz klastrze [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) platformy Azure warto umożliwia szybkie skalowanie możliwości klaster w górę lub w dół, bez zachowania zestaw wstępnie obliczeń węzeł maszyny wirtualne. W tym artykule pokazano, jak dodać węzłów na żądanie "serii" (wystąpienia roli Pracownik uruchomiony w usłudze w chmurze) jako zasoby obliczeń do węzła głównego w Azure. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

![Węzły serii][burst]

Kroki opisane w tym artykule pomocy możesz szybko dodać węzły Azure opartej na chmurze HPC Pack węzła głównego maszyn wirtualnych test lub wdrożenia koncepcji. Czynności wysokiego poziomu są takie same jak kroki umożliwiające "serii Azure" Dodawanie chmury obliczyć możliwości klastrze HPC Pack lokalnego. Samouczek zobacz [Konfigurowanie hybrydowego obliczyć klaster z Microsoft HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Szczegółowe wskazówki i zagadnienia związane z wdrożeń produkcji zobacz [serii Azure z Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).


## <a name="prerequisites"></a>Wymagania wstępne

* **Węzła głównego HPC Pack wdrożony w maszyn wirtualnych Azure** — możesz użyć autonomiczny węzła głównego maszyn wirtualnych lub taki, który jest częścią większych klaster. Aby utworzyć autonomiczny węzła głównego, zobacz [Rozmieszczanie HPC Pack głowy węzła w maszyn wirtualnych Azure](virtual-machines-windows-hpcpack-cluster-headnode.md). Automatyczne opcji wdrażania klaster HPC Pack zobacz [Opcje tworzenia i zarządzania klaster HPC systemu Windows Azure z Microsoft HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md).

    >[AZURE.TIP] Jeśli utworzyć klaster w Azure za pomocą [skryptu wdrażania HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) , może obejmować węzły Azure serii automatycznego wdrożenia. Zapoznaj się z przykładami w tym artykule.

* **Azure subskrypcji** — Dodawanie Azure węzły, możesz wybrać tej samej subskrypcji wdrażane węzła głównego Głosowa, lub innej subskrypcji (lub subskrypcje).

* **Przydział rdzenie** - może być konieczne zwiększyć przydział rdzeni, zwłaszcza jeśli zainstalujesz kilka węzłów Azure z wielordzeniowych o rozmiarach. Aby zwiększyć przydziału, [Otwórz żądanie pomocy technicznej online klienta](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) bezpłatnie.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Krok 1: Tworzenie usługi w chmurze i kontem miejsca do magazynowania dla węzłów Azure

Aby skonfigurować następujące zasoby, które są potrzebne do wdrożenia węzły Azure za pomocą portal Azure klasyczny lub równoważnego narzędzia:

* Nowe usługi w chmurze Azure
* Nowe konto Azure miejsca do magazynowania

>[AZURE.NOTE] Nie ponownie użyć istniejącej usługi w chmurze w ramach subskrypcji. 

**Zagadnienia dotyczące**

* Konfigurowanie usługi w chmurze oddzielnych dla każdego szablonu Azure węzeł, który ma zostać utworzona. Jednak może zawierać tego samego konta miejsca do magazynowania dla wielu szablonów węzeł.

* Firma Microsoft zaleca Znajdź usługi w chmurze i konto miejsca do magazynowania dla wdrożenia w tym samym regionie Azure.




## <a name="step-2-configure-an-azure-management-certificate"></a>Krok 2: Konfigurowanie certyfikatu usługi zarządzania Azure

Aby dodać węzły Azure jako zasoby obliczeń, należy certyfikatu zarządzania węzła głównego i przekaż certyfikatów skojarzony z subskrypcją Azure na potrzeby wdrożenia.

W tym scenariuszu możesz wybrać **Domyślny certyfikat zarządzania Azure HPC** HPC Pack instaluje i automatycznie konfiguruje węzła głównego. Ten certyfikat jest przydatne do testowania celów i wdrożenia koncepcji. Aby użyć tego certyfikatu, Przekaż plik 2012\Bin\hpccert.cer C:\Program Files\Microsoft HPC Pack z węzła głównego maszyn wirtualnych z subskrypcją. Aby przekazać certyfikatu w [portal Azure klasyczny](https://manage.windowsazure.com), kliknij pozycję **Ustawienia** > **Certyfikaty zarządzania**.

Aby uzyskać dodatkowych opcji, aby skonfigurować certyfikat zarządzania zobacz [scenariuszy, aby skonfigurować certyfikat zarządzania Azure w przypadku wdrożeń serii Azure](http://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>Krok 3: Wdrażanie Azure węzły z klastrem



Czynności, aby dodać i uruchomić Azure węzły w tym scenariuszu zazwyczaj są takie same jak kroki z lokalnego węzła głównego. Aby uzyskać więcej informacji zobacz następujące sekcje w [czynności, aby wdrożyć węzły Azure za pomocą Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx):

* Tworzenie szablonu węzeł Azure

* Dodawanie Azure węzły do klastrów HPC systemu Windows

* Rozpoczynanie węzły (Obsługa administracyjna) Azure

Po dodaniu i rozpocząć węzły są gotowe do umożliwia wykonywanie zadań klaster.

W razie wystąpienia problemów podczas wdrażania Azure węzły zobacz [Rozwiązywanie problemów z wdrożeń Azure węzły z Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Następne kroki

* Aby użyć rozmiaru wystąpienia obliczeniowych węzły serii, zobacz zagadnienia w [o serii H i obliczeniowych maszyny wirtualne A serii](virtual-machines-windows-a8-a9-a10-a11-specs.md).

* Jeśli chcesz automatycznie Zwiększ lub Zmniejsz Azure zasobów komputerowych zgodnie z pracą klaster, zobacz [automatyczne powiększanie i zmniejszanie zasobów Azure do uruchamiania w klastrze HPC Pack](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png
