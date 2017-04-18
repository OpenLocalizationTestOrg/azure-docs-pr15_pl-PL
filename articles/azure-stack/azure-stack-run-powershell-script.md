<properties
    pageTitle="Wdrażanie Azure stos Zapewnić | Microsoft Azure"
    description="Dowiedz się, jak Zapewnić stos Azure przygotowywanie i uruchamianie skryptu programu PowerShell do wdrożenia Zapewnić stos Azure."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="erikje"/>

# <a name="deploy-azure-stack-poc"></a>Wdrażanie Azure stos zatwierdzania Koncepcji
Aby wdrożyć Zapewnić stos Azure, należy najpierw [Przygotowywanie komputera wdrożenia](#prepare-the-deployment-machine) , a następnie [uruchomić skrypt wdrożenia programu PowerShell](#run-the-powershell-deployment-script).

## <a name="download-and-extract-microsoft-azure-stack-poc-tp2"></a>Pobieranie i wyodrębnianie Microsoft Azure stos Zapewnić TP2

Zanim zaczniesz, upewnij się, że możesz 85 GB miejsca.

1. Pobieranie Azure stos Zapewnić TP2 składa się z pliku zip zawierający następujące pliki 12, Sumowanie ~ 20 GB:
    - 1 MicrosoftAzureStackPOC.EXE
2. Przejrzyj ekran umowę licencyjną i informacji o Kreatorze wyodrębnianie własny, a następnie kliknij przycisk **Dalej**.
3. Przejrzyj ekran zasady zachowania poufności informacji oraz informacji o Kreatorze wyodrębnianie własny, a następnie kliknij przycisk **Dalej**.
4. Wybieranie miejsca docelowego dla plików, można wyodrębnić, kliknij przycisk **Dalej**.
    - Wartość domyślna to: <drive letter>:\<bieżący folder > \Microsoft Zapewnić stos Azure
5. Przejrzyj ekran lokalizacji docelowej oraz informacji o Kreatorze wyodrębnianie własny, a następnie kliknij **wyodrębnić** , aby wyodrębnić CloudBuilder.vhdx (~44.5 GB) i pliki ThirdPartyLicenses.rtf.

> [AZURE.NOTE] Po wyodrębnieniu plików możesz usunąć pliku zip, aby odzyskać miejsce na komputerze. Lub możesz przenieść pliku zip do innej lokalizacji, jeśli chcesz ponownie rozmieścić możesz nie musisz ponownie pobrać pliki zip.

## <a name="prepare-the-deployment-machine"></a>Przygotowywanie komputera wdrażania

1. Upewnij się, że można fizycznie połączyć się z komputerem wdrożenia lub mieć dostęp konsoli fizycznej (na przykład KVM). Konieczne będzie taki dostęp po ponownym uruchomieniu komputera wdrożenia w kroku 9.

2. Upewnij się, że na komputerze wdrożenia spełnia [minimalne wymagania](azure-stack-deploy.md). [Sprawdzanie wdrażania Azure stos Technical Preview 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) służy do potwierdzenia z własnymi potrzebami.

3. Zaloguj się jako Administrator lokalny na komputerze zatwierdzania Koncepcji.

4. Skopiuj plik CloudBuilder.vhdx do C:\CloudBuilder.vhdx.

    > [AZURE.NOTE] Jeśli nie chcesz używać zalecane skrypt do przygotowania komputera-hosta Zapewnić (kroki 5 — krok 7), należy wprowadzać wszelkie kluczy licencji na stronie aktywacji. Wersja próbna systemu Windows Server 2016 obrazu jest dołączany, a wprowadzając klucz licencji powoduje wygasania ostrzeżenia.

5. Na komputerze, aby Zapewnić Uruchom następujący skrypt programu PowerShell w celu pobierania plików pomocy technicznej TP2 stos Azure:

    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/'
    $LocalPath = 'c:\AzureStack_TP2_SupportFiles'

    # Create folder
    New-Item $LocalPath -type directory

    # Download files
    ( 'BootMenuNoKVM.ps1', 'PrepareBootFromVHD.ps1', 'Unattend.xml', 'unattend_NoKVM.xml') | foreach { Invoke-WebRequest ($uri + $_) -OutFile ($LocalPath + '\' + $_) } 
    ```

    Ten skrypt do pobrania plików pomocy technicznej TP2 stos Azure w folderze określonym przez parametr $LocalPath.
    
6. Otwórz podwyższonym poziomem konsoli programu PowerShell i zmień katalog, do którego skopiowano pliki.
    - 11 N.BIN MicrosoftAzureStackPOC (gdzie N to 1-11)
7. Kliknij prawym przyciskiem myszy MicrosoftAzureStackPOC.EXE > Uruchom jako administrator.

8. Uruchom skrypt PrepareBootFromVHD.ps1. Ten skrypt i plików instalacji są dostępne inne skrypty obsługi znajdujące się wraz z tej kompilacji.
    Istnieje pięć parametrów dla tego skryptu programu PowerShell:
    - CloudBuilderDiskPath (wymagany) — ścieżkę do CloudBuilder.vhdx na HOŚCIE.
    - DriverPath (opcjonalnie) — pozwala dodać sterowniki dla hosta w wirtualnej HD.
    - (Opcjonalnie) — ApplyUnattend parametr ten przełącznik do automatyzowania konfiguracji systemu operacyjnego. Jeśli określony, użytkownik musi podać AdminPassword, aby skonfigurować system operacyjny przy uruchomieniu (wymaga pod warunkiem załączonych plików unattend_NoKVM.xml).
    Jeśli ten parametr nie jest używany, bez dalszego dostosowania zostanie użyty plik unattend.xml ogólne. KVM będzie konieczne dostosowanie wykonania, po jego ponownym uruchomieniu.
    - (Opcjonalnie) — AdminPassword używać tylko po ustawieniu parametru ApplyUnattend wymaga co najmniej sześć znaków.
    - VHDLanguage (opcjonalnie) — określa język wirtualnego dysku twardego, ustawiana domyślnie "en-us".
    Skrypt opisano i zawiera przykładowe użycie, ale jest najbardziej typowe zastosowania:
    
        `.\PrepareBootFromVHD.ps1 -CloudBuilderDiskPath C:\CloudBuilder.vhdx -ApplyUnattend`
    
        Po uruchomieniu tego polecenia dokładnie, należy wprowadzić AdminPassword w wierszu.

9. Po zakończeniu skrypt musi potwierdzić, uruchom ponownie komputer. W przypadku zalogowania innych użytkowników, to polecenie nie powiedzie się. Jeśli to polecenie nie powiedzie się, uruchom następujące polecenie:`Restart-Computer -force` 

10. HOST ponowne uruchomienie do operacyjnego CloudBuilder.vhdx, gdzie będzie nadal występował wdrożenia.

> [AZURE.IMPORTANT] Stos Azure wymaga dostęp do Internetu, bezpośrednio lub za pośrednictwem przezroczysty serwera proxy. Wdrożenie Zapewnić TP2 obsługuje NIC dokładnie jedną dla sieci. Jeśli masz kart, upewnij się, że jest włączony tylko jeden (i wszystkie pozostałe są wyłączone) przed uruchomieniem skryptu wdrażania w następnej sekcji.

## <a name="run-the-powershell-deployment-script"></a>Uruchom skrypt wdrożenia programu PowerShell

1. Zaloguj się jako Administrator lokalny na komputerze zatwierdzania Koncepcji. Użyj poświadczeń określonych w poprzednich krokach.

2. Otwieranie konsoli programu PowerShell podwyższonym poziomem uprawnień.

3. W programie PowerShell uruchom następujące polecenie:`cd C:\CloudDeployment\Configuration`

4. Polecenie Rozmieść:`.\InstallAzureStackPOC.ps1`

5. Po wyświetleniu monitu **Wprowadź hasło** wprowadź hasło, a następnie potwierdź je. Jest to hasło do maszyn wirtualnych. Pamiętaj zarejestrować go.

6. Wprowadź poświadczenia konta usługi Azure Active Directory. Ten użytkownik musi być administrator globalny w dzierżawie katalogu.

7. Proces wdrażania może potrwać kilka godzin, podczas których automatycznego ponownego raz.

    > [AZURE.IMPORTANT] Jeśli chcesz monitorować postęp rozmieszczania, zaloguj się jako azurestack\AzureStackAdmin. Jeśli możesz zalogować się jako administrator lokalny po komputer jest dołączony do domeny, nie zobaczysz postęp rozmieszczania. Uruchom ponownie wdrożenia, nie zamiast tego zaloguj się jako azurestack\AzureStackAdmin do sprawdzania, czy jest uruchomiony.

    Po pomyślnym wdrażania konsoli programu PowerShell są wyświetlane: **WYKONANO: akcji "Wdrożenie"**.

    Wdrażanie nie powiedzie się, możesz [go ponownie](azure-stack-rerun-deploy.md). Lub możesz [ponownie wdróż](azure-stack-redeploy.md) go od podstaw.

### <a name="deployment-script-examples"></a>Przykłady skryptów wdrażania

W przypadku Twojej tożsamości AAD tylko skojarzone z jednego katalogu AAD:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred

Jeśli Twojej tożsamości AAD jest skojarzony z WIĘKSZE niż jeden AAD katalogu:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT> example: user@AADDirName.onmicrosoft.com>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred -AADDirectoryTenantName "<SPECIFIC AAD DIRECTORY example: AADDirName.onmicrosoft.com>"

Jeśli środowiska nie ma DHCP włączone, musi zawierać następujące dodatkowe parametry na jedną z opcji powyżej (przykład użycia dostępne):

    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred
    -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1


### <a name="installazurestackpocps1-optional-parameters"></a>Parametry opcjonalne InstallAzureStackPOC.ps1

| Parametr | Wymagane opcjonalne | Opis |
| --------- | ----------------- | ----------- |
| AADAdminCredential | Opcjonalne | Ustawia usługi Azure Active Directory nazwa użytkownika i hasło. Te poświadczenia Azure może być Identyfikatora organizacji lub Account Microsoft. Aby użyć poświadczeń Account Microsoft, należy pominąć ten parametr w polecenia cmdlet. Pominięcie ten parametr jest wyświetlany monit podręcznego uwierzytelnianie Azure podczas wdrażania. Spowoduje to utworzenie tokeny uwierzytelniania i odświeżania używane podczas wdrażania. |
| AADDirectoryTenantName | Wymagane | Ustawia katalog dzierżawy. Ten parametr służy do określonego katalogu określ miejsce, w którym konto AAD ma uprawnienia do zarządzania wiele katalogów. Pełna nazwa dzierżawę katalogu AAD w formacie <directoryName>. onmicrosoft.com. |
| AdminPassword | Wymagane | Ustawia lokalnego konta administratora i wszystkie inne konta użytkownika w przypadku maszyn wirtualnych utworzonych jako część wdrożenia zatwierdzania Koncepcji. To hasło musi odpowiadać bieżące hasło administratora lokalnego na hoście. |
| AzureEnvironment | Opcjonalne | Wybierz środowisko Azure, z którym chcesz zarejestrować wdrażania stos Azure. Opcje obejmują *Publicznej Azure*, *Azure - Chinach* *Azure - Rządu Stanów Zjednoczonych*. |
| EnvironmentDNS | Opcjonalne | Serwer DNS zostanie utworzona w ramach wdrożenia stos Azure. Aby zezwolić na komputerach wewnątrz rozwiązanie do rozpoznawania nazw poza stempel, podaj istniejącej infrastruktury serwera DNS. Serwer DNS w sygnatur czasowych przekazuje żądań rozpoznawania nazw nieznany do tego serwera. |
| NatIPv4Address | Wymagane do obsługi protokołu NAT DHCP | Ustawia statyczny adres IP MAS BGPNAT01. Ten parametr należy używać tylko, jeśli DHCP nie można przypisać prawidłowy adres IP, aby uzyskać dostęp do Internetu. |
| NatIPv4DefaultGateway | Wymagane do obsługi protokołu NAT DHCP | Ustawia brama domyślna używana z adresem IP dla MAS BGPNAT01. Ten parametr należy używać tylko, jeśli DHCP nie można przypisać prawidłowy adres IP, aby uzyskać dostęp do Internetu.  |
| NatIPv4Subnet | Wymagane do obsługi protokołu NAT DHCP | Podsieć adresów IP prefiks DHCP przez translatora adresów Sieciowych pomocy technicznej. Ten parametr należy używać tylko, jeśli DHCP nie można przypisać prawidłowy adres IP, aby uzyskać dostęp do Internetu.  |
| PublicVLan | Opcjonalne | Ustawia identyfikator VLAN. Ten parametr należy używać tylko, jeśli host i MAS BGPNAT01 należy skonfigurować identyfikator VLAN, aby uzyskać dostęp do sieci fizycznej (i Internet). Na przykład`.\InstallAzureStackPOC.ps1 –Verbose –PublicVLan 305` |
| Uruchom ponownie | Opcjonalne | Użyj tej flagi ponowne uruchomienie wdrożenia.  Wszystkie dane wejściowe poprzedniego jest używana. Ponownie wprowadzania danych udostępnione nie jest obsługiwana, ponieważ kilka wartości unikatowe będą generowane i używane do wdrożenia. |
| TimeServer | Opcjonalne | Użyj ten parametr, jeśli musisz określić serwer określonego czasu. |

## <a name="next-steps"></a>Następne kroki

[Nawiązywanie połączenia z stos Azure](azure-stack-connect-azure-stack.md)
