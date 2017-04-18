<properties
   pageTitle="Włączanie lub wyłączanie monitorowania Azure maszyn wirtualnych"
   description="Opisano, jak włączyć lub wyłączyć Azure maszyn wirtualnych monitorowania"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="enable-or-disable-azure-vm-monitoring"></a>Włączanie lub wyłączanie monitorowania Azure maszyn wirtualnych

W tej sekcji opisano, jak włączyć lub wyłączyć monitorowania w przypadku maszyn wirtualnych uruchomionych Azure. Monitorowanie jest domyślnie włączona w przypadku maszyn wirtualnych Azure wdrożony w [Azure portal](https://portal.azure.com) i monitorowania wykresów są udostępniane domyślnie się kropką 1-minutowy. Można włączyć lub wyłączyć, monitorowanie za pomocą portalu lub Azure interfejs wiersza polecenia dla komputerów Mac, Linux i systemu Windows (polecenie Azure). 

## <a name="enable--disable-monitoring-through-the-azure-portal"></a>Włączanie / wyłączanie monitorowania za pośrednictwem portalu Azure
 
Możesz włączyć monitorowanie usługi Azure Głosowa, która zawiera dane dotyczące wystąpienia w okresach 1-minutowe. (Zastosuj zmiany miejsca do magazynowania). Dane szczegółowe diagnostyki jest dostępna dla maszyn wirtualnych w portalu wykresów lub za pośrednictwem interfejsu API. Domyślnie Azure portal umożliwia monitorowania, ale można ją wyłączyć, zgodnie z poniższym opisem. Możesz włączyć monitorowania podczas maszyn wirtualnych jest uruchomiony lub w stanem zatrzymany.

- Otwórz Azure portalu na ** [https://portal.azure.com](https://portal.azure.com)**

- W lewym okienku nawigacji kliknij maszyn wirtualnych.

- W środowisku maszyn wirtualnych listy wybierz wystąpienie uruchomiona, czy zatrzymana. Błąd maszyn wirtualnych zostanie otwarty.

- Kliknij pozycję "Wszystkie ustawienia".

- Kliknij pozycję "Diagnostyka".

- Zmienianie stanu na włączony lub wyłączony. Możesz również wybrać w tym karta poziom monitorowania szczegóły, które chcesz włączyć komputera wirtualnych.

[Azure.Note] Przełączanie diagnostyki na to ustawienie domyślne, podczas tworzenia nowej maszyny wirtualnej

![Włączanie / wyłączanie monitorowania za pośrednictwem portalu Azure.][1]


## <a name="enable--disable-monitoring-with-azure-cli"></a>Włączanie / wyłączanie monitorowania z polecenie Azure
 
Aby włączyć monitorowanie maszyn wirtualnych Azure.

- Tworzenie pliku o nazwach takich jak PrivateConfig.json o następującej treści.
        {"storageAccountName": "konta miejsca do magazynowania w celu odbioru danych", "storageAccountKey": "klucz konta"}
- Uruchom następujące polecenie, polecenie Azure.

        azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.0 --private-config-path PrivateConfig.json

[Azure.Note] Możesz zmienić z wersji 2.0 do nowszej wersji, gdy są dostępne. 

Szczegółowe informacje o konfigurowaniu monitorowanie metryki i próbkach, można znaleźć w dokumencie — **[Przy użyciu Linux diagnostyczne rozszerzenia maszyn Linux monitorowanie wydajności i dane diagnostyczne](virtual-machines-linux-classic-diagnostic-extension.md).

<!--Image references-->
[1]: ./media/virtual-machines-linux-vm-monitoring/portal-enable-disable.png
 

