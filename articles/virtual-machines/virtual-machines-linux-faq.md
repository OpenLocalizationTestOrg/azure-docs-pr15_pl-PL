<properties
    pageTitle="Często zadawane pytania dotyczące maszyny wirtualne Linux | Microsoft Azure"
    description="Zawiera odpowiedzi na niektóre typowe pytania dotyczące maszyn wirtualnych Linux utworzone za pomocą modelu Menedżera zasobów."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Odpowiedzi na często zadawane pytania dotyczące maszyn wirtualnych Linux

Ten artykuł dotyczy często zadawane pytania dotyczące maszyn wirtualnych Linux utworzone w Azure przy użyciu modelu wdrożenia Menedżera zasobów. Dla wersji systemu Windows w tym temacie zobacz [często zadawane pytania dotyczące maszyn wirtualnych systemu Windows](virtual-machines-windows-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Co można uruchamiać na maszyn wirtualnych Azure?

Wszystkie subskrybentów można uruchamiać oprogramowanie serwera na Azure maszyn wirtualnych. Aby uzyskać więcej informacji zobacz [Linux na Azure-Endorsed dystrybucji](virtual-machines-linux-endorsed-distros.md)


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Ile miejsca do magazynowania można używać z maszyny wirtualnej?

Każdy dysk danych może być do 1 TB. Liczba dysków danych, których można używać zależy od rozmiaru maszyny wirtualnej. Aby uzyskać szczegółowe informacje zobacz [rozmiarów maszyn wirtualnych](virtual-machines-linux-sizes.md).

Konto Azure magazynowania zawiera miejsca do magazynowania dla dysku systemu operacyjnego i wszelkie dyski danych. Każdy dysk jest plikiem VHD przechowywane jako blob strony. Ceny szczegóły, zobacz [Szczegóły ceny miejsca do magazynowania](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Jak można uzyskać dostęp Moje maszyn wirtualnych?

Ustanowienia połączenia zdalnego, aby zalogować się do maszyny wirtualnej, przy użyciu Secure Shell (SSH). Zobacz instrukcje dotyczące nawiązywania połączenia [z systemem Windows](virtual-machines-linux-ssh-from-windows.md) lub [Linux i Mac](virtual-machines-linux-mac-create-ssh-keys.md). Domyślnie SSH pozwala maksymalnie 10 połączeń jednocześnie. Liczbę tę można zwiększyć, edytując plik konfiguracji.


Jeśli występują problemy, zapoznaj się z [połączenia Rozwiązywanie problemów z Secure Shell (SSH)](virtual-machines-linux-troubleshoot-ssh-connection.md).


## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Czy można używać dysku tymczasowym (-deweloperów-sdb1) do przechowywania danych?

Nie należy używać dysku tymczasowym (-deweloperów-sdb1) do przechowywania danych. Jest dostępna tylko do tymczasowego przechowywania danych. Istnieje ryzyko utraty danych, którego nie można odzyskać.


## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Czy skopiować lub klonowanie istniejących maszyn wirtualnych Azure?

Wartość Tak. Aby uzyskać instrukcje zobacz [jak utworzyć kopię Linux maszyny wirtualnej w modelu wdrożenia Menedżera zasobów](virtual-machines-linux-copy-vm.md).


## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Dlaczego nie widzę Kanada centralnej i regiony Wschód Kanada za pomocą Menedżera zasobów Azure?

Dwie nowe obszary Kanada Środkowej i Wschodniej Kanady nie są automatycznie zarejestrowane dla Tworzenie maszyn wirtualnych istniejącej subskrypcji Azure. Tej rejestracji odbywa się automatycznie po wdrożeniu maszyny wirtualnej za pośrednictwem portalu Azure do innego regionu, za pomocą Menedżera zasobów Azure. Po wdrożeniu maszyny wirtualnej do innego regionu Azure nowe obszary powinny być dostępne dla kolejnych maszyn wirtualnych.


## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Czy można dodać NIC do mojej maszyn wirtualnych po jego utworzeniu?

Wartość nie. Dodawanie karty Sieciowej można wykonać tylko w czasie tworzenia.


## <a name="are-there-any-computer-name-requirements"></a>Czy istnieją wymagań nazwę komputera?

Wartość Tak. Nazwa komputera może zawierać maksymalnie 64 znaki. Zobacz [wskazówki dotyczące nazewnictwa infrastruktury](virtual-machines-linux-infrastructure-naming-guidelines.md) uzyskać więcej informacji dotyczących nazw zasobów.


## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Jakie są wymagania nazwy użytkownika, podczas tworzenia maszyny?

Nazwy użytkownika musi być 1 do 64 znaków.

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

Hasła musi być 6-72 znaków i spełniają 3 poza następujące wymagania złożoności 4:

- Znaki dolnym
- Znaki górnym
- Masz cyfrę
- Masz znaków specjalnych (Regex odpowiadać [\W_])

Następujące hasła nie są dozwolone:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$ w programie word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Hasło!</td>
        <td style="text-align:center">Hasła1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
