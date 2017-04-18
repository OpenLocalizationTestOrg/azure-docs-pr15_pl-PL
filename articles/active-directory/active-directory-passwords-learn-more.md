<properties
    pageTitle="Dowiedz się więcej: Zarządzanie Azure AD hasło | Microsoft Azure"
    description="Zagadnienia zaawansowane na zarządzanie Azure AD hasła, w tym hasła zapisu działania zabezpieczeń zapisu hasło, jak działa portalu do resetowania hasła i dane, które są używane przez resetowania hasła."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="learn-more-about-password-management"></a>Dowiedz się więcej na temat zarządzania hasłami

> [AZURE.IMPORTANT] **Czy jesteś tutaj, ponieważ występują problemy z logowaniem?** Jeśli tak, [Oto jak można zmienić i zresetować swoje hasło](active-directory-passwords-update-your-own-password.md).

Jeśli zostały już rozmieszczone Zarządzanie hasłami lub są tylko chcesz dowiedzieć się więcej o techniczne nitty niepowtarzalne temat działania przed wdrożeniem, w tej sekcji pozwala przystępny Przegląd techniczne pojęcia dotyczące usługi. Omówiono następujące czynności:

* [**Omówienie zapisu hasła**](#password-writeback-overview)
  - [Jak działa zapisu hasło](#how-password-writeback-works)
  - [Scenariusze obsługiwane w przypadku zapisu hasła](#scenarios-supported-for-password-writeback)
  - [Model zabezpieczeń zapisu hasła](#password-writeback-security-model)
* [**Jak do resetowania hasła portalu pracy?**](#how-does-the-password-reset-portal-work)
  - [Jakie dane są używane przez resetowania hasła?](#what-data-is-used-by-password-reset)
  - [Jak uzyskać dostęp do hasła Resetuj dane dla użytkowników](#how-to-access-password-reset-data-for-your-users)

## <a name="password-writeback-overview"></a>Omówienie zapisu hasła
Hasło zapisu jest [Azure Active Directory połączyć](active-directory-aadconnect.md) składnik, który może być włączony i używane przez bieżącego subskrybentów Azure Active Directory Premium. Aby uzyskać więcej informacji zobacz [Azure Active Directory wersje](active-directory-editions.md).

Hasło zapisu umożliwia konfigurowanie dzierżawy usługi cloud zapisać hasła, aby odczytywał lokalnej usługi Active Directory.  Eliminuje możesz od konieczności Konfigurowanie i zarządzanie nimi rozwiązanie Resetowanie hasła samoobsługowego skomplikowane lokalnego i zapewnia wygodny sposób opartej na chmurze dla użytkowników, aby zresetować swoje hasło lokalnego miejsce, w którym są one.  Poniżej niektóre z najważniejszych funkcji programu hasło zapisu:

- **Zero opinii opóźnienie.**  Hasło zapisu jest operacji synchroniczne.  Użytkowników będzie powiadamiany natychmiast, gdy hasło nie spełnia zasad lub nie możesz zresetować ani zmienić dla dowolnego powodu.
- **Obsługa resetowania hasła dla użytkowników za pomocą usług AD FS lub inne technologie federacji.**  Z wystornowaniem hasło, jak kont użytkowników federacyjnych są synchronizowane do dzierżawy usługi Azure AD będą one możliwość zarządzania ich w lokalnym AD haseł z chmury.
- **Obsługa resetowanie haseł dla użytkowników przy użyciu hasła mieszania synchronizacji.** Gdy usługi resetowania hasła wykryje, że konto użytkownika zsynchronizowaną jest włączone dla synchronizacji haseł skrótu, resetowania zarówno w tego konta lokalnego, jak i w chmurze hasło jednocześnie.
- **Obsługa zmienianie haseł z panel dostępu i usługi Office 365.**  Gdy federacyjnych lub synchronizacji haseł użytkowników pochodzą zmieniać swoje hasła wygasłą lub wygasła, firma Microsoft będzie zapisywać hasła w środowisku lokalnym AD.
- **Obsługuje zapis ponownie hasła, gdy administrator Resetowanie ich z** [**Portal azure zarządzania**](https://manage.windowsazure.com).  Gdy resetuje administratora, który chcesz hasła użytkownika w [Portalu zarządzania Azure](https://manage.windowsazure.com), jeśli użytkownik ma być federacyjne lub synchronizacji haseł, firma Microsoft będzie ustawić hasło, które administrator wybiera na lokalnym ogłoszenie, także.  To nie jest obecnie obsługiwane w portalu administracyjnym pakietu Office.
- **Wymusza swojej lokalnej zasady haseł AD.**  Podczas swojego hasła użytkownika firma Microsoft upewnij się, że spełnia swoje lokalnie zasad AD przed zatwierdzeniem go do tego katalogu.  Ta opcja uwzględnia historii, złożoność, wiek, filtry haseł i innych ograniczeń hasła, zdefiniowanych w swojej lokalnej AD.
- **Nie wymaga reguł zapory dla ruchu przychodzącego.**  Hasło zapisu używa przekazywania Bus usługi Azure jako podstawowego kanału komunikacji, co oznacza, że nie trzeba otworzyć dowolny ruchu przychodzącego portów zapory dla tej funkcji do pracy.
- **Nie jest obsługiwane w przypadku kont użytkowników, które występują w chronionym grup w usłudze Active Directory w lokalnej.** Aby uzyskać więcej informacji o grupach chronionego zobacz [chroniony kont i grup w usłudze Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

### <a name="how-password-writeback-works"></a>Jak działa zapisu hasła
Hasło zapisu występują trzy główne składniki:

- Usługa w chmurze resetowania hasła (to jest w pełni zintegrowany stron Zmień hasło Azure AD)
- Specyficzne dla dzierżawy Bus usługi Azure przekazywania
- Punkt końcowy do resetowania hasła lokalna

Mieściły się ze sobą, zgodnie z opisem w poniżej diagramu:

  ![][001]

Synchronizowania mieszania hasła lub federacyjnych czy użytkownika przychodzące Resetowanie lub zmienianie własnego hasła w chmurze, są następujące:

1.  Firma Microsoft Sprawdź, jakiego typu haseł użytkownik ma.  Jeśli widać, że hasło odbywa się w środowisku lokalnym, następnie firma Microsoft upewnij się, że usługa zapisu jest i rozpocząć pracę.  Jeśli jest to, możemy zabezpieczeń umożliwia użytkownikowi kontynuować, jeśli nie jest możemy przekazać użytkownikowi, którego nie można tutaj zresetować swoje hasło.
2.  Następnie użytkownik przekazuje bramy uwierzytelniania odpowiednią i osiągnie ekranie resetowania hasła.
3.  Użytkownik wybiera nowe hasło i potwierdza go.
4.  Po kliknięciu Prześlij, możemy Szyfruj przy użyciu kluczy został utworzony podczas procesu konfigurowania zapisu hasła zwykłego tekstu.
5.  Po szyfrowania hasła, możemy umieścić ładunek, który przesłane przez kanał HTTPS do Twojej dzierżawy określonej usługi bus relay (możemy również skonfigurować dla Ciebie podczas procesu konfigurowania zapisu).  Ta funkcja przekazywania jest chroniony hasłem losowo wygenerowanym schematów instalacji lokalnego.
6.  Gdy wiadomość osiągnie bus usługi, punkt końcowy resetowania hasła automatycznie wznowieniu i widzi, że ma żądanie resetowania oczekujących na.
7.  Usługa następnie wyszukuje użytkownika w danym za pomocą atrybutu kotwicy chmury.  Dla tego odnośnika pomyślnie obiekcie użytkownika, musi istnieć w obszarze łącznik AD, muszą być połączone z odpowiedni obiekt MV i muszą być połączone z odpowiedni obiekt łącznika AAD. Na końcu, aby synchronizacja, aby znaleźć to konto użytkownika, łącza obiekt łącznika AD MV musi mieć reguły Synchronizuj `Microsoft.InfromADUserAccountEnabled.xxx` łącze.  To jest potrzebna, ponieważ połączenie przychodzące z chmury, aparatu synchronizacji używa atrybutu cloudAnchor do wyszukania obiekt AAD łącznika miejsca, a następnie się łącze obiektu MV, a następnie się łącze obiektu AD. Ponieważ może istnieć kilka obiektów AD (wielu las) dla tego użytkownika, na podstawie aparat synchronizacji `Microsoft.InfromADUserAccountEnabled.xxx` łącze, wybierz właściwą.
8.  Gdy konto użytkownika zostanie znaleziony, możemy próbę Resetowanie hasła bezpośrednio w odpowiedni las AD.
9.  W przypadku pomyślnego operacji Ustawianie hasła, możemy przekazać użytkownikowi swoje hasło zostały zmodyfikowane, i są przydatne w drodze Wesołych.
10. Jeśli hasło ustawione operacja kończy się niepowodzeniem, możemy zwrócił błąd użytkownikowi i umożliwiać spróbuj ponownie.  Operacja może się niepowodzeniem, ponieważ usługa nie działa, ponieważ hasła, że wybrane nie spełnia zasady organizacji, ponieważ nie można odnaleźć użytkownika w lokalnym AD, lub wielu sytuacjach.  Firma Microsoft może mieć określonej wiadomości wiele z tych przypadków, a następnie przekazać użytkownikowi, jakie możliwości Aby rozwiązać ten problem.

### <a name="scenarios-supported-for-password-writeback"></a>Scenariusze obsługiwane w przypadku zapisu hasła
W poniższej tabeli opisano, jakie scenariusze są obsługiwane przez które wersje naszych możliwości synchronizacji.  Zaleca się na ogół zainstalować najnowszą wersję programu [Narzędzie Azure AD Connect](active-directory-aadconnect.md#install-azure-ad-connect) , jeśli chcesz używać zapisu hasła.

  ![][002]

### <a name="password-writeback-security-model"></a>Model zabezpieczeń zapisu hasła
Hasło zapisu to usługa wysoce bezpieczeństwa i niezawodności.  Aby upewnić się, że Twoje informacje są chronione, możemy włączyć modelu 4 tiered zabezpieczeń, który został opisany poniżej.

- **Dzierżawy określonych usług bus relay** — podczas konfigurowania usługi, możemy skonfigurować przekazywania bus specyficzne dla dzierżawy usługi, który jest chroniony losowo wygenerowanym silne hasło, które Microsoft nigdy nie ma dostęp do.
- **Zablokowane dół klucza szyfrowania kryptograficznie silnych haseł** — po utworzeniu przekazywania bus usługi, tworzymy silnego klucza symetrycznej, której użyjemy do szyfrowania hasła nadejdzie przez sieć.  Ten klucz znajduje się tylko w sklepie poufne firmy w chmurze, której jest intensywnie zablokowane i inspekcji, tak jak każde hasło w katalogu.
- **TLS standardowe branżowe** — Resetowanie hasła, lub zmiana operacji występuje w chmurze, możemy zrobić hasła zwykłego tekstu i szyfrowania kluczem publicznym.  Firma Microsoft następnie plop który do wiadomości HTTPS, który są wysyłane przez zaszyfrowanego kanału za pomocą certyfikatów SSL firmy Microsoft z przekazywaniem bus swojej usługi.  Po odebraniu wiadomości do usługi Bus usługi agenta lokalna wznowieniu, uwierzytelnia się przy użyciu silne hasło, które zostały wcześniej został wygenerowany Bus usługi, przejmuje zaszyfrowaną wiadomość, odszyfrowuje je Ustawianie hasła za pośrednictwem interfejsu API programu AD DS UstawianieHasła przy użyciu klucz prywatny, możemy wygenerowania, a następnie prób.  Ten krok jest, co pozwala na wymuszanie zasad hasła lokalna AD (złożoność, wiek, historii, filtry itp.) w chmurze.
- **Zasady wygasania wiadomości** — na końcu, jeśli z jakiegoś powodu wiadomości znajduje w Bus usługi ponieważ usługi lokalna nie działa, zostanie ono Przekroczono limit czasu i usuwane po upływie kilku minut w celu zwiększenia zabezpieczeń jeszcze bardziej.

## <a name="how-does-the-password-reset-portal-work"></a>Jak do resetowania hasła portalu pracy?
Gdy użytkownik przechodzi do portalu do resetowania hasła, przepływ pracy jest kopać w celu ustalenia, czy danego konta użytkownika jest prawidłowa, jakie organizacji, która użytkowników, do której należy tam, gdzie jest zarządzany hasła użytkownika oraz czy użytkownik ma licencję na korzystanie z funkcji.  Przeczytaj poniższe czynności, aby poznać logiki strony resetowania hasła.

1.  Użytkownik kliknie nie może uzyskać dostępu do konta łącze lub przejść bezpośrednio do [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2.  Użytkownik wprowadza identyfikator użytkownika i przekazuje testy captcha.
3.  Azure AD weryfikuje, jeśli użytkownik będzie mógł korzystać z tej funkcji, wykonując następujące czynności:
    - Sprawdza, czy użytkownik ma ta funkcja jest włączona i przypisana licencja Azure AD.
        - Jeśli użytkownik nie posiada ta funkcja jest włączona lub przypisana licencja, użytkownik zostanie poproszony o skontaktuj się z administratorem tej osoby, aby zresetować swoje hasło.
    - Sprawdza, czy użytkownik ma po prawej stronie wezwania danych zdefiniowane dla swojego konta zgodnie z zasadami administratora.
        - Jeśli zasady wymaga tylko jeden wezwanie, następnie zapewnia się, że użytkownik ma odpowiednie dane zdefiniowane dla co najmniej jednej problemach, korzystając z zasad administratora.
          - Jeśli użytkownik nie jest skonfigurowana, użytkownik jest zalecane jest skontaktuj się z administratorem tej osoby, aby zresetować swoje hasło.
        - Jeśli zasady wymaga dwóch problemach, następnie zapewnia się, że użytkownik ma odpowiednie dane zdefiniowanych dla co najmniej dwóch problemach, korzystając z zasad administratora.
          - Jeśli użytkownik nie jest skonfigurowany, a następnie możemy użytkownik jest zalecane jest skontaktuj się z administratorem tej osoby, aby zresetować swoje hasło.
    - Sprawdza, czy hasło użytkownika odbywa się w środowisku lokalnym (federacyjnych lub sprzedał synchronizacji haseł mieszania).
       - Jeśli hasło użytkownika odbywa się w środowisku lokalnym wdrożeniu zapisu, aby przejść do uwierzytelnienia i zresetować swoje hasło jest dozwolona użytkownika.
       - Jeśli zapisu nie jest wdrożona i hasło użytkownika odbywa się w środowisku lokalnym, użytkownik zostanie poproszony o skontaktuj się z administratorem tej osoby, aby zresetować swoje hasło.
4.  Jeśli okaże się, czy użytkownik ma możliwość pomyślnie zresetować swoje hasło, a następnie kieruje użytkownika przez proces resetowania.

Dowiedz się więcej o wdrażanie zapisu hasła w [Wprowadzenie: Zarządzanie hasłami AD Azure](active-directory-passwords-getting-started.md).

### <a name="what-data-is-used-by-password-reset"></a>Jakie dane są używane przez resetowania hasła?
Tabela wytyczne miejscem i sposobem danych jest używana podczas resetowania hasła i jest przeznaczona do pomagające zdecydować, które opcje uwierzytelniania są odpowiednie dla Twojej organizacji. W tej tabeli zawiera również wymagań formatowania na wypadek miejsce, w którym dane mają być udostępniane imieniu użytkownikom wprowadzania ścieżki, które nie sprawdzania poprawności danych.

> [AZURE.NOTE] Telefon w biurze nie jest wyświetlana w portalu rejestracji, ponieważ obecnie użytkownicy nie będą mogli edytować tej właściwości w katalogu.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Nazwa metody kontaktu</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Element danych usługi Azure Active Directory</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Używane / można ustawić miejsce, w którym?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Wymagania dotyczące formatu</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Telefon w biurze</p>
            </td>
            <td>
              <p>Numer telefonu</p>
              <p>przykład Set-MsolUser - UserPrincipalName JWarner@contoso.com numer telefonu-"1234567890 + 1 x 1234"</p>
            </td>
            <td>
              <p>Używane w:</p>
              <p>Portal resetowania hasła</p>
              <p>Wartość z:</p>
              <p>Numer telefonu jest można ustawić z programu PowerShell, DirSync Portal Azure zarządzania i portalu administracyjnego usługi Office</p>
            </td>
            <td>
              <p>+ xxxyyyzzzz ccc (np. 1234567890 + 1)</p>
              <ul>
                <li class="unordered">
Podaj kod kraju<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Podaj numer kierunkowy (jeśli dotyczy)<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Musisz podać kod + na wierzchu kraju<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Może zawierać spacji między kod kraju i pozostałe liczby<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Rozszerzenia nie są obsługiwane, jeśli masz wszystkie rozszerzenia określony, firma Microsoft usuwa go z wybranego numeru przed wysłaniem przez telefon.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Telefon komórkowy</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>LUB</p>
              <p>MobilePhone</p>
              <p>(Uwierzytelnianie telefonu jest używany w przypadku danych zawartych w przeciwnym razie to przechodzi do pola Telefon komórkowy).</p>
              <p>przykład Set-MsolUser - UserPrincipalName JWarner@contoso.com - MobilePhone "1234567890 + 1 x 1234"</p>
            </td>
            <td>
              <p>Używane w:</p>
              <p>Portal resetowania hasła</p>
              <p>Portal rejestracji</p>
              <p>Wartość z: </p>
              <p>AuthenticationPhone jest można ustawić z poziomu portalu rejestracji resetowania hasła lub portal rejestracji MFA.</p>
              <p>Można ustawić z programu PowerShell, DirSync Portal Azure zarządzania i portalu administracyjnego usługi Office jest MobilePhone</p>
            </td>
            <td>
              <p>+ xxxyyyzzzz ccc (np. 1234567890 + 1)</p>
              <ul>
                <li class="unordered">
Podaj kod kraju.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Podaj numer kierunkowy, (jeśli dotyczy).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Masz Podaj kod + na wierzchu kraju.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Może zawierać spacji między kod kraju i pozostałe liczby.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Rozszerzenia nie są obsługiwane, jeśli masz wszystkie rozszerzenia określony, możemy należy ją zignorować podczas wysyłania przez telefon.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Alternatywny adres E-mail</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>LUB</p>
              <p>AlternateEmailAddresses [0] </p>
              <p>(Uwierzytelniania wiadomości E-mail jest używana w przypadku dane, w przeciwnym razie to przechodzi do pola alternatywny adres E-mail).</p>
              <p>Uwaga: pole alternatywny adres e-mail zostało określone jako tablica ciągów w katalogu.  Firma Microsoft korzysta z pierwszego wpisu w tej tablicy.</p>
              <p>przykład Set-MsolUser - UserPrincipalName JWarner@contoso.com - AlternateEmailAddresses"email@live.com"</p>
            </td>
            <td>
              <p>Używane w:</p>
              <p>Portal resetowania hasła</p>
              <p>Portal rejestracji</p>
              <p>Wartość z: </p>
              <p>AuthenticationEmail jest można ustawić z poziomu portalu rejestracji resetowania hasła lub portal rejestracji MFA.</p>
              <p>Można ustawić z programu PowerShell, portalu zarządzania Azure i portalu administracyjnego usługi Office jest AlternateEmail</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">user@domain.com</a>lub甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
Wiadomości e-mail, należy wykonać standardowe formatowanie jako na.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Wiadomości e-mail Unicode są obsługiwane.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Zabezpieczenia pytań i odpowiedzi</p>
            </td>
            <td>
              <p>Nie można zmodyfikować bezpośrednio w katalogu.</p>
            </td>
            <td>
              <p>Używane w:</p>
              <p>Portal resetowania hasła</p>
              <p>Portal rejestracji </p>
              <p>Wartość z: </p>
              <p>Jedynym sposobem ustawienia pytania dotyczące zabezpieczeń jest za pośrednictwem portalu zarządzania Azure.</p>
              <p>Jedynym sposobem ustawienia odpowiedzi na pytania dotyczące zabezpieczeń dla danego użytkownika jest za pośrednictwem portalu rejestracji.</p>
            </td>
            <td>
              <p>Maksymalna liczba znaków 200 i min 3 znaków masz pytania dotyczące zabezpieczeń</p>
              <p>Odpowiedzi mają max 40 znaków i min 3 znaków</p>
            </td>
          </tr>
        </tbody></table>

###<a name="how-to-access-password-reset-data-for-your-users"></a>Jak uzyskać dostęp do hasła Resetuj dane dla użytkowników
####<a name="data-settable-via-synchronization"></a>Dane można ustawić za pośrednictwem synchronizacji
Następujące pola mogą być synchronizowane ze źródeł lokalnych:

* Telefon komórkowy
* Telefon w biurze

####<a name="data-settable-with-azure-ad-powershell"></a>Dane można ustawić przy użyciu programu AD PowerShell Azure
Dostępna w ramach Azure AD programu PowerShell i interfejsu API wykresu są następujące pola:

* Alternatywny adres E-mail
* Telefon komórkowy
* Telefon w biurze
* Telefon uwierzytelniania
* Uwierzytelnianie wiadomości E-mail

####<a name="data-settable-with-registration-ui-only"></a>Dane można ustawić tylko rejestracji interfejsu użytkownika
Następujące pola są dostępne jedynie za pośrednictwem rejestracji SSPR interfejsu użytkownika (https://aka.ms/ssprsetup):

* Zabezpieczenia pytań i odpowiedzi

####<a name="what-happens-when-a-user-registers"></a>Co się dzieje, gdy użytkownik rejestruje?
Rejestruje nazwę użytkownika, strona rejestracji będzie **zawsze** ustaw następujące pola:

* Telefon uwierzytelniania
* Uwierzytelnianie wiadomości E-mail
* Zabezpieczenia pytań i odpowiedzi

Jeśli podano wartość dla **telefonu komórkowego** lub **Alternatywny adres E-mail**użytkownicy mogą od razu używać tych resetowanie haseł, nawet jeśli nie zostały one zarejestrowane w usłudze.  Ponadto użytkownicy podczas rejestrowania po raz pierwszy, zobacz te wartości i modyfikowanie ich życzenie.  Jednak po ich pomyślnie zarejestrować, te wartości będzie być zachowywane w polach **Uwierzytelniania telefonu** i **Adres E-mail uwierzytelniania** odpowiednio.

Zagnieżdżanie może być przydatne do odblokowania duża liczba użytkowników, aby użyć SSPR, umożliwiając użytkownikom sprawdzić poprawność tych informacji przez proces rejestracji.

####<a name="setting-password-reset-data-with-powershell"></a>Ustawianie hasła Resetowanie danych za pomocą programu PowerShell
Można ustawić wartości w następujących polach z Azure AD programu PowerShell.

* Alternatywny adres E-mail
* Telefon komórkowy
* Telefon w biurze

Aby rozpocząć pracę, musisz najpierw [pobrać](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)i zainstalować moduł Azure AD programu PowerShell.  Gdy jest zainstalowany, można wykonać poniższe czynności, aby skonfigurować każdego pola.

#####<a name="alternate-email"></a>Alternatywny adres E-mail
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

#####<a name="mobile-phone"></a>Telefon komórkowy
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

#####<a name="office-phone"></a>Telefon w biurze
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

####<a name="reading-password-reset-data-with-powershell"></a>Do resetowania hasła odczytu danych za pomocą programu PowerShell
Można przeczytać wartości w następujących polach z Azure AD programu PowerShell.

* Alternatywny adres E-mail
* Telefon komórkowy
* Telefon w biurze
* Telefon uwierzytelniania
* Uwierzytelnianie wiadomości E-mail

Aby rozpocząć pracę, musisz najpierw [pobrać](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)i zainstalować moduł Azure AD programu PowerShell.  Gdy jest zainstalowany, można wykonać poniższe czynności, aby skonfigurować każdego pola.

#####<a name="alternate-email"></a>Alternatywny adres E-mail
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

#####<a name="mobile-phone"></a>Telefon komórkowy
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

#####<a name="office-phone"></a>Telefon w biurze
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

#####<a name="authentication-phone"></a>Telefon uwierzytelniania
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

#####<a name="authentication-email"></a>Uwierzytelnianie wiadomości E-mail
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

## <a name="links-to-password-reset-documentation"></a>Łącza do hasła Resetowanie dokumentacji
Poniżej znajdują się łącza do wszystkich stron dokumentacji Azure AD hasło:

* **Czy jesteś tutaj, ponieważ występują problemy z logowaniem?** Jeśli tak, [Oto jak można zmienić i zresetować swoje hasło](active-directory-passwords-update-your-own-password.md).
* [**Działania**](active-directory-passwords-how-it-works.md) — Dowiedz się więcej o sześć różnych składników usługi i każdej czy
* [**Wprowadzenie**](active-directory-passwords-getting-started.md) — Dowiedz się, jak użytkownicy mogą zresetować i zmieniać swoje hasła w chmurze lub lokalnych
* [**Dostosowywanie**](active-directory-passwords-customize.md) — Dowiedz się, jak dostosować wygląd i sposób działania i zachowanie usługi do potrzeb danej organizacji
* [**Najważniejsze wskazówki**](active-directory-passwords-best-practices.md) — Dowiedz się, jak szybko wdrażanie i efektywne zarządzanie hasłami w organizacji
* [**Uzyskaj więcej informacji**](active-directory-passwords-get-insights.md) — Dowiedz się więcej o naszych zintegrowane funkcje raportowania
* [**Często zadawane pytania**](active-directory-passwords-faq.md) — odpowiedzi na często zadawane pytania
* [**Rozwiązywanie problemów**](active-directory-passwords-troubleshoot.md) — Dowiedz się, jak szybko Rozwiązywanie problemów z usługą



[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"
