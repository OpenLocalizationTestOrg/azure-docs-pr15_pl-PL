<properties
   pageTitle="W przypadku maszyn wirtualnych systemu Windows za pomocą SAP | Microsoft Azure"
   description="Wyczyść dotyczące korzystania z systemu SAP w Windows wirtualnych maszyn platformy Microsoft Azure"
   services="virtual-machines-windows,virtual-network,storage"
   documentationCenter="saponazure"
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-service-management"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="10/04/2016"
   ms.author="sedusch"/>

# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>W przypadku maszyn wirtualnych systemu Windows w Azure za pomocą systemu SAP

Obliczanie jest powszechnie używany termin, który jest uzyskania bardziej ważność w branży informatycznej małych firm do dużych i międzynarodowej firmy w chmurze. Microsoft Azure jest chmurze platforma usług firmy Microsoft, która oferuje szeroką gamę nowych możliwości. Teraz klienci będą mogli szybko zapewnianie i usuwanie aplikacji jako usług w chmurze, dzięki czemu nie są ograniczone do ograniczeń technicznych lub Budżetowanie. Zamiast poświęcania czasu i budżetu do infrastruktury sprzętowej, firmy skoncentrować się na aplikacji, procesów biznesowych i korzyści dla klientów i użytkowników.

Microsoft Azure maszyn wirtualnych firma Microsoft udostępnia pełna infrastruktury jako platformy usług (IaaS). SAP NetWeaver podstawie aplikacje są obsługiwane na maszyn wirtualnych Azure (IaaS). Oficjalne dokumenty poniżej opisano sposób planowania i implementowania aplikacji SAP NetWeaver oparte na maszyn wirtualnych systemu Windows w Azure. Można także zaimplementować SAP NetWeaver podstawie aplikacji w [środowisku maszyn wirtualnych systemu Linux](virtual-machines-linux-classic-sap-get-started.md).

[AZURE.INCLUDE [virtual-machines-common-classic-sap-get-started](../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP NetWeaver Azure - HA

Nazwa: SAP NetWeaver Azure - klaster SAP ASCS-SCS wystąpień przy użyciu klaster pracy awaryjnej serwera systemu Windows Azure z SIOS DataKeeper

Podsumowanie: "ten dokument opisano, jak skonfigurować wysokiej dostępności SAP ASCS-SCS Azure za pomocą SIOS DataKeeper. SAP — chroni ich pojedynczy punkt awarii składników, takich jak SAP ASCS-SCS lub usługi replikacji kolejkowania wywołań zwrotnych z konfiguracji klaster pracy awaryjnej systemu Windows Server, które wymagają dysków udostępnionych. Składniki SAP są istotne dla funkcje systemu SAP. W związku z tym funkcji wysokiej dostępności musi znajdować się w miejscu, aby upewnić się, że te składniki można wyważonego błąd serwera lub maszyny jako gotowy z konfiguracji systemu Windows klaster zera i środowiskach funkcji Hyper-V. Od sierpnia 2015 Azure sam nie zawiera zdecydowanie dostępnych konfiguracji wymagane dla tych krytycznej składników SAP podstawą dysków udostępnionych, które są wymagane dla systemu Windows. Jednak za pomocą produktu DataKeeper przez SIOS konfiguracji klaster pracy awaryjnej systemu Windows Server, stosownie do potrzeb dla SAP ASCS-SCS mogą być wbudowane na platformie Azure IaaS. W tym dokumencie opisano w podejściu krok do kroku Instalowanie konfiguracji klaster pracy awaryjnej systemu Windows Server przy użyciu udostępnionego dysk dostarczony przez Datakeeper SIOS platformy Azure. Papier wyjaśniono szczegóły w konfiguracji, po stronie Azure, systemu Windows i SAP w celu ułatwienia konfiguracji wysoką dostępność pracy w optymalny sposób. Papier uzupełnia dokumentacji instalacji SAP i notatki SAP, reprezentujące podstawowego zasoby dotyczące instalacji i wdrożenia oprogramowania SAP na określonych platformach.

Aktualizacja: Sierpień 2015 r.

[Pobierz ten przewodnik](http://go.microsoft.com/fwlink/?LinkId=613056)
