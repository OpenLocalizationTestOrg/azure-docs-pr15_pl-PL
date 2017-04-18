<properties
    pageTitle="Omówienie maszyn wirtualnych systemu Windows | Microsoft Azure"
    description="Informacje na temat tworzenia i zarządzania nimi maszyn wirtualnych systemu Windows w Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="davidmu"/>

# <a name="overview-of-windows-virtual-machines-in-azure"></a>Omówienie maszyn wirtualnych systemu Windows platformy Azure

Azure maszyn wirtualnych (maszyn wirtualnych) jest jednym z wielu typów [na żądanie, skalowalna zasobów komputerowych](../app-service-web/choose-web-site-cloud-service-vm.md) oferująca Azure. Zazwyczaj możesz wybrać maszyny, jeśli chcesz mieć większą kontrolę nad środowiska komputerowego niż oferują inne opcje. Ten artykuł zawiera informacje o czym należy pamiętać przed utworzeniem maszyny, jak utworzyć i jak zarządzać.

Maszyn wirtualnych Azure umożliwia elastyczne wirtualizacji bez konieczności zakupu i obsługa sprzętu uruchamianej go. Nadal potrzebny zachować maszyn wirtualnych przez wykonywanie zadań, takich jak konfigurowanie, poprawianie i instalowanie oprogramowania, uruchamianej na nim.

Azure maszyn wirtualnych można używać na różne sposoby. Oto kilka przykładów są:

- **Projektowania i testowania** — oferty Azure maszyny wirtualne w szybki i łatwy sposób na tworzenie komputera przy użyciu konfiguracji wymaganych do kodu i przetestować aplikację.
- **Aplikacje w chmurze** — ponieważ żądanie aplikacji mogą się zmieniać, może być zrozumiałe ekonomicznych być uruchamiany maszyn wirtualnych w Azure. Zapłacić za pośrednictwem dodatkowe SMS i po ich potrzebuje i zamknięcie ich nie zostanie.
- **Rozszerzone centrum danych** — maszyn wirtualnych w Azure wirtualną sieć łatwo można łączyć do sieci swojej organizacji.

Liczba maszyny wirtualne przez aplikację można skalować w górę i się niezależnie od jest wymagane do własnych potrzeb.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>Co trzeba wziąć pod uwagę przed utworzeniem maszyny?

Istnieje zawsze do bardzo dużej liczby [zagadnienia projektowe](virtual-machines-windows-infrastructure-virtual-machine-guidelines.md) , podczas tworzenia infrastruktury aplikacji platformy Azure. Te aspekty maszyny są należy wziąć pod uwagę przed rozpoczęciem pracy:

- Nazwy zasobów aplikacji
- W lokalizacji przechowywania zasobów
- Rozmiar maszyn wirtualnych
- Maksymalna liczba maszyny wirtualne, które można utworzyć
- System operacyjny, który jest uruchamiany maszyn wirtualnych
- Konfiguracja maszyn wirtualnych po uruchomieniu
- Zasoby pokrewne, których potrzebuje maszyn wirtualnych

### <a name="naming"></a>Nadawanie nazw

Maszyna wirtualna ma [nazwę](virtual-machines-windows-infrastructure-naming-guidelines.md) do niego przypisana i ma nazwę komputera skonfigurowanego jako część systemu operacyjnego. Nazwa maszyny może zawierać maksymalnie 15 znaków.

Jeśli Azure umożliwia tworzenie dysku system operacyjny, nazwę komputera i nazwę maszyn wirtualnych są takie same. Jeśli możesz [przekazać i używanie własnego obrazu](virtual-machines-windows-upload-image.md) zawierającej wcześniej skonfigurowane systemu operacyjnego i za jej pomocą tworzyć maszyny wirtualnej, nazwy może być inne. Zaleca się podczas przekazywania pliku obrazu dokonaj nazwy komputera w systemie operacyjnym i maszyny wirtualnej nazwę tak samo.

### <a name="locations"></a>Lokalizacje

Wszystkie zasoby utworzone w Azure są rozmieszczane wielu [regionach geograficznych](https://azure.microsoft.com/regions/) na świecie. Zazwyczaj regionu jest określana mianem **lokalizacji** podczas tworzenia maszyn wirtualnych. W przypadku Głosowa lokalizację określa, gdzie są przechowywane wirtualnych dyskach twardych.

W poniższej tabeli przedstawiono niektóre ze sposobów zapewniających zostanie wyświetlona lista dostępnych lokalizacji.

| Metoda | Opis |
| ------ | ----------- |
| Azure portal | Wybierz lokalizację z listy podczas tworzenia maszyn wirtualnych. |
| Azure programu PowerShell | Użyj polecenia [Get-AzureRmLocation](https://msdn.microsoft.com/library/mt619449.aspx) . |
| INTERFEJSU API USŁUGI REST | Użyj operacji [listy lokalizacji](https://msdn.microsoft.com/library/dn790540.aspx) . |

### <a name="vm-size"></a>Rozmiar pamięci Wirtualnej

[Rozmiar](virtual-machines-windows-sizes.md) maszyn wirtualnych, którego używasz, jest określona przez obciążenie pracą, który chcesz uruchomić. Rozmiar, którą należy wybrać następnie określa czynników, takich jak przetwarzania power, pamięci i miejsca do magazynowania. Azure oferuje szeroką gamę rozmiarów do obsługi wielu typów zastosowania.

Azure opłat [Cena za godzinę](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) na podstawie rozmiaru i system operacyjny maszyn. Częściowy godzin Azure opłat tylko w przypadku minut używane. Magazyn jest po cenach i naliczany oddzielnie.

### <a name="vm-limits"></a>Limity maszyn wirtualnych

Subskrypcja obejmuje domyślne [limitów przydziału](../azure-subscription-service-limits.md) w miejsce, w którym mogą mieć wpływ wdrażania wielu maszyny wirtualne projektu. Limit bieżącego na podstawie subskrypcji na to 20 maszyny wirtualne w rozbiciu na regiony. Limity można powstałe przechowywania żąda wzrost bilet pomocy technicznej.

### <a name="operating-system-disks-and-images"></a>System operacyjny dysków i obrazów

Maszyn wirtualnych umożliwia przechowywanie ich systemu operacyjnego (OS) i dane [wirtualnych dyskach twardych (VHD)](virtual-machines-windows-about-disks-vhds.md) . Wirtualnych dysków twardych są też używane dla obrazów, które możesz wybrać do zainstalowania systemu operacyjnego. 

Azure udostępnia wiele [obrazów marketplace](https://azure.microsoft.com/marketplace/virtual-machines/) korzystania z różnych wersji i typów systemy operacyjne Windows Server. Obrazy Marketplace są oznaczane obrazu w programie publisher, oferty sku i wersji (zwykle wersji jest określona jako r). 

W poniższej tabeli opisano niektóre metody znajdziesz informacje dotyczące obrazu.

| Metoda | Opis |
| ------ | ----------- |
| Azure portal | Automatycznie określono wartości dla Ciebie, po wybraniu obraz używany. |
| Azure programu PowerShell | [Get AzureRMVMImagePublisher](https://msdn.microsoft.com/library/mt603484.aspx) -lokalizacji "położenie"<BR>[Get AzureRMVMImageOffer](https://msdn.microsoft.com/library/mt603824.aspx) -lokalizacji "położenie"-"publisherName" w programie Publisher<BR>[Get AzureRMVMImageSku](https://msdn.microsoft.com/library/mt619458.aspx) -lokalizacji "położenie"-"publisherName" w programie Publisher — oferty "offerName" |
| Interfejsy API REST | [Lista obraz wydawcy](https://msdn.microsoft.com/library/mt743702.aspx)<BR>[Lista ofert obrazu](https://msdn.microsoft.com/library/mt743700.aspx)<BR>[Lista obraz SKU](https://msdn.microsoft.com/library/mt743701.aspx) |

Użytkownik może [przekazać i używanie własnego obrazu](virtual-machines-windows-upload-image.md) , a po wykonaniu nazwę wydawcy, oferty i sku nie są używane.

### <a name="extensions"></a>Rozszerzenia

Maszyn wirtualnych [rozszerzenia](virtual-machines-windows-extensions-features.md) zapewniają dodatkowe możliwości maszyn wirtualnych za pośrednictwem konfiguracji wdrażania wpis i zadaniach.

Te typowe zadania można wykonywać przy użyciu rozszerzeń:

- **Uruchamianie skryptów niestandardowych** — [Niestandardowego rozszerzenia skrypt](virtual-machines-windows-extensions-customscript.md) ułatwia konfigurowanie obciążenia na maszyn wirtualnych, uruchamiając skrypt, gdy zainicjowano obsługę administracyjną maszyn wirtualnych.
- **Rozmieszczanie i zarządzanie nimi konfiguracji** — [rozszerzenie programu PowerShell potrzeby stan konfiguracji (DSC)](virtual-machines-windows-extensions-dsc-overview.md) ułatwia konfigurowanie DSC na maszyny do zarządzania konfiguracji i środowiska.
- **Zbieranie danych diagnostyki** — [Rozszerzenia diagnostyki Azure](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) ułatwia konfigurowanie maszyn wirtualnych zbieranie danych diagnostyki, który może być używany do monitorowania kondycji aplikacji.

### <a name="related-resources"></a>Zasoby pokrewne

Zasoby w tej tabeli są używane przez maszyn wirtualnych i muszą istnieć lub można utworzyć po utworzeniu maszyn wirtualnych.

| Zasób | Wymagane | Opis |
| -------- | -------- | ----------- |
| [Grupa zasobów](../azure-resource-manager/resource-group-overview.md) | Tak | Maszyn wirtualnych muszą znajdować się w grupie zasobów. |
| [Konto miejsca do magazynowania](../storage/storage-create-storage-account.md) | Tak | Maszyn wirtualnych wymaga konta miejsca do magazynowania do przechowywania jej wirtualnych dyskach twardych. |
| [Wirtualna sieć](../virtual-network/virtual-networks-overview.md) | Tak | Maszyn wirtualnych musi należeć do wirtualnej sieci. |
| [Publiczny adres IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) | Brak | Maszyn wirtualnych może mieć publiczny adres IP do niego przypisana zdalnego dostępu do niego. |
| [Interfejs sieciowy](../virtual-network/virtual-network-network-interface-overview.md) | Tak | Maszyn wirtualnych musi sieciowej można komunikować się w sieci. |
| [Dyski danych](virtual-machines-windows-attach-disk-portal.md) | Brak | Maszyn wirtualnych może zawierać dyski danych, aby rozwinąć możliwości przechowywania danych. |

## <a name="how-do-i-create-my-first-vm"></a>Jak utworzyć Mój pierwszy maszyn wirtualnych?

Masz kilka możliwości tworzenia usługi maszyn wirtualnych. Wybór, jakie mają być zależy od środowiska, którego używasz. 

W poniższej tabeli podano informacje ułatwiające rozpoczęcie tworzenia usługi maszyn wirtualnych.

| Metoda | Artykuł |
| ------ | ------- |
| Azure portal | [Tworzenie wirtualnych komputera z systemem Windows za pomocą portalu](virtual-machines-windows-hero-tutorial.md) |
| Szablony | [Tworzenie maszyny wirtualnej systemu Windows za pomocą szablonu Menedżera zasobów](virtual-machines-windows-ps-template.md) |
| Azure programu PowerShell | [Tworzenie maszyn wirtualnych systemu Windows przy użyciu programu PowerShell](virtual-machines-windows-ps-create.md) |
| SDK klienta | [Wdrażanie zasobów Azure za pomocą C#](virtual-machines-windows-csharp.md) |
| Interfejsy API REST | [Tworzenie lub aktualizowanie maszyny](https://msdn.microsoft.com/library/mt163591.aspx) |

Cele tak się nie dzieje, ale czasami wystąpią problemy. Jeśli sytuacja się nie dzieje użytkownikowi, spójrz na informacje znajdujące się w [problemy wdrożenia Rozwiązywanie problemów z Menedżera zasobów do tworzenia maszyny wirtualnej systemu Windows Azure](virtual-machines-windows-troubleshoot-deployment-new-vm.md).

## <a name="how-do-i-manage-the-vm-that-i-created"></a>Jak zarządzać maszyn wirtualnych utworzonym przeze mnie?

Maszyny wirtualne można zarządzać za pomocą portalu oparte na przeglądarce, narzędzia wiersza polecenia z obsługą podczas tworzenia skryptów lub bezpośrednio za pośrednictwem interfejsów API. Niektóre zadania związane z zarządzaniem typowe, które można wykonać są uzyskiwanie informacji na temat Głosowa, logowanie się do Głosowa, zarządzanie dostępność i tworzenia kopii zapasowych.

### <a name="get-information-about-a-vm"></a>Uzyskiwanie informacji na temat maszyny

W poniższej tabeli przedstawiono niektóre metody, który można uzyskać informacje o maszyny.

| Metoda | Opis |
| ------ | ----------- |
| Azure portal | W menu Centrum kliknij **maszyn wirtualnych** , a następnie wybierz maszyn wirtualnych z listy. Na karta dla maszyn wirtualnych masz dostęp do informacji, ustawiania wartości i monitorowanie metryki. |
| Azure programu PowerShell | Aby dowiedzieć się, jak zarządzać maszyny wirtualne przy użyciu programu PowerShell zobacz [Zarządzanie maszyn wirtualnych Azure za pomocą Menedżera zasobów i programu PowerShell](virtual-machines-windows-ps-manage.md). |
| INTERFEJSU API USŁUGI REST | Aby uzyskać informacje o maszyn wirtualnych przy użyciu operacji [maszyn wirtualnych uzyskiwanie informacji](https://msdn.microsoft.com/library/mt163682.aspx) . |
| SDK klienta | Aby dowiedzieć się, jak zarządzanie za pomocą C# maszyny wirtualne Zobacz [Zarządzanie maszyn wirtualnych Azure za pomocą Menedżera zasobów Azure i C#](virtual-machines-windows-csharp-manage.md). |

### <a name="log-on-to-the-vm"></a>Zaloguj się do maszyn wirtualnych

Przycisk Połącz w portalu Azure, aby [rozpocząć sesję pulpitu zdalnego (RDP)](virtual-machines-windows-connect-logon.md). Może czasami wystąpić problem podczas próby połączenia zdalnego. Jeśli sytuacja się nie dzieje użytkownikowi, zapoznaj się z informacjami pomocy, w [oknie połączenia Rozwiązywanie problemów z pulpitu zdalnego Azure maszyn wirtualnych z systemem Windows](virtual-machines-windows-troubleshoot-rdp-connection.md).

### <a name="manage-availability"></a>Zarządzanie dostępności

Jest ważne dowiedzieć się, jak [zapewnić wysoką dostępność](virtual-machines-windows-manage-availability.md) aplikacji. Ta konfiguracja wymaga tworzenia wielu maszyny wirtualne, aby upewnić się, że działa co najmniej jeden.

Aby wdrożenia kwalifikować się do naszych 99.95 maszyn wirtualnych umowy o poziomie usług należy zainstalować dwie lub więcej maszyny wirtualne działa z pracą wewnątrz [Ustawianie dostępności](virtual-machines-windows-infrastructure-availability-sets-guidelines.md). Ta konfiguracja zapewnia pośrednictwem usługi SMS są rozmieszczane wiele domen błędów i są rozmieszczane hosts z różnych konserwacja systemu windows. Pełna [Umowa dotycząca poziomu usług Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) wyjaśniono gwarantowanej dostępność Azure jako całość.
 
### <a name="back-up-the-vm"></a>Tworzenie kopii zapasowych maszyn wirtualnych
 
[Usługi odzyskiwania magazynu](../backup/backup-introduction-to-azure-backup.md) jest używany do ochrony danych oraz majątku w usługach zarówno Azure i przywracania kopii zapasowych Azure witryny. Możesz użyć magazynu usługi odzyskiwania, [wdrażania](../backup/backup-azure-vms-automation.md)i zarządzać nimi kopii zapasowych maszyny wirtualne wdrożony Menedżera zasobów, przy użyciu programu PowerShell. 

## <a name="next-steps"></a>Następne kroki

- W przypadku usługi przeznaczenie do pracy z maszyny wirtualne Linux, spójrz na [Azure i Linux](virtual-machines-linux-azure-overview.md).
- Dowiedz się więcej o wskazówkach wokół konfigurowania infrastruktury [Instruktaż infrastruktury przykład Azure](virtual-machines-windows-infrastructure-example.md).
- Upewnij się, że obserwować [Najważniejsze wskazówki dotyczące uruchamiania maszyn wirtualnych systemu Windows Azure](virtual-machines-windows-guidance-compute-single-vm.md).


