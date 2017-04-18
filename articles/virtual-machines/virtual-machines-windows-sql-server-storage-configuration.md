<properties
    pageTitle="Konfiguracja miejsca do magazynowania dla programu SQL Server w maszyny wirtualne | Microsoft Azure"
    description="W tym temacie opisano, jak Azure konfiguruje miejsca do magazynowania dla maszyny wirtualne programu SQL Server podczas inicjowania obsługi administracyjnej (model wdrożenia Menedżera zasobów). Wyjaśniono również, jak można skonfigurować miejsca do magazynowania dla swojego istniejącego maszyny wirtualne serwera SQL."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="ninarn"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/04/2016"
    ms.author="ninarn" />

# <a name="storage-configuration-for-sql-server-vms"></a>Konfiguracja miejsca do magazynowania dla maszyny wirtualne programu SQL Server

Po skonfigurowaniu obraz maszyn wirtualnych programu SQL Server w Azure portalu ułatwia Automatyzowanie konfiguracji magazynowania. Ta opcja uwzględnia związanych Głosowa, udostępnienie magazynu programu SQL Server i konfigurowanie go aby zoptymalizować pod kątem wymagań dotyczących wydajności zależnych od miejsca do magazynowania.

W tym temacie wyjaśniono, jak Azure konfiguruje miejsca do magazynowania dla maszyny wirtualne programu SQL Server podczas inicjowania obsługi administracyjnej oraz dla istniejących maszyny wirtualne. Ta konfiguracja jest oparty na [Wydajność najważniejsze wskazówki](virtual-machines-windows-sql-performance.md) dla maszyny wirtualne Azure uruchomiony program SQL Server.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]model klasyczny wdrożenia.

## <a name="prerequisites"></a>Wymagania wstępne
Aby użyć ustawień konfiguracji automatycznego przechowywania, komputer wirtualnych wymaga następujące właściwości:

- Zainicjowano obsługę administracyjną z [obrazu z galerii programu SQL Server](virtual-machines-windows-sql-server-iaas-overview.md#option-1-deploy-a-sql-vm-per-minute-licensing).
- Używa [model wdrożenia Menedżera zasobów](../resource-manager-deployment-model.md).
- Korzysta z [magazynu Premium](../storage/storage-premium-storage.md).

## <a name="new-vms"></a>Nowe maszyny wirtualne
W poniższych sekcjach opisano sposób konfigurowania miejsca do magazynowania dla nowych maszyn wirtualnych programu SQL Server.

### <a name="azure-portal"></a>Azure Portal
Podczas inicjowania obsługi administracyjnej maszyn wirtualnych Azure za pomocą obrazu z galerii programu SQL Server, możesz automatycznie skonfigurować miejsca do magazynowania dla Twojej nowej maszyn wirtualnych. Możesz określić magazynowania, wartości graniczne wydajności i typ obciążenia pracą. Następujące zrzut ekranu przedstawia karta konfiguracji miejsca do magazynowania, które są używane podczas maszyn wirtualnych SQL inicjowania obsługi administracyjnej.

![Konfiguracja programu SQL Server maszyn wirtualnych miejsca do magazynowania podczas inicjowania obsługi administracyjnej](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

W zależności od dokonanych wyborów, Azure wykonuje następujące zadania miejsca do magazynowania w konfiguracji po utworzeniu maszyn wirtualnych:

- Tworzy i dołącza premium miejsca do magazynowania danych dysków maszyn wirtualnych.
- Konfiguruje dyski danych, aby umożliwić dostęp do programu SQL Server.
- Konfiguruje dyski danych do puli miejsca do magazynowania, zależnie od określonej wydajność i rozmiar (operacji i/o na SEKUNDĘ i przepustowość) wymagań.
- Przypisuje nowy dysk komputera wirtualnych puli miejsca do magazynowania.
- Optymalizuje ten nowy dysk według typu obciążenie pracą określonej (magazynowanie danych, transakcyjna przetwarzania lub ogólne).

Szczegółowe informacje o jak Azure skonfigurował ustawienia miejsca do magazynowania zobacz [sekcję konfiguracji miejsca do magazynowania](#storage-configuration). Aby zapoznać się z pełną opisem sposobu tworzenia maszyn wirtualnych programu SQL Server w Azure Portal zobacz [Samouczek obsługi administracyjnej](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Szablony Zarządzanie zasobów
Jeśli korzystasz z następujących szablonów Menedżera zasobów, dwa dyski danych premium dołączonych domyślnie z konfiguracji puli miejsca do magazynowania. Jednak można dostosować tych szablonów, aby zmienić liczbę dysków danych premium, które zostały dołączone do maszyny wirtualnej.

- [Tworzenie maszyn wirtualnych z automatycznej kopii zapasowej](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
- [Tworzenie maszyn wirtualnych przy użyciu automatycznego poprawiania](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
- [Tworzenie maszyn wirtualnych przy użyciu integracji AKV](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Istniejące maszyny wirtualne
Dla istniejących maszyny wirtualne programu SQL Server można zmodyfikować niektóre ustawienia miejsca do magazynowania w portalu Azure. Wybierz swojego maszyn wirtualnych, przejdź do obszaru Ustawienia, a następnie wybierz Konfiguracja programu SQL Server. Karta Konfiguracja programu SQL Server zawiera bieżące użycie miejsca do magazynowania z maszyn wirtualnych. Wszystkie dyski, znajdują się na swojej maszyn wirtualnych są wyświetlane na tym wykresie. Dla każdego dysku ilość miejsca do magazynowania zostanie wyświetlony na cztery sekcje:

- Danych SQL
- Dziennik SQL
- Inne (magazynowanie-SQL)
- Dostępne

![Konfigurowanie miejsca do magazynowania dla istniejącego programu SQL Server maszyn wirtualnych](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

Aby skonfigurować przestrzeni dyskowej, aby dodać nowy dysk lub rozszerzanie istniejącego dysku, kliknij łącze Edytuj powyżej wykresu.

Opcje konfiguracji, które są wyświetlane są różne w zależności od tego, czy używasz tej funkcji przed. Podczas korzystania z po raz pierwszy, można określić z wymaganiami miejsca do magazynowania dla nowego dysku. Jeśli wcześniej używano tę funkcję do utworzenia dysku, możesz rozszerzyć tego dysku.

### <a name="use-for-the-first-time"></a>Użyj po raz pierwszy
Jeśli jest używany po raz pierwszy za pomocą tej funkcji można określić wydajność i rozmiar limitów miejsca do magazynowania nowego dysku. To doświadczenie jest podobna do co chcesz wyświetlać na czas inicjowania obsługi administracyjnej. Podstawowa różnica polega na tym, że nie masz uprawnień do określania typu obciążenie pracą. To ograniczenie zapobiega przerywania istniejących konfiguracji programu SQL Server na komputerze wirtualnych.

![Konfigurowanie programu SQL Server miejsca do magazynowania suwaki](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

Azure tworzy nowy dysk oparte na sieci. W tym scenariuszu Azure wykonuje następujące zadania konfiguracji miejsca do magazynowania:

- Tworzy i dołącza premium miejsca do magazynowania danych dysków maszyn wirtualnych.
- Konfiguruje dyski danych, aby umożliwić dostęp do programu SQL Server.
- Konfiguruje dyski danych do puli miejsca do magazynowania, zależnie od określonej wydajność i rozmiar (operacji i/o na SEKUNDĘ i przepustowość) wymagań.
- Przypisuje nowy dysk komputera wirtualnych puli miejsca do magazynowania.

Szczegółowe informacje o jak Azure skonfigurował ustawienia miejsca do magazynowania zobacz [sekcję konfiguracji miejsca do magazynowania](#storage-configuration).

### <a name="add-a-new-drive"></a>Dodawanie nowego dysku
Jeśli masz już skonfigurowany miejsca do magazynowania na maszyn wirtualnych usługi SQL Server, rozwijanie miejsca do magazynowania powoduje wyświetlenie dwie nowe opcje. Pierwszą opcję jest dodanie nowego dysku można zwiększyć poziom wydajności usługi maszyn wirtualnych.

![Dodawanie nowego dysku do maszyny SQL](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

Jednak po dodaniu dysku, należy wykonać kilka dodatkowych ręcznej konfiguracji uzyskanie zwiększenie wydajności.

### <a name="extend-the-drive"></a>Rozszerzanie dysku
Na inną opcję rozwijanie miejsca do magazynowania ma rozszerzenie istniejący dysk. Ta opcja zwiększa dostępnego magazynu dla dysku, ale nie zwiększa wydajność. Przy użyciu puli miejsca do magazynowania nie można zmienić liczbę kolumn, po utworzeniu puli miejsca do magazynowania. Liczba kolumn określa liczbę zapisów równoległe, które mogą być rozłożone na dysków danych. W związku z tym wszelkie dyski dodano danych nie można zwiększyć wydajność. Można tylko zapewniają więcej miejsca do magazynowania danych zapisywane. To ograniczenie również oznacza, że, w przypadku rozszerzania dysku, liczba kolumn określa minimalną liczbę dyski danych, które można dodać. Dlatego po utworzeniu puli miejsca do magazynowania z dyski cztery danych Liczba kolumn jest również cztery. Każdym razem, gdy rozszerzenie przestrzeni dyskowej, możesz dodać dyski co najmniej czterech danych.

![Rozszerzanie dysku dla maszyny SQL](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Konfiguracja magazynu
W tej sekcji znajdują się informacje na zmiany konfiguracji miejsca do magazynowania, które Azure wykonuje automatycznie podczas konfiguracji w Azure Portal lub SQL maszyn wirtualnych inicjowania obsługi administracyjnej.

- Po zaznaczeniu mniej niż dwa TBs miejsca do magazynowania dla swojego maszyn wirtualnych Azure nie tworzy puli miejsca do magazynowania.
- Jeśli wybrano co najmniej dwa TBs miejsca do magazynowania dla swojego maszyn wirtualnych, Azure konfiguruje puli miejsca do magazynowania. Następną sekcję w tym temacie zawiera szczegółowe informacje o konfiguracji puli miejsca do magazynowania.
- Zawsze Konfiguracja automatyczna magazynu używa [magazynu premium](../storage/storage-premium-storage.md) P30 dyski danych. W związku z tym jest mapowania 1:1 między numeru zaznaczonego terabajtów i liczba dysków danych dołączone do Twojej maszyn wirtualnych.

Aby uzyskać informacje o cenach, zobacz stronę [ceny miejsca do magazynowania](https://azure.microsoft.com/pricing/details/storage) na karcie **Ilość miejsca do magazynowania** .

### <a name="creation-of-the-storage-pool"></a>Tworzenie puli magazynu
Azure przybierać następujące ustawienia, aby utworzyć puli miejsca do magazynowania na maszyny wirtualne programu SQL Server.

| Ustawienie | Wartość |
|-----|-----|
| Pasek rozmiar  | 256 KB (magazynowanie danych); 64 KB (transakcji) |
| Rozmiary dysku | 1 TB |
| Pamięć podręczna | Odczyt |
| Rozmiar alokacji | Rozmiar jednostki alokacji NTFS 64 KB |
| Inicjowanie błyskawiczne pliku | Włączone |
| Blokowanie stron w pamięci | Włączone |
| Odzyskiwanie | Odzyskiwanie proste (nie elastyczność) |
| Liczba kolumn | Liczba dysków danych<sup>1</sup> |
| Tymczasowe lokalizacji | Przechowywane na dyskach danych<sup>2</sup> |

<sup>1</sup> po utworzeniu puli miejsca do magazynowania, nie można zmienić liczbę kolumn w puli miejsca do magazynowania.

<sup>2</sup> to ustawienie dotyczy tylko pierwszy dysk tworzonych za pomocą funkcji Konfiguracja miejsca do magazynowania.

## <a name="workload-optimization-settings"></a>Ustawienia optymalizacji obciążenie pracą
W poniższej tabeli opisano dostępne opcje typu pracą trzy i ich odpowiednich optymalizacje:

| Typ obciążenia pracą | Opis | Optymalizacja |
|-----|-----|-----|
| **Ogólne** | Ustawienie domyślne, które obsługuje większość obciążenia | Brak |
| **Przetwarzanie transakcji** | Optymalizuje magazyn obciążenia OLTP tradycyjnych bazy danych | Flaga śledzenia 1117<br/>Flaga śledzenia 1118 |
| **Magazynowanie danych** | Optymalizuje magazyn obciążenia analityczne i raportowania | Flaga śledzenia 610<br/>Flaga śledzenia 1117 |

>[AZURE.NOTE] Typ obciążenia pracą można określić tylko w przypadku, gdy obsługi administracyjnej maszyny wirtualnej SQL, zaznaczając go w kroku konfiguracji miejsca do magazynowania.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać innych tematów związanych z programem SQL Server w maszyny wirtualne Azure zobacz [Programu SQL Server na maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md).
