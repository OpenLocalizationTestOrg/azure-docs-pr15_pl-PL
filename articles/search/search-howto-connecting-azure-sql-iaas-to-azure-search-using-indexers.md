<properties 
    pageTitle="Konfigurowanie połączenia z indeksowanie wyszukiwania Azure z programu SQL Server na Azure maszyn wirtualnych | Microsoft Azure | Indeksatory" 
    description="Włączanie połączenia szyfrowanego i konfigurowanie zapory do zezwalania na połączenia z programem SQL Server na Azure maszyn wirtualnych (maszyn wirtualnych) z indeksowanie wyszukiwania Azure." 
    services="search" 
    documentationCenter="" 
    authors="jack4it" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="09/26/2016" 
    ms.author="jackma"/>

# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-server-on-an-azure-vm"></a>Konfigurowanie połączenia z indeksowanie wyszukiwania Azure z programu SQL Server na maszyn wirtualnych Azure

Opisanymi w [Połączenie bazy danych SQL Azure do wyszukiwania Azure za pomocą indeksatory](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md#frequently-asked-questions)tworzenia indeksatory przed **Programu SQL Server na Azure maszyny wirtualne** (lub **Pośrednictwem SQL Azure SMS** dla krótkiej) jest obsługiwana przez wyszukiwanie Azure, ale istnieje kilka związane z zabezpieczeniami wymagania wstępne dotyczące przejmują pierwszego. 

**Zadania, czas trwania:** Około 30 minut, wykonując już zainstalowany certyfikat na maszyn wirtualnych.

## <a name="enable-encrypted-connections"></a>Włączanie połączenia szyfrowanego

Wyszukiwanie Azure wymaga zaszyfrowanego kanału dla wszystkich żądań indeksowania przez publiczne połączenie internetowe. W tej sekcji przedstawiono czynności, aby to działało.

1. Sprawdź właściwości certyfikatu, aby sprawdzić, czy nazwy podmiotu jest w pełni kwalifikowaną nazwę domeny (FQDN) programu maszyn wirtualnych Azure. Aby wyświetlić właściwości służy narzędzie, takie jak CertUtils lub przystawki Certyfikaty. Nazwy FQDN można przejść z sekcji Essentials karta usługi maszyn wirtualnych w polu **Etykieta nazwy publiczny adres IP adres DNS** w [Azure portal](https://portal.azure.com/).

    - Dla maszyny wirtualne utworzone przy użyciu nowszego szablonu **Menedżera zasobów** , nazwy FQDN sformatowaną jako `<your-VM-name>.<region>.cloudapp.azure.com`. 

    - Dla starszych maszyny wirtualne utworzone jako **Klasyczny** maszyn wirtualnych, nazwy FQDN sformatowaną jako `<your-cloud-service-name.cloudapp.net>`. 

2. Konfigurowanie programu SQL Server przy użyciu certyfikatu przy użyciu Edytora rejestru (regedit). 

    Mimo że Menedżer konfiguracji programu SQL Server jest często używany do tego zadania, nie można jej używać w tym scenariuszu. Nie można go znaleźć zaimportowanego certyfikatu, ponieważ Kwalifikowaną maszyn wirtualnych Azure nie są zgodne z nazwy FQDN określone przez maszyn wirtualnych (identyfikuje domenę jako komputera lokalnego lub domeny sieci, do której jest podłączony). Gdy nazwy nie są zgodne, umożliwia określenie certyfikat regedit.

    - W regedit, przejdź do klucza rejestru: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
    `[MSSQL13.MSSQLSERVER]` Część zależy od wersji i nazwę wystąpienia. 

    - Ustaw wartość klucza **certyfikatu** **odcisku palca** certyfikatu SSL, które zostały zaimportowane do maszyn wirtualnych.

    Istnieje kilka sposobów uzyskiwania odcisku palca, niektóre lepiej niż inne. Jeśli skopiujesz z przystawki **Certyfikaty** w konsoli MMC można prawdopodobnie wpłynie na niewidoczne prowadzące znaków [zgodnie z opisem w tym artykule pomocy technicznej](https://support.microsoft.com/kb/2023869/), co powoduje błąd podczas próby połączenia. Istnieje kilka rozwiązań dla rozwiązania tego problemu. Najłatwiej jest backspace na, a następnie ponownie wpisz pierwszy znak odcisku palca, aby usunąć znak wiodący w polu wartość klucza regedit. Za pomocą innego narzędzia można także skopiować odcisku palca.

3. Udzielanie uprawnień do konta usługi. 

    Upewnij się, że konto usługi programu SQL Server jest udzielone odpowiednie uprawnienia na klucz prywatny certyfikatu SSL. Jeśli natłoku ten krok, SQL Server nie zostanie uruchomiona. Za pomocą przystawki **Certyfikaty** lub **CertUtils** dla tego zadania.

4. Ponowne uruchamianie usługi SQL Server.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>Konfigurowanie połączenia programu SQL Server w maszyn wirtualnych

Po skonfigurowaniu połączenia szyfrowanego wymagane przez funkcję wyszukiwania Azure istnieją dodatkowe czynności konfiguracyjne wewnętrznych do programu SQL Server na maszyny wirtualne Azure. Jeśli nie zrobiono tego wcześniej, następnym krokiem jest zakończenie konfiguracji przy użyciu jednego z następujących artykułów:

- **Menedżer zasobów** maszyn wirtualnych zobacz [Łączenie do programu SQL Server maszyny wirtualnej Azure za pomocą Menedżera zasobów](../virtual-machines/virtual-machines-windows-sql-connect.md). 

- Aby **Klasyczny** maszyn wirtualnych zobacz [Łączenie do programu SQL Server maszyn wirtualnych w klasycznym Azure](../virtual-machines/virtual-machines-windows-classic-sql-connect.md).

W szczególności zapoznaj się z sekcją w każdym artykułu "nawiązywania połączenia przez internet".

## <a name="configure-the-network-security-group-nsg"></a>Konfigurowanie grupy zabezpieczeń sieci (NSG)

Nie jest nietypowe skonfigurowanie NSG i odpowiadające im Azure punktu końcowego lub listy kontroli dostępu (ACL) w celu ułatwienia dostępu na poziomie innym stronom maszyn wirtualnych usługi Azure. Prawdopodobnie oznacza to, że zostały zrobione tego wcześniej umożliwia własne logiki aplikacji nawiązać połączenie maszyn wirtualnych usługi SQL Azure. Jest nie różni się połączenia wyszukiwania Azure maszyn wirtualnych usługi SQL Azure. 

Łącza poniżej podano instrukcje na konfiguracji NSG w przypadku wdrożeń maszyn wirtualnych. Wykonaj te instrukcje do list ACL punktu końcowego Azure wyszukiwanie na podstawie jego adresu IP.

> [AZURE.NOTE] Zobacz [Co to jest grupa zabezpieczeń sieci?](../virtual-network/virtual-networks-nsg.md)

- **Menedżer zasobów** maszyn wirtualnych zobacz [Tworzenie NSGs w przypadku wdrożeń ARM](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

- Dla **Klasyczny** maszyn wirtualnych zobacz [Tworzenie NSGs w przypadku wdrożeń klasyczny](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

Adresy IP mogą stanowić kilka problemów, które są łatwe rozwiązać, jeśli wiadomo, problem i potencjalnymi rozwiązaniami. Zalecenia dotyczące obsługi problemy związane z adresów IP w tej listy można znaleźć w pozostałych sekcjach.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>Ograniczanie dostępu do adresu IP usługi wyszukiwania

Zdecydowanie zaleca się ograniczyć dostęp do adresu IP usługi wyszukiwania w ACL zamiast ustawiania maszyny wirtualne programu SQL Azure otwartej do żadnych żądań połączeń. Można łatwo sprawdzić adres IP przez polecenie ping nazwy FQDN (na przykład `<your-search-service-name>.search.windows.net`) usługi wyszukiwania.

#### <a name="managing-ip-address-fluctuations"></a>Zarządzanie zmianami adres IP

Jeśli usługi wyszukiwania zawiera tylko jedną jednostkę wyszukiwania (czyli jednej replice i jedną partycją), adres IP ulegną zmianie podczas ponownego uruchamiania usługi rutynową, unieważnieniu istniejących list ACL z adresem IP usługi wyszukiwania.

Jednym ze sposobów uniknięcia błędów kolejnych połączeń jest używana więcej niż jednej replice i jedną partycją podczas wyszukiwania Azure. Ten sposób zwiększa koszt, ale również rozwiązuje ten problem adres IP. W polu Wyszukaj Azure adresów IP nie zmienia się masz więcej niż jedną jednostkę wyszukiwania.

Druga metoda jest zezwala na połączenie zakończyć się niepowodzeniem, a następnie ponownie skonfigurować ACL w NSG. Średnio można się spodziewać adresy IP, aby zmienić co kilka tygodni. Dla klientów, którzy wykonaj indeksowanie kontroli na podstawie rzadko takie rozwiązanie może być rentowny.

Trzecia podejście rentowny (ale nie szczególnie bezpiecznego) jest Określ zakres adresów IP Azure region, w którym jest obsługi administracyjnej usługi wyszukiwania. Lista zakresów adresów IP, z których publiczne adresy IP są przydzielane Azure zasobów jest opublikowany w [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-the-azure-search-portal-ip-addresses"></a>Zawierać adresy IP portal Azure wyszukiwania

Jeśli tworzenie indeksatora za pomocą portalu Azure logiczny portal Azure wyszukiwania również musi mieć dostęp do maszyn wirtualnych usługi SQL Azure podczas tworzenia. Adresy IP portal Azure wyszukiwania znajduje się przez polecenie ping `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Następne kroki

Z konfiguracją przeszkadza możesz teraz określić programu SQL Server na maszyn wirtualnych Azure jako źródła danych dla indeksowanie wyszukiwania Azure. Aby uzyskać więcej informacji, zobacz [Łączenie bazy danych SQL Azure do wyszukiwania Azure za pomocą indeksatory](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md) .
