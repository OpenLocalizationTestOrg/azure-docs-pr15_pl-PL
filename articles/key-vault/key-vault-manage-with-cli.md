<properties
    pageTitle="Zarządzanie klucza magazynu przy użyciu interfejsu wiersza polecenia | Microsoft Azure"
    description="Użyj tego samouczka, aby zautomatyzować typowe zadania w klucza magazynu przy użyciu interfejsu wiersza polecenia"
    services="key-vault"
    documentationCenter=""
    authors="BrucePerlerMS"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="bruceper"/>

# <a name="manage-key-vault-using-cli"></a>Zarządzanie klucza magazynu przy użyciu interfejsu wiersza polecenia #
Azure magazynu klucza jest dostępna w większości regionów. Aby uzyskać więcej informacji zobacz [Klucz magazynu ceny strony](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Wprowadzenie  
Użyj tego samouczka, ułatwiające rozpoczęcie pracy z magazynu klucza Azure, aby utworzyć kontener zaostrzony (magazynu) platformy Azure, przechowywanie i zarządzać nimi klucze szyfrowania hasła Azure. Jego przeprowadzi Cię przez proces Tworzenie magazynu, zawierającą klucz lub hasło, które następnie można z aplikacją Azure za pomocą interfejsu wiersza polecenia Azure między platformami. Następnie przedstawiono sposób aplikacji można użyć tego klucza lub hasło.

**Szacowany czas wykonania:** 20 minut

>[AZURE.NOTE]  Ten samouczek nie zawiera instrukcje dotyczące programowania aplikacji Azure, że jeden z kroków zawiera, który wskazuje, jak zezwolić aplikacji użyj klawisza lub tajny w klucza magazynu.
>
>Obecnie nie można skonfigurować Azure klucza magazynu w portalu Azure. Użyj zamiast tego następujące instrukcje interfejs wiersza polecenia między platformami. Lub, aby uzyskać instrukcje programu PowerShell Azure, [Ten samouczek równoważne](key-vault-get-started.md).

Informacje o magazynu klucza Azure, zobacz [Co to jest Azure klucza magazynu?](key-vault-whatis.md)

## <a name="prerequisites"></a>Wymagania wstępne
Aby użyć tego samouczka, musisz mieć następujące czynności:

- Subskrypcję usługi Microsoft Azure. Jeśli nie masz, można Załóż [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial).
- Interfejs wiersza polecenia wersji 0.9.1 lub nowszej. Aby zainstalować najnowszą wersję i połączyć do subskrypcji usługi Azure, zobacz [Instalowanie i Konfigurowanie interfejsu wiersza polecenia i Platform Azure](../xplat-cli-install.md).
- Aplikacja zostanie skonfigurowana za pomocą klucza lub hasła, które są tworzone w tym samouczku. Przykładowa aplikacja jest dostępna z [Centrum pobierania Microsoft](http://www.microsoft.com/download/details.aspx?id=45343). Aby uzyskać instrukcje Zobacz tym pliku Readme.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Uzyskiwanie pomocy z interfejsu wiersza polecenia i Platform Azure

Tego samouczka przyjęto założenie, że zaznajomieni z interfejsu wiersza polecenia (imprezie, końcowych, wiersz polecenia)

Pomocy lub -h parametr można wyświetlić Pomoc dotyczącą określonego polecenia. Alternatywnie azure pomocy [polecenie], że format [opcje] można także zwraca te same informacje. Jeśli na przykład wszystkie następujące polecenia zwraca te same informacje:

    azure account set --help

    azure account set -h

    azure help account set

W razie wątpliwości dotyczących parametrów wymagane przez polecenie, zobacz Pomoc przy użyciu — pomoc, -h lub azure help [polecenie].

Można również przeczytać następujące samouczki, aby zapoznać się z Menedżerem zasobów Azure Azure i Platform wiersza polecenia:

- [Jak zainstalować i skonfigurować interfejs wiersza polecenia i Platform Azure](../xplat-cli-install.md)
- [Za pomocą interfejsu wiersza polecenia i Platform Azure z Azure Menedżera zasobów](../xplat-cli-azure-resource-manager.md)


## <a name="connect-to-your-subscriptions"></a>Nawiązywanie połączenia z subskrypcji

Aby zalogować się przy użyciu konta organizacji, użyj następującego polecenia:

    azure login -u username -p password

lub jeśli chcesz się zalogować, wpisując interakcyjne

    azure login

>[AZURE.NOTE]  Metody logowania działa wyłącznie z konta organizacji. Konto organizacji to użytkownik, który jest zarządzane przez organizację i zdefiniowane w dzierżawie usługi Azure Active Directory organizacji.


Jeśli obecnie nie ma konta organizacji i przy użyciu konta Microsoft w celu zalogowania do subskrypcji usługi Azure, można łatwo można utworzyć jedną wykonaj następujące czynności.

1.  Zaloguj się do logowania się do [Portalu zarządzania Azure](https://manage.windowsazure.com/)i kliknij pozycję w usłudze Active Directory.
2.  Jeśli katalog nie istnieje, wybierz opcję Utwórz katalogu i wprowadź wymagane informacje.
3.  Zaznacz katalogu i dodawanie nowego użytkownika. Ten nowy użytkownik jest konto organizacji. Podczas tworzenia użytkownika będzie dostarczanych z obu adresu e-mail dla użytkownika i hasła tymczasowego. Zapisz te informacje, ponieważ jest używana w kolejnym kroku.
4.  Z poziomu portalu wybierz pozycję Ustawienia, a następnie wybierz pozycję Administratorzy. Wybierz przycisk Dodaj, a następnie dodać nowego użytkownika jako administratora współpracujących. Dzięki temu konta organizacji zarządzać subskrypcją Azure.
5.  Na koniec Wyloguj się z portalem Azure, a następnie zaloguj się ponownie przy użyciu nowego konta organizacji. Jeśli jest to pierwszy rejestrowania czasu przy użyciu tego konta, pojawi się zmienianie.

Aby uzyskać więcej informacji na temat za pomocą konta organizacji z Microsoft Azure zobacz [konta w usłudze Microsoft Azure jako organizacji](../active-directory/sign-up-organization.md).

Jeśli masz wiele subskrypcji i chcesz określić określonej służących do magazynu klucza Azure, wpisz poniższe czynności, aby wyświetlić subskrypcje dla Twojego konta:

    azure account list

Następnie aby określić subskrypcję, wpisz:

    azure account set <subscription name>

Aby uzyskać więcej informacji o konfigurowaniu interfejsu wiersza polecenia i Platform Azure zobacz [jak instalowanie i Konfigurowanie interfejsu wiersza polecenia i Platform Azure](../xplat-cli-install.md).


## <a name="switch-to-using-azure-resource-manager"></a>Przełączyć się przy użyciu Menedżera zasobów Azure

Magazyn klucza wymagają Azure Menedżera zasobów, więc wpisz poniższe czynności, aby przełączyć się do trybu Menedżer zasobów Azure:

    azure config mode arm

## <a name="create-a-new-resource-group"></a>Tworzenie nowej grupy zasobów

Korzystając z Menedżera zasobów Azure, wszystkie zasoby pokrewne są tworzone w grupie zasobów. Ten samouczek zostanie utworzony nowej grupy zasobów "ContosoResourceGroup".

    azure group create 'ContosoResourceGroup' 'East Asia'

Pierwszy parametr jest nazwa grupy zasobów i drugi parametr jest lokalizacja. Aby określić położenie, użyj polecenia `azure location list` do identyfikowania określania alternatywnej lokalizacji do tego, w tym przykładzie. Aby uzyskać więcej informacji, wpisz:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>Dostawcy zasobów magazynu klucza rejestru
Upewnij się, że tego dostawcy zasobów magazynu klucza jest zarejestrowana w ramach subskrypcji:

`azure provider register Microsoft.KeyVault`

To tylko musi zostać wykonane raz na subskrypcję.


## <a name="create-a-key-vault"></a>Tworzenie klucza magazynu

Używanie `azure keyvault create` polecenie, aby utworzyć klucza magazynu. Ten skrypt występują trzy parametry obowiązkowe: Nazwa grupy zasobów, nazwa klucza magazynu i lokalizacji geograficznej.

Na przykład użycie magazynu nazwę ContosoKeyVault, nazwę grupy zasobów ContosoResourceGroup i lokalizacji wschodnioazjatycki, wpisz:

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

Wynik tego polecenia zawiera właściwości magazynu klucza, która została właśnie utworzona. Dwie najważniejsze właściwości są następujące:

- **Nazwa**: W tym przykładzie jest ContosoKeyVault. Użyje tej nazwy innych poleceń cmdlet klucza magazynu.
- **vaultUri**: W tym przykładzie jest https://contosokeyvault.vault.azure.net. Aplikacje korzystające z magazynu za pośrednictwem jej interfejsu API usługi REST, należy użyć tego identyfikatora URI.

Konto Azure jest teraz autoryzowany do operacji na tym klucza magazynu. Jeszcze nikt inny nie jest.


## <a name="add-a-key-or-secret-to-the-key-vault"></a>Dodaj klucz lub hasło do klucza magazynu

Azure klucza magazynu utworzyć klucz oprogramowanie chronione za Ciebie, należy użyć `azure key create` polecenia i wpisz następujące polecenie:

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

Jednak jeśli masz istniejący klucz w pliku .pem zapisany jako plik lokalny w pliku o nazwie softkey.pem, który chcesz przekazać do magazynu klucza Azure, wpisz poniższe czynności, aby zaimportować klucza z. Plik PEM chroni klucz przez oprogramowanie w usłudze klucza magazynu:

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

Można teraz odwoływać się klucz utworzony lub przekazane do magazynu klucza Azure, przy użyciu jej identyfikatora URI. Zawsze pobranie bieżącej wersji przy użyciu **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** , a następnie użyj **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** , aby uzyskać ten określonej wersji.

Aby dodać hasło do magazynu, która jest hasło o nazwie SQLPassword i, który zawiera wartość z Pa$ $w0rd do magazynu klucza Azure, wpisz następujący ciąg:

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

Można teraz odwoływać się to hasło dodanego do magazynu klucza Azure, przy użyciu jej identyfikatora URI. Zawsze pobranie bieżącej wersji przy użyciu **https://ContosoVault.vault.azure.net/secrets/SQLPassword** , a następnie użyj **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** , aby uzyskać ten określonej wersji.

Załóżmy wyświetlić klucz lub hasło, która została właśnie utworzona:

- Aby wyświetlić klucz, należy wpisać:`azure keyvault key list --vault-name 'ContosoKeyVault'`
- Aby wyświetlić poufnych, należy wpisać:`azure keyvault secret list --vault-name 'ContosoKeyVault'`


## <a name="register-an-application-with-azure-active-directory"></a>Zarejestrowanie aplikacji z usługą Azure Active Directory

Ten krok zazwyczaj przeniesienie może być wykonywane przez dewelopera, na innym komputerze. Nie dotyczy Azure klucza magazynu, ale jest dołączany w tym celu zapewnienia kompletności.


>[AZURE.IMPORTANT] Aby zakończyć samouczka, konta, Magazyn i aplikacji, która zostanie zarejestrowana w tym kroku muszą być w tym samym katalogu Azure.

Aplikacje, które używają klucza magazynu musi uwierzytelniania za pomocą tokenu z usługi Azure Active Directory. Aby to zrobić, właściciel aplikacji należy najpierw zarejestrować aplikacji w ich usługi Azure Active Directory. Na koniec rejestracji właściciela aplikacji otrzymuje następujące wartości:


- **Identyfikator aplikacji** (nazywane także identyfikator klienta) i **klucz uwierzytelniania** (nazywane także wspólne hasło). Aplikacja musi przedstawić obie te wartości usługi Azure Active Directory, aby uzyskać tokenu. Jak aplikacja jest skonfigurowana w tym celu zależy od aplikacji. Na podstawie próbki klucza magazynu właściciela aplikacji ustawia te wartości w pliku app.config.



Aby zarejestrować aplikacji w usłudze Azure Active Directory:

1. Zaloguj się do portalu Azure.
2. Po lewej stronie kliknij pozycję **Usługi Active Directory**, a następnie wybierz katalogu, w którym będzie zarejestrować aplikację. <br> <br> Uwaga: Możesz wybrać katalogu, zawierającym Azure subskrypcję, z którym został utworzony z magazynu kluczy. Jeśli nie znasz katalogu to jest, kliknij pozycję **Ustawienia**, identyfikowania subskrypcji, z którym został utworzony z magazynu kluczy i zanotuj nazwę katalogu wyświetlane w ostatniej kolumnie.

3. Kliknij pozycję **aplikacje**. Jeśli aplikacji nie zostały dodane do katalogu, ta strona będzie wyświetlana tylko łącze **Dodaj aplikację** . Kliknij łącze lub Alternatywnie możesz kliknąć pozycję **Dodaj** na pasku poleceń.
4.  W kreatorze **Dodawanie aplikacji** na **co chcesz zrobić?** strony, kliknij pozycję **Dodaj aplikację, którą rozwija się w mojej organizacji**.
5.  Na stronie **Przekaż nam informacje o aplikacji** Określ nazwę dla aplikacji i wybierz **Interfejs API sieci WEB i/lub aplikacji sieci WEB** (ustawienie domyślne). Kliknij przycisk Dalej.
6.  Na stronie **Właściwości aplikacji** określ **Adres URL logowania na** i **Aplikacji identyfikator URI** dla aplikacji sieci web. Jeśli aplikacji nie ma te wartości, można wprowadzać dla tego kroku (na przykład można określić http://test1.contoso.com dla obu pól). Nie ma znaczenia, jeśli istnieją następujące witryny; Co to jest ważne jest aplikacji identyfikator URI dla każdej aplikacji różne dla każdej aplikacji w katalogu. Za pomocą tego ciągu katalogu identyfikowanie aplikacji.
7.  Kliknij ikonę ukończone, aby zapisać zmiany w kreatorze.
8.  Na stronie Szybkie uruchamianie kliknij przycisk **Konfiguruj**.
9.  Przewiń do sekcji **klawiszy** , wybierz czas trwania, a następnie kliknij przycisk **ZAPISZ**. Strona odświeża i zostaną wyświetlone wartości klucza. Tej wartości klucza i wartość **Identyfikator klienta** musi skonfigurować aplikację. (Instrukcje dotyczące konfiguracji będą specyficzne dla aplikacji.)
10. Skopiuj wartość Identyfikatora klienta na tej stronie użyjesz w następnym kroku, ustawianie uprawnień do magazynu.




## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Autoryzuj aplikacji użyj klawisza lub hasło

Aby zezwolić aplikacji, aby uzyskać dostęp do klucza lub hasła w magazyn, użyj `azure keyvault set-policy` polecenia.

Na przykład jeśli Twoja nazwa magazynu jest ContosoKeyVault aplikację, którą chcesz zezwolić ma identyfikator klienta 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed i chcesz zezwolić aplikacji odszyfrowywanie i zaloguj się za pomocą klawiszy w swojej magazynu, następnie uruchom następujące polecenie:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '["decrypt","sign"]'

>[AZURE.NOTE] Jeśli korzystasz z wiersza polecenia systemu Windows, należy zastąpić pojedynczy cudzysłów podwójny cudzysłów i również escape wewnętrznych podwójny cudzysłów. Na przykład: "[\"odszyfrowywanie\",\"znak\"]".

Jeśli chcesz zezwolić tej samej aplikacji, aby odczytać hasła do magazynu, uruchom następujące polecenie:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '["get"]'

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Jeśli chcesz używać sprzętu zabezpieczeń moduły ##

W odniesieniu do zapewnienia dodanego można importować lub wygenerowania kluczy w modułach zabezpieczeń sprzętu (HSM), które nigdy nie pozostawić granicę HSM. HSM są FIPS 140-2 poziomu 2 sprawdzana poprawność. Jeśli to wymaganie nie dotyczy użytkownika, pominąć tę sekcję i przejdź do [usunięcia klucza magazynu i skojarzone klucze i hasła](#delete-the-key-vault-and-associated-keys-and-secrets).

Aby utworzyć klucze chroniony HSM, musisz mieć subskrypcji magazynu, która obsługuje chroniony HSM klucze.

Po utworzeniu keyvault Dodaj parametr "sku":

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

Oprogramowanie chronione kluczach (jak pokazano powyżej) i chroniony HSM można dodać do tego magazynu. Aby utworzyć klucz chroniony HSM, ustaw parametr miejsca docelowego "HSM":

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

Następujące polecenie umożliwia importowanie klucz z pliku .pem na komputerze. To polecenie importuje klucz do HSM w usłudze klucza magazynu:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

Następne polecenie importuje "wprowadzić własny klucz" pakiet (BYOK). Umożliwia generowanie klucza w swojej lokalnej HSM i przenieść ją do HSM w usłudze klucza magazynu, bez opuszczania granicę HSM klucza:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

Aby uzyskać bardziej szczegółowe instrukcje na temat wygenerować ten pakiet BYOK, Dowiedz się, [jak korzystać z klawiszy HSM-Protected z magazynu klucza Azure](key-vault-hsm-protected-keys.md).


## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Usuwanie klucza magazynu i skojarzonych z nim kluczy i haseł

Jeśli nie potrzebujesz już klucza magazynu i klucza lub hasła, które zawiera, możesz usunąć klucza magazynu przy użyciu polecenia Usuń azure keyvault:

    azure keyvault delete --vault-name 'ContosoKeyVault'

Lub można usunąć cały zasób Azure grupy, która zawiera kluczowe magazynu i inne zasoby, które zostały uwzględnione w tej grupie:

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Inne polecenia Azure interfejs wiersza polecenia i Platform

Inne polecenia, że może być przydatne do zarządzania Azure klucza magazynu.

To polecenie wyświetla tabelaryczny wyświetlania wszystkich kluczy i wybranych właściwości:

    azure keyvault key list --vault-name 'ContosoKeyVault'

To polecenie wyświetla pełną listę właściwości dla określonego klucza:

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

To polecenie wyświetla tabelaryczny wyświetlania wszystkich nazw tajne i wybranych właściwości:

    azure keyvault secret list --vault-name 'ContosoKeyVault'

Oto przykładowy sposób usunięcia klucza:

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Oto przykładowy sposób usunięcia określonego hasła:

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a>Następne kroki

Programowania odwołania, zobacz [Przewodnik programisty Azure klucza magazynu](key-vault-developers-guide.md).
