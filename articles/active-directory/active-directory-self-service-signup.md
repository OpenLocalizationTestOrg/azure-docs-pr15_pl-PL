<properties
    pageTitle="Co to jest samoobsługowego tworzenia konta dla Azure? | Microsoft Azure"
    description="Omówienie samodzielnego tworzenia konta dla Azure, jak zarządzać proces zapisywania i jak przejęcie kontroli nad nazwy domeny DNS."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/23/2016"
    ms.author="curtand"/>


# <a name="what-is-self-service-signup-for-azure"></a>Co to jest samoobsługowego tworzenia konta dla Azure?

W tym temacie wyjaśniono procesu samodzielnego tworzenia konta i jak przejęcie kontroli nad nazwy domeny DNS.  

## <a name="why-use-self-service-signup"></a>Dlaczego warto używać samodzielnego tworzenia konta?

- Uzyskaj odbiorców do usług, które mają szybciej.
- Tworzenie oferty usługi opartej na poczcie e-mail.
- Tworzenie przepływów opartej na poczcie e-mail zapisywania szybko Zezwalanie użytkownikom na tworzenie tożsamości za pomocą aliasów e-mail ich łatwego do zapamiętania pracy.
- Niezarządzanej katalogi Azure mogą zostać wprowadzone później w zarządzanych katalogi i ponownie innych usług.

## <a name="terms-and-definitions"></a>Terminy i definicje

+ **Zamów samodzielnego**: jest to metoda za pomocą której użytkownik rejestruje się do usługi w chmurze i tożsamości tworzone automatycznie dla nich w usłudze Azure Active Directory (Azure AD) opartego na swojej domeny poczty e-mail.
+ **Katalog niezarządzanej Azure**: jest to katalog, w której jest tworzona tej tożsamości. Katalog niezarządzanej jest katalogu, w którym ma administratora globalnego.
+ **Weryfikacja e-mail użytkownika**: jest to typ konta użytkownika w Azure AD. Użytkownik, który ma tożsamości tworzone automatycznie po zalogowaniu do sklepu internetowego oferty nosi nazwę użytkownika zweryfikowana poczty e-mail. Weryfikacja e-mail użytkownika jest członkiem zwykła katalogu oznakowane creationmethod = EmailVerified.

## <a name="user-experience"></a>Środowisko użytkownika

Załóżmy na przykład użytkownika, którego wiadomości e-mail jest Dan@BellowsCollege.com otrzymuje poufnych plików pocztą e-mail. Pliki są chronione przez usługi Azure Rights Management (Azure RMS). Ale Paweł w organizacji, szkoły miech nie po zarejestrowaniu dla usługi Azure RMS, ani wdrożono Active Directory RMS. W tym przypadku Paweł, można zarejestrować bezpłatnej subskrypcji usługi RMS dla osób, aby móc odczytywać plików chronionych.

Jeśli Paweł pierwszego użytkownika przy użyciu adresu e-mail z BellowsCollege.com do utworzenia konta tego samodzielnego oferuje, a następnie niezarządzanej katalogu zostaną utworzone dla BellowsCollege.com w Azure AD. Jeśli inni użytkownicy z domeny BellowsCollege.com konta dla tej oferty lub podobne oferująca samoobsługowe, mają one także konta użytkowników zweryfikowana poczty e-mail utworzone w tym samym katalogu niezarządzanej platformy Azure.

## <a name="admin-experience"></a>Środowiska pracy — administrator

Administrator właściciela nazwy domeny DNS niezarządzanej katalogu Azure może przejąć lub scalić katalogu po potwierdzający własność. Następne sekcje opisują środowiska pracy administrator bardziej szczegółowo, ale Oto Podsumowanie:

- Po wykonaniu nad Azure niezarządzanej stają się po prostu administratora globalnego niezarządzanej katalogu. Czasami nazywa wewnętrznych przejęcia.
- Gdy scalanie niezarządzanej katalogu Azure, Dodawanie domeny DNS niezarządzanej katalogu do katalogu Azure zarządzanych i mapowania użytkowników do zasobów jest tworzona tak użytkownicy mogą nadal uzyskać dostęp do usług bez zakłóceń. Jest to nazywane zewnętrznych przejęcia.

## <a name="what-gets-created-in-azure-active-directory"></a>Co zostanie utworzony w usłudze Active Directory platformy Azure?

#### <a name="directory"></a>katalogu

- Azure Active directory dla domeny utworzono jednego katalogu dla domeny.
- Katalog Azure AD ma nie rolą administratora globalnego.

#### <a name="users"></a>Użytkownicy

- Dla każdego użytkownika logujący się obiekt użytkownika zostanie utworzona w katalogu Azure AD.
- Każdy obiekt użytkownika jest oznaczony jako zewnętrzny.
- Każdego użytkownika, wyznaczona programu access z usługą podpisaną konta w usłudze.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Jak przejmowanie Samoobsługowe Azure AD katalogu dla domeny własnym?

Użytkownik rościć sobie Samoobsługowe Azure AD katalogu przy sprawdzaniu poprawności domeny. Sprawdzanie poprawności domeny okaże się, że jesteś właścicielem domeny, tworząc rekordy DNS.

Istnieją dwa sposoby przejęcia DNS katalogu Azure AD:

- wewnętrzna przejęcia (Administrator zostanie odnaleziona niezarządzanej katalogu Azure i chce przekształcić zarządzany katalog)
- Przejmowanie zewnętrznych (Administrator próbuje dodać nową domenę do ich zarządzanych katalogu Azure)

Może Cię zainteresować sprawdzania poprawności właścicielem domeny, ponieważ przejęcia niezarządzanej katalogu po użytkownik wykona samodzielnego tworzenia konta lub może być Dodawanie nowej domeny do istniejącego katalogu zarządzane. Na przykład masz domenę o nazwie contoso.com i chcesz dodać nową domenę o nazwie contoso.co.uk lub contoso.uk.

## <a name="what-is-domain-takeover"></a>Co to jest przejęcia domeny?  

W tej sekcji omówiono sposób sprawdzić, czy jesteś właścicielem domeny

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Co to jest sprawdzanie poprawności domeny i dlaczego jest używane?

Aby można było wykonywać operacje w katalogu, Azure AD wymaga, sprawdź poprawność własności domeny DNS.  Sprawdzanie poprawności domeny umożliwia rościć sobie katalogu, a następnie promowanie katalogu Samoobsługowe zarządzany katalog lub scalić istniejący katalog zarządzanych katalogu Sklep internetowy.

## <a name="examples-of-domain-validation"></a>Przykłady sprawdzania poprawności domeny

Istnieją dwa sposoby przejęcia DNS katalogu:

+ Przejmowanie wewnętrznych (na przykład administrator zostanie odnaleziona samoobsługowe, niezarządzanej katalogu i chce przekształcić zarządzany katalog)
+ Przejmowanie zewnętrznych (na przykład administratorem próbuje dodać nową domenę do katalogu zarządzanych)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>Wewnętrzna przejęcia - promowanie samoobsługowe, niezarządzanej katalogu katalogiem zarządzanych

Po wykonaniu wewnętrznych przejęcia katalogu otrzymuje konwertowane z katalogu niezarządzanej zarządzanych katalogu. Należy wykonać sprawdzanie nazwy domeny DNS, gdzie utworzyć rekord MX lub rekordu TXT w strefie DNS. Ta akcja:

+ Sprawdza, czy jesteś właścicielem domeny
+ Służy do katalogu zarządzanych
+ Oznacza jesteś administratorem globalnym katalogu

Załóżmy, że administrator IT z szkoły miech wykryje, że użytkownikom szkoły zalogowali Samoobsługowe oferty. Zarejestrowany właściciel DNS nazw BellowsCollege.com, IT administrator można sprawdzić poprawność własności nazwy DNS platformy Azure i następnie przejąć niezarządzanej katalogu. Katalog staje się zarządzanych katalogu, a IT administrator jest przypisana rola administratora globalnego katalogu BellowsCollege.com.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Zewnętrzne przejęcia - scalić istniejący katalog zarządzanych katalogu Sklep internetowy

W zewnętrznym przejęcia masz już zarządzany katalog i chcesz dołączyć do tego katalogu zarządzane przez wszystkich użytkowników i grup z katalogu niezarządzanej, zamiast dwóch własnych oddzielić katalogów.

Jako administrator zarządzanych katalogu dodać domenę, a tej domeny stanie się mieć niezarządzanej katalog skojarzony z nim.

Na przykład załóżmy, że jesteś administratorem IT i masz już zarządzany katalog dla Contoso.com, nazwę domeny, który jest zarejestrowany dla Twojej organizacji. Odkrywanie, że użytkownikom z organizacji wykonywanej samodzielnego tworzenia konta przez oferująca przy użyciu nazwy domeny poczty e-mail user@contoso.co.uk, czyli innej nazwy domeny, której właścicielem Twojej organizacji. Obecnie Ci użytkownicy mają konta w niezarządzanej katalogu contoso.co.uk.

Nie chcesz, aby zarządzać dwóch oddzielnych katalogów, aby scalić niezarządzanej katalog contoso.co.uk na istniejące w katalogu IT zarządzane contoso.com.

Przejmowanie zewnętrznych rozpoczyna ten sam proces sprawdzania poprawności DNS jako przejęcia wewnętrzny.  Czym różnicę: użytkowników i usług są mapowane ponownie do katalogu IT zarządzane.

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>Jaki jest wpływ wykonywania przejęcia zewnętrznych?

Z zewnętrznego przejęcia mapowanie użytkowników do zasobów jest tworzone użytkownicy nadal mogą uzyskiwać dostęp do usług bez zakłóceń. Wiele aplikacji, w tym usługi RMS dla użytkowników indywidualnych, oraz obsługiwać mapowania użytkowników do zasobów, a użytkownicy nadal mogą uzyskać dostęp do tych usług bez zmian. Jeśli aplikacji nie obsługuje skuteczne mapowanie użytkowników do zasobów, przejęcia zewnętrznych mogą być jawnie blokowane aby uniemożliwić użytkownikom niskiej środowiska.

#### <a name="directory-takeover-support-by-service"></a>Obsługa przejęcia Directory przez usługę

Obecnie są obsługiwane przejęcia następujące usługi:

- RMS


Przejęcia wkrótce będzie pomocniczych następujące usługi:

- PowerBI

Poniższy nie i wymagają dodatkowych administratorów akcji do migracji danych użytkownika po zewnętrznych przejęcia.

- Usługa OneDrive i programie SharePoint


## <a name="how-to-perform-a-dns-domain-name-takeover"></a>Jak przeprowadzić przejęcia nazwy domeny DNS

Istnieje kilka opcji dotyczących sprawdzana poprawność domeny (i wykonaj przejęcia, w razie potrzeby):

1.  Portal Azure zarządzania

    Przejmowanie zostanie wywołana, wykonując dodanie domeny.  Jeśli katalog już istnieje dla domeny, masz opcję, aby wykonać zewnętrznych przejęcia.

    Zaloguj się do portalu Azure przy użyciu swoich poświadczeń.  Przejdź do istniejącego katalogu, a następnie **Dodaj domenę**.

2.  Usługi Office 365

    Za pomocą opcji na stronie [Zarządzanie domenami](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) w usłudze Office 365 do współdziałania z domenami i rekordy DNS. Zobacz [Weryfikowanie domeny w usłudze Office 365](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).

3.  Środowiska Windows PowerShell

    Poniższe czynności są wymagane do wykonania przy sprawdzaniu poprawności przy użyciu programu Windows PowerShell.

    Krok    |   Polecenie cmdlet umożliwia
    ------- | -------------
    Tworzenie obiektu poświadczeń | Uzyskiwanie poświadczeń
    Nawiązywanie połączenia z Azure AD | Łączenie MsolService
    Pobierz listę domen   | Get-MsolDomain
    Tworzenie wezwanie  | Get-MsolDomainVerificationDns
    Tworzenie rekordu DNS   | Wykonaj następujące czynności na serwerze DNS
    Sprawdź największym wyzwaniem    | Potwierdź MsolEmailVerifiedDomain

Na przykład:

1. Nawiązywanie połączenia z Azure AD przy użyciu poświadczeń użytych w celu odpowiedzieć oferująca samoobsługowego: importowanie modułu MSOnline $msolcred = msolservice łączenie poświadczeń get-poświadczeń $msolcred

2. Pobierz listę domen:

    Get-MsolDomain

3. Następnie uruchom polecenie cmdlet Get-MsolDomainVerificationDns, aby utworzyć wezwanie:

    Get-MsolDomainVerificationDns nazwa_domeny — *twoja_nazwa_domeny* — DnsTxtRecord tryb

    Na przykład:

    Get-MsolDomainVerificationDns — nazwa_domeny contoso.com — tryb DnsTxtRecord

4. Skopiuj wartość (wyzwania), która jest zwracana tego polecenia.

    Na przykład:

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9

5. W swojej publicznej nazw DNS Utwórz rekord txt DNS, który zawiera wartość, który został skopiowany w poprzednim kroku.

    Nazwa tego rekordu jest nazwą domeny nadrzędnej, więc jeśli tworzysz ten rekord zasobu przy użyciu roli DNS z systemu Windows Server, pozostaw Wklej puste i tylko nazwę rekordu wartość w polu tekstowym

6. Uruchom polecenie cmdlet MsolDomain Potwierdź, aby zweryfikować największym wyzwaniem:

    Potwierdź MsolEmailVerifiedDomain - DomainName *twoja_nazwa_domeny*

    na przykład:

    Potwierdź MsolEmailVerifiedDomain - DomainName contoso.com

Pomyślnie wezwanie zwraca monit bez błędów.

## <a name="how-do-i-control-self-service-settings"></a>Jak określić ustawienia Samoobsługowe?

Administratorzy mają dwa formanty Samoobsługowe dzisiaj. Kontrolować:

- Czy użytkownicy mogą dołączyć do katalogu za pośrednictwem poczty e-mail.
- Czy użytkownicy mogą licencji się dla aplikacji i usług.


### <a name="how-can-i-control-these-capabilities"></a>Jak można kontrolować te funkcje?

Administrator może skonfigurować te funkcje przy użyciu tych parametrów polecenia cmdlet Set-MsolCompanySettings Azure AD:

+ **AllowEmailVerifiedUsers** Określa, czy użytkownik można utworzyć lub Dołącz niezarządzanej katalog. Jeśli ten parametr jest ustawiony na $false, nikt zweryfikowana poczty e-mail mogą dołączyć do katalogu.
+ **AllowAdHocSubscriptions** określa możliwość użytkownikom wykonywanie samodzielnego tworzenia konta. Jeśli ten parametr jest ustawiony na $false, użytkownicy nie mogą wykonywać samodzielnego tworzenia konta.


### <a name="how-do-the-controls-work-together"></a>Jak formanty działają razem?

Tych parametrów może służyć w połączeniu, aby zdefiniować bardziej precyzyjnie kontrolować samodzielnego tworzenia konta. Na przykład następujące polecenie umożliwi użytkownikom wykonywanie samodzielnego, ale tylko wtedy, jeśli ci użytkownicy mają już konto w Azure AD (innymi słowy, użytkownicy potrzebują konta zweryfikowana poczty e-mail do utworzenia nie można wykonać samodzielnego tworzenia konta):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Następujące schematu blokowego wyjaśniono wszystkie różne kombinacje dla tych parametrów i wynikowe warunki dla katalogu i samodzielnego w górę.

![][1]

Aby uzyskać więcej informacji i przykłady dotyczące używania tych parametrów zobacz [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx).

## <a name="see-also"></a>Zobacz też

-  [Jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md)

-  [Azure programu PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)

-  [Informacje dotyczące poleceń Cmdlet Azure](https://msdn.microsoft.com/library/azure/jj554330.aspx)

-  [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
