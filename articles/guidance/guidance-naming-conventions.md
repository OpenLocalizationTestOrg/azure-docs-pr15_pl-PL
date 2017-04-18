<properties
   pageTitle="Zalecane konwencje nazewnictwa dla zasobów Azure | Wskazówki dotyczące | Microsoft Azure"
   description="Zalecane konwencje nazewnictwa Azure zasobów. Jak nadać nazwę maszyn wirtualnych, kont miejsca do magazynowania, sieci, wirtualnych sieci, podsieci i innych obiektów Azure"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/27/2016"
   ms.author="christb"/>
   
# <a name="recommended-naming-conventions-for-azure-resources"></a>Zalecane konwencje nazewnictwa dla zasobów Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Ważne jest wybór nazwy dla wszystkich zasobów w programie Microsoft Azure, ponieważ:

- Trudno później zmienić nazwę.
- Nazwy musi spełniać wymagania ich określonego typu zasobu.

Spójne konwencje nazewnictwa ułatwiają zasobów Znajdź. Można również określić roli zasobu w rozwiązaniu.

Ten artykuł dotyczy podsumowanie reguł nazewnictwa i ograniczeń zasobów Azure i zestaw według planu bazowego zalecenia dotyczące konwencje nazewnictwa.  Tych zaleceń można użyć jako punktu początkowego dla własnego konwencje określonych do własnych potrzeb.  

Klucz do sukcesu z konwencji nazewnictwa jest ustanawianie i po ich przez aplikacje i organizacjach. 

## <a name="naming-subscriptions"></a>Nadawanie nazw subskrypcji

Określając Azure subskrypcje, pełne nazwy wprowadź opis kontekstu i przeznaczenie każdej subskrypcji Wyczyść.  Podczas pracy w środowisku z wiele subskrypcji, obserwowanie udostępnionego konwencji nazewnictwa poprawić przejrzystość.

Jest zalecane deseń nazewnictwa subskrypcji:

`<Company> <Department (optional)> <Product Line (optional)> <Environment>`

- Firmy, zwykle będzie taka sama dla każdej subskrypcji. W niektórych firmach usunięcie może mieć jednak firmy podrzędnego w strukturze organizacji. Te firmy mogą być zarządzane przez centralną grupę IT. W takich przypadkach może być rozróżniane za o nazwa firmy nadrzędnej (*Contoso*) i nazwa firmy podrzędny (*Wiatru Północnej*).

- Dział to nazwa organizacji, w której pracować grupa osób. Ten element w obszarze nazw jako opcjonalne.

- Wiersz produktu jest specyficzna nazwa produktu lub funkcji, która jest wykonywane z poziomu działu.
Jest to zazwyczaj opcjonalne dla usług i aplikacji wewnętrznej stronie. Jednak zaleca służących do publicznych usług, które wymagają łatwe rozdzielanie i identyfikowanie (na przykład dla jasnym faktur rekordy).

- Środowisko jest nazwę opisującą cyklu wdrażania aplikacji i usług, takich jak deweloperów, pytania i odpowiedzi — lub zlecenie.

| Firma | Dział | Linia produktu lub usługi | Środowisko | Imię i nazwisko  |
----------| ---------- | ----------------------- | ----------- | ---------- |
| Contoso | SocialGaming | AwesomeService | Produkcji | Contoso SocialGaming AwesomeService produkcji |
| Contoso | SocialGaming | AwesomeService | Deweloperów | Contoso SocialGaming AwesomeService deweloperów |
| Contoso | GO | InternalApps | Produkcji | Contoso IT InternalApps produkcji |
| Contoso | GO | InternalApps | Deweloperów | Contoso IT deweloperów InternalApps |

<!-- TODO; include more information about organizing subscriptions for application deployment, pods, etc. -->

## <a name="use-affixes-to-avoid-ambiguity"></a>Aby uniknąć niejednoznaczności skorzystać afiksów

Podczas nadawania nazw zasobów w Azure, zaleca się używanie typowych prefiksy lub sufiksy do identyfikowania typu i kontekst zasobu.  Podczas wszystkich informacji o typie, metadane, kontekst jest dostępny programowy, stosowanie typowych afiksów upraszcza graficzna Identyfikacja.  Włączenie afiksów Konwencja nazewnictwa, należy jasno określić, czy Afiks jest na początku nazwy (prefiks) lub na końcu (sufiks).  

Na przykład w tym miejscu są dwie możliwe nazwy usługi hostingu Aparat obliczania:

- SvcCalculationEngine (prefiks)
- CalculationEngineSvc (sufiks)

Afiksów może odnosić się do różnych aspektów opisujące określonego zasobów. W poniższej tabeli przedstawiono kilka przykładów zazwyczaj używany.

| Proporcji | Przykład | Notatki |
| ------ | ------- | ----- |
| Środowisko | deweloperów, zlecenie, aparatu pytania i odpowiedzi | Określa środowisko dla zasobu |
| Lokalizacja | UW (nam zachód), ue (nam Wschód) | Określa region, w którym zostanie wdrożony zasobu |
| Wystąpienia | 01, 02 | Dla zasobów, które mają więcej niż jednego wystąpienia nazwanego (serwery sieci web, itp.). |
| Produktu lub usługi | Usługa | Służy do identyfikowania produktu, aplikacji lub usługi, która obsługuje zasobu |
| Rola | SQL, sieci web, wiadomości | Określa rolę skojarzonego zasobu |

Podczas tworzenia określonych konwencję nazewnictwa dla Twojej firmy lub projektów, jest ważne wybierz zestaw wspólnych afiksów oraz ich pozycji (sufiks lub prefiksu).

## <a name="naming-rules-and-restrictions"></a>Nadawanie nazw reguł i ograniczeń

Wymusza każdego typu zasobem lub usługą Azure zestaw nazw ograniczeń i zakres; wszelkie konwencji nazewnictwa lub deseniu muszą spełniać wymagania zasady nazewnictwa i zakres.  Na przykład podczas nazwę maszyny map na nazwę DNS (a więc musi być unikatowe we wszystkich Azure), nazwę VNET jest ograniczone do utworzonych w grupie zasobów.

Na ogół uniknąć znaków specjalnych (`-` lub `_`) jako pierwszego lub ostatniego znaku w dowolną nazwę. Te znaki spowoduje, że większość reguł sprawdzania poprawności kończy się niepowodzeniem.

| Kategoria | Usługa lub obiektu | Zakres | Długość | Obudowy | Prawidłowe znaki | Sugerowane deseniem | Przykład |
| ------------- | ----------------- | ----- | ------ | ------ | ---------------- | ----------------- | ------- |
| Grupa zasobów | Grupa zasobów | Globalny | 1-64 | Uwzględniana wielkość liter | Alfanumeryczny, podkreślenie i łącznik | `<service short name>-<environment>-rg` | `profx-prod-rg` |
| Grupa zasobów | Konfigurowanie dostępności | Grupa zasobów | 1-80 | Uwzględniana wielkość liter | Alfanumeryczny, podkreślenie i łącznik | `<service-short-name>-<context>-as` | `profx-sql-as` |
| Ogólne | Znacznik | Skojarzonej jednostce | 512 (nazwa), 256 (wartość) | Uwzględniana wielkość liter | Alfanumeryczny | `"key" : "value"` | `"department" : "Central IT"` |
| Obliczenia | Maszyn wirtualnych | Grupa zasobów | 1-15 | Uwzględniana wielkość liter | Alfanumeryczny, podkreślenie i łącznik | `<name>-<role>-<instance>` | `profx-sql-001` |
| Miejsca do magazynowania | Nazwę konta magazynu (dane) | Globalny | 3-24 | Małe litery | Alfanumeryczny | `<service short name><type><number>` | `profxdata001` |
| Miejsca do magazynowania | Nazwę konta magazynu (dyski) | Globalny | 3-24 | Małe litery | Alfanumeryczny | `<vm name without dashes>st<number>` | `profxsql001st0` |
| Miejsca do magazynowania | Nazwa kontenera | Konto miejsca do magazynowania | 3-63 |   Małe litery | Alfanumeryczny i łącznika | `<context>` | `logs` |
| Miejsca do magazynowania | Nazwy obiektów blob | Kontener | 1-1024. | Uwzględnij wielkość liter | Dowolny znak adresu URL | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Miejsca do magazynowania | Nazwa kolejki | Konto miejsca do magazynowania | 3-63 | Małe litery | Alfanumeryczny i łącznika | `<service short name>-<context>-<num>` | `awesomeservice-messages-001` |
| Miejsca do magazynowania | Nazwa tabeli | Konto miejsca do magazynowania | 3-63 |Uwzględniana wielkość liter | Alfanumeryczny | `<service short name>-<context>` | `awesomeservice-logs` |
| Miejsca do magazynowania | Nazwa pliku | Konto miejsca do magazynowania | 3-63 | Małe litery | Alfanumeryczny | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Sieci | Wirtualna sieć (VNet) | Grupa zasobów | 2-64 | Bez uwzględniania wielkości liter | Alfanumeryczny, kreski, podkreślenie i okresem. | `<service short name>-[section]-vnet` | `profx-vnet` |
| Sieci | Podsieć | VNet nadrzędnej | 2-80 | Bez uwzględniania wielkości liter | Alfanumeryczny, podkreślenie, kreskę i okresu | `<role>-subnet` | `gateway-subnet` |
| Sieci | Interfejs sieciowy | Grupa zasobów | 1-80 | Bez uwzględniania wielkości liter | Alfanumeryczny, łącznika, podkreślenie i okresem. | `<vmname>-<num>nic` | `profx-sql1-1nic` |
| Sieci | Grupa zabezpieczeń sieci | Grupa zasobów | 1-80 | Bez uwzględniania wielkości liter | Alfanumeryczny, łącznika, podkreślenie i okresem. | `<service short name>-<context>-nsg` | `profx-app-nsg` |
| Sieci | Reguła grupy zabezpieczeń sieci | Grupa zasobów | 1-80 | Bez uwzględniania wielkości liter | Alfanumeryczny, łącznika, podkreślenie i okresem. | `<descriptive context>` | `sql-allow` |
| Sieci | Publiczny adres IP | Grupa zasobów | 1-80 | Bez uwzględniania wielkości liter | Alfanumeryczny, kreski, podkreślenie i okresem. | `<vm or service name>-pip` | `profx-sql1-pip` |
| Sieci | Usługa równoważenia obciążenia | Grupa zasobów | 1-80 | Bez uwzględniania wielkości liter | Alfanumeryczny, kreski, podkreślenie i okresem. | `<service or role>-lb` | `profx-lb` |
| Sieci | Ładowanie konfiguracji odpowiednie zasady | Usługa równoważenia obciążenia | 1-80 | Bez uwzględniania wielkości liter | Alfanumeryczny, kreski, podkreślenie i okresem. | `descriptive context` | `http` |

<!-- TODO fill in the rest of these resources
| Networking | Azure Application Gateway | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `<service or role>-aag` | `profx-aag`
| Networking | Azure Application Gateway Connection | Azure Application Gateway | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `` | TODO
| Networking | Traffic Manager Profile | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | TODO | TODO
-->

## <a name="organizing-resources-with-tags"></a>Organizowanie zasobów przy użyciu tagów

Menedżer zasobów Azure obsługuje znakowania podmioty dowolnego ciągów tekstowych w celu identyfikowania kontekstu i usprawnianie automatyzacji.  Na przykład tag `"sqlVersion: "sql2014ee"` można określić maszyny wirtualne we wdrożeniu uruchomiony program SQL Server 2014 Enterprise Edition uruchamiania automatycznego skryptu z nimi.  Znaczniki należy uzupełnić i wzbogacić kontekstu boku konwencje nazewnictwa wybrana.

> [AZURE.TIP]Jeden innych zalet znaczników jest, że znaczniki należeć do kilku grup zasobów, umożliwia łączenie i powiązania między różnymi wdrożeń jednostek.

Każdy zasób lub grupa zasobów może zawierać maksymalnie **15** znaczników. Nazwa znacznika jest ograniczona do 512 znaków, a wartość tagu jest ograniczona do 256 znaków.

Aby uzyskać więcej informacji na zasób znakowania odwołują się do [Używanie znaczników w celu organizowania Azure zasobów](../resource-group-using-tags.md).

Typowe znakowania przypadków użycia, należą:

- **Rozliczenia**; Grupowanie zasobów i kojarzenie ich z rozliczeń lub opłaty ponownie kodów.
- **Identyfikacji kontekstu usługi**; Identyfikacji grup zasobów w różnych grup zasobów dla typowych operacji i grupowania
- **Kontekst zabezpieczeń i kontroli dostępu**; Identyfikator roli administracyjnej według portfela system, usługa, aplikacji, wystąpienie, itp.

> [AZURE.TIP]Znakowanie wcześniej — często znacznik.  Lepiej mają planu bazowego znakowania schematu w miejscu i Dostosuj przez godzinę zamiast zakresie wyposażenia po zakończeniu.  

Przykład niektóre typowe znakowania metod:

| Nazwa znacznika | Klawisz | Przykład | Komentarz |
| -------- | --- | ------- | ------- |
| Rachunek / wewnętrzna identyfikator obciążenia zwrotnego | Adres rachunku  | `IT-Chargeback-1234` | We/Wy wewnętrzny lub kod rozliczeń |
| Operator lub pojedynczy bezpośrednio odpowiedzialne (DRI) | managedBy | `joe@contoso.com`  | Alias lub adres e-mail |
| Nazwa projektu | Nazwa projektu | `myproject`  | Nazwa linii projektu lub produktu |
| Wersja projektu | Wersja projektu | `3.4`  | Wersji programu project lub linii produktów |
| Środowisko | środowisko | `<Production, Staging, QA >` | Identyfikator środowiska | 
| Warstwy | warstwy | `Front End, Back End, Data` | Identyfikacja warstwa lub roli/kontekstu |
| Profil danych | dataProfile | `Public, Confidential, Restricted, Internal` | Uwzględnianie danych przechowywanych w zasobu |
 
## <a name="tips-and-tricks"></a>Porady i wskazówki

Niektóre typy zasobów mogą wymagać dodatkowych care o nadawaniu nazw i konwencjach.

### <a name="virtual-machines"></a>Maszyn wirtualnych

Szczególnie w topologii większe starannie nazewnictwa maszyn wirtualnych usprawnia identyfikowanie ról i przeznaczenia każdego komputera, a także umożliwienie przewidywalne skryptów.

> [AZURE.WARNING]Co maszyny wirtualnej Azure zawiera nazwę zasobu Azure i nazwa systemu operacyjnego hosta.  
> Jeśli nazwa zasobu i nazwa hosta są różne oraz zarządzanie nimi maszyny wirtualne może być trudne należy unikać.
> Na przykład jeśli maszyny wirtualnej został utworzony na podstawie VHD już zawiera skonfigurowanego systemu operacyjnego z nazwa hosta.

- [Konwencje nazewnictwa maszyny wirtualne serwera systemu Windows](https://support.microsoft.com/en-us/kb/188997)

<!-- TODO - recommendations on naming VMs. -->

### <a name="storage-accounts-and-storage-entities"></a>Konta miejsca do magazynowania i jednostek miejsca do magazynowania

Istnieją dwa przypadki służą przede wszystkim dla konta miejsca do magazynowania — tworzenie kopii dysków dla maszyny wirtualne i dane są przechowywane w tabelach, kolejek i obiektów blob.  Konta miejsca do magazynowania na potrzeby maszyn wirtualnych dysków należy wykonaj konwencji nazewnictwa kojarzenia ich z elementem nadrzędnym nazwę maszyn wirtualnych (i potencjalną potrzebę wielu kont miejsca do magazynowania dla zaawansowanych maszyn wirtualnych wersji produktu, również zastosować sufiks numer).

> [AZURE.TIP]Konta magazynu — dla danych lub dysków, należy wykonaj konwencji nazewnictwa, która pozwala na wielu kont przestrzeni dyskowej posłużyć (to znaczy zawsze za pomocą sufiks numeryczny).

Można tak skonfigurować niestandardową nazwę domeny do uzyskiwania dostępu do danych typu blob na koncie magazyn Azure go.
Domyślny punkt końcowy usługi obiektów Blob `https://mystorage.blob.core.windows.net`.

Ale można mapować domenę niestandardową (na przykład www.contoso.com) do punktu końcowego obiektów blob dla Twojego konta miejsca do magazynowania, można też przejść do blob danych na swoim koncie miejsca do magazynowania przy użyciu tej domeny. Na przykład przy użyciu niestandardowej nazwy domeny `http://mystorage.blob.core.windows.net/mycontainer/myblob` może być używany jako `http://www.contoso.com/mycontainer/myblob`.

Aby uzyskać więcej informacji na temat konfigurowania tej funkcji zapoznaj się z [Konfigurowanie niestandardowej nazwy domeny dla usługi punktu końcowego usługi Magazyn obiektów Blob](../storage/storage-custom-domain-name.md).

Aby uzyskać więcej informacji o nadawaniu nazw obiektów blob, kontenerów i tabel:

- [Nadawanie nazw i odwoływanie się do kontenerów, obiektów blob i metadanych](https://msdn.microsoft.com/library/dd135715.aspx)
- [Nadawanie nazw kolejek i metadanych](https://msdn.microsoft.com/library/dd179349.aspx)
- [Nadawanie nazw tabel](https://msdn.microsoft.com/library/azure/dd179338.aspx)

Nazwy obiektów blob mogą zawierać dowolną kombinację znaków, ale musi poprawnie wyjściowym zastrzeżone znaki adresu URL. Unikanie obiektów blob, których nazwa kończy się kropką (.), ukośnik (/), albo sekwencji lub połączenia tych elementów. W Konwencji ukośnik jest **wirtualna** separator katalogu. Nie należy używać ukośnika odwrotnego (\) w nazwach obiektów blob. Klient interfejsy API może zezwolić na jego, ale następnie nie można poprawnie skrótu, a podpisy nie będą zgodne.

Nie jest możliwe modyfikowanie nazwę konta magazynu lub kontenera po jego utworzeniu.
Jeśli chcesz użyć pod nową nazwą, możesz ją usunąć i utworzyć nową.

> [AZURE.TIP] Zaleca się ustanowić konwencję nazewnictwa dla wszystkich kont miejsca do magazynowania i typy przed wchodzących na rozwoju nowej usługi lub aplikacji.

## <a name="example---deploying-an-n-tier-service"></a>Przykład — wdrażanie usługi n warstwowych

W tym przykładzie zdefiniowano konfigurację usługi n warstwowych składający się z programu IIS serwerach frontonu (znajdujące się w maszyny wirtualne serwera systemu Windows), z programem SQL Server (znajdujące się w dwóch maszyny wirtualne serwera systemu Windows), klaster ElasticSearch (znajdujące się w 6 Linux maszyny wirtualne) i skojarzone magazynu kont, wirtualnych sieci zasobów grupy i Usługa równoważenia obciążenia.

Początek definiując kontekstowe konwencje dla tej aplikacji:

| Jednostki | Konwencja | Opis  |
| ------ | ---------- | ------------ |  
| Nazwa usługi | `profx` | Krótka nazwa aplikacji lub usługi wdrażania |
| Środowisko | `prod` | To jest wdrożenia produkcji (w przeciwieństwie do aparatu pytania i odpowiedzi, test itp.) |

Z tego planu bazowego firma Microsoft może następnie zaprezentowano Konwencji dla każdego typu zasobów:

| Typ zasobu | Konwencja Base | Przykład |
| ------------- | --------------- | ------- |
| Subskrypcji | `<Company> <Department (optional)> <Product Line (optional)> <Environment>` | `Contoso IT InternalApps Profx Production` |
| Grupa zasobów | `servicename-rg` | `profx-rg` |
| Wirtualna sieć | `servicename-vnet` | `profx-vnet` |
| Podsieć | `role-subnet` | `sql-vnet` |
| Usługa równoważenia obciążenia | `servicename-lb` | `profx-lb` |
| Maszyn wirtualnych | `servicename-role[number]` | `profx-sql0` |
| Konto miejsca do magazynowania | `<vmnamenodashes>st<num>` | `profxsql0st0` |

Jak pokazano w następującej diagramu:

![diagram topologii aplikacji] (media/guidance-naming-conventions/guidance-naming-convention-example.png "Topologia aplikacji próbki")

## <a name="sample---azure-cli-script-for-deploying-the-sample-above"></a>Przykładowe — polecenie Azure skrypt do wdrażania próbki powyżej

```bash
#!/bin/sh

#####################################################################
# Sample script using the Azure CLI to build out an application 
# demonstrating naming conventions.  
#
# Note; this script is not intended for production deployment, as it does 
# not create availability sets, configure network security rules, etc.
#####################################################################

# Set up variables to build out the naming conventions for deploying
# the cluster  
LOCATION=eastus2
APP_NAME=profx
ENVIRONMENT=prod
USERNAME=testuser
PASSWORD="testpass"

# Set up the tags to associate with items in the application
TAG_BILLTO="InternalApp-ProFX-12345"
TAGS="billTo=${TAG_BILLTO}"

# Explicitly set the subscription to avoid confusion as to which subscription
# is active/default
SUBSCRIPTION=3e9c25fc-55b3-4837-9bba-02b6eb204331

# Set up the names of things using recommended conventions
RESOURCE_GROUP="${APP_NAME}-${ENVIRONMENT}-rg"
VNET_NAME="${APP_NAME}-vnet"

# Set up the postfix variables attached to most CLI commands
POSTFIX="--resource-group ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}"

##########################################################################################
# Set up the VM conventions for Linux and Windows images

# For Windows, get the list of URN's via 
# azure vm image list ${LOCATION} MicrosoftWindowsServer WindowsServer 2012-R2-Datacenter
WINDOWS_BASE_IMAGE=MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20160126

# For Linux, get the list or URN's via 
# azure vm image list ${LOCATION} canonical ubuntuserver
LINUX_BASE_IMAGE=canonical:ubuntuserver:16.04.0-DAILY-LTS:16.04.201602130

#########################################################################################
## Define functions 
create_vm ()
{
    vm_name=$1
    vnet_name=$2
    subnet_name=$3
    os_type=$4
    vhd_path=$5
    vm_size=$6
    diagnostics_storage=$7

    # Create the network interface card for this VM
    azure network nic create --name "${vm_name}-0nic" --subnet-name ${subnet_name} --subnet-vnet-name ${vnet_name} \
        --tags="${TAGS}" ${POSTFIX}

    # Create the storage account for this vm's disks (premium locally redundant storage -> PLRS)
    # Note the ${var//-/} syntax to remove dashes from the vm name
    storage_account_name=${vm_name//-/}st01
    azure storage account create --type=PLRS --tags "${TAGS}" ${POSTFIX} "${storage_account_name}"

    # Map the name of the diagnostics storage account to a blob URI for boot diagnostics
    # This is (currently) required when deploying with a named premium storage account 
    diag_blob="https://${diagnostics_storage}.blob.core.windows.net/"

    # Create the VM
    azure vm create --name ${vm_name} --nic-name "${vm_name}-0nic" --os-type ${os_type} \
        --image-urn ${vhd_path} --vm-size ${vm_size} --vnet-name ${vnet_name} --vnet-subnet-name ${subnet_name} \
        --storage-account-name "${storage_account_name}" --storage-account-container-name vhds --os-disk-vhd "${vm_name}-osdisk.vhd" \
        --admin-username "${USERNAME}" --admin-password "${PASSWORD}" \
        --boot-diagnostics-storage-uri "${diag_blob}" \
        --tags="${TAGS}" ${POSTFIX} 
}

###################################################################################################
# Create resources

# Step 1 - create the enclosing resource group
azure group create --name "${RESOURCE_GROUP}" --location "${LOCATION}" --tags "${TAGS}" --subscription "${SUBSCRIPTION}"

# Step 2 - create the network security groups

# Step 3 - create the networks (VNet and subnets)
azure network vnet create --name "${VNET_NAME}" --address-prefixes="10.0.0.0/8" --tags "${TAGS}" ${POSTFIX}
# TODO - does subnet support tagging?
azure network vnet subnet create --name gateway-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.1.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-master-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.2.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-data-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.3.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name sql-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.4.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}

# Step 4 - define the load balancer and network security rules
azure network lb create --name "${APP_NAME}-lb" ${POSTFIX}
# In a production deployment script, we'd create load balancer rules and 
# network security groups here

# Step 5 - create a diagnostics storage account
diagnostics_storage_account=${APP_NAME//-/}diag
azure storage account create --type=LRS --tags "${TAGS}" ${POSTFIX} "${diagnostics_storage_account}"

# Step 6.1 - Create the gateway VMs
for i in `seq 1 4`;
do
    create_vm "${APP_NAME}-gw${i}" "${APP_NAME}-vnet" "gateway-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}" 
done    

# Step 6.2 - Create the ElasticSearch master and data VMs
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-master${i}" "${APP_NAME}-vnet" "es-master-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-data${i}" "${APP_NAME}-vnet" "es-data-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done

# Step 6.3 - Create the SQL VMs
create_vm "${APP_NAME}-sql0" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
create_vm "${APP_NAME}-sql1" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
```
