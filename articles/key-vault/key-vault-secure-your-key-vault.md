<properties
    pageTitle="Bezpiecznego magazynu usługi klucza | Microsoft Azure"
    description="Zarządzanie uprawnieniami dostępu dla klucza magazynu do zarządzania magazynami i klucze i hasła. Model uwierzytelniania i autoryzacji klucza magazynu oraz sposobu zabezpieczenia usługi klucza magazynu"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/07/2016"
    ms.author="ambapat"/>


# <a name="secure-your-key-vault"></a>Bezpiecznego magazynu usługi klucza

Azure magazynu klucza jest usługa w chmurze, który chroni kluczy szyfrowania i hasła (na przykład certyfikatów, parametry połączenia, hasła) dla aplikacji chmury. Ponieważ te dane są poufne i business krytyczne, ma być bezpieczny dostęp do Twojej magazynami najważniejszych, tak aby tylko autoryzowane aplikacji i użytkowników dostęp do kluczowych magazynu. Ten artykuł zawiera omówienie modelu magazynu klucza dostępu, wyjaśniono uwierzytelniania i autoryzacji i opisano, jak bezpieczny dostęp do klucza magazynu w chmurze aplikacji z przykładem.

## <a name="overview"></a>Omówienie

Dostęp do klucza magazynu jest kontrolowane przez dwóch oddzielnych interfejsów: płaszczyzny zarządzania i płaszczyzną danych. Dla obu płaszczyznach pisane z wielkiej litery uwierzytelniania i autoryzacji jest wymagane przed rozmówcy (użytkownika lub aplikacji) można uzyskać dostęp do klucza magazynu. Uwierzytelnianie Określa identyfikator rozmówcy, gdy autoryzacji steruje tym, jakie wywołujący będzie mógł wykonywać operacje.

Uwierzytelniania zarówno płaszczyzny zarządzania i płaszczyzny danych za pomocą usługi Azure Active Directory. W przypadku autoryzacji płaszczyzny zarządzania używa kontrola dostępu oparta na rolach (RBAC) podczas płaszczyzny danych używa magazynu klucza zasadę dostępu.

Oto krótkie omówienie tematy:

[Uwierzytelnianie za pomocą usługi Azure Active Directory](#authentication-using-azure-active-direcrory) — w tej sekcji wyjaśniono, jak uwierzytelnia dzwoniący z usługi Azure Active Directory, aby uzyskać dostęp do klucza magazynu za pomocą płaszczyzny zarządzania i płaszczyzny danych. 

[Płaszczyzny zarządzania i płaszczyzną danych](#management-plane-and-data-plane) — płaszczyzny zarządzania i płaszczyzną danych są dwie płaszczyzny dostępu na potrzeby uzyskiwania dostępu do swojego klucza magazynu. Każdy płaszczyzny programu access obsługuje określonych operacji. W tej sekcji opisano dostępu do punktów końcowych, operacje obsługiwane i uzyskać dostęp do sterowania metodę przy każdej z płaszczyzn. 

[Kontrola dostępu płaszczyzny zarządzania](#management-plane-access-control) — w tej sekcji, które przyjrzymy zezwolenia na dostęp do operacji płaszczyzny zarządzania przy użyciu kontrola dostępu oparta na rolach.

[Kontrola dostępu płaszczyzny danych](#data-plane-access-control) — w tej sekcji opisano sposób użycia klucza magazynu zasady dostępu do kontrolowania dostępu płaszczyzny danych.

[Przykład](#example) — w tym przykładzie opisano, jak skonfigurować kontroli dostępu do swojego klucza magazynu umożliwia trzech różnych zespołów (zespołu zabezpieczeń, deweloperzy i operatory i audytorów) wykonywania określonych zadań można opracowywać i monitorowanie aplikacji w Azure i zarządzanie nimi.


## <a name="authentication-using-azure-active-directory"></a>Uwierzytelnianie za pomocą usługi Azure Active Directory

Po utworzeniu klucza magazynu w subskrypcji usługi Azure jest automatycznie kojarzone z subskrypcji usługi Azure Active Directory dzierżawy. Wszystkich osób dzwoniących (użytkowników i aplikacji) muszą być zarejestrowane w tym dzierżawy, aby uzyskać dostęp do tego magazynu kluczy. Aplikacja lub użytkownik musi uwierzytelniania usługi Azure Active Directory, aby uzyskać dostęp do klucza magazynu. Dotyczy to zarówno zarządzania płaszczyzny płaszczyzny dostępu do danych. W obu przypadkach aplikacja ma dostęp do klucza magazynu na dwa sposoby:

-  **użytkownik + aplikacji programu access** — zazwyczaj jest to dla aplikacji, które dostęp do kluczowych magazynu imieniu zalogowany użytkownik. Azure programu PowerShell, Azure Portal są przykłady tego typu dostępu. Istnieją dwa sposoby do udzielania dostępu użytkownikom: jednym ze sposobów jest do udzielania dostępu użytkownikom, aby oni dostęp do klucza magazynu z dowolnej aplikacji i innym sposobem jest udzielić użytkownikowi dostępu do magazynu klucza tylko wtedy, gdy korzystają z określonej aplikacji (nazywane złożonego tożsamości). 
-  **dostęp tylko do aplikacji** - dla aplikacji, które są uruchamiane demon usług, tło zadań itp. Tożsamości aplikacji ma uprawnienia do magazynu kluczy.

W przypadku obu typów aplikacji Aplikacja uwierzytelnia z usługi Azure Active Directory przy użyciu jednej z [obsługiwanych metod uwierzytelniania](../active-directory/active-directory-authentication-scenarios.md) i uzyskuje tokenu. Metody uwierzytelniania zależy od typu aplikacji. Następnie aplikacja używa ten token i przesyła żądanie interfejsu API usługi REST do klucza magazynu. W przypadku dostępu do funkcji zarządzania płaszczyzny żądania są wysyłane za pośrednictwem punktu końcowego Azure Menedżera zasobów. Podczas uzyskiwania dostępu do danych płaszczyzny, magazynu rozmów aplikacji bezpośrednio do klucza punktu końcowego. Dowiedz się więcej na [Przepływ uwierzytelniania całego](../active-directory/active-directory-protocols-oauth-code.md). 

Nazwa zasobu, do której aplikacja żąda tokenu różni się w zależności od tego, czy aplikacja uzyskuje dostęp do zarządzania płaszczyzny lub płaszczyzny danych. W związku z tym nazwa zasobu jest zarządzania płaszczyzny lub dane płaszczyzny końców opisane w tabeli w dalszej części, w zależności od środowiska Azure.

O jeden pojedynczy mechanizm uwierzytelniania do zarządzania i płaszczyzny danych ma własny zalety:

- Organizacje centralnie kontrolować dostęp do wszystkich magazynami najważniejszych w organizacji
- Jeśli użytkownik opuści, były natychmiast utracisz dostęp do wszystkich magazynami najważniejszych w organizacji
- Organizacje można dostosować uwierzytelniania za pomocą opcji w usłudze Azure Active Directory (na przykład włączenie uwierzytelnianie wieloskładnikowe Aby zwiększyć bezpieczeństwo)

## <a name="management-plane-and-data-plane"></a>Płaszczyzny zarządzania i płaszczyzną danych

Azure magazynu klucza jest dostępne za pośrednictwem Menedżera zasobów Azure model wdrażania usługi Azure. Podczas tworzenia klucza magazynu zostanie wyświetlony kontener wirtualny wewnątrz której można tworzyć innych obiektów, takich jak klucze, certyfikaty i hasła. Następnie można uzyskać dostęp do klucza magazynu korzystania płaszczyzny zarządzania i płaszczyzny danych w celu wykonywania określonych operacji. Interfejs płaszczyzny zarządzania służy do zarządzania do klucza magazynu siebie, takie jak tworzenie, usuwanie, aktualizowanie atrybutów klucza magazynu i ustawienie zasad dostępu dla samolotu danych. Interfejs płaszczyzny danych umożliwia dodawanie, usuwanie, modyfikowanie i przy użyciu klawiszy, hasła i certyfikaty przechowywane w swojej klucza magazynu.

Zarządzanie interfejsów płaszczyzny płaszczyzny i danych są dostępne przez różne punkty końcowe (patrz tabela). Drugiej kolumny w tabeli opisano nazw DNS dla te punkty końcowe w różnych środowiskach Azure. Trzecia kolumna w tym artykule opisano czynności wykonywane od osi każdego programu access. Każdy płaszczyzny dostępu też ma własny mechanizmu kontroli dostępu: kontrola dostępu płaszczyzny zarządzania jest Ustaw przy użyciu Azure Resource Manager Role-Based kontroli dostępu (RBAC), aby podczas kontroli dostępu płaszczyzny danych jest zestawu przy użyciu klucza magazynu zasad dostępu.

| Płaszczyzny programu Access | Punkty końcowe programu Access | Operacje | Mechanizmu kontroli dostępu|
|--------------|------------------|--------------------|--------|
| Płaszczyzny zarządzania|**Globalne:**<br> Management.Azure.com:443<br><br> **Chiny Azure:**<br> Management.chinacloudapi.CN:443<br><br> **Azure Rządu Stanów Zjednoczonych:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Niemcy:**<br> Management.microsoftazure.de:443 | Tworzenie/odczytu/aktualizowanie i usuwanie klucza magazynu <br> Ustawianie zasad dostępu dla klucza magazynu<br>Ustawianie tagi klucza magazynu | Kontrola dostępu oparta na rolach Menedżera zasobów Azure (RBAC) |
| Płaszczyzny danych | **Globalne:**<br> &lt;Nazwa magazynu&gt;. vault.azure.net:443<br><br> **Chiny Azure:**<br> &lt;Nazwa magazynu&gt;. vault.azure.cn:443<br><br> **Azure Rządu Stanów Zjednoczonych:**<br> &lt;Nazwa magazynu&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Niemcy:**<br> &lt;Nazwa magazynu&gt;. vault.microsoftazure.de:443 | Dla kluczy: Odszyfrowywanie, szyfrowanie, UnwrapKey, WrapKey, sprawdź, zaloguj się, uzyskiwanie, listy, aktualizowanie, tworzenie, importowanie, usuwanie, kopii zapasowej, Przywracanie<br><br> Aby uzyskać hasła: uzyskiwanie, listy, zestaw, Usuń | Zasady dostępu klucza magazynu|

Ustawienia zarządzania płaszczyzny i danych płaszczyzny kontroli dostępu działają niezależnie od siebie. Na przykład jeśli chcesz udzielić dostęp do aplikacji, aby użyć klawiszy w klucza magazynu, należy przyznać uprawnienia dostępu do płaszczyzny danych przy użyciu klucza magazynu zasady dostępu i niedostępna płaszczyzny zarządzania jest wymagane dla tej aplikacji. I odwrotnie, jeśli użytkownik ma mieć możliwość więcej właściwości magazynu i znaczników, ale istnieje możliwość klawiszy, hasła lub certyfikaty, możesz przyznać temu użytkownikowi "Odczyt" przy użyciu RBAC i dostępowi do płaszczyzny danych nie jest wymagane.

## <a name="management-plane-access-control"></a>Kontrola dostępu płaszczyzny zarządzania

Płaszczyzny zarządzania składa się z operacji, które wpływają na klucza magazyn się. Można na przykład, tworzenie lub usuwanie klucza magazynu. Możesz wyświetlić listę magazynów w subskrypcji. Można pobrać właściwości klucza magazynu (taki jak jednostka SKU znaczniki) i ustawić klucza magazynu zasady dostępu, które sterują użytkowników i aplikacje, które mogą uzyskiwać dostęp klucze i hasła klucza magazynu. Kontrola dostępu płaszczyzny zarządzania używa RBAC. Zobacz pełną listę operacji klucza magazynu, które mogą być wykonywane za pośrednictwem płaszczyzny zarządzania w tabeli w poprzednich sekcji. 

### <a name="role-based-access-control-rbac"></a>Kontrola dostępu oparta na rolach (RBAC)
Każdy Azure Subskrypcja obejmuje usługi Azure Active Directory. Użytkownicy, grupy i aplikacji z katalogu można udzielić dostępu do zarządzania zasobami Azure subskrypcji korzystające z modelu wdrożenia Azure Menedżera zasobów. Ten rodzaj kontrola dostępu jest określane jako kontrola dostępu oparta na rolach (RBAC). Aby zarządzać dostęp, służy [Azure portal](https://portal.azure.com/), [polecenie Azure narzędzia](../xplat-cli-install.md), [programu PowerShell](../powershell-install-configure.md)lub [Interfejsów API pozostałych Menedżera zasobów Azure](https://msdn.microsoft.com/library/azure/dn906885.aspx).

Z modelem Azure Menedżera zasobów możesz utworzyć do klucza magazynu w zasobów grupy i kontrolowanie dostępu do zarządzania linii tego klucza magazynu przy użyciu usługi Azure Active Directory. Na przykład możesz przyznać użytkowników lub grupy możliwość zarządzania magazynami najważniejszych w określonej grupy zasobów.

Przypisując odpowiednich ról RBAC można udzielić dostępu użytkownikami, grupami i aplikacje w określonym zakresie. Na przykład aby udzielić dostępu użytkownika do zarządzania magazynami najważniejszych czy przypisywanie roli wstępnie zdefiniowanych "magazynu kluczowych współautorów" temu użytkownikowi w określonym zakresie. Zakres w tym przypadku będzie subskrypcji, grupa zasobów lub po prostu określonego klucza magazynu. Rola przypisana na poziomie subskrypcji dotyczy wszystkich grup zasobów i zasobów w ramach tej subskrypcji. Rola przypisana na poziomie grupy zasobów dotyczy wszystkich zasobów w danej grupy zasobów. Rola przypisana dla określonego zasobu dotyczą tylko tego zasobu. Istnieje kilka wstępnie zdefiniowanych ról (zobacz [RBAC: wbudowane role](../active-directory/role-based-access-built-in-roles.md)), a jeśli wstępnie zdefiniowane role nie odpowiada Twoim potrzebom można także zdefiniować własne role.

>[AZURE.IMPORTANT] Należy zauważyć, że jeśli użytkownik ma współautora uprawnienia (RBAC) do płaszczyzny zarządzania klucza magazynu, Anna może udzielić sobie dostęp do samolotu danych, przez ustawienie klucza magazynu zasadę dostępu, która kontroluje dostęp do danych płaszczyzny. Dlatego zalecane jest ściśle kontrolować, kto ma dostęp "Współautora" do usługi magazynami najważniejszych zapewnienie tylko autoryzowanych osoby można uzyskać dostęp do i zarządzania magazynami najważniejszych, klawiszy, hasła i certyfikatów.

## <a name="data-plane-access-control"></a>Kontrola dostępu płaszczyzny danych

Płaszczyzny klucza magazynu danych składa się z operacji, które wpływają na obiektów w klucza magazynu, takich jak klucze, certyfikaty i hasła.  Ta opcja uwzględnia klucza, takich jak utworzone operacje importowania, aktualizacji, listy, kopia zapasowa i przywracanie klawiszy, cryptographic operacji, takich jak znak, sprawdź, szyfrowanie, odszyfrowywanie, zawinąć, cofnąć Zawijanie i ustaw znaczniki i inne atrybuty dla kluczy. Podobnie dla hasła, które zawiera, jak ustawić listę, Usuń.

Otrzymuje dostęp do danych płaszczyzny przez ustawienie zasad dostępu dla klucza magazynu. Użytkownikowi, grupie lub aplikacja musi mieć uprawnienia współautora (RBAC) dla zarządzania samolotu dla klucza magazynu móc ustawić zasady dostępu dla tego klucza magazynu. Użytkownikowi, grupie lub aplikacji może mieć przyznany dostęp do wykonywania określonych operacji klawiszy lub hasła w klucza magazynu. kluczowe magazynu pomocy technicznej do 16 wpisy zasady dostępu klucza magazynu. Utwórz grupę zabezpieczeń usługi Azure Active Directory i dodawanie użytkowników do tej grupy, aby udzielić dostępu do danych płaszczyzny kilku użytkownikom klucza magazynu.

### <a name="key-vault-access-policies"></a>kluczowe magazynu zasady dostępu

zasady dostępu klucza magazynu oddzielnie Udziel uprawnień do kluczy, certyfikaty i hasła. Na przykład można nadać użytkownikowi dostępu do tylko klucze, ale nie uprawnienia dla hasła. Jednak uprawnień dostępu do klawisze hasła lub certyfikaty są na poziomie magazynu. Innymi słowy zasady dostępu klucza magazynu nie obsługuje poziomu uprawnień do obiektu. [Azure portal](https://portal.azure.com/), [polecenie Azure narzędzia](../xplat-cli-install.md), [programu PowerShell](../powershell-install-configure.md)lub [klucza magazynu interfejsy API zarządzania pozostałych](https://msdn.microsoft.com/library/azure/mt620024.aspx) umożliwia ustawianie zasad dostępu dla klucza magazynu.

>[AZURE.IMPORTANT] Uwaga Stosowanie klucza magazynu zasady dostępu na poziomie magazynu. Na przykład po udzieleniu uprawnień do tworzenia i usuwania kluczy użytkownika będzie mogła wykonywać te operacje na wszystkich kluczy w tym klucza magazynu.

## <a name="example"></a>Przykład

Załóżmy, że tworzysz aplikację, która używa certyfikatu SSL, Azure miejsca do magazynowania do przechowywania danych i kluczem RSA 2048-bitowy dla operacji logowania. Załóżmy, że ta aplikacja jest uruchomiona w maszyny (lub Ustawianie skali maszyn wirtualnych). Można przechowywać wszystkie hasła aplikacji przy użyciu klucza magazynu i przechowywanie uruchamiania certyfikat, który jest używany przez aplikację do uwierzytelniania usługi Azure Active Directory przy użyciu klucza magazynu.

Tak poniżej przedstawiono podsumowanie wszystkie klucze i hasła, które mają być przechowywane w klucza magazynu.

- **Certyfikat SSL** - używanym do szyfrowania SSL
- **Klucz magazynowania** — umożliwia uzyskiwanie dostępu do konta miejsca do magazynowania
- **Klucz RSA 2048-bitowy** — używane dla operacji logowania
- **Certyfikat uruchamiania** — używany do uwierzytelniania usługi Azure Active Directory, aby uzyskać dostęp do klucza magazynu pobierać klucz magazynowania i używania kluczy RSA do podpisu.

Teraz Przyjrzyjmy spotkania z osobami, które są zarządzanie, wdrażania i inspekcji tej aplikacji. W tym przykładzie użyjemy trzy role.

- **Zespół zabezpieczeń** — zazwyczaj są to personelem INFORMATYCZNYM z "urząd CSO (Dyrektor zabezpieczeń)" lub równoważne, odpowiedzialny za naruszenia prawidłowego hasła, takich jak certyfikatów SSL używany do logowania, parametry połączenia dla baz danych, klawiszy konta magazynu kluczy RSA.
- **Deweloperów i operatorów** — są to osób, które rozwoju tej aplikacji, a następnie wdrożyć platformy Azure. Zwykle nie są one częścią zespołu zabezpieczeń i w związku z tym nie powinny one mieć dostęp do dane poufne, takie jak certyfikatów SSL kluczy RSA, ale ich wdrażanie aplikacji powinny mieć dostęp do tych.
- **Obrachunkowy** — jest to zazwyczaj inny zestaw osób, samodzielnie z deweloperzy i ogólne pracowników działu informatycznego. Ich odpowiedzialność jest Przejrzyj właściwego użycia i konserwacja certyfikatów, kluczy itp i zachować zgodność z standardy zabezpieczeń danych. 

Istnieje jedna rola więcej, który wykracza poza zakres tej aplikacji, ale tutaj odpowiednich mają być wymienione i będzie subskrypcji (lub grupa zasobów) administratora. Administrator subskrypcji konfiguruje uprawnienia dostępu początkowej dla zespołu zabezpieczeń. W tym miejscu przyjęto założenie, że administrator subskrypcji udzieliła dostępu do zespołu zabezpieczeń do grupy zasobów, w których wszystkich zasobów potrzebnych dla tej reside aplikacji.

Teraz zobaczmy, akcje każdej roli wykonuje w kontekście tej aplikacji.

- **Zabezpieczenia zespołu**
    - Tworzenie magazynami najważniejszych
    - Włącza funkcję klucza magazynu rejestrowania
    - Dodawanie kluczy i haseł
    - Tworzenie kopii zapasowej kluczy awarii
    - Ustawianie klucza magazynu zasady dostępu, aby udzielić uprawnień dla użytkowników i aplikacji w celu wykonywania określonych operacji
    - Okresowo użycia klawiszy i hasła
- **Deweloperów i operatorów**
    - Odwołania do ładowania początkowego i certyfikatów SSL (odciski palców), klucz magazynowania (tajny URI) i logowanie klucz (klawisz URI) od zespołu zabezpieczeń
    - Opracowywanie i wdrażanie aplikacji, która programowy dostęp do kluczy i haseł
- **Obrachunkowy**
    - Przeglądanie dzienników zastosowania o potwierdzenie właściwego użycia klucza i hasło i zgodność ze standardami zabezpieczeń danych

Teraz zobaczmy, jakie uprawnienia dostępu do klucza magazynu są wymagane przez każdą rolą (i aplikacja) wykonanie przypisanych zadań. 

| Rola użytkownika    | Uprawnienia płaszczyzny zarządzania | Uprawnienia płaszczyzny danych |
|--------------|------------------------------|------------------------|
|Zabezpieczenia zespołu|Magazyn kluczowych współautorów|Klucze: kopia zapasowa, tworzenie, usuwanie, uzyskiwanie, importowanie, listy i przywracanie <br> Hasła: wszystkie|
|Deweloperów i operatorów| kluczowe magazynu wdrażanie uprawnień tak, aby maszyny wirtualne, wdrożeniem można pobierać hasła z magazynu klucza | Brak |
|Obrachunkowy| Brak | Klucze: listy<br>Hasła: listy|
|Aplikacji| Brak | Klucze: logowania<br>Hasła: pobieranie |

>[AZURE.NOTE] Audytorów muszą lista uprawnienie kluczy i hasła, więc ich inspekcja atrybuty kluczy i haseł, które nie są wysyłane w dziennikach, takie jak znaczniki, dat aktywacji i ważności.

Oprócz uprawnienia do magazynu klucza wszystkie trzy role muszą również dostęp do innych zasobów. Na przykład, aby można było wdrożyć maszyny wirtualne (lub Web Apps itp.) Deweloperów i operatorów muszą również "Współautora" dostęp do tych typów zasobów. Audytorów musi mieć konta miejsca do magazynowania, gdzie są przechowywane dzienniki klucza magazynu.

Ponieważ ten artykuł jest zabezpieczanie dostępu do swojego klucza magazynu, możemy tylko przedstawić istotne części odnoszących się do tego tematu i pominąć szczegóły dotyczące wdrażanie certyfikatów, uzyskiwanie dostępu do kluczy i hasła programowy itp. Szczegóły te są już objęte innym programie. Wdrażanie certyfikatów przechowywanych w klucza magazynu do maszyny wirtualne są omówione w [blogu](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/), a jest [Przykładowy kod](https://www.microsoft.com/download/details.aspx?id=45343) dostępne pokazano, jak za pomocą uruchamiania certyfikatu uwierzytelniania Azure AD, aby uzyskać dostęp do klucza magazynu.

Większość uprawnień dostępu można przyznać za pomocą portalu Azure, ale aby udzielić uprawnień szczegółowego może być konieczne Azure programu PowerShell (lub polecenie Azure), aby uzyskać pożądany wynik. 

Załóżmy następujące wstawki programu PowerShell:

- Administrator usługi Azure Active Directory utworzył grupy zabezpieczeń, reprezentujących trzy role, to znaczy Contoso Security Team Devops aplikacji firmy Contoso, audytorów aplikacji firmy Contoso. Administrator dodał również użytkowników do grup, do których należą.

- **ContosoAppRG** jest grupa zasobów, w którym znajdują się wszystkie zasoby. **contosologstorage** to, gdzie są przechowywane pliki dziennika. 

- Konto **ContosoKeyVault** i miejsca do magazynowania klucza magazynu używane dla dzienników klucza magazynu **contosologstorage** musi być w tej samej lokalizacji Azure


Najpierw administrator subskrypcji przypisuje "klucza magazynu współautora" i ról użytkowników dostępu administratora zespołowi zabezpieczeń. Dzięki temu zespołu zabezpieczeń do zarządzania dostęp do innych zasobów oraz zarządzania magazynami najważniejszych w grupie zasobów ContosoAppRG.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Poniższy skrypt pokazano, jak utworzyć zespołu zabezpieczeń magazynu kluczowych, ustawienia rejestrowania i ustawianie uprawnień dostępu dla pozostałych ról i aplikacji. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionToKeys backup,create,delete,get,import,list,restore -PermissionToSecrets all

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devlopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $role

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionToKeys list -PermissionToSecrets list
```

Roli niestandardowe zdefiniowane, tylko jest przypisane do subskrypcji, której jest tworzona grupa zasobów ContosoAppRG. Jeśli niestandardowe ról będzie używana dla innych projektów w innych subskrypcji, jego zakres może mieć więcej subskrypcje dodane.

Przypisanie roli niestandardowych dla deweloperów i operatorów uprawnień "Wdrażanie i akcji" jest ograniczone do grupy zasobów. Dzięki temu tylko maszyny wirtualne utworzone w grupie zasobów "ContosoAppRG" otrzymają hasła (certyfikatu SSL i uruchamiania certyfikatu). Wszelkie maszyny wirtualne tworzone przez członka zespołu deweloperów ops w innych grupa zasobów, nie będzie można uzyskać te hasła, nawet jeśli ta osoba wiedziała tajne identyfikatory URI.

W tym przykładzie przedstawiono prosty scenariusz. Scenariusze dotyczące rzeczywistych może być bardziej złożone i może być konieczne dopasowanie uprawnienia do usługi klucza magazynu, w zależności od potrzeb. Na przykład w naszym przykładzie przyjęto założenie, tego zespołu zabezpieczeń zapewni klucz i tajny odwołania (URI i odciski palców) tego zespołu deweloperów i operatorów muszą zostać utworzone odwołanie w swoich aplikacjach. W związku z tym ich nie trzeba udzielić dostęp do wszystkich danych płaszczyzny deweloperów i operatorów. Należy również zauważyć, że w tym przykładzie omówiono Zabezpieczanie usługi klucza magazynu. Podobne należy rozważyć bezpiecznego za [pośrednictwem usługi SMS](https://azure.microsoft.com/services/virtual-machines/security/), [kont miejsca do magazynowania](../storage/storage-security-guide.md) i inne zasoby Azure.

>[AZURE.NOTE] Uwaga: W tym przykładzie przedstawiono sposób kluczowe magazynu programu access zostaną zablokowane w produkcji. Deweloperzy powinien mieć własne subskrypcji lub grupa zasobów miejsce, w którym mają pełne uprawnienia do zarządzania ich magazynów, maszyny wirtualne i konta miejsca do magazynowania gdzie one opracowywania aplikacji.


## <a name="resources"></a>Zasoby

-   [Kontrola dostępu oparta na rolach Azure Active Directory](../active-directory/role-based-access-control-configure.md)

    W tym artykule wyjaśniono, kontrola dostępu oparta na rolach katalogowej Active Azure i sposobu jej działania.

-   [RBAC: Wbudowana ról](../active-directory/role-based-access-built-in-roles.md)

    W tym artykule opisano wbudowanych Role dostępne w RBAC.

-   [Opis wdrażania Menedżera zasobów i wdrażania klasyczny](../resource-manager-deployment-model.md)

    W tym artykule omówiono wdrożenia Menedżera zasobów i modeli klasyczny wdrażania i wyjaśniono korzyści wynikające z używania grup zasobów i Menedżera zasobów

-    [Zarządzanie kontrola dostępu oparta na rolach przy użyciu programu PowerShell Azure](../active-directory/role-based-access-control-manage-access-powershell.md)

     W tym artykule wyjaśniono, jak zarządzać kontrola dostępu oparta na rolach przy użyciu programu PowerShell Azure

-   [Zarządzanie kontrola dostępu oparta na rolach przy użyciu interfejsu API usługi REST](../active-directory/role-based-access-control-manage-access-rest.md)

    W tym artykule pokazano, jak za pomocą interfejsu API usługi REST Zarządzanie RBAC.

-   [Kontrola dostępu oparta na rolach dla usługi Microsoft Azure z Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    To łącze do klipu wideo w kanale 9 z konferencji Ignite 2015 MS. W tej sesji porozmawiać o dostęp do zarządzania i funkcje raportowania w Azure i Poznaj najlepsze rozwiązania wokół zabezpieczanie dostępu do Azure subskrypcji przy użyciu usługi Azure Active Directory.

-   [Autoryzuj dostęp do aplikacji sieci web przy użyciu OAuth 2.0 i usługi Azure Active Directory](../active-directory/active-directory-protocols-oauth-code.md)

    W tym artykule opisano pełną OAuth 2.0 przepływ uwierzytelniania z usługą Azure Active Directory.

-   [kluczowe magazynu interfejsy API pozostałych zarządzania](https://msdn.microsoft.com/library/azure/mt620024.aspx)

    Ten dokument jest odwołanie do interfejsów API pozostałych programistycznie, zarządzanie z magazynu kluczy wraz z ustawieniem zasady dostępu klucza magazynu.

-   [kluczowe magazynu interfejsy API REST](https://msdn.microsoft.com/library/azure/dn903609.aspx)

    Łącze do najważniejszych magazynu dokumentacji interfejsu API usługi REST.

-   [Kontrola dostępu klucza](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)

    Łącze do klucza dostępu kontrola dokumentacji.

-   [Kontrola dostępu poufne](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)

    Łącze do klucza dostępu kontrola dokumentacji.

-   Zasady dostępu [Ustawianie](https://msdn.microsoft.com/library/mt603625.aspx) i [Usuwanie](https://msdn.microsoft.com/library/mt619427.aspx) klucza magazynu przy użyciu programu PowerShell

    Łącza do odwołanie dokumentacji poleceń cmdlet środowiska PowerShell do zarządzania zasadami dostępu klucza magazynu.

## <a name="next-steps"></a>Następne kroki

Pobieranie samouczek wprowadzenie przez administratora zobacz [Rozpoczynanie pracy z Azure magazynu klucza](key-vault-get-started.md).

Aby uzyskać więcej informacji o korzystaniu z rejestrowania dla magazynu klucza zobacz [Rejestrowanie Azure klucza magazynu](key-vault-logging.md).

Aby uzyskać więcej informacji o użyciu Azure magazynu klucza klucze i hasła zobacz [temat kluczy i hasła](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Jeśli masz pytania dotyczące klucza magazynu, odwiedź stronę [Azure magazynu klucza fora](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
