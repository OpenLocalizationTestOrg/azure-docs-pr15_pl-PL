<properties
   pageTitle="Szyfrowanie dysków Azure dla systemu Windows i Linux oraz IaaS maszyny wirtualne | Microsoft Azure"
   description="Papier omówiono Microsoft Azure dysku szyfrowania dla systemu Windows i Linux oraz IaaS w maszyny wirtualne."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="krkhan"/>


#<a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a>Szyfrowanie dysków Azure dla systemu Windows i Linux oraz IaaS maszyny wirtualne

Microsoft Azure zdecydowanie zobowiązuje się do zapewnienia prywatności danych, suwerenności danych i umożliwia sterowanie usługi Azure hostowanej danych za pomocą wielu zaawansowane technologii szyfrowania, sterowanie i zarządzanie nimi klucze szyfrowania, inspekcji i kontroli dostępu do danych. Dzięki temu klienci Azure wybrać rozwiązanie, które najlepiej odpowiada ich potrzebami. W tym dokumencie możemy podstawowe informacje na temat technologii rozwiązania "Azure szyfrowanie dysków dla systemu Windows i Linux oraz IaaS maszyn wirtualnych w" Aby chronić i ochrony danych w celu spełniają organizacji bezpieczeństwa i zgodności zobowiązań. Papier zawiera szczegółowe wskazówki na temat korzystania z funkcji szyfrowania dysku Azure tym scenariusze obsługiwane i użytkownik napotka.

**Uwaga**: niektóre zalecenia zawartych w może spowodować danych, sieci lub użycie zasobu obliczeń uzyskując dodatkowe koszty licencji lub subskrypcji.

## <a name="overview"></a>Omówienie

Azure szyfrowanie dysku jest nowe możliwości, które umożliwia szyfrowanie dysków maszyn wirtualnych systemu Windows i Linux oraz IaaS. Azure szyfrowania dysku wykorzystuje branżowe standardowa funkcja [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) systemu Windows i funkcji [Kryptograficznego DM](https://en.wikipedia.org/wiki/Dm-crypt) Linux oraz zapewnienie szyfrowania woluminu systemu operacyjnego i dyski danych. Rozwiązanie jest zintegrowany z magazynu [Klucza Azure](https://azure.microsoft.com/documentation/services/key-vault/) ułatwiające kontrolowanie i zarządzanie nimi kluczy szyfrowania dysku i hasła w ramach subskrypcji klucza magazynu, zapewniając, że wszystkie dane w dyski maszyn wirtualnych są szyfrowane na pozostałych w magazynie Azure.

Szyfrowanie dysków Azure dla systemu Windows i Linux oraz IaaS w maszyny wirtualne jest teraz **Ogólnodostępną** we wszystkich Azure publicznej regionach pośrednictwem standardowej SMS i maszyny wirtualne z nośnikami premium.

### <a name="encryption-scenarios"></a>Scenariusze szyfrowania

Rozwiązanie szyfrowanie dysków Azure obsługuje następujących scenariuszy:

- Włącz szyfrowanie dla nowych IaaS maszyny wirtualne utworzone na podstawie wstępnie zaszyfrowanych wirtualnego dysku twardego i kluczy szyfrowania
- Włącz szyfrowanie dla nowych IaaS maszyny wirtualne utworzone na podstawie zdjęcia z galerii Azure
- Włącz szyfrowanie dla istniejących IaaS maszyny wirtualne z platformy Azure
- Wyłączanie szyfrowania na maszyny wirtualne IaaS systemu Windows
- Wyłączanie szyfrowania na dyskach danych dla maszyny wirtualne IaaS Linux

Rozwiązanie dla maszyny IaaS wirtualne włączenie platformy Microsoft Azure obsługuje następujące czynności:

- Integracja z magazynu klucza Azure
- Standardowa warstwa maszyny wirtualne — [A, D, DS, G, maszyny wirtualne IaaS serii GS itp](https://azure.microsoft.com/pricing/details/virtual-machines/)
- Włącz szyfrowanie dla systemu Windows i Linux oraz IaaS w maszyny wirtualne
- Wyłączanie szyfrowania na dyskach systemu operacyjnego i danych dla maszyny wirtualne IaaS systemu Windows
- Wyłączanie szyfrowania na dyskach danych dla maszyny wirtualne IaaS Linux
- Włącz szyfrowanie dla IaaS maszyny wirtualne systemu operacyjnego klienta systemu Windows
- Włącz szyfrowanie ilości ze ścieżkami instalacji
- Włącz szyfrowanie dla maszyny wirtualne Linux skonfigurowano system programowy RAID 
- Włącz szyfrowanie dla systemu Windows maszyny wirtualne skonfigurowany ze spacjami miejsca do magazynowania
- Obsługiwane są wszystkie regiony publicznej Azure

Rozwiązanie nie obsługuje następujących scenariuszy, funkcji i technologii w wersji:

- Podstawowe warstwa IaaS maszyny wirtualne
- Należy wyłączyć szyfrowanie na dysku komputera z systemem operacyjnym maszyny wirtualne IaaS Linux
- Maszyny wirtualne IaaS utworzony przy użyciu metody tworzenia maszyn wirtualnych klasyczny
- Integracja z lokalnej usługi zarządzania kluczami
- Systemu Windows Server 2016 Technical Preview nie jest obsługiwane w tej wersji
- Azure plików (udziale plików Azure), sieciowy system plików (NFS), dynamiczne wielkości, Windows maszyny wirtualne skonfigurowany z systemami programowy RAID


### <a name="encryption-features"></a>Funkcje szyfrowania

Gdy włączysz i wdrażanie szyfrowanie dysków Azure maszyny wirtualne IaaS Azure następujące funkcje są włączone, w zależności od konfiguracji opisane:

- Szyfrowanie woluminu OS do ochrony objętość uruchamiania w pozostałych w magazynie klienta
- Szyfrowanie danych woluminu s ochrony ilości danych w pozostałych w magazynie klienta
- Wyłączanie szyfrowania na dyskach systemu operacyjnego i danych dla maszyny wirtualne IaaS systemu Windows
- Wyłączanie szyfrowania na dyskach danych dla maszyny wirtualne IaaS Linux
- Informacje dotyczące zabezpieczania kluczy szyfrowania i hasła klienta Azure magazynu klucza subskrypcji
- Raportowanie stanu szyfrowania zaszyfrowanych maszyn wirtualnych IaaS
- Usuwanie ustawienia konfiguracji szyfrowania dysku z maszyny wirtualnej IaaS

Szyfrowanie dysków Azure IaaS maszyny Wirtualne dla systemu Windows i Linux oraz rozwiązania zawierają rozszerzenie szyfrowania dysku systemu Windows, rozszerzenie szyfrowania dysku Linux, poleceń cmdlet środowiska PowerShell szyfrowania dysku, dysku szyfrowania interfejsu wiersza polecenia cmdlet i szablony Menedżera zasobów Azure szyfrowania dysku. Rozwiązanie szyfrowania Azure dysku jest obsługiwana w IaaS maszyny wirtualne systemem Windows lub z systemem operacyjnym Linux. Aby uzyskać więcej informacji o obsługiwane systemy operacyjne Zobacz poniższą sekcję wymagania wstępne.

**Uwaga**: jest bez dodatkowych opłat do szyfrowania dysków maszyn wirtualnych przy użyciu szyfrowania dysku Azure.

### <a name="value-proposition"></a>Korzyści użytkowania

Rozwiązanie zarządzania szyfrowania dysku Azure udostępnia następujące potrzeb biznesowych w chmurze:

-   Maszyn IaaS są zabezpieczone spoczynku przy użyciu standardowe szyfrowanie technologie adres organizacji bezpieczeństwa i zgodności z przepisami.
-   Maszyn IaaS uruchamiania w obszarze klienta klawiszy i zasady, oraz ich inspekcji ich zastosowania w klucza magazynu.


### <a name="encryption-workflow"></a>Szyfrowanie przepływu pracy

Wysokiego poziomu kroki wymagane w celu umożliwienia szyfrowania dysku dla systemu Windows i Linux oraz maszyn są:

1. Klient wybiera scenariusz szyfrowania z powyższych scenariuszach szyfrowania
2. Odbiorcy wybranych Włączanie szyfrowania dysku za pośrednictwem szablonu Menedżera zasobów szyfrowania Azure dysku lub polecenia cmdlet PS lub polecenie polecenia i Określa konfigurację szyfrowania

    - Dla tego scenariusza wirtualnego dysku twardego klienta szyfrowane przekazywania klienta zaszyfrowanych wirtualny dysk twardy do ich miejsca do magazynowania konta i szyfrowania materiału klucza swój klucz magazynu i podaj Konfiguracja szyfrowania, aby włączyć szyfrowanie na nowe maszyny IaaS
    - Dla nowych maszyn wirtualnych utworzone na podstawie galerii Azure oraz maszyn istniejących już uruchomiony w Azure, nabywca poda Konfiguracja szyfrowania, aby włączyć szyfrowanie na maszyn wirtualnych IaaS

3. Klienta udziela dostępu do platformy Azure czytanie materiału klucza szyfrowania (systemy BitLocker szyfrowania klawiszy dla systemu Windows i hasło dla Linux) z ich klucza magazynu, aby włączyć szyfrowanie na maszyn wirtualnych IaaS
4. Nabywca poda Azure AD tożsamości aplikacji do zapisu materiału klucza szyfrowania ich klucza magazynu, aby włączyć szyfrowanie na maszyn wirtualnych IaaS scenariuszach wymienionych w #2 powyżej
5.  Azure aktualizuje model usługi maszyn wirtualnych szyfrowania i konfiguracji klucza magazynu i przepisy szyfrowane maszyn wirtualnych dla klienta

![Microsoft ochrony przed złośliwym oprogramowaniem platformy Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a>Odszyfrowywanie przepływu pracy

Wysokiego poziomu kroki wymagane do wyłączyć szyfrowanie dysku maszyn IaaS są:

1. Odbiorca zdecyduje się wyłączyć szyfrowanie (odszyfrowywanie) na uruchomionego maszyn wirtualnych IaaS platformy Azure za pomocą szablonu Menedżera zasobów szyfrowania Azure dysku lub PS poleceń cmdlet i Określa konfigurację odszyfrowywanie.
2. Krok szyfrowania Wyłącz wyłącza szyfrowania woluminu systemu operacyjnego lub danych lub obu tych elementów na uruchomionego maszyn wirtualnych IaaS systemu Windows. Jednak OS wyłączania dysku szyfrowania Linux nie jest obsługiwane wymienione w dokumentacji powyżej. Krok Wyłącz jest dozwolone tylko w przypadku dysków na maszyny wirtualne Linux. 
4. Azure aktualizuje model usługi maszyn wirtualnych i maszyn wirtualnych IaaS oznaczono odszyfrowane. Zawartość maszyn wirtualnych nie są już szyfrowane spoczynku.
5. Wyłącz operacja szyfrowania nie powoduje usunięcia magazynu kluczy klienta i materiału klucza szyfrowania-BitLocker szyfrowania klawiszy dla systemu Windows lub hasło Linux.

## <a name="prerequisites"></a>Wymagania wstępne

Poniżej przedstawiono wymagania wstępne dotyczące włączyć szyfrowanie dysku Azure na maszyny wirtualne IaaS Azure obsługiwane scenariuszy wymienionym w sekcji Przegląd

- Użytkownik musi mieć prawidłowy aktywną subskrypcję Azure utworzyć zasoby w Azure w regionach obsługiwane
- Azure szyfrowania dysku jest obsługiwana w następujących systemu Windows server SKU osoby — Windows Server 2008 R2, Windows Server 2012 i Windows Server 2012 R2. Systemu Windows Server 2016 Technical Preview nie jest obsługiwane w tej wersji.
- Azure szyfrowanie dysku jest obsługiwane na następujące komputerze klienckim SKU osoby — systemu Windows 8 i klienta systemu Windows 10.

**Uwaga**: dla systemu Windows Server 2008 R2, .net framework 4,5 musi być zainstalowana przed włączeniem szyfrowania platformy Azure. Możesz zainstalować go z witryny Windows update, instalując aktualizacja opcjonalna "Microsoft .NET Framework w 4.5.2 dla systemu Windows Server 2008 R2 64-bitowych systemów ([KB2901983](https://support.microsoft.com/kb/2901983))"

- Azure szyfrowania dysku jest obsługiwana w następujących serwer Linux SKU - Ubuntu, CentOS, SUSE i SUSE Linux Enterprise Server (SLES) i Red funkcję Enterprise Linux.

**Uwaga**: system operacyjny Linux dysku szyfrowania jest obecnie obsługiwana w następujących CentOS dystrybucji - RHEL 7.2 Linux 7.2, Ubuntu 16.04

- Wszystkie zasoby (Ex: Klucz magazynu, magazyn kont, maszyn wirtualnych itd.,) musi należeć do samego regionu Azure i subskrypcji.

**Uwaga**: szyfrowanie dysku Azure wymaga, że magazynu klucza i maszyny wirtualne znajdują się w tym samym regionie Azure. Konfigurowanie je w regionie osobnych spowoduje błąd w włączenie funkcji szyfrowania Azure dysku.

- Instalowanie i konfigurowanie Azure klucza magazynu użycia szyfrowania dysku Azure, sekcji **ustawienie i konfigurowanie Azure klucza magazynu użycia szyfrowania dysku Azure** w sekcji *wymagania wstępne dotyczące* tego artykułu.
- Instalowanie i konfigurowanie aplikacji Azure AD w usłudze Azure Active directory użycia szyfrowania dysku Azure, sekcji w sekcji *wymagania wstępne dotyczące* **instalacji aplikacji Azure AD w usłudze Azure Active Directory** tego artykułu.
- Konfigurowanie i skonfigurować zasady dostępu magazynu klucza stosowania Azure AD, zobacz **Ustawienia dostępu do magazynu klucz zasad stosowania Azure AD** w sekcji *wymagania wstępne dotyczące* części tego artykułu.
- Aby przygotować wstępnie zaszyfrowanych wirtualnego dysku twardego systemu Windows, zobacz sekcję **Przygotowywanie wstępnie zaszyfrowanych wirtualnego dysku twardego systemu Windows** w dodatku w tym artykule.
- Przygotowywanie wirtualnego dysku twardego Linux wstępnie zaszyfrowane, sekcji **Przygotowywanie wirtualnego dysku twardego Linux wstępnie zaszyfrowane** w dodatku tego artykułu.
- Platformy Azure musi mieć dostęp do kluczy szyfrowania lub hasła klienta Azure klucza magazynu, aby można było uzyskiwać do nich dostęp maszyn wirtualnych, aby uruchomić i odszyfrowywanie wielkość maszyn wirtualnych systemu operacyjnego. Aby udzielić uprawnień dostępu do klienta klucza magazynu platformy Azure, właściwości **enabledForDiskEncryption** musi być ustawiona na magazynu klucza tego wymogu. Zapoznaj się z sekcją **ustawienie i konfigurowanie Azure klucza magazynu użycia szyfrowania dysku Azure** w dodatku w tym artykule, aby uzyskać więcej informacji.
- Tajny klucz magazynu i adresy URL (KEK) klucza szyfrowania muszą być wersji. Azure wymusza to ograniczenie przechowywania wersji. Zobacz poniżej przykłady prawidłowych hasło i KEK adres URL:
    - Przykład prawidłowego adresu URL tajne:   *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
    - Przykłady prawidłowych KEK KRK:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
- Szyfrowanie Azure dysków nie obsługuje numery portów jest określony jako część magazynu klucz tajny i adresy URL KEK. Poniżej podano przykłady obsługiwanych adres URL magazynu klucz:
    - Adres URL magazynu klucza niezaakceptowanych   *https://contosovault.vault.azure.net:443-hasła contososecret-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
    - Adres URL zaakceptowanych klucza magazynu:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
- Aby włączyć szyfrowanie dysku Azure funkcji maszyny wirtualne IaaS musi spełniać następujące wymagania konfiguracji punktu końcowego sieci: 
    - Maszyn wirtualnych IaaS muszą mieć możliwość nawiązywanie połączenia z punktu końcowego usługi Azure Active Directory \[Login.windows.net\] uzyskanie tokenu nawiązać Azure klucza magazynu
    - Maszyn wirtualnych IaaS muszą mieć możliwość połączenia się z magazynu klucza Azure końcowym do zapisu klawiszy skonfigurowała magazynu kluczy klienta
    - Maszyn wirtualnych IaaS muszą mieć możliwość nawiązywanie połączenia z repozytorium rozszerzenia Azure i konto Azure miejsca do magazynowania, które pliki wirtualnego dysku twardego punkt końcowy Azure miejsca do magazynowania

**Uwaga:** Jeśli zasady dotyczące zabezpieczeń ogranicza dostępu za pośrednictwem SMS Azure Internetem, można rozwiązać powyżej identyfikatora URI, do którego potrzebujesz łączności i konfigurowanie określonej reguły, aby umożliwić nawiązywanie połączenia wychodzące adresy IP.

- Najnowsza wersja pakietu SDK programu PowerShell Azure wersji umożliwia skonfigurowanie szyfrowanie dysków Azure. Pobierz najnowszą wersję programu [Azure PowerShell Zwolnij](https://github.com/Azure/azure-powershell/releases)

**Uwaga:** Azure szyfrowanie dysków nie jest obsługiwane na [Zestawu SDK programu PowerShell Azure w wersji 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016). Jeśli wyświetlany jest błąd związany przy użyciu programu PowerShell Azure 1.1.0, zobacz artykuł [Azure dysku szyfrowania błędów związanych z Azure programu PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).

- Aby uruchomić jakichkolwiek poleceń polecenie Azure i skojarzyć subskrypcji Azure, należy najpierw zainstalować wersję Azure polecenie:
    - Aby zainstalować polecenie Azure i skojarzyć subskrypcji Azure, zobacz, [jak zainstalować i skonfigurować polecenie Azure](../xplat-cli-install.md)
    - Za pomocą interfejsu wiersza polecenia Azure dla komputerów Mac, Linux i systemu Windows przy użyciu Menedżera zasobów Azure, zobacz [poniżej](azure-cli-arm-commands.md)
- Rozwiązanie szyfrowania dysku Azure za pomocą zewnętrznego ochrona klucza funkcji BitLocker dla maszyny wirtualne IaaS systemu Windows. Jeśli do maszyny wirtualne są domeny sprzężone, nie push zasad grupy Wymuszaj TPM funkcje ochrony. Zobacz [Ten artykuł](https://technet.microsoft.com/library/ee706521) , aby uzyskać szczegółowe informacje na temat zasad grupy dla "Zezwalaj funkcji BitLocker bez zgodnego modułu TPM".
- Azure dysku szyfrowania wstępne skrypt programu PowerShell do tworzenia aplikacji Azure AD, tworzenie nowego klucza magazynu lub istniejącego magazynu klucza konfiguracji i włączyć szyfrowanie znajduje się [poniżej](https://github.com/Azure/azure-powershell/blob/dev/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).

#### <a name="setup-the-azure-ad-application-in-azure-active-directory"></a>Konfigurowanie aplikacji Azure AD w usłudze Azure Active Directory

Jeśli szyfrowania musi być włączona uruchomionego maszyn wirtualnych w Azure, szyfrowanie dysków Azure generuje i zapisuje klucze szyfrowania z magazynu klucza. Zarządzania kluczami szyfrowania w klucza magazynu wymaga uwierzytelniania Azure AD.

W tym celu należy utworzyć aplikację Azure AD. Uzyskać szczegółowe instrukcje dotyczące rejestrowania aplikacji znajdują się w tym miejscu, w sekcji sekcja "Pobierz tożsamości dla aplikacji" w tym [blogu](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).  Ten wpis zawiera również liczbę praktyczne przykłady o inicjowania obsługi administracyjnej i konfigurowaniu usługi magazynu klucza. Na potrzeby uwierzytelniania uwierzytelniania opartego na tajny klienta lub mogą być używane uwierzytelnianie klienta na podstawie certyfikatu Azure AD.

##### <a name="client-secret-based-authentication-for-azure-ad"></a>Tajny klienta podstawie uwierzytelniania dla Azure AD

Sekcjach masz niezbędne czynności, aby skonfigurować klienta tajne zależności uwierzytelniania dla Azure AD.

##### <a name="create-a-new-azure-ad-app-using-azure-powershell"></a>Tworzenie nowej aplikacji Azure AD przy użyciu programu PowerShell Azure

Utwórz nowy za pomocą polecenia cmdlet programu PowerShell poniżej Azure AD aplikacji:

    $aadClientSecret = “yourSecret”
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

**Uwaga:** $azureAdApplication.ApplicationId jest Azure AD ClientID i $aadClientSecret jest klienta hasła, które należy użyć później, aby włączyć MDE. Tajny klienta Azure AD powinny chronić prawidłowo.


##### <a name="provisioning-the-azure-ad-client-id-and-secret-from-the-azure-classic-deployment-model-portal"></a>Obsługa administracyjna Azure AD identyfikator klienta i hasło z Azure klasycznego modelu wdrożenia portalu

Azure identyfikator klienta AD i hasło można także zapewnić przy użyciu Azure klasycznego modelu wdrożenia portalu na https://manage.windowsazure.com, wykonaj poniższe czynności, aby wykonać to zadanie:

1. Kliknij kartę usługi Active Directory, jak pokazano na poniższym rysunku:

![Szyfrowanie dysków Azure](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2. kliknij polecenie Dodaj aplikację, a następnie wpisz nazwę aplikacji, tak jak pokazano poniżej:

![Szyfrowanie dysków Azure](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3.w kliknij przycisk strzałki i skonfiguruj właściwości tej aplikacji, tak jak pokazano poniżej:

![Szyfrowanie dysków Azure](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4. Kliknij znacznik wyboru w lewym dolnym rogu, aby zakończyć. Zostanie wyświetlona strona Konfiguracja tej aplikacji. Zwróć uwagę, że identyfikator Azure AD klienta znajduje się w dolnej części strony, jak pokazano na poniższym rysunku.

![Szyfrowanie dysków Azure](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5 Zapisz hasło Azure AD klienta, kliknij przycisk Zapisz. Kliknij przycisk Zapisz przycisk i zanotuj hasło w polu tekstowym klawiszy, to jest Azure AD tajny klienta. Tajny klienta Azure AD powinny chronić prawidłowo.

![Szyfrowanie dysków Azure](./media/azure-security-disk-encryption/disk-encryption-fig7.png)


**Uwaga:** tego przepływu powyżej nie jest obsługiwana w portalu.

##### <a name="use-an-existing-app"></a>Użyj istniejącej aplikacji

Aby wykonać poniższe polecenia potrzebne moduł Azure AD programu PowerShell, który można znaleźć [tutaj](https://technet.microsoft.com/library/jj151815.aspx).

**Uwaga:** polecenia poniżej muszą zostać wykonane w nowym oknie programu PowerShell. Aby wykonać te polecenia nie należy używać programu PowerShell Azure lub w oknie Menedżer zasobów Azure. Przyczyną tego zalecenia jest ponieważ tych poleceń cmdlet znajdują się w MSOnline module lub Azure AD programu PowerShell.

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your AAD app>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a>Certyfikat oparty uwierzytelniania dla Azure AD

> [AZURE.NOTE] Uwierzytelnianie certyfikatów podstawie AAD nie jest obecnie obsługiwane na maszyny wirtualne Linux.

Sekcjach masz niezbędne kroki, aby skonfigurować uwierzytelnianie certyfikatu opartego Azure AD.

##### <a name="create-a-new-azure-ad-app"></a>Tworzenie nowej aplikacji Azure AD

Wykonanie polecenia cmdlet programu PowerShell poniżej, aby utworzyć nową aplikację Azure AD:

**Uwaga:** Zamienianie `yourpassword` ciąg poniżej za pomocą bezpiecznego hasła i chronić hasła.

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

Po zakończeniu tej czynności przekazać plik PFX do magazynu klucza i Włącz zasadę dostępu, aby wdrożyć ten certyfikat maszyny.

##### <a name="use-an-existing-azure-ad-app"></a>Użyj istniejącej aplikacji Azure AD
Jeśli konfigurujesz uwierzytelniania certyfikatu opartego na istniejącej aplikacji za pomocą poleceń cmdlet środowiska PowerShell poniżej. Upewnij się wykonać je w nowym oknie programu PowerShell.

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your AAD app>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

Po zakończeniu tej czynności przekazać plik PFX do magazynu klucza i Włącz zasadę dostępu, aby wdrożyć ten certyfikat maszyny.

##### <a name="upload-a-pfx-file-to-key-vault"></a>Przekaż plik PFX do magazynu klucza
Można przeczytać, to [w blogu](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx) szczegółowe wyjaśnienie, na czym polega ten proces. Jednak poniżej poleceń cmdlet środowiska PowerShell jest potrzebne do tego zadania. Upewnij się, że wykonywanie ich z konsoli Azure programu PowerShell:

**Uwaga:** Zamienianie `yourpassword` ciąg poniżej za pomocą bezpiecznego hasła i chronić hasła.

    $certLocalPath = 'C:\certs\myaadapp.pfx'
    $certPassword = "yourpassword"
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’

    $fileContentBytes = get-content $certLocalPath -Encoding Byte
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

    $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certPassword"
    }
    "@

    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

    Switch-AzureMode -Name AzureResourceManager
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName –EnabledForDeployment

##### <a name="deploy-a-certificate-in-key-vault-to-an-existing-vm"></a>Wdrażanie certyfikatu w klucza magazynu istniejącego maszyn wirtualnych
Po zakończeniu przekazywania PFX, umożliwia Wdroż certyfikatu w klucza magazynu istniejącego maszyn wirtualnych poniższe czynności:

    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName


#### <a name="setting-key-vault-access-policy-for-the-azure-ad-application"></a>Ustawianie zasad klucza magazynu w programie Access na podstawie Azure AD

Aplikacja Azure AD wymaga praw dostępu do kluczy lub hasła magazyn. Polecenie cmdlet [Set-AzureKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/dn903607.aspx) umożliwia udzielanie uprawnień do aplikacji przy użyciu identyfikator klienta (który został wygenerowany, gdy aplikacja została zarejestrowana) jako wartości parametru — ServicePrincipalName. [Ten wpis w blogu](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx) kilka przykładów, na której można czytać. Poniżej masz również przykładowy sposób wykonać to zadanie przy użyciu programu PowerShell:

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

**Uwaga**: szyfrowanie dysków Azure należy skonfigurować następujące zasady dostępu do aplikacji klienckich AAD - uprawnienia "WrapKey" i "Set"

## <a name="terminology"></a>Terminologia

Skorzystaj z tabeli terminologia jako odwołanie do zrozumienia typowe terminy używane przez tej technologii:

| Terminologia           | Definicja                                                                                                                                                                                                                                   |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Azure AD                   | Azure AD jest [Usługi Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Konto Azure AD jest wstępnie wymagane do uwierzytelniania, przechowywania i pobierania hasła z magazynu klucza.                                                                                                        |
| Azure magazynu klucza [AKV] | Azure magazynu klucza to usługa zarządzania kluczami szyfrowania według sprawdzana poprawność FIPS moduły sprzętu w celu zapewnienia usługi klucze szyfrowania i hasła poufnych bezpieczne., zapoznaj się z dokumentacją [Magazynu klucz](https://azure.microsoft.com/services/key-vault/) , aby uzyskać więcej informacji.          |
| ARM                   | Azure Menedżera zasobów                                                                                                                                                                                                                       |
| Funkcji BitLocker             | [Funkcja BitLocker](https://technet.microsoft.com/library/hh831713.aspx) jest branżowe rozpoznany technologię szyfrowania woluminu systemu Windows umożliwia szyfrowanie dysków na maszyny wirtualne IaaS systemu Windows                                                                                                                  |
| BEK                   | Klucze szyfrowania funkcji BitLocker są używane do szyfrowania woluminu uruchamiania systemu operacyjnego i ilości danych. Klucze funkcji BitLocker są ochronę informacji w Azure magazynu kluczy klienta jako hasła.                                                                              |
| POLECENIE                   | [Azure interfejsu wiersza polecenia](../xplat-cli-install.md)                                                                                                                                                                                                                 |
| DM kryptograficznego              | [Metoda Crypt DM](https://en.wikipedia.org/wiki/Dm-crypt) jest podsystemu szyfrowania przezroczysty dysku z systemem Linux umożliwia szyfrowanie dysków na pośrednictwem Linux IaaS SMS                                                                                                                           |
| KEK                   | Klucz szyfrowania jest kluczem asymetrycznym (RSA 2048) używane do ochrony lub zawijanie tego hasła, jeśli to konieczne. Można udostępnić chroniony HSM oprogramowanie chronione klawisz lub. Aby uzyskać więcej informacji zapoznaj się z dokumentacją [Azure klucza magazynu](https://azure.microsoft.com/services/key-vault/) Aby uzyskać więcej informacji |
| PS poleceń cmdlet.            | [Polecenia cmdlet programu PowerShell Azure](powershell-install-configure.md)                                                                                                                                                                                                                                           |

### <a name="setting-and-configuring-azure-key-vault-for-azure-disk-encryption-usage"></a>Ustawienie i konfigurowanie Azure klucza magazynu dla Azure dysku zastosowania szyfrowania

Szyfrowanie dysków Azure chroni kluczy szyfrowania dysku i hasła usługi Azure klucza magazynu. Wykonaj kroki w poszczególnych sekcjach poniżej konfigurację magazynu klucza użycia szyfrowania Azure dysku.

#### <a name="create-a-new-key-vault"></a>Tworzenie nowego magazynu klucza
Aby utworzyć nowy magazyn klucza, użyj jednej z opcji wymienionych poniżej:

- Szablon "101 — tworzenie-KeyVault" Menedżera zasobów znajduje się [w tym miejscu](https://github.com/Azure/azure-quickstart-templates/blob/master/101-create-key-vault/azuredeploy.json)
- Za pomocą programu PowerShell Azure [poleceń cmdlet klucza magazynu](https://msdn.microsoft.com/library/dn868052.aspx).
- Za pomocą portalu Menedżera zasobów Azure.

**Uwaga:** Jeśli masz już konfiguracji magazynu klucz za subskrypcję, przejdź do następnej sekcji.

![Azure klucza magazynu](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="provisioning-a-key-encryption-key-optional"></a>Obsługa administracyjna szyfrowania klucz (opcjonalnie)

Jeśli chcesz zawijanie kluczy szyfrowania funkcji BitLocker za pomocą klucza szyfrowania klucza (KEK) w przypadku dodatkowej warstwy zabezpieczeń, należy dodać KEK do magazynu usługi klucz do użycia w procesie inicjowania obsługi administracyjnej.  Aby utworzyć nowy klucz szyfrowania w magazynu klucz, należy użyć polecenia cmdlet [AzureKeyVaultKey Dodaj](https://msdn.microsoft.com/library/dn868048.aspx) . Możesz też zaimportować KEK z usługi zarządzania kluczami lokalnego HSM. Aby uzyskać więcej informacji można znaleźć w temacie [Klucza magazynu](https://azure.microsoft.com/documentation/services/key-vault/)dokumentacji.

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

KEK można dodać z portal Azure Menedżera zasobów także przy użyciu UX. magazynu klucza Azure

![Azure klucza magazynu](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions-to-allow-the-azure-platform-access-to-the-keys-and-secrets"></a>Ustawianie uprawnień do magazynu klucza umożliwienie dostępu platformy Azure do klucze i hasła

Platformy Azure musi mieć dostęp do kluczy szyfrowania lub hasła usługi Azure klucza magazynu, aby można było uzyskiwać do nich dostęp do maszyn wirtualnych do uruchamiania i odszyfrowywania wielkość. Aby udzielić uprawnień do platformy Azure, tak, aby można uzyskać dostęp do magazynu klucza, właściwości *enabledForDiskEncryption* musi być ustawiona na magazynu klucza. Właściwość enabledForDiskEncryption można ustawić na swojej klucza magazynu przy użyciu klucza magazynu PS polecenia cmdlet:

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

Można także ustawić właściwość *enabledForDiskEncryption* , odwiedź witrynę https://resources.azure.com. Wymienione przed, należy ustawić właściwość *enabledForDiskEncryption* na magazynu usługi klucza. W przeciwnym razie wdrażanie zakończy się niepowodzeniem.

Możesz skonfigurować zasady dostępu dla aplikacji AAD z interfejsu użytkownika magazynu klucz:

![Azure klucza magazynu](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure klucza magazynu](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

Upewnij się, czy klucz magazynu jest włączona na potrzeby szyfrowania dysku w "Zaawansowane zasady dostępu":

![Azure klucza magazynu](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a>Scenariusze wdrażania szyfrowania dysku i środowiska użytkownika

Istnieje wiele scenariuszy, że możesz włączyć szyfrowanie dysków i kroki mogą się różnić zgodnie z tego scenariusza. Sekcjach obejmuje bardziej szczegółowe scenariuszom.

### <a name="enable-encryption-on-new-iaas-vms-created-from-the-azure-gallery"></a>Włącz szyfrowanie dla nowych maszyn IaaS utworzone z galerii Azure

Szyfrowanie dysków można włączyć dla nowych maszyn wirtualnych systemu Windows IaaS z galerii Azure platformy Azure za pomocą tego szablonu Menedżera zasobów opublikowanych [w tym miejscu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image). Kliknij przycisk "Wdrażanie do Azure" na tym szablonie Azure Szybki Start Konfiguracja szyfrowania wprowadzania w karta parametry, a następnie kliknij przycisk OK. Wybierz subskrypcję, grupa zasobów, lokalizacji grupa zasobów, warunki prawne i umowy i kliknij przycisk Utwórz, aby włączyć szyfrowanie na nowe maszyny IaaS.

**Uwaga:** Ten szablon umożliwia utworzenie nowego zaszyfrowanych maszyny systemu Windows za pomocą obrazu z galerii systemu Windows Server 2012.

Szyfrowanie dysków można włączyć na nowy IaaS RedHat Linux 7.2 maszyn wirtualnych z macierz RAID 0 200 GB za pomocą [tego](https://aka.ms/fde-rhel) szablonu Menedżera zasobów. Po wdrożeniu szablonu upewnij się, maszyn wirtualnych szyfrowania stanu z `Get-AzureRmVmDiskEncryptionStatus` polecenia cmdlet, zgodnie z opisem w sekcji "[dysku szyfrowania systemu operacyjnego uruchomionego maszyny Linux](#encrypting-os-drive-on-a-running-linux-vm)". Jeśli komputer zwraca stan `VMRestartPending`, uruchom ponownie maszyn wirtualnych.

Szczegóły parametrów szablonu Menedżera zasobów można wyświetlić dla nowych maszyn wirtualnych z galerii Azure scenariusza przy użyciu Identyfikatora Azure AD klienta w poniższej tabeli:

| Parametr                        | Opis|
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| adminUserName                 | Nazwa użytkownika administratora dla maszyny wirtualnej                                                                                                                           |
| adminPassword                 | Hasło użytkownika administratora dla maszyny wirtualnej                                                                                                                       |
| newStorageAccountName         | Nazwę konta magazynu do przechowywania systemu operacyjnego i danych wirtualnych dysków twardych                                                                                                             |
| vmSize                        | Rozmiar maszyn wirtualnych. Obecnie obsługiwane są tylko serię standardowy A, D i G                                                                                          |
| virtualNetworkName            | Nazwa VNet, do którego NIC maszyn wirtualnych powinna należeć do.                                                                                                            |
| subnetName                    | Nazwa podsieci w vNet, do którego NIC maszyn wirtualnych powinna należeć do                                                                                               |
| AADClientID                   | Identyfikator klienta aplikacji Azure AD, która ma uprawnienia do zapisu hasła klucza magazynu                                                                                       |
| AADClientSecret               | Tajny klienta aplikacji Azure AD, która ma uprawnienia do zapisu hasła klucza magazynu                                                                                   |
| keyVaultURL                   | Adres URL magazynu klucz, do którego BitLocker klucz powinny zostać przekazane do. Można uzyskać przy użyciu polecenia cmdlet: (Get AzureRmKeyVault - VaultName-ResourceGroupName). VaultURI |
| keyEncryptionKeyURL           | Adres URL szyfrowania klucz, który jest używany do szyfrowania generowany klucz funkcji BitLocker. Jest to opcjonalne.                                                               |
| keyVaultResourceGroup         | Grupa zasobów z magazynu klucza                                                               |
| vmName                        | Nazwa maszyn wirtualnych, na których szyfrowania ma zostać wykonane operacji


**Uwaga:** KeyEncryptionKeyURL jest opcjonalny parametr. Możesz zabrać ze sobą własne KEK do dalszej ochronę informacji klucza szyfrowania danych (tajny hasło) w klucza magazynu.

### <a name="enable-encryption-on-new-iaas-vms-created-from-customer-encrypted-vhd-and-encryption-keys"></a>Włącz szyfrowanie dla nowych maszyn IaaS utworzone za pomocą klawiszy wirtualnego dysku twardego szyfrowane klienta i szyfrowania

W tym scenariuszu można włączyć, ponieważ do szyfrowania przy użyciu szablonu Menedżera zasobów, poleceń cmdlet środowiska PowerShell lub poleceń interfejsu wiersza polecenia. W sekcjach poniżej będzie opisano bardziej szczegółowe, szablonu Menedżera zasobów i poleceń interfejsu wiersza polecenia.

Postępuj zgodnie z instrukcjami z jednej z tych sekcji dotyczące przygotowywania wstępnie zaszyfrowanych obrazy, które mogą być używane w Azure. Po utworzeniu obrazu kroki opisane w następnej sekcji służą do tworzenia zaszyfrowanych maszyn wirtualnych Azure.

- [Przygotowywanie przed zaszyfrowanych wirtualnego dysku twardego systemu Windows](#preparing-a-pre-encrypted-windows-vhd)
- [Przygotowywanie wirtualnego dysku twardego Linux wstępnie zaszyfrowanych](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-resource-manager-template"></a>Za pomocą szablonu Menedżera zasobów

Szyfrowanie dysków można włączyć dla klienta szyfrowane wirtualnego dysku twardego przy użyciu szablonu Menedżera zasobów opublikowanych [w tym miejscu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm). Kliknij przycisk "Wdrażanie do Azure" na tym szablonie Azure Szybki Start Konfiguracja szyfrowania wprowadzania w karta parametry, a następnie kliknij przycisk OK. Wybierz subskrypcję, grupa zasobów, lokalizacji grupa zasobów, warunki prawne i umowy i kliknij przycisk Utwórz, aby włączyć szyfrowanie na nowy IaaS maszyn wirtualnych.

W poniższej tabeli opisano szczegóły parametrów szablonu Menedżera zasobów scenariusz wirtualnego dysku twardego klienta szyfrowane:

| Parametr                        | Opis|
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| newStorageAccountName | Nazwa konta miejsca do magazynowania do przechowywania zaszyfrowanych OS wirtualnego dysku twardego. To konto miejsca do magazynowania powinien zostały już utworzone w tej samej grupy zasobów i w tym samym miejscu jako maszyn wirtualnych                                                     |
| osVhdUri              | Identyfikator URI OS wirtualnego dysku twardego z konta miejsca do magazynowania                                                                                                                                                                                      |
| osType                | Typ produktu systemu operacyjnego (Windows i Linux).                                                                                                                                                                                         |
| virtualNetworkName    | Nazwa VNet, do którego NIC maszyn wirtualnych powinna należeć do. To należy zostały już utworzone w tej samej grupy zasobów i w tym samym miejscu jako maszyn wirtualnych                                                                     |
| subnetName            | Nazwa podsieci w vNet, do którego NIC maszyn wirtualnych powinna należeć do                                                                                                                                                     |
| vmSize                | Rozmiar maszyn wirtualnych. Obecnie obsługiwane są tylko serię standardowy A, D i G                                                                                                                                                |
| keyVaultResourceID    | Identyfikowanie klucza ResourceID magazynu zasobów w imieniu. Można uzyskać przy użyciu polecenia cmdlet programu PowerShell: (Get AzureRmKeyVault - VaultName &lt;yourKeyVaultName&gt; - ResourceGroupName &lt;yourResourceGroupName&gt;). ResourceId |
| keyVaultSecretUrl     | Adres URL klucza szyfrowania dysku obsługi administracyjnej w klucza magazynu                                                                                                                                                                |
| keyVaultKekUrl        | Adres URL klucz szyfrowania jest szyfrowanie klucza szyfrowania wygenerowanych dysku                                                                                                                                       |
| vmName               | Nazwa maszyn wirtualnych IaaS   |


#### <a name="using-powershell-cmdlets"></a>Przy użyciu poleceń cmdlet programu PowerShell

Szyfrowanie dysków można włączyć dla klienta szyfrowane wirtualnego dysku twardego przy użyciu polecenia cmdlet PS opublikowanych [w tym miejscu](https://msdn.microsoft.com/library/azure/mt603746.aspx).  

#### <a name="using-cli-commands"></a>Za pomocą poleceń interfejsu wiersza polecenia

Wykonaj poniższe czynności, aby włączyć szyfrowanie dysków w tym scenariuszu za pomocą poleceń interfejsu wiersza polecenia:

1. Ustawić zasady dostępu dla magazynu klucz:
    - Możesz ustawić flagę "EnabledForDiskEncryption":`azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
    - Ustawianie uprawnień do aplikacji Azure AD zapisywanie hasła KeyVault:`azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]`
2. Aby włączyć szyfrowanie na istniejące uruchamianie maszyn wirtualnych, wpisz:  *azure maszyn wirtualnych Włącz dysku szyfrowanie — grupa zasobów <resourceGroupName> — nazwa <vmName> — aad klienta identyfikator <aadClientId> — aad klucz tajny klienta <aadClientSecret> — dysku szyfrowania klucza magazynu url <keyVaultURL> — dysku szyfrowania klucza magazynu id <keyVaultResourceId> *
3. Pobierz stan szyfrowania: *"azure maszyn wirtualnych Pokaż dysku szyfrowania stanu — Grupa zasobów <resourceGroupName> — nazwa <vmName> — json"*
4. Aby włączyć szyfrowania na nowy maszyn wirtualnych z klienta szyfrowane wirtualny dysk twardy, należy użyć poniższe parametry z poleceniem "Tworzenie azure maszyn wirtualnych":
    - dysk szyfrowania klucza magazynu id < dysku szyfrowania klucza magazynu id >
    - dysk szyfrowania klucza url < dysku szyfrowania klucza url >
    - klucz szyfrowania klucza magazynu id < klucza szyfrowania id--klucza magazynu->
    - klucz szyfrowania klucza url < klucz szyfrowania klucza url >


### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a>Włącz szyfrowanie dla istniejących lub uruchamianie maszyn wirtualnych systemu Windows IaaS platformy Azure

W tym scenariuszu można włączyć, ponieważ do szyfrowania przy użyciu szablonu Menedżera zasobów, poleceń cmdlet środowiska PowerShell lub poleceń interfejsu wiersza polecenia. Poniższe sekcje będą opisano bardziej szczegółowe włącz go za pomocą szablonu Menedżera zasobów i poleceń interfejsu wiersza polecenia.

#### <a name="using-resource-manager-template"></a>Za pomocą szablonu Menedżera zasobów

Szyfrowanie dysków można włączyć na istniejące uruchamianie maszyn wirtualnych systemu Windows IaaS platformy Azure za pomocą tego szablonu Menedżera zasobów opublikowanych [w tym miejscu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm). Kliknij przycisk "Wdrażanie do Azure" na tym szablonie Azure Szybki Start Konfiguracja szyfrowania wprowadzania w karta parametry, a następnie kliknij przycisk OK. Wybierz subskrypcję, grupa zasobów, lokalizacji grupa zasobów, warunki prawne i umowy, a następnie kliknij przycisk Utwórz, aby Włącz szyfrowanie dla maszyn wirtualnych IaaS istniejące uruchomiony.

Szczegóły parametrów szablonu Menedżera zasobów w scenariuszu maszyn wirtualnych istniejące uruchomiony przy użyciu Identyfikatora Azure AD klienta są dostępne w poniższej tabeli:

| Parametr                 | Opis|
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AADClientID            | Identyfikator klienta aplikacji Azure AD, która ma uprawnienia do zapisu hasła klucza magazynu                                                                                                                                              |
| AADClientSecret         | Tajny klienta aplikacji Azure AD, która ma uprawnienia do zapisu hasła klucza magazynu                                                                                                                                          |
| keyVaultName | Nazwa magazynu klucz, do którego BitLocker klucz powinny zostać przekazane do. Można uzyskać przy użyciu polecenia cmdlet: (Get AzureRmKeyVault - ResourceGroupName <yourResourceGroupName>). Vaultname   |
|  keyEncryptionKeyURL   | Adres URL szyfrowania klucz, który jest używany do szyfrowania generowany klucz funkcji BitLocker. Opcjonalnie, jeśli wybierzesz `nokek` na liście rozwijanej UseExistingKek. Jeśli wybierzesz `kek` na liście rozwijanej UseExistingKek, musisz wprowadzić wartość keyEncryptionKeyURL                                                                                                                        |
| volumeType             | Typ woluminu, na które szyfrowania przeprowadzana jest operacja. Prawidłowe wartości to "OS", "Dane", "Wszystkie"                                                                                                                     |
| sequenceVersion         | Wersja sekwencji działania funkcji BitLocker. Zwiększyć ten numer wersji każdorazowo przeprowadzana jest operacja szyfrowania dysku na tym samym maszyn wirtualnych                                                                             |
| vmName                 | Nazwa maszyn wirtualnych, na których szyfrowania ma zostać wykonane operacji


**Uwaga:** KeyEncryptionKeyURL jest opcjonalny parametr. Możesz zabrać ze sobą własne KEK do dalszej ochronę informacji klucza szyfrowania danych (tajny szyfrowania funkcji BitLocker) w klucza magazynu.

#### <a name="using-powershell-cmdlets"></a>Przy użyciu poleceń cmdlet programu PowerShell

Zobacz wpis w blogu **Szyfrowanie dysków Azure zapoznanie z programem PowerShell Azure** [część 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) i [część 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx) , aby uzyskać szczegółowe informacje na temat włączania szyfrowania przy użyciu szyfrowania dysku Azure za pomocą poleceń cmdlet PS.

#### <a name="using-cli-commands"></a>Za pomocą poleceń interfejsu wiersza polecenia

Wykonaj poniższe czynności, aby włączyć szyfrowania istniejące i uruchamianie maszyn wirtualnych systemu Windows IaaS platformy Azure za pomocą poleceń interfejsu wiersza polecenia:

1. Ustawić zasady dostępu dla magazynu klucz:
    - Ustawienie flagi "EnabledForDiskEncryption": "azure keyvault zestawu zasady--nazwa magazynu <keyVaultName> — PRAWDA włączone dla dysku szyfrowanie"
    - Ustawianie uprawnień do aplikacji Azure AD zapisywanie hasła KeyVault: "azure keyvault zestawu zasady--nazwa magazynu <keyVaultName> — spn <aadClientID> — uprawnienia do kluczy [\"wszystkie\"] — uprawnienia do hasła [\"wszystkie\"]"
2. Aby włączyć szyfrowanie na istniejące uruchamianie maszyn wirtualnych, wpisz:  *azure maszyn wirtualnych Włącz dysku szyfrowanie — grupa zasobów <resourceGroupName> — nazwa <vmName> — aad klienta identyfikator <aadClientId> — aad klucz tajny klienta <aadClientSecret> — dysku szyfrowania klucza magazynu url <keyVaultURL> — dysku szyfrowania klucza magazynu id <keyVaultResourceId> *
3. Pobierz stan szyfrowania: *"azure maszyn wirtualnych Pokaż dysku szyfrowania stanu — Grupa zasobów <resourceGroupName> — nazwa <vmName> — json"*
4. Aby włączyć szyfrowania na nowy maszyn wirtualnych z klienta szyfrowane wirtualny dysk twardy, należy użyć poniższe parametry z poleceniem "Tworzenie azure maszyn wirtualnych":
    - dysk szyfrowania klucza magazynu id < dysku szyfrowania klucza magazynu id >
    - dysk szyfrowania klucza url < dysku szyfrowania klucza url >
    - klucz szyfrowania klucza magazynu id < klucza szyfrowania id--klucza magazynu->
    - klucz szyfrowania klucza url < klucz szyfrowania klucza url >


### <a name="enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure"></a>Włącz szyfrowanie dla istniejących lub uruchamianie maszyn wirtualnych Linux IaaS platformy Azure

Szyfrowanie dysków można włączyć na istniejące uruchamianie maszyn wirtualnych Linux IaaS platformy Azure za pomocą tego szablonu Menedżera zasobów opublikowanych [w tym miejscu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm). Kliknij przycisk "Wdrażanie do Azure" na tym szablonie Azure Szybki Start Konfiguracja szyfrowania wprowadzania w karta parametry, a następnie kliknij przycisk OK. Wybierz subskrypcję, grupa zasobów, lokalizacji grupa zasobów, warunki prawne i umowy, a następnie kliknij przycisk Utwórz, aby Włącz szyfrowanie dla maszyn wirtualnych IaaS istniejące uruchomiony.

W poniższej tabeli opisano szczegóły parametrów szablonu Menedżera zasobów w scenariuszu maszyn wirtualnych istniejące uruchomiony przy użyciu Identyfikatora Azure AD klienta:

| Parametr                 | Opis|
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AADClientID            | Identyfikator klienta aplikacji Azure AD, która ma uprawnienia do zapisu hasła klucza magazynu                                                                                                                                              |
| AADClientSecret         | Tajny klienta aplikacji Azure AD, która ma uprawnienia do zapisu hasła klucza magazynu                                                                                                                                          |
| keyVaultName | Nazwa magazynu klucz, do którego BitLocker klucz powinny zostać przekazane do. Można uzyskać przy użyciu polecenia cmdlet: (Get AzureRmKeyVault - ResourceGroupName <yourResourceGroupName>). Vaultname   |
|  keyEncryptionKeyURL   | Adres URL szyfrowania klucz, który jest używany do szyfrowania generowany klucz funkcji BitLocker. Jest to opcjonalne, jeśli na liście rozwijanej UseExistingKek wybierz pozycję "nokek". Jeśli wybierzesz "kek" na liście rozwijanej UseExistingKek, musisz wprowadzić wartość keyEncryptionKeyURL                                                                                                                        |
| volumeType             | Typ woluminu, na które szyfrowania przeprowadzana jest operacja. Prawidłowe obsługiwane są następujące wartości "OS" / "Wszystkie" (w przypadku RHEL 7.2, CentOS 7.2 i Ubuntu 16.04) i "Dane" dla wszystkich innych distros.                                                                                                                      |
| sequenceVersion         | Wersja sekwencji działania funkcji BitLocker. Zwiększyć ten numer wersji każdorazowo przeprowadzana jest operacja szyfrowania dysku na tym samym maszyn wirtualnych                                                                             |
| vmName                 | Nazwa maszyn wirtualnych, na których szyfrowania ma zostać wykonane operacji
| Hasło              | Wpisz silne hasło jako klucza szyfrowania danych                                                                                                                                                                       |                                                                                                                                                                                                                                                      

**Uwaga:** KeyEncryptionKeyURL jest opcjonalny parametr. Możesz zabrać ze sobą własne KEK do dalszej ochronę informacji klucza szyfrowania danych (tajny hasło) w klucza magazynu.

#### <a name="cli-commands"></a>Polecenie polecenia

Szyfrowanie dysków można włączyć dla klienta szyfrowane za pomocą polecenia interfejsu wiersza polecenia zainstalowano [tutaj](../xplat-cli-install.md)wirtualnego dysku twardego. Wykonaj poniższe czynności, aby włączyć szyfrowania istniejące i uruchamianie maszyn wirtualnych Linux IaaS platformy Azure za pomocą poleceń interfejsu wiersza polecenia:

1. Ustawić zasady dostępu dla magazynu klucz:
    - Ustawienie flagi "EnabledForDiskEncryption": "azure keyvault zestawu zasady--nazwa magazynu <keyVaultName> — PRAWDA włączone dla dysku szyfrowanie"
    - Ustawianie uprawnień do aplikacji Azure AD zapisywanie hasła KeyVault: "azure keyvault zestawu zasady--nazwa magazynu <keyVaultName> — spn <aadClientID> — uprawnienia do kluczy [\"wszystkie\"] — uprawnienia do hasła [\"wszystkie\"]"
2. Aby włączyć szyfrowanie na istniejące uruchamianie maszyn wirtualnych, wpisz:  *azure maszyn wirtualnych Włącz dysku szyfrowanie — grupa zasobów <resourceGroupName> — nazwa <vmName> — aad klienta identyfikator <aadClientId> — aad klucz tajny klienta <aadClientSecret> — dysku szyfrowania klucza magazynu url <keyVaultURL> — dysku szyfrowania klucza magazynu id <keyVaultResourceId> *
3. Pobierz stan szyfrowania: "azure maszyn wirtualnych Pokaż dysku szyfrowania stanu — Grupa zasobów <resourceGroupName> — nazwa <vmName> — json"
4. Aby włączyć szyfrowania na nowy maszyn wirtualnych z klienta szyfrowane wirtualny dysk twardy, należy użyć poniższe parametry z poleceniem "Tworzenie azure maszyn wirtualnych".
    - *dysk szyfrowania klucza magazynu id < dysku szyfrowania klucza magazynu id >*
    - *dysk szyfrowania klucza url < dysku szyfrowania klucza url >*
    - *klucz szyfrowania klucza magazynu id < klucza szyfrowania id--klucza magazynu->*
    - *klucz szyfrowania klucza url < klucz szyfrowania klucza url >*

### <a name="get-encryption-status-of-an-encrypted-iaas-vm"></a>Sprawdzić stan szyfrowania zaszyfrowanych maszyn wirtualnych IaaS

Możesz uzyskać stan szyfrowania przy użyciu portal Azure Menedżera zasobów, [poleceń cmdlet środowiska PowerShell](https://msdn.microsoft.com/library/azure/mt622700.aspx) lub poleceń interfejsu wiersza polecenia. Poniższych sekcjach wyjaśniono, jak sprawdzić stan szyfrowania za pomocą Azure portal i poleceń interfejsu wiersza polecenia.

#### <a name="get-encryption-status-of-an-encrypted-windows-vm-using-azure-resource-manager-portal"></a>Sprawdzić stan szyfrowania za pomocą Menedżera zasobów Azure portalu zaszyfrowanych maszyn wirtualnych systemu Windows

Stan szyfrowania maszyn wirtualnych IaaS możesz przejść z portal Azure Menedżera zasobów. Logowanie do portal Azure pod adresem https://portal.azure.com/, kliknij łącze maszyn wirtualnych w menu po lewej stronie, aby wyświetlić widok podsumowania maszyn wirtualnych w ramach subskrypcji. Widok maszyn wirtualnych można filtrować, wybierając z menu rozwijanego subskrypcji Nazwa subskrypcji. Polecenie kolumn znajdujących się u góry menu strony maszyn wirtualnych. Zaznacz kolumnę szyfrowanie dysków z karta wybierz kolumnę, a następnie kliknij przycisk Aktualizuj. Powinna być widoczna kolumna szyfrowania dysku przedstawiający stan szyfrowania "Enabled" lub "Nie włączono" dla każdego maszyn wirtualnych, jak pokazano na poniższej ilustracji.

![Microsoft ochrony przed złośliwym oprogramowaniem platformy Azure](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-encryption-status-of-an-encrypted-windowslinux-iaas-vm-using-disk-encryption-ps-cmdlet"></a>Sprawdzić stan szyfrowania szyfrowanego (Windows i Linux) IaaS maszyn wirtualnych przy użyciu szyfrowania dysku polecenia Cmdlet PS
Stan szyfrowania maszyn wirtualnych IaaS możesz przejść z dysku szyfrowania PS cmdlet "Get-AzureRmVMDiskEncryptionStatus". Aby uzyskać ustawienia szyfrowania dla swojego maszyn wirtualnych, wpisz w sesji programu Azure PowerShell:

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName
    
    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

Wynik Get-AzureRmVMDiskEncryptionStatus można sprawdzić na potrzeby szyfrowania klucza adresy URL.
    
    C:\> $status = Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMNam
    e $VMName -ExtensionName $ExtensionName
    C:\> $status.OsVolumeEncryptionSettings

    DiskEncryptionKey                                                 KeyEncryptionKey                                               Enabled
    -----------------                                                 ----------------                                               -------
    Microsoft.Azure.Management.Compute.Models.KeyVaultSecretReference Microsoft.Azure.Management.Compute.Models.KeyVaultKeyReference    True


    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey.SecretUrl
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a
    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey
    
    SecretUrl                                                                                                               SourceVault
    ---------                                                                                                               -----------
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a Microsoft.Azure.Management....

Wartość ustawienia OSVolumeEncrypted i DataVolumesEncrypted są ustawione na "Szyfrowane" przedstawiający szyfrowane zarówno wielkość używa szyfrowania Azure dysku. Zobacz wpis w blogu **Szyfrowanie dysków Azure zapoznanie z programem PowerShell Azure** [część 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) i [część 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx) , aby uzyskać szczegółowe informacje na temat włączania szyfrowania przy użyciu szyfrowania dysku Azure za pomocą poleceń cmdlet PS.

**Uwaga**: na pośrednictwem Linux SMS `Get-AzureRmVMDiskEncryptionStatus` polecenia cmdlet trwa minut 3-4, aby raportować stan szyfrowania.

#### <a name="get-encryption-status-of-the-iaas-vm-from-disk-encryption-cli-command"></a>Pobieranie stanu szyfrowania maszyn wirtualnych IaaS z dysku szyfrowania polecenie polecenia

Stan szyfrowania maszyn wirtualnych IaaS możesz przejść z dysku szyfrowania polecenia polecenie *azure maszyn wirtualnych Pokaż dysku szyfrowania stanu*. Aby uzyskać ustawienia szyfrowania dla swojego maszyn wirtualnych, wpisz w sesji Azure polecenie:

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a>Wyłączanie szyfrowania na uruchamianie maszyn wirtualnych IaaS systemu Windows

Można wyłączyć szyfrowania na uruchomionego systemu Windows lub maszyn wirtualnych IaaS Linux przy użyciu szablonu Menedżera zasobów szyfrowania Azure dysku lub polecenia cmdlet PS i Określa konfigurację odszyfrowywanie.


##### <a name="windows-vm"></a>Maszyn wirtualnych systemu Windows

Krok szyfrowania Wyłącz wyłącza szyfrowania woluminu systemu operacyjnego lub danych lub obu tych elementów na uruchomionego maszyn wirtualnych IaaS systemu Windows. Nie można wyłączyć woluminu systemu operacyjnego i pozostaw wielkość danych szyfrowane. Podczas etapu szyfrowania Wyłącz model Azure wdrażania klasyczny aktualizuje model usługi maszyn wirtualnych i maszyn wirtualnych IaaS systemu Windows jest oznaczony jako odszyfrowane. Zawartość maszyn wirtualnych nie są już szyfrowane spoczynku. Wyłączanie szyfrowania nie powoduje usunięcia magazynu kluczy klienta i materiału klucza szyfrowania, który jest BitLocker szyfrowania klawiszy dla systemu Windows i hasło dla Linux. 

##### <a name="linux-vm"></a>Linux maszyn wirtualnych

Krok szyfrowania Wyłącz wyłącza szyfrowanie woluminu danych na uruchomionego maszyn wirtualnych IaaS Linux

**Uwaga**: wyłączenie szyfrowania na dysku z systemem operacyjnym jest niedozwolone maszyny wirtualne Linux.

##### <a name="disable-encryption-on-existingrunning-iaas-vm-in-azure-using-resource-manager-template"></a>Wyłączanie szyfrowania maszyn wirtualnych IaaS istniejące uruchomiony w Azure za pomocą szablonu Menedżera zasobów

Szyfrowanie dysków może zostać wyłączona na uruchamianie maszyn wirtualnych IaaS systemu Windows za pomocą tego szablonu Menedżera zasobów opublikowanych [w tym miejscu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm). Kliknij przycisk "Wdrażanie do Azure" na tym szablonie Azure Szybki Start Konfiguracja odszyfrowywanie wprowadzania w karta parametry, a następnie kliknij przycisk OK. Wybierz subskrypcję, grupa zasobów, lokalizacji grupa zasobów, warunki prawne i umowy i kliknij przycisk Utwórz, aby włączyć szyfrowanie na nowe maszyny IaaS.

Dla Głosowa Linux [ten](https://aka.ms/decrypt-linuxvm) szablon umożliwia wyłączanie szyfrowania.

Menedżer zasobów szczegóły parametrów szablonu wyłączania szyfrowania uruchamiania maszyn wirtualnych IaaS:

| vmName         | Nazwa maszyn wirtualnych, na których szyfrowania ma zostać wykonane operacji                                                                                                                                                                       |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| volumeType     | Typ woluminu, na które odszyfrowywanie przeprowadzana jest operacja. Prawidłowe wartości to "OS", "Dane", "Wszystkie". **Uwaga:** Nie można wyłączyć szyfrowania uruchamiania głośność maszyn wirtualnych IaaS systemu Windows z systemem operacyjnym i uruchamiania bez konieczności wyłączania szyfrowania woluminu "Dane". **Uwaga**: wyłączenie szyfrowania na dysku z systemem operacyjnym jest niedozwolone maszyny wirtualne Linux. |
| sequenceVersion | Wersja sekwencji działania funkcji BitLocker. Zwiększyć ten numer wersji każdorazowo operacji odszyfrowywania dysku jest wykonywane na tym samym maszyn wirtualnych                                                                                          |

##### <a name="disable-encryption-on-existingrunning-iaas-vm-in-azure-using-ps-cmdlet"></a>Wyłączanie szyfrowania maszyn wirtualnych IaaS istniejące uruchomiony w Azure za pomocą polecenia cmdlet PS

Aby wyłączyć przy użyciu polecenia cmdlet PS, [Wyłącz AzureRmVMDiskEncryption](https://msdn.microsoft.com/library/azure/mt715776.aspx) polecenia cmdlet wyłącza szyfrowania na infrastrukturę jako maszyny wirtualnej usługi (IaaS). To polecenie cmdlet obsługuje systemu Windows i Linux oraz maszyny wirtualne. To polecenie cmdlet instaluje rozszerzenia na komputerze wirtualnych, aby wyłączyć szyfrowanie. Jeśli parametr Nazwa nie jest określony, zostanie utworzony rozszerzenie przy użyciu domyślnej nazwy "AzureDiskEncryption dla systemu Windows maszyny wirtualne". 

Na maszyny wirtualne Linux rozszerzenie "AzureDiskEncryptionForLinux" jest używany.

**Uwaga**: maszyny wirtualnej ponownym uruchomieniu tego polecenia cmdlet. 

## <a name="appendix"></a>Dodatek

### <a name="connect-to-your-subscription"></a>Nawiązywanie połączenia z subskrypcji

Upewnij się, zapoznaj się z sekcją *wymagania wstępne* , w tym dokumencie, przed kontynuowaniem. Po upewnieniu się, że zostały spełnione wszystkie wymagania wstępne, wykonaj poniższe czynności, aby nawiązać połączenie subskrypcji:

1. rozpocząć sesję programu PowerShell Azure i zaloguj się do konta Azure za pomocą następującego polecenia:

    Login-AzureRmAccount

2. Jeśli masz wiele subskrypcji i chcesz określić określonej korzystać, wpisz poniższe czynności, aby wyświetlić subskrypcje dla Twojego konta:

    Get-AzureRmSubscription

3 Aby określić subskrypcji, którą chcesz użyć, wpisz:

    Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>

4. Aby sprawdzić, czy subskrypcja skonfigurowane jest poprawny, należy wpisać:

    Get-AzureRmSubscription

5 Aby upewnij się, że są zainstalowane cmdlet szyfrowanie dysków Azure, należy wpisać:

    Get-command *diskencryption*

6 powinna być widoczna poniżej dane wyjściowe Potwierdzanie instalacji Azure programu PowerShell szyfrowania dysku:

    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     

### <a name="preparing-a-pre-encrypted-windows-vhd"></a>Przygotowywanie przed zaszyfrowanych wirtualnego dysku twardego systemu Windows
Sekcjach są niezbędne przygotowania wdrażania jako zaszyfrowanych wirtualnego dysku twardego w Azure IaaS wstępnie zaszyfrowanych wirtualnego dysku twardego systemu Windows. Czynności są używane związanymi z przygotowywaniem i uruchomić świeży maszyn wirtualnych (wirtualnego dysku twardego) na funkcji Hyper-V lub Azure systemu windows.

#### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a>Aktualizowanie zasad grupy, aby umożliwić innych niż TPM ochrony systemu operacyjnego
Musisz skonfigurować ustawienia zasad grupy funkcji BitLocker o nazwie dysków funkcją BitLocker, znajduje się w obszarze zasady komputera lokalnego \Computer Configuration\Administrative składniki administracyjne\Składniki systemu. Zmienić to ustawienie: *Dyski systemu operacyjnego — Wymagaj dodatkowego uwierzytelniania podczas uruchamiania - Zezwalaj na funkcję BitLocker bez zgodnego modułu TPM* , jak pokazano na poniższej ilustracji:

![Microsoft ochrony przed złośliwym oprogramowaniem platformy Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a>Zainstaluj składniki funkcji BitLocker
Dla systemu Windows Server 2012 i powyżej Użyj poniżej polecenia:

    dism /online /Enable-Feature /all /FeatureName:Bitlocker /quiet /norestart

Do użytku w systemie Windows Server 2008 R2 poniżej polecenia:

    ServerManagerCmd -install BitLockers

#### <a name="prepare-os-volume-for-bitlocker-using-bdehdcfg"></a>Przygotowywanie woluminu systemu operacyjnego za pomocą funkcji BitLocker`bdehdcfg`

Wykonanie polecenia poniżej, aby skompresować partycją systemu operacyjnego i przygotowywanie komputera do funkcji BitLocker.

    bdehdcfg -target c: shrink -quiet

#### <a name="using-bitlocker-to-protect-the-os-volume"></a>Ochrona woluminu systemu operacyjnego za pomocą funkcji BitLocker
Używanie [`manage-bde`](https://technet.microsoft.com/library/ff829849.aspx) polecenie, aby włączyć szyfrowania woluminu uruchamiania przy użyciu zewnętrznych ochrony klucza i umieść klucz zewnętrzny (plik .bek) na zewnętrzny dysk lub głośność. Szyfrowanie będą dostępne na wielkość systemu i uruchamiania po ponownym.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

**Uwaga:** Maszyn wirtualnych należy przygotować się z wirtualnego dysku twardego osobne danych i zasobów na wprowadzenie klucz zewnętrzny przy użyciu funkcji BitLocker.

#### <a name="encrypting-os-drive-on-a-running-linux-vm"></a>Ponieważ do szyfrowania dysku systemu operacyjnego uruchomionego maszyny Linux

Szyfrowanie dysku systemu operacyjnego uruchomionego maszyny Linux są obsługiwane na distros następujące czynności:

- RHEL 7.2
- CentOS 7.2
- Ubuntu 16.04

Wymagania wstępne dotyczące szyfrowania dysku systemu operacyjnego:

- Należy utworzyć maszyn wirtualnych z obrazu z galerii Azure w portal Azure Menedżera zasobów.
- Azure maszyn wirtualnych z co najmniej 4 GB pamięci RAM (zalecane rozmiar jest 7 GB).
- (W przypadku RHEL i CentOS). SELinux musi być [wyłączony](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) na maszyn wirtualnych. Maszyn wirtualnych należy ponownie uruchomić co najmniej raz po wyłączeniu SELinux.


##### <a name="steps"></a>Czynności

1. Tworzenie maszyn wirtualnych przy użyciu jednego z distros wymienione powyżej.

Dla CentOS 7.2 szyfrowania dysku systemu operacyjnego jest obsługiwana przez specjalne obraz. Aby użyć tego obrazu, określ "7.2n" jako używaną wersję podczas tworzenia maszyn wirtualnych:

    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"

2. Konfigurowanie maszyn wirtualnych zależnie od potrzeb. Jeśli zamierzasz szyfrować wszystkie (OS + danych) dyski dane, które dyski muszą być określony i instalację z /etc/fstab.

> [AZURE.NOTE] Należy użyć UUID =... Określ dyskach danych w fstab/itp/zamiast określający nazwę urządzenia bloku, np /dev/sdb1. Szyfrowanie kolejności dyski zmieni się na maszyn wirtualnych. Jeśli do maszyn wirtualnych zależy od określonej kolejności urządzeń blok nie będzie można zainstalować je po szyfrowania.

3 sesje SSH Wyloguj.

4. Aby zaszyfrować system operacyjny, określ volumeType jako "wszystkie" lub "OS" podczas [Włączanie szyfrowania](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).

> [AZURE.NOTE] Wszystkie procesy przestrzeń użytkownika, które nie działają jako `systemd` usług pozostawia się z `SIGKILL`. Maszyn wirtualnych są należy ponownie uruchomić. Należy zaplanować przestoje maszyn wirtualnych podczas włączania OS dysku szyfrowania uruchomionego maszyn wirtualnych.

5 okresowo monitorować postęp szyfrowania zgodnie z instrukcjami zawartymi w [następnej sekcji](#monitoring-os-encryption-progress).

6 po Get-AzureRmVmDiskEncryptionStatus wyświetlana "VMRestartPending", należy ponownie uruchomić usługi maszyn wirtualnych albo logując się go lub za pośrednictwem portalu-programu PowerShell i polecenie.

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName
    
    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, please reboot the VM

Zalecane jest zapisywanie [uruchamiania diagnostyki](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) pakietu maszyn wirtualnych *przed* ponownym uruchomieniu.

#### <a name="monitoring-os-encryption-progress"></a>Monitorowanie postępu szyfrowania systemu operacyjnego

Istnieją trzy sposoby monitorowanie postępu szyfrowania systemu operacyjnego.

1. należy użyć polecenia cmdlet Get-AzureRmVmDiskEncryptionStatus i Sprawdzanie pola ProgressMessage: 
 
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started

Po maszyn wirtualnych osiągnie "OS dysku szyfrowania pracę" potrwa około 40-50 minut na Premium magazynowania kopii maszyn wirtualnych.

Z powodu [problemu #388](https://github.com/Azure/WALinuxAgent/issues/388) WALinuxAgent `OsVolumeEncrypted` i `DataVolumesEncrypted` się pojawić jako `Unknown` w niektórych distros. Z WALinuxAgent wersji 2.1.5 lub wyższej ten problem zostanie rozwiązany automatycznie. W przypadku, gdy pojawi się `Unknown` w wyniku kwerendy, można sprawdzić stan szyfrowania dysku za pomocą podglądu zasobu Azure.

Przejdź do [Podglądu zasobów Azure](https://resources.azure.com/), a następnie rozwiń węzeł tej hierarchii w panelu zaznaczenia po lewej stronie:

~~~~
 |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
~~~~                

W InstanceView przewiń w dół, aby wyświetlić stan szyfrowania dysków.

![Widok wystąpienia maszyn wirtualnych](./media/azure-security-disk-encryption/vm-instanceview.png)

2. Przyjrzyj się [diagnostyki uruchamiania](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/). Wiadomości od rozszerzenie MDE powinny być poprzedzony `[AzureDiskEncryption]`.

3.w logowania do maszyn wirtualnych za pomocą SSH i wprowadzenie dziennika rozszerzenie z

    /var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

Zaloguj się do maszyn wirtualnych w trakcie szyfrowania system operacyjny nie jest zalecane. W związku z tym dzienniki powinny być skopiowane tylko wtedy, gdy nie powiodła się dwóch innych metod.

#### <a name="preparing-a-pre-encrypted-linux-vhd"></a>Przygotowywanie wirtualnego dysku twardego Linux wstępnie zaszyfrowanych

##### <a name="ubuntu-16"></a>Ubuntu 16

###### <a name="configure-encryption-during-distro-install"></a>Konfigurowanie szyfrowania podczas instalacji distro

1. Wybierz pozycję "Konfigurowanie zaszyfrowanych wielkości" po podziału dysków.

![Ustawienia Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Tworzenie dysku osobnych uruchamiania, których nie może być zaszyfrowany. Szyfrowanie dysku głównego.

![Ustawienia Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3.w Podaj hasło. Jest to hasło, które wysyłasz do KeyVault.

![Ustawienia Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. zakończenia podziału.

![Ustawienia Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5 podczas uruchamiania maszyn wirtualnych, zostanie wyświetlony monit dla hasło. Za pomocą hasła, podaną w kroku 3.

![Ustawienia Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6 Przygotowywanie maszyn wirtualnych do przekazywania do Azure za pomocą [tych instrukcjach](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/). Ostatnim krokiem (cofanie ubezpieczeń maszyn wirtualnych) nie będą uruchamiane jeszcze.

###### <a name="configure-encryption-to-work-with-azure"></a>Konfigurowanie szyfrowania do pracy z platformy Azure

1. Tworzenie pliku w obszarze /usr/local/sbin/azure_crypt_key.sh, zawartości w obszarze skrypt poniżej. Zwróć uwagę KeyFileName, ponieważ jest to nazwa pliku hasło przez Azure.

    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do
        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED to find suitable passphrase file ..." >&2
        echo -n "Try to enter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi


2. Zmienianie konfiguracji kryptograficznego w */etc/crypttab*. Jego powinna wyglądać następująco:

    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh

3.w Jeśli edytują *azure_crypt_key.sh* w systemie Windows i Linux skopiowana, pamiętaj uruchomić *dos2unix /usr/local/sbin/azure_crypt_key.sh*.

4. Dodaj wykonywalny uprawnienia do skryptu:

    chmod +x /usr/local/sbin/azure_crypt_key.sh

4. edytować */etc/initramfs-tools/modules* dołączając wierszy:

    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1

5 uruchamianie `update-initramfs -u -k all` initramfs, aby zaktualizować `keyscript` została uwzględniona.
6 można teraz deprovision maszyn wirtualnych.

![Ustawienia Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

7 przejście do następnego kroku i [przekazać do wirtualnego dysku twardego](#upload-encrypted-vhd-to-an-azure-storage-account) do Azure.

##### <a name="opensuse-132"></a>openSUSE 13.2

###### <a name="configure-encryption-during-distro-install"></a>Konfigurowanie szyfrowania podczas instalacji distro

1. Wybierz "Szyfrowanie głośność grupę", gdy podziału dysków. Podaj hasło. Jest to hasło, które wysyłasz do KeyVault.

![Konfigurowanie openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. Uruchom maszyn wirtualnych, przy użyciu swojego hasła.

![Konfigurowanie openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3.w przygotowywanie maszyn wirtualnych do przekazywania do Azure za pomocą [tych instrukcjach](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131). Ostatnim krokiem (cofanie ubezpieczeń maszyn wirtualnych) nie będą uruchamiane jeszcze.

###### <a name="configure-encryption-to-work-with-azure"></a>Konfigurowanie szyfrowania do pracy z platformy Azure

1. edytowanie /etc/dracut.conf i Dodaj poniższy wiersz:

    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"

2. komentarz te wiersze do końca pliku "-usr/lib/dracut/modules.d/90crypt/module-setup.sh":

    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator



3.w dołączyć następujący wiersz na początku pliku "-usr/lib/dracut/modules.d/90crypt/parse-crypt.sh"

    DRACUT_SYSTEMD=0

i zmienić wszystkie wystąpienia

    if [ -z "$DRACUT_SYSTEMD" ]; then

Aby

    if [ 1 ]; then

4. edytowanie /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh i dołączanie to po "urządzenia Otwórz LUKS #"

    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done


5 uruchamianie "/ usr/sbin-dracut - f - v" aktualizacji initrd.

6 do Azure można teraz deprovision maszyn wirtualnych i [przekazywanie do wirtualnego dysku twardego](#upload-encrypted-vhd-to-an-azure-storage-account) .

##### <a name="centos-7"></a>CentOS 7

###### <a name="configure-encryption-during-distro-install"></a>Konfigurowanie szyfrowania podczas instalacji distro

1. Wybierz pozycję "szyfruj moje dane" podczas podziału dysków.

![Ustawienia centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Upewnij się, że "Szyfrowanie" jest zaznaczone dla partition głównego.

![Ustawienia centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3.w Podaj hasło. Jest to hasło, które wysyłasz do KeyVault.

![Ustawienia centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. podczas uruchamiania maszyn wirtualnych, zostanie wyświetlony monit dla hasło. Za pomocą hasła, podaną w kroku 3.

![Ustawienia centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5 przygotowywanie maszyn wirtualnych do przekazywania do Azure za pomocą [tych instrukcjach](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70). Ostatnim krokiem (cofanie ubezpieczeń maszyn wirtualnych) nie będą uruchamiane jeszcze.

6 do Azure można teraz deprovision maszyn wirtualnych i [przekazywanie do wirtualnego dysku twardego](#upload-encrypted-vhd-to-an-azure-storage-account) .

###### <a name="configure-encryption-to-work-with-azure"></a>Konfigurowanie szyfrowania do pracy z platformy Azure

1. edytowanie /etc/dracut.conf i Dodaj poniższy wiersz:

    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"

2. komentarz te wiersze do końca pliku "-usr/lib/dracut/modules.d/90crypt/module-setup.sh":

    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator



3.w dołączyć następujący wiersz na początku pliku "-usr/lib/dracut/modules.d/90crypt/parse-crypt.sh"

    DRACUT_SYSTEMD=0

i zmienić wszystkie wystąpienia

    if [ -z "$DRACUT_SYSTEMD" ]; then

Aby

    if [ 1 ]; then

4. edytowanie /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh i dołączanie to po "urządzenia Otwórz LUKS #"

    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done


5 uruchamianie "/ usr/sbin-dracut - f - v" aktualizacji initrd.

![Ustawienia centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-to-an-azure-storage-account"></a>Przekazywanie zaszyfrowanych wirtualnego dysku twardego do konta usługi Azure miejsca do magazynowania
Po włączeniu funkcji BitLocker Szyfrowanie pr kryptograficznego DM szyfrowanie lokalnych wirtualny dysk twardy zaszyfrowanych należy przekazać do swojego konta miejsca do magazynowania.

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-disk-encryption-secret-for-the-pre-encrypted-vm-to-key-vault"></a>Przekazywanie dysku szyfrowania hasła dla wstępnie zaszyfrowanych maszyn wirtualnych do magazynu klucza
Tajny szyfrowania dysku uzyskane w wyniku wcześniej należy przekazać jako poufne w klucza magazynu. Magazyn klucz musi mieć uprawnienia dla klienta AAD oraz szyfrowanie dysków.


    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $KeyVault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>Tajny szyfrowania dysku nie są szyfrowane przy użyciu KEK
[Ustawianie AzureKeyVaultSecret](https://msdn.microsoft.com/library/dn868050.aspx) używane do obsługi administracyjnej hasła w klucza magazynu. W przypadku maszyny wirtualnej systemu Windows plik bek jest kodowane jako ciąg base64 i następnie przekazywane do klucza magazynu przy użyciu polecenia cmdlet Set-AzureKeyVaultSecret. Linux hasło jest kodowane jako ciąg base64, a następnie przekazać do magazynu klucza. Ponadto upewnij się, że następujące tagi są ustawione podczas tworzenia tego hasła w klucza magazynu.

    # This is the passphrase that was provided for encryption during distro install
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

`$secretUrl` Są uwzględniane w następnym kroku dla [Dołączanie dysku z systemem operacyjnym bez użycia KEK](#without-using-a-kek).

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>Tajny szyfrowania dysku zaszyfrowanych za pomocą KEK

Opcjonalnie można zaszyfrować hasło z kluczem szyfrowania klucza przed przekazaniem do magazynu klucza. Najpierw szyfrowanie hasła przy użyciu klucza szyfrowania klucza za pomocą [interfejsu API](https://msdn.microsoft.com/library/azure/dn878066.aspx) zawijania. Wynik tej operacji zawijania jest ciąg zakodowany w adresie URL base64, który jest następnie przekazane jako poufne, przy użyciu polecenia cmdlet [Set-AzureKeyVaultSecret](https://msdn.microsoft.com/library/dn868050.aspx) .

    # This is the passphrase that was provided for encryption during distro install
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id

`$KeyEncryptionKey` i `$secretUrl` stosuje się w następnym kroku do [dołączania dysku systemu operacyjnego przy użyciu KEK](#using-a-kek).

### <a name="specify-secret-url-when-attaching-os-disk"></a>Określ adres URL tajne, podczas dołączania dysku systemu operacyjnego

#### <a name="without-using-a-kek"></a>Bez użycia KEK

Podczas dołączania dysku OS `$secretUrl` musi być przekazywane. Adres URL został wygenerowany w sekcji ["tajny szyfrowania dysku nie zaszyfrowanych za pomocą KEK"](#disk-encryption-secret-not-encrypted-with-a-kek).

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a>Przy użyciu KEK

Podczas dołączania dysku OS `$KeyEncryptionKey` i `$secretUrl` muszą zostać przekazane. Adres URL został wygenerowany w sekcji ["tajny szyfrowania dysku zaszyfrowanych za pomocą KEK"](#disk-encryption-secret-encrypted-with-a-kek).

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id

## <a name="download-this-guide"></a>Pobierz ten przewodnik
Ten przewodnik można pobrać z [Galerii TechNet](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).


## <a name="for-more-information"></a>Aby uzyskać więcej informacji
[Eksplorowanie szyfrowania Azure dysku przy użyciu programu PowerShell Azure](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)

[Eksplorowanie szyfrowania Azure dysku przy użyciu programu PowerShell Azure — część 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
