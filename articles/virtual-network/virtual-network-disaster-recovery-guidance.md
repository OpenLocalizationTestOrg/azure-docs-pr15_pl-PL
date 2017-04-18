<properties
    pageTitle="Co należy zrobić w przypadku awarii usługi Azure, wpływające na ochronę Azure wirtualnych sieci | Microsoft Azure"
    description="Dowiedz się, co należy zrobić w przypadku awarii usługi Azure, wpływające na ochronę Azure wirtualnych sieci."
    services="virtual-network"
    documentationCenter=""
    authors="NarayanAnnamalai"
    manager="jefco"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="virtual-network"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="narayan;aglick"/>

#<a name="virtual-network--business-continuity"></a>Wirtualna sieć — ciągłości

##<a name="overview"></a>Omówienie

Wirtualna sieć (VNet) jest logiczna reprezentacja sieci w chmurze. Umożliwia definiowanie własnych prywatnych przestrzeni adresów IP i segmentów sieci na podsieci. VNets stanowi granicę zaufania do obsługi usługi zasobów obliczeń, takich jak maszyn wirtualnych Azure i usług w chmurze (web pracownik role). VNet umożliwia bezpośrednie prywatne komunikacji IP między zasobami obsługiwanymi w nim. Wirtualna sieć mogą być również połączone z siecią lokalnego za pośrednictwem jedną z opcji hybrydowe, takich jak Brama VPN lub ExpressRoute.
 
VNet jest tworzona w zakresie regionu. Możesz utworzyć VNets z tym samym przestrzeni adresów w dwóch różnych regionów (to znaczy nam Wschód i zachód nam, ale nie można połączyć je bezpośrednio ze sobą). 

##<a name="business-continuity"></a>Zapewnianie ciągłości

Może istnieć kilka różnych sposobów aplikacji może zostać zerwane. Danego regionu można całkowicie obcięty z powodu naturalne awarii lub częściowej awarii z powodu błędu wielu urządzeń i usług. Wpływ na usługę VNet różni się w każdej z tych sytuacji.

**P: co możesz zrobić w przypadku awarii do całego regionu to znaczy, jeśli obszar jest całkowicie graniczny z powodu naturalne awarii? Co się dzieje wirtualnych sieci hostowana w regionie?**

O: wirtualnej sieci i zasobów w regionie dotyczy pozostaje niedostępne w czasie awarii usługi.

![Diagram prostego wirtualnej sieci](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**P: co mogę ponownego tworzenia tej samej sieci wirtualnej w innym regionie?**

O: wirtualnej sieci (VNet) to dość lightweight zasobów. Można wywołać API Azure, aby utworzyć VNet z tego samego obszaru adres w innym regionie. Aby odtworzyć w tym samym środowisku, którego nie ma w regionie, którego dotyczy problem, należy ponownie wdrożyć z usługami w chmurze (web pracownik role) i maszyn wirtualnych, które trzeba było interfejsu API. Możesz również trzeba aż bramą VPN i nawiązywanie połączenia z siecią lokalnego, jeśli wcześniej używano lokalnego łączności (na przykład wdrożenia hybrydowego).

Instrukcje dotyczące tworzenia VNet znajdują się [w tym miejscu](./virtual-networks-create-vnet-arm-pportal.md). 

**P: czy replice VNet w danym regionie on odtworzony w innym regionie związaną z wcześniejszym przygotowaniem?**

O: tak, można utworzyć dwa VNets za pomocą samego prywatne przestrzeni adresów IP i zasobów na dwóch różnych regionów wyprzedzeniem. Jeśli klienta została hostingu dostępnych usług w VNet przez internet, ich można ustawić Konfigurowanie Menedżera ruch ruchu geo trasy do obszaru, który jest aktywny. Jednak klienta nie może połączyć dwie VNets z tego samego obszaru adres do ich sieci lokalnej jako może spowodować problemy routingu. W tym czasie awarii i utratę VNet w jednym regionie klienta można połączyć inne VNet w obszarze dostępne z dopasowanymi przestrzeni adresów do ich sieci lokalnej.

Instrukcje dotyczące tworzenia VNet znajdują się [w tym miejscu](./virtual-networks-create-vnet-arm-pportal.md).
