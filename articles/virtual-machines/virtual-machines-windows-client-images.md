<properties
   pageTitle="Przy użyciu obrazów klienta systemu Windows dla deweloperów/testowanie scenariusze | Microsoft Azure"
   description="Jak używać korzyści z subskrypcji programu Visual Studio do wdrożenia systemu Windows 7 i 8-10 Azure dla deweloperów/testowanie scenariuszy"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="iainfou"/>

# <a name="using-windows-client-in-azure-for-devtest-scenarios"></a>Przy użyciu klienta systemu Windows w Azure dla deweloperów/testowanie scenariuszy

Można użyć systemu Windows 7, Windows 8 lub Windows 10 platformy Azure dla deweloperów/testowanie scenariuszy pod warunkiem, że masz odpowiednie subskrypcję programu Visual Studio (dawniej MSDN). W tym artykule przedstawiono wymagania dotyczące uruchomionego klienta w systemie Windows Azure i stosowania obrazów Galeria Azure uprawnień.


## <a name="subscription-eligibility"></a>Uprawnienia do subskrypcji
Do projektowania i testowania aktywne subskrybentów programu Visual Studio (osoby, które nabyli licencję subskrypcyjną programu Visual Studio) można użyć klienta systemu Windows. Klient systemu Windows może być używany na własny sprzęt i Azure maszyn wirtualnych z systemem w typu Azure subskrypcji. Klienta w systemie Windows może nie być wdrożony używane na Azure normalny komercyjny lub używane przez osoby, które nie są aktywne subskrybentów programu Visual Studio.

Dla wygody wprowadzono niektóre obrazy systemu Windows 10 dostępne z galerii Azure w [odpowiedniej deweloperów/testowanie oferuje](#eligible-offers). Visual Studio subskrybentów w dowolnego typu oferty można także [odpowiednio przygotować i tworzenie](virtual-machines-windows-prepare-for-upload-vhd-image.md) 64-bitowej systemu Windows 7, Windows 8 lub obraz systemu Windows 10 i [Przekaż Azure](virtual-machines-windows-upload-image.md). Użyj pozostaje ograniczone do standardowego/testowanie przez aktywne subskrybentów programu Visual Studio.


## <a name="eligible-offers"></a>Odpowiedniej oferty
W poniższej tabeli przedstawiono oferty identyfikatorów, które można skopiować do wdrożenia systemu Windows 10 za pośrednictwem galerii Azure. Obrazy systemu Windows 10 są widoczne tylko dla następujących oferty. Wizualne subskrybentów Studio, którzy potrzebne do uruchamiania systemu Windows klienta w polu Typ oferty różnych wymaga [odpowiednio przygotować i tworzenie](virtual-machines-windows-prepare-for-upload-vhd-image.md) 64-bitowych obraz systemu Windows 7, Windows 8 lub Windows 10 i [, a następnie przekaż Azure](virtual-machines-windows-upload-image.md).

| Nazwa oferty | Numer oferty | Obrazy dostępne klienta |
|:-----------|:------------:|:-----------------------:|
| [Repartycyjny deweloperów Test](https://azure.microsoft.com/offers/ms-azr-0023p/)                          | 0023P | Windows 10 |
| [Subskrybenci Visual Studio przedsiębiorstwa (MPN)](https://azure.microsoft.com/offers/ms-azr-0029p/)      | 0029P | Windows 10 |
| [Subskrybenci Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)          | 0059P | Windows 10 |
| [Subskrybenci Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)     | 0060P | Windows 10 |
| [Visual Studio Premium z (świadczenie) w witrynie MSDN](https://azure.microsoft.com/offers/ms-azr-0061p/)       | 0061P | Windows 10 |
| [Subskrybenci Visual Studio przedsiębiorstwa](https://azure.microsoft.com/offers/ms-azr-0063p/)            | 0063P | Windows 10 |
| [Subskrybenci Visual Studio przedsiębiorstwa (BizSpark)](https://azure.microsoft.com/offers/ms-azr-0064p/) | 0064P | Windows 10 |
| [Przedsiębiorstwo deweloperów Test](https://azure.microsoft.com/ofers/ms-azr-0148p/)                              | 0148P | Windows 10 |


## <a name="check-your-azure-subscription"></a>Sprawdzanie subskrypcji Azure
Jeśli nie znasz swojego Identyfikatora oferty, można uzyskać go za pomocą Azure portal lub portal konta.

Identyfikator subskrypcji oferta jest identyfikowany w karta "Subskrypcji" w Azure portal:

![Szczegóły oferty identyfikator z portalu Azure](./media/virtual-machines-windows-client-images/offer_id_azure_portal.png) 

Możesz także wyświetlić identyfikator oferty z poziomu [karty "Subskrypcji"](http://account.windowsazure.com/Subscriptions) portal Azure konta:

![Szczegóły oferty identyfikator z portalu konta na platformie Azure](./media/virtual-machines-windows-client-images/offer_id_azure_account_portal.png) 


## <a name="next-steps"></a>Następne kroki
Teraz można wdrożyć usługi maszyny wirtualne przy użyciu [programu PowerShell](virtual-machines-windows-ps-create.md), [Szablony Menedżera zasobów](virtual-machines-windows-ps-template.md)lub [Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
