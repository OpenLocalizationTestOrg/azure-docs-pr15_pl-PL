<properties
    pageTitle="Często zadawane pytania dotyczące systemu Windows maszyny wirtualne | Microsoft Azure"
    description="Zawiera odpowiedzi na niektóre typowe pytania dotyczące maszyn wirtualnych systemu Windows utworzone za pomocą modelu Menedżera zasobów."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Często zadawane pytania dotyczące maszyn wirtualnych systemu Windows 


Ten artykuł dotyczy często zadawane pytania dotyczące maszyn wirtualnych systemu Windows utworzone w Azure przy użyciu modelu wdrożenia Menedżera zasobów. Wersję Linux w tym temacie zobacz [często zadawane pytania dotyczące maszyn wirtualnych Linux](virtual-machines-linux-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Co można uruchamiać na maszyn wirtualnych Azure?

Wszystkie subskrybentów można uruchamiać oprogramowanie serwera na Azure maszyn wirtualnych. Aby informacji na temat zasad świadczenia pomocy technicznej dla w Azure uruchomione oprogramowanie serwera firmy Microsoft zobacz [pomocy technicznej dla maszyn wirtualnych Azure oprogramowania serwera firmy Microsoft](https://support.microsoft.com/kb/2721672)

Niektóre wersje systemu Windows 7 i Windows 8.1 są dostępne dla subskrybentów korzyści MSDN Azure i deweloperów MSDN i przetestować repartycyjny subskrybentów, dla zadań projektowania i testowania. Aby uzyskać szczegółowe informacje, łącznie z instrukcjami i informacje o ograniczeniach zobacz [obrazów klienta systemu Windows dla subskrybentów MSDN](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/). 


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Ile miejsca do magazynowania można używać z maszyny wirtualnej?

Każdy dysk danych może być do 1 TB. Liczba dysków danych, których można używać zależy od rozmiaru maszyny wirtualnej. Aby uzyskać szczegółowe informacje zobacz [rozmiarów maszyn wirtualnych](virtual-machines-windows-sizes.md).

Konto Azure magazynowania zawiera miejsca do magazynowania dla dysku systemu operacyjnego i wszelkie dyski danych. Każdy dysk jest plikiem VHD przechowywane jako blob strony. Ceny szczegóły, zobacz [Szczegóły ceny miejsca do magazynowania](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Jak można uzyskać dostęp Moje maszyn wirtualnych?

Ustanowienia połączenia zdalnego przy użyciu połączenia pulpitu zdalnego (RDP) dla maszyn wirtualnych systemu Windows. Aby uzyskać instrukcje zobacz [jak połączyć się i zalogować się do Azure maszyn wirtualnych z systemem Windows](virtual-machines-windows-connect-logon.md). Maksymalnie dwa równoczesne połączenia są obsługiwane, chyba że serwer jest skonfigurowany jako hosta sesji usług pulpitu zdalnego.  


Jeśli masz problemy z pulpitu zdalnego, zobacz [Rozwiązywanie problemów z pulpitu zdalnego połączenia do systemu Windows Azure maszyny wirtualnej](virtual-machines-windows-troubleshoot-rdp-connection.md). 

Jeśli znasz funkcji Hyper-V, być może szukasz podobne do VMConnect narzędzia. Azure nie oferuje podobne narzędzie, ponieważ dostęp do maszyny wirtualnej nie jest obsługiwana.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Czy można używać dysku tymczasowym (dysk D: domyślnie) do przechowywania danych?

Nie należy używać dysku tymczasowym do przechowywania danych. Jest tylko tymczasowe miejscem do magazynowania, tak samo ryzyko utraty danych, którego nie można odzyskać. Gdy maszyna wirtualna zostanie przeniesiony do innego hosta, mogą zostać utracone dane. Zmienianie rozmiaru maszyny wirtualnej, aktualizowanie hoście lub awarię sprzętu na hoście przedstawiono kilka przyczyn, dla których może przenieść maszyny wirtualnej.

Jeśli masz aplikację, która musi być litera D:, należy ponownie przypisać litery dysków tak, aby dysku tymczasowym używa innej niż D:. Aby uzyskać instrukcje zobacz [Zmienianie literę dysku tymczasowym systemu Windows](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Jak można zmienić literę dysku tymczasowym?

Możesz zmienić literę przenosząc plik strony i ponowne przypisywanie liter dysków, ale będzie to konieczne, wykonaj kroki opisane w określonej kolejności. Aby uzyskać instrukcje zobacz [Zmienianie literę dysku tymczasowym systemu Windows](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Czy można dodać do zestawu dostępności istniejących maszyn wirtualnych?

Wartość nie. Usługi maszyn wirtualnych jako część zestawu dostępność, musisz utworzyć maszyn wirtualnych w zestawie. Obecnie nie ma umożliwia dodawanie maszyny dostępności po jego utworzeniu.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Czy mogę przekazać maszyny wirtualnej Azure?

Wartość Tak. Aby uzyskać instrukcje zobacz [przekazywanie obraz maszyn wirtualnych systemu Windows Azure](virtual-machines-windows-upload-image.md)

## <a name="can-i-resize-the-os-disk"></a>Czy mogę zmienić rozmiar dysku systemu operacyjnego?

Wartość Tak. Aby uzyskać instrukcje zobacz [jak rozwiń dysk systemu operacyjnego maszyny wirtualnej w grupie zasobów Azure](virtual-machines-windows-expand-os-disk.md).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Czy skopiować lub klonowanie istniejących maszyn wirtualnych Azure?

Wartość Tak. Aby uzyskać instrukcje zobacz [jak utworzyć kopię maszyny wirtualnej systemu Windows w modelu wdrożenia Menedżera zasobów](virtual-machines-windows-vhd-copy.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Dlaczego nie widzę Kanada centralnej i regiony Wschód Kanada za pomocą Menedżera zasobów Azure?

Dwie nowe obszary Kanada Środkowej i Wschodniej Kanady nie są automatycznie zarejestrowane dla Tworzenie maszyn wirtualnych istniejącej subskrypcji Azure. Tej rejestracji odbywa się automatycznie po wdrożeniu maszyny wirtualnej za pośrednictwem portalu Azure do innego regionu, za pomocą Menedżera zasobów Azure. Po wdrożeniu maszyny wirtualnej do innego regionu Azure nowe obszary powinny być dostępne dla kolejnych maszyn wirtualnych.

## <a name="does-azure-support-linux-vms"></a>Azure obsługuje pośrednictwem Linux SMS?

Wartość Tak. Aby szybko utworzyć maszyny Linux wypróbować działanie, zobacz [Tworzenie maszyny Linux Azure za pomocą portalu](virtual-machines-linux-quick-create-portal.md).

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Czy można dodać NIC do mojej maszyn wirtualnych po jego utworzeniu?

Wartość nie. Dodawanie karty Sieciowej można wykonać tylko w czasie tworzenia.

## <a name="are-there-any-computer-name-requirements"></a>Czy istnieją wymagań nazwę komputera?

Wartość Tak. Nazwa komputera może zawierać maksymalnie 15 znaków. Zobacz [wskazówki dotyczące nazewnictwa infrastruktury](virtual-machines-windows-infrastructure-naming-guidelines.md) uzyskać więcej informacji dotyczących nazw zasobów.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Jakie są wymagania nazwy użytkownika, podczas tworzenia maszyny?

Nazwy użytkowników może zawierać maksymalnie 20 znaków i nie może kończyć się kropką ("."). 

Następujące nazwy użytkowników, nie są dozwolone:

<table>
    <tr>
        <td style="text-align:center">Administrator </td><td style="text-align:center"> Administrator </td><td style="text-align:center"> użytkownika </td><td style="text-align:center"> Użytkownik 1</td>
    </tr>
    <tr>
        <td style="text-align:center">Test </td><td style="text-align:center"> UŻYTKOWNIK2 </td><td style="text-align:center"> Test1 </td><td style="text-align:center"> UŻYTKOWNIK3</td>
    </tr>
    <tr>
        <td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> do</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> adm </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> ASPNET</td>
    </tr>
    <tr>
        <td style="text-align:center">wykonywanie kopii zapasowych </td><td style="text-align:center"> konsoli </td><td style="text-align:center"> David </td><td style="text-align:center"> gościa</td>
    </tr>
    <tr>
        <td style="text-align:center">Jan </td><td style="text-align:center"> Właściciel </td><td style="text-align:center"> główny </td><td style="text-align:center"> Serwer</td>
    </tr>
    <tr>
        <td style="text-align:center">SQL </td><td style="text-align:center"> Pomoc techniczna </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> dotyczącego</td>
    </tr>
    <tr>
        <td style="text-align:center">Test2 </td><td style="text-align:center"> test3 </td><td style="text-align:center"> user4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>

## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Jakie są wymagania dotyczące haseł, podczas tworzenia maszyny?

Hasła musi być 8 123 znaków i spełniają 3 poza następujące wymagania złożoności 4:

- Znaki dolnym
- Znaki górnym
- Masz cyfrę
- Masz znaków specjalnych (Regex odpowiadać [\W_])

Następujące hasła nie są dozwolone:

<table>
    <tr>
        <td style="text-align:center">abc@123</td><td style="text-align:center">P@$$w0rd</td><td style="text-align:center">P@ssw0rd</td><td style="text-align:center">P@ssword123</td><td style="text-align:center">Pa$ w programie word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td><td style="text-align:center">Hasło!</td><td style="text-align:center">Hasła1</td><td style="text-align:center">Password22</td><td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
