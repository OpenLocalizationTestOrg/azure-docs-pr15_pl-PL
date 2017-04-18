<properties
    pageTitle="Rozpoczynanie pracy z magazynu klucza Azure | Microsoft Azure"
    description="Użyj tego samouczka, aby ułatwić Ci rozpoczęcie pracy z magazynu klucza Azure, aby utworzyć kontener zaostrzony w Azure, przechowywanie i zarządzać nimi klucze szyfrowania hasła Azure."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>

# <a name="get-started-with-azure-key-vault"></a>Rozpoczynanie pracy z magazynu klucza Azure #
Azure magazynu klucza jest dostępna w większości regionów. Aby uzyskać więcej informacji zobacz [Klucz magazynu ceny strony](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Wprowadzenie  
Użyj tego samouczka, aby ułatwić Ci rozpoczęcie pracy z magazynu klucza Azure, aby utworzyć kontener zaostrzony (magazynu) platformy Azure, przechowywanie i zarządzać nimi klucze szyfrowania hasła Azure. Jego przeprowadzi Cię przez proces Tworzenie magazynu, zawierającą klucz lub hasło, które następnie można z aplikacją Azure za pomocą programu PowerShell Azure. Następnie przedstawiono sposób aplikacji można użyć tego klucza lub hasła.

**Szacowany czas wykonania:** 20 minut

>[AZURE.NOTE]  Ten samouczek nie zawiera instrukcje dotyczące napisać Azure aplikację, że jeden z kroków zawiera, to znaczy, jak zezwolić aplikacji użyj klawisza lub tajny w klucza magazynu.
>
>Samouczku Azure programu PowerShell. Aby interfejs wiersza polecenia i Platform, zobacz [tego samouczka równoważne](key-vault-manage-with-cli.md).

Informacje o magazynu klucza Azure, zobacz [Co to jest Azure klucza magazynu?](key-vault-whatis.md)

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, musisz mieć następujące czynności:

- Subskrypcję usługi Microsoft Azure. Jeśli nie masz, możesz zalogować [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).
- Azure programu PowerShell, **Minimalna wersja 1.1.0**. Aby zainstalować Azure programu PowerShell i skojarzyć subskrypcji Azure, zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md). Jeśli zainstalowano już Azure programu PowerShell i nie znasz wersji, z poziomu konsoli programu PowerShell Azure, wpisz `(Get-Module azure -ListAvailable).Version`. Jeśli masz Azure programu PowerShell wersji 0.9.1 za pośrednictwem 0.9.8 zainstalowany, nadal możesz używać tego samouczka z drobnych zmian. Na przykład, należy użyć `Switch-AzureMode AzureResourceManager` poleceń i niektóre polecenia Azure klucza magazynu zostały zmienione. Aby uzyskać listę poleceń cmdlet magazynu klucz dla wersji 0.9.1 za pośrednictwem 0.9.8 zobacz [Polecenia cmdlet magazynu klucza Azure](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.98\).aspx). 
- Aplikacja zostanie skonfigurowana za pomocą klucza lub hasła, które są tworzone w tym samouczku. Przykładowa aplikacja jest dostępna z [Centrum pobierania Microsoft](http://www.microsoft.com/en-us/download/details.aspx?id=45343). Aby uzyskać instrukcje Zobacz tym pliku Readme.


Ten samouczek jest przeznaczony dla początkujących Azure programu PowerShell, ale przyjęto założenie, że wiesz, podstawowe pojęcia, takie jak moduły, polecenia cmdlet i sesji. Aby uzyskać więcej informacji zobacz [Wprowadzenie do programu Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx).

Aby uzyskać szczegółową pomoc dla dowolnego polecenia cmdlet, który zostanie wyświetlony w tym samouczku, użyj polecenia cmdlet **Get-Help** .

    Get-Help <cmdlet-name> -Detailed

Na przykład aby uzyskać pomoc dotyczącą polecenia cmdlet **AzureRmAccount logowania** , wpisz:

    Get-Help Login-AzureRmAccount -Detailed

Można również przeczytać następujące samouczki, aby zapoznać się z Menedżerem zasobów Azure w programie PowerShell Azure:

- [Jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md)
- [Przy użyciu programu PowerShell Azure przy użyciu Menedżera zasobów](../powershell-azure-resource-manager.md)


## <a id="connect"></a>Nawiązywanie połączenia z subskrypcji ##

Rozpocznij sesję programu PowerShell Azure i zaloguj się do konta Azure za pomocą następującego polecenia:  

    Login-AzureRmAccount 

Należy zauważyć, że jeśli korzystasz z konkretnego wystąpienia Azure, na przykład Azure dla instytucji rządowych, użyj parametru - środowiska przy użyciu tego polecenia. Na przykład:`Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

W oknie podręcznym przeglądarki wprowadź Azure nazwa użytkownika i hasło. Azure programu PowerShell pobiera wszystkie subskrypcje, które są skojarzone z tym kontem i domyślnie używa pierwszego.

Jeśli masz wiele subskrypcji i chcesz określić określonej służących do magazynu klucza Azure, wpisz poniższe czynności, aby wyświetlić subskrypcje dla Twojego konta:

    Get-AzureRmSubscription

Następnie aby określić subskrypcję, wpisz:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Aby uzyskać więcej informacji o konfigurowaniu Azure programu PowerShell zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).


## <a id="resource"></a>Tworzenie nowej grupy zasobów ##

Korzystając z Menedżera zasobów Azure, wszystkie zasoby pokrewne są tworzone w grupie zasobów. Firma Microsoft utworzy nową grupę zasobów o nazwie **ContosoResourceGroup** dla tego samouczka:

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a id="vault"></a>Tworzenie klucza magazynu ##

Polecenia cmdlet [New AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt603736\(v=azure.300\).aspx) umożliwia utworzenie klucza magazynu. To polecenie cmdlet występują trzy parametry obowiązkowe: **Nazwa grupy zasobów**, **klucza magazynu nazwy**i **lokalizacji geograficznej**.

Na przykład użycie magazynu nazwę **ContosoKeyVault**, nazwę grupy zasobów **ContosoResourceGroup**i lokalizacji **wschodnioazjatycki**, wpisz:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Wynik to polecenie cmdlet zawiera właściwości magazynu klucza, która została właśnie utworzona. Dwie najważniejsze właściwości są następujące:

- **Nazwa magazynu**: W tym przykładzie jest **ContosoKeyVault**. Użyje tej nazwy innych poleceń cmdlet klucza magazynu.
- **Identyfikator URI magazynu**: W tym przykładzie jest https://contosokeyvault.vault.azure.net/. Aplikacje korzystające z magazynu za pośrednictwem jej interfejsu API usługi REST, należy użyć tego identyfikatora URI.

Konto Azure jest teraz autoryzowany do operacji na tym klucza magazynu. Jeszcze nikt inny nie jest.

>[AZURE.NOTE]  Jeśli zostanie wyświetlony błąd **subskrypcji nie została zarejestrowana jako nazw "Microsoft.KeyVault"** podczas próby tworzenia do nowego klucza magazynu, uruchom `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` , a następnie uruchom ponownie polecenie Nowy AzureRmKeyVault. Aby uzyskać więcej informacji zobacz [AzureRmResourceProvider rejestru](https://msdn.microsoft.com/library/azure/mt759831\(v=azure.300\).aspx).
>

## <a id="add"></a>Dodaj klucz lub hasło do klucza magazynu ##

Jeśli chcesz Azure klucza magazynu utworzyć klucz oprogramowanie chronione za Ciebie, należy użyć polecenia cmdlet [Dodaj AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) i wpisz następujące polecenie:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Jednak jeśli masz istniejący klucz oprogramowanie chronione w. Plik PFX zapisany na dysku C:\ w pliku o nazwie softkey.pfx, który chcesz przekazać do magazynu klucza Azure, wpisz poniższe czynności, aby ustawić zmiennych **securepfxpwd** o podanie hasła **123** dla. Plik PFX:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Następnie wpisz poniższe czynności, aby zaimportować klucza z. Plik PFX chroni klucz przez oprogramowanie w usłudze klucza magazynu:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


Można teraz odwoływać się ten klucz utworzony lub przekazane do magazynu klucza Azure, przy użyciu jej identyfikatora URI. Zawsze pobranie bieżącej wersji przy użyciu **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** , a następnie użyj **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** , aby uzyskać ten określonej wersji.  

Aby wyświetlić identyfikator URI dla tego klucza, należy wpisać:

    $Key.key.kid

Aby dodać hasło do magazynu, jest to hasło o nazwie SQLPassword i ma wartość Pa$ $w0rd do magazynu klucza Azure, najpierw przekonwertować tę wartość z Pa$ $w0rd bezpiecznego ciąg wpisując następujące:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Następnie wpisz następujące polecenie:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Można teraz odwoływać się to hasło dodanego do magazynu klucza Azure, przy użyciu jej identyfikatora URI. Zawsze pobranie bieżącej wersji przy użyciu **https://ContosoVault.vault.azure.net/secrets/SQLPassword** , a następnie użyj **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** , aby uzyskać ten określonej wersji.

Aby wyświetlić identyfikator URI dla tego hasła, wpisz:

    $secret.Id

Załóżmy wyświetlić klucz lub hasło, która została właśnie utworzona:

- Aby wyświetlić klucz, należy wpisać:`Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
- Aby wyświetlić poufnych, należy wpisać:`Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Teraz z magazynu kluczy i klucza lub hasło jest gotowy do aplikacji do użytku. Należy zezwolić aplikacji do korzystania z nich.  

## <a id="register"></a>Zarejestrowanie aplikacji z usługą Azure Active Directory ##

Ten krok zazwyczaj przeniesienie może być wykonywane przez dewelopera, na innym komputerze. Nie dotyczy Azure klucza magazynu, ale są przeznaczone dla kompletności.


>[AZURE.IMPORTANT] Aby zakończyć samouczka, konta, Magazyn i aplikacji, która zostanie zarejestrowana w tym kroku muszą być w tym samym katalogu Azure.

Aplikacje, które używają klucza magazynu musi uwierzytelniania za pomocą tokenu z usługi Azure Active Directory. Aby to zrobić, właściciel aplikacji należy najpierw zarejestrować aplikacji w ich usługi Azure Active Directory. Na koniec rejestracji właściciela aplikacji otrzymuje następujące wartości:


- **Identyfikator aplikacji** (nazywane także identyfikator klienta) i **klucz uwierzytelniania** (nazywane także wspólne hasło). Aplikacja musi przedstawić obie te wartości usługi Azure Active Directory, aby uzyskać tokenu. Jak aplikacja jest skonfigurowana w tym celu zależy od aplikacji. Na podstawie próbki klucza magazynu właściciela aplikacji ustawia te wartości w pliku app.config.

Aby zarejestrować aplikacji w usłudze Azure Active Directory:

1. Zaloguj się do portalu klasyczny Azure.
2. Po lewej stronie kliknij pozycję **Usługi Active Directory**, a następnie wybierz katalogu, w którym będzie zarejestrować aplikację. <br> <br> **Uwaga:** Po zaznaczeniu tego samego katalogu, zawierającego Azure subskrypcję, z którym został utworzony z magazynu kluczy. Jeśli nie znasz katalogu to jest, kliknij pozycję **Ustawienia**, identyfikowania subskrypcji, z którym został utworzony z magazynu kluczy i zanotuj nazwę katalogu wyświetlane w ostatniej kolumnie.

3. Kliknij pozycję **aplikacje**. Jeśli aplikacji nie zostały dodane do katalogu, ta strona zawiera tylko łącze **Dodaj aplikację** . Kliknij łącze lub Alternatywnie możesz można kliknąć pozycję **Dodaj** na pasku poleceń.
4.  W kreatorze **Dodawanie aplikacji** na **co chcesz zrobić?** strony, kliknij pozycję **Dodaj aplikację, którą rozwija się w mojej organizacji**.
5.  Na stronie **Przekaż nam informacje o aplikacji** Określ nazwę dla aplikacji, a następnie wybierz **Interfejs API sieci WEB i/lub aplikacji sieci WEB** (ustawienie domyślne). Kliknij przycisk **Dalej** .
6.  Na stronie **Właściwości aplikacji** określ **Adres URL logowania na** i **Aplikacji identyfikator URI** dla aplikacji sieci web. Jeśli aplikacji nie ma te wartości, można wprowadzać dla tego kroku (na przykład można określić http://test1.contoso.com dla obu pól). Nie ma znaczenia, jeśli istnieje tych witryn. Co to jest ważne jest aplikacji identyfikator URI dla każdej aplikacji różne dla każdej aplikacji w katalogu. Za pomocą tego ciągu katalogu identyfikowanie aplikacji.
7.  Kliknij ikonę **ukończone** , aby zapisać zmiany w kreatorze.
8.  Na stronie **Szybkie uruchamianie** kliknij przycisk **Konfiguruj**.
9.  Przewiń do sekcji **klawiszy** , wybierz czas trwania, a następnie kliknij przycisk **ZAPISZ**. Strona odświeża i zostaną wyświetlone wartości klucza. Tej wartości klucza i wartość **Identyfikator klienta** musi skonfigurować aplikację. (Instrukcje dotyczące konfiguracji są specyficzne dla aplikacji).
10. Skopiuj wartość Identyfikatora klienta na tej stronie użyjesz w następnym kroku, ustawianie uprawnień do magazynu.

## <a id="authorize"></a>Autoryzuj aplikacji użyj klawisza lub hasło ##

Aby autoryzować aplikacji, aby uzyskać dostęp do klucza lub hasła w magazyn, należy użyć polecenia cmdlet  [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625\(v=azure.300\).aspx) .

Na przykład jeśli Twoja nazwa magazynu to **ContosoKeyVault** i aplikację, którą chcesz zezwolić ma identyfikator klienta 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed i chcesz zezwolić aplikacji odszyfrowywanie i zaloguj się za pomocą klawiszy w swojej magazynu, uruchom następujące polecenie:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Jeśli chcesz zezwolić tej samej aplikacji, aby odczytać hasła do magazynu, uruchom następujące polecenie:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>Jeśli chcesz używać sprzętu zabezpieczeń moduły ##

W odniesieniu do zapewnienia dodanego można importować lub wygenerowania kluczy w modułach zabezpieczeń sprzętu (HSM), które nigdy nie pozostawić granicę HSM. HSM są FIPS 140-2 poziomu 2 sprawdzana poprawność. Jeśli to wymaganie nie dotyczy użytkownika, pominąć tę sekcję i przejdź do [usunięcia klucza magazynu i skojarzone klucze i hasła](#delete).

Aby utworzyć klucze chroniony HSM, należy użyć [warstwa usług Azure klucza magazynu Premium do obsługi chroniony HSM klawiszy](https://azure.microsoft.com/pricing/free-trial/). Ponadto należy zauważyć, że ta funkcja nie jest dostępna dla Chin Azure.


Podczas tworzenia klucza magazynu należy dodać parametr **- SKU** :


    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



Oprogramowanie chronione kluczach (jak pokazano powyżej) i chroniony HSM można dodać do tego magazynu kluczy. Aby utworzyć klucz chroniony HSM, ustaw **— miejsce docelowe** parametr "HSM":

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

Następujące polecenie umożliwia importowanie klucza z. Plik PFX na Twoim komputerze. To polecenie importuje klucz do HSM w usłudze klucza magazynu:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


Następne polecenie importuje "wprowadzić własny klucz" pakiet (BYOK). W tym scenariuszu umożliwia generowanie klucza w swojej lokalnej HSM i przenieść ją do HSM w usłudze klucza magazynu, bez opuszczania granicę HSM klucza:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Aby uzyskać bardziej szczegółowe instrukcje dotyczące do generowania tego pakietu BYOK zobacz [jak wygenerować i transfer chroniony HSM klawisze Azure klucza magazynu](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Usuwanie klucza magazynu i skojarzonych z nim kluczy i haseł ##

Jeśli nie potrzebujesz już klucza magazynu i klucza lub hasła, które zawiera, możesz usunąć klucza magazynu przy użyciu polecenia cmdlet [AzureRmKeyVault Usuń](https://msdn.microsoft.com/library/azure/mt619485\(v=azure.300\).aspx) :

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Lub można usunąć cały zasób Azure grupy, która zawiera kluczowe magazynu i inne zasoby, które zostały uwzględnione w tej grupie:

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>Inne polecenia cmdlet programu PowerShell Azure ##

Inne polecenia, które mogą okazać się przydatne do zarządzania Azure klucza magazynu:

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: To polecenie otrzymuje tabelaryczny wyświetlania wszystkich kluczy i wybranych właściwości.
- `$Keys[0]`: To polecenie wyświetla pełną listę właściwości dla określonego klucza
- `Get-AzureKeyVaultSecret`: To polecenie wyświetla tabelaryczny wyświetlania wszystkich nazw tajne i wybranych właściwości.
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Przykład jak usunąć klucza.
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Przykład jak usunąć określone hasło.


## <a id="next"></a>Następne kroki ##

Samouczek monitowania używa magazynu klucza Azure w aplikacji sieci web zobacz [Używanie Azure klucza magazynu z aplikacji sieci Web](key-vault-use-from-web-application.md).

Aby zobaczyć, jak jest używany do magazynu klucza, zobacz [Azure klucza magazynu rejestrowania](key-vault-logging.md).

Aby uzyskać listę najnowszych polecenia cmdlet programu PowerShell Azure dla magazynu klucza Azure zobacz [Polecenia cmdlet magazynu klucza Azure](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.300\).aspx). 
 

Programowania odwołania, zobacz [Przewodnik programisty Azure klucza magazynu](key-vault-developers-guide.md).
