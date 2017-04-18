<properties
 pageTitle="Obliczyć wyniki testu dla maszyny wirtualne Linux | Microsoft Azure"
 description="Porównaj wyniki testów wydajności obliczeń CoreMark dla maszyny wirtualne Azure systemem Linux"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="cynthn"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="cynthn"/>

# <a name="compute-benchmark-scores-for-linux-vms"></a>Obliczyć wyniki testu dla maszyny wirtualne Linux

Następujące wyniki testu CoreMark pokazywania wydajności obliczeń dla osoby Azure wysokiej wydajności maszyn wirtualnych — zestawienie uruchomiony Ubuntu. Obliczenia, wyniki testu są również dostępne dla [Maszyny wirtualne systemu Windows](virtual-machines-windows-compute-benchmark-scores.md).




## <a name="a-series---compute-intensive"></a>Seria A-obliczeniowych


Rozmiar | vCPUs | Węzły NUMA | PROCESOR | Zostanie uruchomiony | Liczba iteracji na sekundę | OdchStd
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_A8 | 8 | 1 | Intel Xeon E5 Procesora — 2670 0 @ 2,6 GHz| 179 | 110,294 | 554
Standard_A9 | 16 | 2 | Intel Xeon E5 Procesora — 2670 0 @ 2,6 GHz| 189 | 210,816| 2,126
Standard_A10 | 8 | 1 | Intel Xeon E5 Procesora — 2670 0 @ 2,6 GHz| 188 | 110,025 | 1,045
Standard_A11 | 16 | 2 | Intel Xeon E5 Procesora — 2670 0 @ 2,6 GHz| 188 | 210,727| 2,073

## <a name="dv2-series"></a>Seria Dv2


Rozmiar | vCPUs | Węzły NUMA | PROCESOR | Zostanie uruchomiony | Liczba iteracji na sekundę | OdchStd
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_D1_v2 | 1 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 140 | 14,852 | 780
Standard_D2_v2 | 2 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 133 | 29,467 | 1,863
Standard_D3_v2 | 4 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 139 | 56,205 | 1,167
Standard_D4_v2 | 8 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 126 | 108,543 | 3,446
Standard_D5_v2 | 16 | 2 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 126 | 205,332 | 9,998
Standard_D11_v2 | 2 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 125 | 28,598 | 1,510
Standard_D12_v2 | 4 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 131 | 55,673 | 1,418
Standard_D13_v2 | 8 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 140 | 107,986 | 3,089
Standard_D14_v2 | 16 | 2 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 140 | 208,186 | 8,839
Standard_D15_v2 | 20 | 2 | Intel Xeon E5 2673 v3 @ 2,4 GHz |28 | 268,560 | 4,667

## <a name="f-series"></a>Seria F

Rozmiar | vCPUs | Węzły NUMA | PROCESOR | Zostanie uruchomiony | Liczba iteracji na sekundę | OdchStd
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_F1 | 1 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 154 | 15,602 | 787
Standard_F2 | 2 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 126 | 29,519 | 1,233
Standard_F4 | 4 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 147 | 58,709 | 1,227
Standard_F8 | 8 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 224 | 112,772 | 3,006
Standard_F16 | 16 | 2 | Intel Xeon E5 2673 v3 @ 2,4 GHz | 42 | 218,571 | 5,113



## <a name="g-series"></a>G serii


Rozmiar | vCPUs | Węzły NUMA | PROCESOR | Zostanie uruchomiony | Liczba iteracji na sekundę | OdchStd
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_G1 | 2 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 83 | 31,310 | 2,891
Standard_G2 | 4 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 84 | 60,112 | 3,537
Standard_G3 | 8 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 84 | 107,522 | 4,537
Standard_G4 | 16 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 83 | 195,116 | 5,024
Standard_G5 | 32 | 2 | Intel Xeon E5-2698B v3 @ 2 GHz | 84 | 360,329 | 14,212

## <a name="gs-series"></a>Serii GS


Rozmiar | vCPUs | Węzły NUMA | PROCESOR | Zostanie uruchomiony | Liczba iteracji na sekundę | OdchStd
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_GS1 | 2 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 84 | 28,613 | 1,884
Standard_GS2 | 4 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 83 | 54,348 | 3,474
Standard_GS3 | 8 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 83 | 104,564 | 1,834
Standard_GS4 | 16 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 84 | 194,111 | 4,735
Standard_GS5 | 32 | 2 | Intel Xeon E5-2698B v3 @ 2 GHz | 84 | 357,396 | 16,228


## <a name="h-series"></a>Seria H

Rozmiar | vCPUs | Węzły NUMA | PROCESOR | Zostanie uruchomiony | Liczba iteracji na sekundę | OdchStd
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_H8 | 8 | 1 | Intel Xeon E5 2667 v3 @ 3,2 GHz | 28 | 140,782 | 2,512
Standard_H16 | 16 | 2 | Intel Xeon E5 2667 v3 @ 3,2 GHz | 35 | 275,289 | 7,110 
Standard_H18m | 8 | 1 | Intel Xeon E5 2667 v3 @ 3,2 GHz | 28 | 139,071 | 3,988 
Standard_H16m | 16 | 2 | Intel Xeon E5 2667 v3 @ 3,2 GHz | 28 | 275,988 | 6,963 
Standard_H16r | 16 | 2 | Intel Xeon E5 2667 v3 @ 3,2 GHz | 28 | 273,982 | 6,069 
Standard_H16mr | 16 | 2 | Intel Xeon E5 2667 v3 @ 3,2 GHz | 28 | 274,523 | 5 698. 



## <a name="about-coremark"></a>Informacje o CoreMark

Numery Linux były obliczane, uruchamiając [CoreMark](http://www.eembc.org/coremark/faq.php) na Ubuntu. CoreMark skonfigurowano liczbę wątków równa Liczba procesorów wirtualnych, a wartość współbieżności PThreads. Liczba iteracji docelowej została dostosowana oparte na obniżenie wydajności, aby zapewnić środowisko uruchomieniowe co najmniej 20 sekund (zwykle dłużej). Ostateczny wynik reprezentuje liczbę iteracji wykonane podzielona przez liczbę sekund, potrzebnych do testu. Każdego testu zostało uruchomione co najmniej siedem godzin na każdy maszyn wirtualnych. Sprawdza (z wyjątkiem dla H series_ zostały uruchomione w października 2015 r. wielu maszyny wirtualne w każdej Azure regionu publicznego maszyn wirtualnych był obsługiwany w dniu Uruchom.

## <a name="next-steps"></a>Następne kroki



* Pojemność, szczegóły dysku i dodatkowe zagadnienia dotyczące wybierania między rozmiarów maszyn wirtualnych zobacz [rozmiarów maszyn wirtualnych](virtual-machines-linux-sizes.md).

* Aby uruchomić skrypty CoreMark maszyny wirtualne Linux, Pobierz [pack skrypt CoreMark](http://download.microsoft.com/download/3/0/5/305A3707-4D3A-4599-9670-AAEB423B4663/AzureCoreMarkScriptPack.zip).