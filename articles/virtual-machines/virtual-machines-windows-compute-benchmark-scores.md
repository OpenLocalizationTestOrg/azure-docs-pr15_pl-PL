<properties
 pageTitle="Obliczyć wyniki testu dla systemu Windows maszyny wirtualne | Microsoft Azure"
 description="Porównaj wyniki testów wydajności obliczeń SPECint dla maszyny wirtualne Azure z systemem Windows Server"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="cynthn"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="cynthn"/>

# <a name="compute-benchmark-scores-for-windows-vms"></a>Obliczyć wyniki testu dla maszyny wirtualne systemu Windows

Następujące wyniki testu SPECInt pokazywania wydajności obliczeń dla osoby Azure wysokiej wydajności maszyn wirtualnych — zestawienie z systemem Windows Server. Obliczenia, wyniki testu są również dostępne dla [Maszyny wirtualne Linux](virtual-machines-linux-compute-benchmark-scores.md).



## <a name="a-series---compute-intensive"></a>Seria A-obliczeniowych


Rozmiar | vCPUs | Węzły NUMA | PROCESOR | Zostanie uruchomiony | Średnia bazowej stopy | OdchStd
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_A8 | 8 | 1 | Intel Xeon E5 Procesora — 2670 0 @ 2,6 GHz | 10 | 236.1 | 1.1
Standard_A9 | 16 | 2 | Intel Xeon E5 Procesora — 2670 0 @ 2,6 GHz | 10 | 450.3 | 7.0
Standard_A10 | 8 | 1 | Intel Xeon E5 Procesora — 2670 0 @ 2,6 GHz | 5 | 235.6 | 0,9
Standard_A11 | 16 | 2 | Intel Xeon E5 Procesora — 2670 0 @ 2,6 GHz |7 | 454.7 | 4.8

## <a name="dv2-series"></a>Seria Dv2


Rozmiar | vCPUs | Węzły NUMA | PROCESOR | Zostanie uruchomiony | Średnia bazowej stopy | OdchStd
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_D1_v2 | 1 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 83 | 36.6 | 2.6
Standard_D2_v2 | 2 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 27 | 70.0 | 3,7
Standard_D3_v2 | 4 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 19 | 130.5 | 4.4
Standard_D4_v2 | 8 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 19 | 238.1 | 5,2
Standard_D5_v2 | 16 | 2 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 14 | 460.9 | 15,4
Standard_D11_v2 | 2 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 19 | 70,1 | 3,7
Standard_D12_v2 | 4 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 2 | 132.0 | 1.4
Standard_D13_v2 | 8 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 17 | 235.8 | 3.8
Standard_D14_v2 | 16 | 2 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 15 | 460.8 | 6.5


## <a name="g-series-gs-series"></a>G serii, serii GS


Rozmiar | vCPUs | Węzły NUMA | PROCESOR | Zostanie uruchomiony | Średnia bazowej stopy | OdchStd
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_G1, Standard_GS1  | 2 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 31 | 71.8 | 6.5
Standard_G2, Standard_GS2 | 4 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 5 | 133.4 | 13.0
Standard_G3, Standard_GS3 | 8 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 6 | 242.3 | 6.0
Standard_G4, Standard_GS4 | 16 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 15 | 398.9 | 6.0
Standard_G5, Standard_GS5 | 32 | 2 | Intel Xeon E5-2698B v3 @ 2 GHz | 22 | 762.8 | 3,7

## <a name="h-series"></a>Seria H

Rozmiar | vCPUs | Węzły NUMA | PROCESOR | Zostanie uruchomiony | Liczba iteracji na sekundę | OdchStd
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_H8 | 8 | 1 | Intel Xeon E5 2667 v3 @ 3,2 GHz | 5 | 297.4 | 0,9
Standard_H16 | 16 | 2 | Intel Xeon E5 2667 v3 @ 3,2 GHz | 5 | 575.8 | 6.8
Standard_H8m | 8 | 1 | Intel Xeon E5 2667 v3 @ 3,2 GHz | 5 | 297.0 | 1.2
Standard_H16m | 16 | 2 | Intel Xeon E5 2667 v3 @ 3,2 GHz | 5 | 572.2 | 3.9
Standard_H16r | 16 | 2 | Intel Xeon E5 2667 v3 @ 3,2 GHz | 5 | 573.2 | 2.9
Standard_H16mr | 16 | 2 | Intel Xeon E5 2667 v3 @ 3,2 GHz | 7 | 569.6 | 2,8

## <a name="about-specint"></a>Informacje o SPECint

Numery Windows zostały obliczana, uruchamiając [SPECint 2006](https://www.spec.org/cpu2006/results/rint2006.html) w systemie Windows Server. SPECint została uruchomiona za pomocą opcji stóp bazowych (SPECint_rate2006) z jedną kopią na core. SPECint składa się z 12 oddzielne badania, każdy uruchamianie trzy razy, sporządzanie mediana z każdego badania i waga ich do utworzenia złożonych wynik. Następnie uruchomienia tych testów u wielu maszyny wirtualne o podanie średnia wyniki wyświetlane.


## <a name="next-steps"></a>Następne kroki

* Pojemność, szczegóły dysku i dodatkowe zagadnienia dotyczące wybierania między rozmiarów maszyn wirtualnych zobacz [rozmiarów maszyn wirtualnych](virtual-machines-windows-sizes.md).
