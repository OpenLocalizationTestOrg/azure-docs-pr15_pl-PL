<properties
   pageTitle="Dostawców obliczeń, sieci i miejsca do magazynowania | Microsoft Azure"
   description="Omówienie obliczeń, sieci i dostawców zasobów magazynowania (CRP, NRP i SRP) dla aplikacji Linux w modelu wdrożenia Menedżera zasobów Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/19/2015"
   ms.author="tomfitz"/>

# <a name="azure-compute-network-and-storage-providers-for-linux-applications-under-azure-resource-manager-deployment-model"></a>Azure dostawców obliczeń, sieci i miejsca do magazynowania dla aplikacji Linux w obszarze modelu wdrożenia Menedżera zasobów Azure

Włączenie funkcji obliczeń, sieci i miejsca do magazynowania z modelem wdrożenia Menedżera zasobów Azure istotnie będzie uprościć, rozmieszczanie i zarządzanie skomplikowanych aplikacji uruchomionych IaaS. Wiele aplikacji wymaga kombinacji zasobów, w tym wirtualnej sieci, konto miejsca do magazynowania, maszyn wirtualnych i karty sieciowej. Model wdrażania Menedżera zasobów Azure oferuje możliwość utworzenia szablonu JSON wdrażać i zarządzanie te zasoby razem jako jeden wniosek.

[AZURE.INCLUDE [virtual-machines-common-compare-deployment-models](../../includes/virtual-machines-common-compare-deployment-models.md)]
