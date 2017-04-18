<properties
    pageTitle="Azure AD Connect: Użytkownika, zaloguj się | Microsoft Azure"
    description="Azure AD Connect logowania dla ustawień niestandardowych."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/16/2016"
    ms.author="billmath"/>



# <a name="azure-ad-connect-user-sign-on-options"></a>Azure AD łączenie użytkownika logowania na temat opcji

Narzędzie Azure AD Connect umożliwia użytkownikom logowanie do zarówno w chmurze, jak i w lokalnych zasobów przy użyciu tych samych haseł. Ten artykuł zawiera opis podstawowych pojęć związanych z modelu każdej tożsamości ułatwiające wybór tożsamości, że ma być używany dla z logowaniem się do Azure AD.

Jeśli znasz już model tożsamości Azure AD i chcesz dowiedzieć się więcej o określonej metody, po prostu kliknij poniżej odpowiedni temat.

* [Synchronizacji haseł](#password-synchronization)
* [Federacyjnych logowania jednokrotnego (z usługami ADFS)](#federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm)


## <a name="choosing-a-user-sign-in-method"></a>Wybieranie metody logowania użytkownika
Do większości organizacji, którzy chcą po prostu włączyć użytkownika Logowanie do usługi Office 365, aplikacje władz akredytacji bezpieczeństwa i innych Azure AD na podstawie zasobów, zaleca się domyślna opcja synchronizacji haseł.
Niektóre organizacje jednak określonego powody przy użyciu federacyjnych znaku opcji, takich jak usług AD FS.  W tym:

- Twoja organizacja ma już usług AD FS lub 3rd strony dostawcy Federacji wdrożony
- Zasady dotyczące zabezpieczeń zabrania synchronizowanie hasła w chmurze
- Wymaganie użytkownicy odczuwać Bezproblemowa logowania jednokrotnego (bez dodatkowe hasła zostanie wyświetlony monit) podczas uzyskiwania dostępu do chmury zasobów z domeny komputerach połączonych za pomocą sieci firmowej
- Wymaganie niektórych określonych możliwości, które ma usług AD FS
    - Uwierzytelnianie wieloskładnikowe lokalnego przy użyciu innego dostawcy lub kart inteligentnych (Dowiedz się więcej o innych firm MFA dostawców usług AD FS w systemie Windows Server 2012 R2)
    - Aktywne funkcje integracji katalogów, takich jak Blokada konta wygładzone lub zasady godzin hasło i pracy AD
    - Warunkowego dostępu do obu lokalnej i w chmurze zasobów za pomocą rejestracji urządzenia, Dołącz Azure AD lub zasady w usłudze MDM Intune

### <a name="password-synchronization"></a>Synchronizacja haseł
Z synchronizacją haseł mieszania hasła użytkowników są synchronizowane z usługi Active Directory w lokalnej Azure AD.  Po zmianie hasła lub resetowanie w środowisku lokalnym, nowe hasła są synchronizowane natychmiast Azure AD tak, aby użytkownicy mogą zawsze za pomocą tego samego hasła dla zasobów chmury jak lokalnego.  Hasła nigdy nie są wysyłane do Azure AD ani przechowywane w Azure AD w postaci zwykłego tekstu.
Synchronizacji haseł można razem z hasła zapisu z powrotem włączyć tego elementu do resetowania hasła usługi w Azure AD.

<center>![Chmury](./media/active-directory-aadconnect-user-signin/passwordhash.png)</center>

[Więcej informacji na temat synchronizacji haseł](active-directory-aadconnectsync-implement-password-synchronization.md)


### <a name="federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm"></a>Federacja przy użyciu usług AD FS nowym lub istniejącym w systemie Windows Server 2012 R2 farma
Z federacyjnych znak na usługi użytkownicy mogli logować usługi Azure AD podstawie haseł lokalnego i znajduje się w sieci firmowej, bez konieczności wprowadzania hasła ponownie.  Opcja federacji z usług AD FS umożliwia wdrażanie nowego lub określić istniejących usług AD FS w systemie Windows Server 2012 R2 farma.  Jeśli wybierzesz umożliwiają określenie istniejącej farmy, tak aby użytkownicy mogli logować się narzędzie Azure AD Connect skonfiguruje zaufania między farmy i Azure AD.

<center>![Chmury](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="to-deploy-federation-with-ad-fs-in-windows-server-2012-r2-you-will-need-the-following"></a>Aby wdrożyć federacji z usług AD FS w systemie Windows Server 2012 R2, będą potrzebne następujące elementy

W przypadku wdrażania nowej farmy serwerów:

- Serwer Windows Server 2012 R2 na serwerze federacyjnym.
- Serwer Windows Server 2012 R2 dla serwera Proxy aplikacji sieci Web.
- plik PFX, z jednego certyfikatu SSL do zamierzonego Federacji nazwa usługi, takie jak fs.contoso.com.

Jeśli jesteś Wdrażanie nowej farmy serwerów lub przy użyciu istniejącej farmy:

- Poświadczenia administratora lokalnego na serwerach federacyjnych.
- Poświadczenia administratora lokalnego na dowolnej grupy roboczej (bez domeny sprzężone) serwerów, na których zamierzasz wdrożyć rola serwera Proxy aplikacji sieci Web.
- Komputer, na którym wykonania Kreatora muszą mieć możliwość nawiązywanie połączenia z innych komputerów, na których chcesz zainstalować usług AD FS lub serwera Proxy aplikacji sieci Web za pomocą zdalnego zarządzania systemem Windows.

[Konfigurowanie usługi rejestracji Jednokrotnej z usług AD FS](./aad-connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) 

#### <a name="sign-on-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>Zaloguj się przy użyciu starszej wersji programu AD FS lub innych firm rozwiązanie

Jeśli została już skonfigurowana chmury logowania przy użyciu starszej wersji programu usług AD FS (na przykład usług AD FS 2.0) lub Federacji inną dostawcę, możesz pominąć logowania użytkownika w konfiguracji przez narzędzie Azure AD Connect.  Pozwoli uzyskać ostatniej synchronizacji i inne możliwości Azure AD Connect jednocześnie nadal korzystając z istniejącego rozwiązania do zalogowania na.

[Listy zgodności Federacji innych firm w usłudze Azure AD](./active-directory-aadconnect-federation-compatibility.md)

## <a name="user-sign-in-and-user-principal-name-upn"></a>Logowania użytkownika i głównej nazwy użytkownika (UPN)

### <a name="understanding-user-principal-name"></a>Opis głównej nazwy użytkownika

W usłudze Active Directory sufiksu głównej nazwy użytkownika domyślnego jest nazwą DNS domeny, w której utworzono konto użytkownika. W większości przypadków jest to nazwa domeny zarejestrowana jako domena przedsiębiorstwa w Internecie. Jednak możesz dodać więcej sufiksy przy użyciu domeny usługi Active Directory i zaufania.

UPN użytkownika jest formatu username@domai. Na przykład dla usługi active directory contoso.com użytkownika Jana może być UPN john@contoso.com. UPN użytkownika jest oparty na RFC 822. Mimo że głównej nazwy użytkownika i poczty e-mail udostępnić takim samym formatem, wartość UPN dla użytkownika może lub mogą być różne na adres e-mail użytkownika.

### <a name="user-principal-name-in-azure-ad"></a>Główna nazwa użytkownika w Azure AD

Azure AD Connect kreatora będzie używać atrybutu userPrincipalName lub umożliwiają określenie atrybutu (w instalacji niestandardowej) do użycia w lokalnej jako głównej nazwy użytkownika w Azure AD. Jest to wartość, która będzie używana do logowania się w usłudze Azure AD. Jeśli wartość atrybutu głównej nazwy użytkownika nie odpowiada zweryfikowaną domeną w Azure AD, następnie Azure AD zostanie on zastąpiony z domyślną. onmicrosoft.com wartość.

Wszystkich katalogów w usłudze Azure Active Directory zawiera nazwę domeny wbudowanych w contoso.onmicrosoft.com formularza, który pozwala na rozpoczęcie pracy z Azure lub innych usług firmy Microsoft. Można poprawić i uprościć logowanie z domen niestandardowych. Aby uzyskać informacje o domenie niestandardowej nazw w Azure AD oraz weryfikacji domeny, przeczytaj [Dodaj nazwę domeny niestandardowej usługi Azure Active Directory](active-directory-add-domain.md#add-your-custom-domain-name-to-azure-active-directory)

## <a name="azure-ad-sign-in-configuration"></a>Azure AD logowania konfiguracji

### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure AD logowania konfiguracji, z Azure AD Connect
Azure AD logowanie jest uzależniony od tego, czy jest możliwe zgodnie z sufiksu głównej nazwy użytkownika użytkownika synchronizowanego do jednej z domen niestandardowych zweryfikowana w katalogu Azure AD Azure AD. Narzędzie Azure AD Connect zapewnia pomocy podczas konfigurowania Azure AD ustawienia logowania, użytkownik Logowanie w chmurze jest podobne do środowiska lokalnego. Narzędzie Azure AD Connect listy sufiksy zdefiniowanych dla domeny i próbuje dopasować je przy użyciu domeny niestandardowej w Azure AD i ułatwia odpowiednią akcję, którą należy uwzględnić.
Strona logowania w Azure AD wyświetla się sufiksy zdefiniowane w usłudze Active Directory w lokalnej i odpowiadające im stan przed każdego sufiksu. Wartości stanu może mieć jedną z poniżej:

| Województwo | Opis | Czynności |
|:----|:----|:----|
|Weryfikacja| Narzędzie Azure AD Connect znaleziono pasującego zweryfikować domenę w Azure AD. Wszyscy użytkownicy dla tej domeny mogli logować się przy użyciu poświadczeń lokalnych| Nie jest potrzebna. |
|Nie został zweryfikowany| Narzędzie Azure AD Connect można znaleźć pasujące domeny niestandardowej w Azure AD, ale nie została zweryfikowana. Zostaną zmienione sufiksu głównej nazwy użytkownika użytkowników tej domeny domyślnej. onmicrosoft.com sufiks po synchronizacji, jeśli domena nie została zweryfikowana. | Weryfikowanie domeny niestandardowej w Azure AD. [Dowiedz się więcej](./active-directory-add-domain.md#verify-the-domain-name-with-azure-ad) |  
|Nie dodane| Narzędzie Azure AD Connect nie można odnaleźć domeny niestandardowej odpowiadające sufiksu głównej nazwy użytkownika. Zostaną zmienione sufiksu głównej nazwy użytkownika użytkowników z tej domeny domyślnej. onmicrosoft.com, jeśli domena nie zostanie dodany i verifeid platformy Azure.| Dodać i zweryfikować domenę niestandardową odpowiadające [Dowiedz się więcej](./active-directory-add-domain.md) sufiksu głównej nazwy użytkownika|

Azure AD strona logowania do listy suffix(s) UPN zdefiniowane dla lokalnej usługi Active Directory i odpowiadające im domeny niestandardowej w Azure AD o bieżącym stanie weryfikacji. W instalacji niestandardowej można wybierać atrybut głównej nazwy użytkownika na stronie **logowania Azure AD** .

![Strona logowania w AD Azure](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

Klikając przycisk Odśwież, aby ponownie pobrać najnowszą stanu domen niestandardowych z Azure AD.

###<a name="selecting-attribute-for-user-principal-name-in-azure-ad"></a>Wybieranie atrybutu dla głównej nazwy użytkownika w Azure AD

UserPrincipalName - atrybut userPrincipalName jest atrybut użytkowników ma być używany podczas ich Zaloguj się do Azure AD i usługi Office 365. Domeny używane, nazywane także sufiksu głównej nazwy użytkownika —, należy zweryfikować w Azure AD przed użytkowników są synchronizowane. Zdecydowanie zaleca się zachować userPrincipalName atrybut domyślne. Jeśli ten atrybut jest bez obsługi routingu i nie można sprawdzić, a następnie jest możliwe wybrać inny atrybut, na przykład wiadomości e-mail jako atrybut, przytrzymując identyfikator logowania. Jest to określane jako identyfikator alternatywny. Wartość atrybutu alternatywny identyfikator muszą być zgodne ze standardem RFC822. Alternatywny identyfikator można korzystać w polach hasło pojedynczego logowania jednokrotnego (SSO), jak i Federacji logowania jednokrotnego logowania rozwiązanie problemu.

>[AZURE.NOTE] Za pomocą alternatywny identyfikator nie jest zgodny z wszystkich obciążeń pracą usługi Office 365. Aby uzyskać więcej informacji zapoznaj się [Konfigurowanie alternatywny identyfikator logowania](https://technet.microsoft.com/library/dn659436.aspx).

#### <a name="different-custom-domain-states-and-effect-on-azure-sign-in-experience"></a>Zjednoczone innej domeny niestandardowej i wpływu na Azure logowanie
Ważne zrozumieć relacji między nimi domeny niestandardowej w katalogu Azure AD jest a sufiksy określone lokalnego. Pozwól nam przejść przez różne możliwe Azure logowania doświadczeń, podczas konfigurowania sycnhronization przy użyciu Azure AD Konstruktor.

Dla poniższe informacje Załóżmy, że firma Microsoft dotyczy contoso.com sufiksu głównej nazwy użytkownika, który jest używany w katalogu lokalnego jako część głównej nazwy użytkownika, na przykład user@contoso.com.

###### <a name="express-settings--password-synchronization"></a>Ustawienia ekspresowe i synchronizacji haseł
| Województwo         | Wpływ na środowisko użytkownika Azure logowania |
|:-------------:|:----------------------------------------|
| Nie dodane | W takim przypadku nie niestandardowej domeny dla contoso.com został dodany w katalogu Azure AD. Użytkownicy, którzy mają głównej nazwy użytkownika lokalnego z sufiksem @contoso.com, nie będzie można używać ich w lokalnej głównej nazwy użytkownika do logowania się do Azure. Zamiast tego należy je za pomocą nowego UPN udostępniony mu przez Azure AD, dodając sufiks Azure AD katalogu. Na przykład, jeśli są synchronizowane użytkownikom Azure AD katalogu azurecontoso.onmicrosoft.com następnie użytkownika lokalnego user@contoso.com otrzyma nazwę UPNuser@azurecontoso.onmicrosoft.com|
| Nie został zweryfikowany | W tym przypadku mamy contoso.com domeny niestandardowej, dodane w katalogu Azure AD, ale nie została jeszcze zweryfikowana. Jeśli możesz wtyczce Synchronizacja użytkowników bez weryfikacji domeny, następnie użytkowników są przypisywane nowe UPN przez Azure AD tylko tak, jak w scenariuszu nie dodane.|
| Weryfikacja | W tym przypadku mamy contoso.com domeny niestandardowej, już dodana, a zweryfikowane w Azure AD dla sufiksu głównej nazwy użytkownika. Użytkownicy będą mogli używać ich w lokalnej głównej nazwy użytkownika, np. user@contoso.com, zalogowania się Azure po są synchronizowane Azure AD|

###### <a name="ad-fs-federation"></a>AD FS Federacji
Nie można utworzyć federacji z domyślnymi. domenie onmicrosoft.com w Azure AD lub niezweryfikowany domeny niestandardowej w Azure AD. Po uruchomieniu kreatora Azure AD Connect w przypadku wybrania niezweryfikowany domen, aby utworzyć federacji z następnie Azure AD Connect wyświetli monit z rekordami niezbędne do utworzenia miejsce, w którym znajduje się systemu DNS dla domeny. Aby uzyskać więcej informacji, zobacz [poniżej](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

W przypadku wybrania użytkownika logowania opcję "Federacji z usług AD FS" musi mieć domenę niestandardową, aby kontynuować tworzenie federacji w Azure AD. Dla naszych dyskusji oznacza to, że firma Microsoft powinny mieć contoso.com domeny niestandardowej, dodane w katalogu Azure AD.

| Województwo         | Wpływ na środowisko użytkownika Azure logowania |
|:-------------:|:----------------------------------------|
| Nie dodane | W tym przypadku narzędzie Azure AD Connect nie można odnaleźć pasujące domeny niestandardowej dla contoso.com sufiksu głównej nazwy użytkownika w katalogu Azure AD. Musisz dodać contoso.com domeny niestandardowej w razie potrzeby użytkowników o zalogowanie się przy użyciu usług AD FS z ich lokalnej głównej, takich jak user@contoso.com.|
| Nie został zweryfikowany | W tym przypadku narzędzie Azure AD Connect zostanie wyświetlony monit z odpowiednich szczegółowych informacji na temat sposobu weryfikowanie domeny w późniejszym terminie|
| Weryfikacja | W takim przypadku możesz przejść dalej z konfiguracją bez dalszych działań|  

## <a name="changing-user-sign-in-method"></a>Zmienianie metoda logowania użytkownika

Możesz zmienić metodę logowania użytkownika z Federacji do synchronizacji haseł z avaialble zadań w narzędzie Azure AD Connect po początkowej konfiguracji narzędzie Azure AD Connect przy użyciu kreatora. Ponownie uruchom Kreatora Azure AD Connect i zostanie wyświetlona lista zadań, które można wykonać. Wybierz pozycję **Zmień użytkownika logowania** z listy zadań. 

![Zmienianie logowania użytkownika"](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Na następnej stronie zostanie wyświetlony monit o podanie poświadczeń dla Azure AD.

![Nawiązywanie połączenia z Azure AD](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

Na stronie **logowania użytkownika** wybierz **Synchronizacji haseł**. Spowoduje to zmianę katalogu z Federacji do jednego zarządzane.

![Nawiązywanie połączenia z Azure AD](./media/active-directory-aadconnect-user-signin/changeusersignin3.png)

>[AZURE.NOTE] Jeśli są tylko tymczasowe przełącznika synchronizacji haseł, sprawdź, **nie należy konwertować kont użytkowników**. Nie sprawdza opcję powoduje przejście do konwersji każdego użytkownika do Federacji i może potrwać kilka godzin.
  
## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).

Dowiedz się więcej o [Narzędzie Azure AD Connect: projektu](active-directory-aadconnect-design-concepts.md)


