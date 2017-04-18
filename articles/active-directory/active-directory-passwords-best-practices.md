<properties
    pageTitle="Najlepsze rozwiązania: Hasła Azure AD zarządzania | Microsoft Azure"
    description="Najlepsze praktyki dotyczące rozmieszczania i zastosowania, dokumentacja użytkownika próbki i przewodniki zarządzania hasłami w usłudze Azure Active Directory."
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

# <a name="deploying-password-management-and-training-users-to-use-it"></a>Wdrażanie zarządzania hasłami i szkolenia korzystać użytkownicy

> [AZURE.IMPORTANT] **Czy jesteś tutaj, ponieważ występują problemy z logowaniem?** Jeśli tak, [Oto jak można zmienić i zresetować swoje hasło](active-directory-passwords-update-your-own-password.md).

Po zresetowaniu hasła włączenie, następnym krokiem, który należy wykonać jest uzyskanie użytkowników w organizacji przy użyciu usługi. Aby to zrobić, musisz upewnij się użytkowników są skonfigurowane do korzystania z usługi prawidłowo, a także czy użytkownicy mają szkolenie muszą być pomyślnego Zarządzanie swoje własne hasła. W tym artykule opisano będzie dla Ciebie następujące pojęcia:

* [**Jak uzyskać użytkowników skonfigurowane dla zarządzania hasłami**](#how-to-get-users-configured-for-password-reset)
  * [Co sprawia, że konto skonfigurowane do resetowania hasła](#what-makes-an-account-configured)
  * [Sposoby samodzielnie wypełnianie danych uwierzytelniania](#ways-to-populate-authentication-data)
* [**Najlepsze sposoby wdrożeniem resetowania dla Twojej organizacji**](#what-is-the-best-way-to-roll-out-password-reset-for-users)
  * [Wdrożenia opartej na poczcie e-mail + przykładowy e-mail komunikacji](#email-based-rollout)
  * [Tworzenie własnych niestandardowych hasło portalu zarządzania dla użytkowników](#creating-your-own-password-portal)
  * [Jak za pomocą wymuszoną rejestracji wymusić użytkownikom rejestrowanie w znaku](#using-enforced-registration)
  * [Jak przekazywać dane uwierzytelniania dla kont użytkowników](#uploading-data-yourself)
* [**Przykładowe użytkownika i materiały szkoleniowe pomocy technicznej (już wkrótce!)**](#sample-training-materials)

## <a name="how-to-get-users-configured-for-password-reset"></a>Jak uzyskać użytkowników skonfigurowany do resetowania hasła
W tej sekcji opisano użytkownikowi różnych metod za pomocą których możesz zdecydować, że każdy użytkownik w organizacji, można użyć Samodzielne resetowanie skutecznie w przypadku, gdy ich zapomnisz hasło hasła.

### <a name="what-makes-an-account-configured"></a>Co sprawia, że skonfigurowano kont
Zanim użytkownika można używać w przypadku resetowania hasła, **Wszystkie** poniższe warunki muszą być spełnione:

1.  Resetowanie hasła musi być włączony w katalogu.  Dowiedz się, jak włączyć resetowania, czytając [umożliwić użytkownikom resetowanie haseł Azure AD](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords) lub [użytkownicy mogli zmieniać swoje hasła AD lub resetowanie](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) hasła
2.  Użytkownik musi mieć licencję.
 - Dla użytkowników w chmurze użytkownik musi mieć ** **płatnej licencji usługi Office 365**lub AAD podstawowe** lub przypisana **Licencja AAD Premium** .
 - Lokalna użytkowników (federacyjna lub synchronizowane mieszania), użytkownik **musi być przypisana licencja AAD Premium**.
3.  Użytkownik musi mieć **Minimalny zestaw danych uwierzytelniania definicja** zgodnie z zasadami bieżącego resetowania hasła.
 - Dane uwierzytelniania jest traktowany jako zdefiniowane Jeśli odpowiadającym mu polem w katalogu zawiera poprawnego dane.
 - Minimalny zestaw danych uwierzytelniania jest zdefiniowane na **co najmniej jedną** opcje uwierzytelniania włączone, jeśli skonfigurowano zasady jednej bramy, lub **co najmniej dwie** opcje uwierzytelniania włączonego, jeśli skonfigurowano zasady dwóch bramy.
4.  Jeśli użytkownik korzysta z konta lokalnego, następnie [Zapisu hasło](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) musi być włączony i włączone

### <a name="ways-to-populate-authentication-data"></a>Sposoby wypełnij dane uwierzytelniania
Istnieje kilka opcji dotyczące określania danych dla użytkowników w organizacji może być używany do resetowania hasła.

- Edytowanie użytkowników w [Portalu zarządzania Azure](https://manage.windowsazure.com) lub [Portalu administracyjnego usługi Office 365](https://portal.microsoftonline.com)
- Synchronizowanie właściwości użytkownika w usłudze Azure Active Directory z domeną usługi Active Directory w lokalnej za pomocą Azure AD Sync
- Aby edytować właściwości użytkownika, wykonując [czynności opisane w tym miejscu](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users)za pomocą programu Windows PowerShell.
- Umożliwia użytkownikom rejestrowanie własne dane prowadząc do portalu rejestracji adresem [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)
- Wymaganie od użytkowników zarejestrować do resetowania się zalogować do swojego konta Azure AD przez ustawienie hasła [**wymagać od użytkowników zarejestrować podczas logowania?**](active-directory-passwords-customize.md#require-users-to-register-when-signing-in) opcji konfiguracji na wartość **Tak**.

Użytkownicy nie muszą zarejestrować resetowania hasła dla systemu do pracy.  Na przykład jeśli masz istniejące telefon komórkowy lub numery telefonów pakietu office w katalogu lokalnym, możesz je synchronizować w Azure AD i firma Microsoft korzysta z nich do automatycznego resetowania hasła.

Można również znaleźć więcej informacji o [używania danych hasłem Resetowanie](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) i [jak można wypełnić uwierzytelniania poszczególnych pól przy użyciu programu PowerShell](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users).

## <a name="what-is-the-best-way-to-roll-out-password-reset-for-users"></a>Co to jest najlepszym sposobem wdrożeniem resetowania hasła dla użytkowników?
Poniżej przedstawiono ogólne rozmieszczenia procedura Resetowanie hasła:

1.  Włącz resetowania w katalogu, przejdź na kartę **Konfiguruj** w [Portalu zarządzania Azure](https://manage.windowsazure.com) i wybierając **Tak** dla opcji **Użytkownicy dostęp do resetowania hasła** hasła.
2.  Przypisywanie licencji odpowiednie dla poszczególnych użytkowników, do której chcesz oferują resetowania w hasła, przechodząc na kartę **licencje** w [Portalu zarządzania Azure](https://manage.windowsazure.com).
3.  Opcjonalnie można ograniczyć resetowania do grupy użytkowników do wdrożenia funkcji powoli czasie ustawienie przełącznika **Ogranicz dostęp do resetowania hasła** na **Tak** i wybierając grupy zabezpieczeń, aby włączyć dla Resetowanie hasła (należy zauważyć, że użytkownicy muszą mieć przypisane licencje).
4.  Poinstruuj użytkowników, aby zresetować, albo wysyłając wiadomość e-mail, aby zarejestrować, jeśli hasło umożliwiające wymuszane rejestracji na panelu programu access lub przekazując danych uwierzytelniania odpowiednią dla tych użytkowników siebie za pośrednictwem DirSync, programu PowerShell lub do [Portalu zarządzania Azure](https://manage.windowsazure.com).  Więcej informacji na temat znajdują się poniżej.
5.  W czasie Przejrzyj rejestrowania, przechodząc do karty raporty wyświetlanie raportu [**Aktywności rejestracji Resetowanie hasła**](active-directory-passwords-get-insights.md#view-password-reset-registration-activity) użytkowników.
6.  Po zarejestrowano dobre liczba użytkowników, obejrzyj je za pomocą hasła, resetowanie, przechodząc do karty raporty wyświetlania raportu [**Działania Resetowanie hasła**](active-directory-passwords-get-insights.md#view-password-reset-activity) .

Istnieje kilka sposobów Poinformuj użytkowników, że można zarejestrować i za pomocą hasła, resetowanie w Twojej organizacji.  Są one wyszczególnione poniżej.

### <a name="email-based-rollout"></a>Rozmieszczenia opartej na poczcie e-mail
Być może Najłatwiejszym sposobem Poinformuj użytkowników o zarejestrować lub użyj hasła, aby zresetować jest, wysyłając wiadomość e-mail, jeśli jest to wymagane.  Oto szablon, którego można użyć w tym celu.  Zachęcamy do Zamień kolory / logo z tymi własny wybranie Dostosuj go do własnych potrzeb.

  ![][001]

Możesz [pobrać szablonu wiadomości e-mail](http://1drv.ms/1xWFtQM).

### <a name="creating-your-own-password-portal"></a>Tworzenie portalem hasła
Jeden strategii działa również w przypadku dużych klientów wdrażanie funkcje zarządzania hasłami umożliwiają tworzenie pojedynczego "hasło portal", który użytkownicy służy do zarządzania wszystkie elementy dotyczące haseł w jednym miejscu.  

Wielu klientów największa wybierz pozycję Utwórz katalog główny wpis DNS, takich jak https://passwords.contoso.com z łączami do hasła Azure AD Resetowanie portalu, portal rejestracji do resetowania hasła oraz zmienianie hasła stron.  Dzięki temu w wiadomościach e-mail lub ulotki, które zostaną wysłane, możesz umieścić pojedynczą do zapamiętania, adres URL, który użytkownicy możesz przechodzić do wykonujących drugiego, aby rozpocząć pracę z usługą.

Aby rozpoczęcie poniżej, firma Microsoft utworzono prosty stronę, która używa najnowszych odpowiada interfejsu użytkownika projekt paradygmatów i będzie działał we wszystkich przeglądarek i urządzeń przenośnych.

  ![][007]

Możesz [pobrać szablon witryny sieci Web w tym miejscu](https://github.com/kenhoff/password-reset-page).  Firma Microsoft zaleca dostosowywanie logo i kolorów do potrzeb danej organizacji.

### <a name="using-enforced-registration"></a>Korzystanie z wymuszoną rejestracji
Jeśli chcesz, aby zarejestrować się resetowania hasła użytkowników, możesz również wymusić ich tak, aby zarejestrować się zalogować do panelu dostęp u [http://myapps.microsoft.com](http://myapps.microsoft.com).  Możesz włączyć tę opcję, na karcie **Konfigurowanie** usługi katalogowej przez włączenie opcji **Wymagaj użytkowników do rejestracji podczas logowania do panelu programu Access** .  

Można też dodatkowo określić, czy zostanie wyświetlony monit o ponowne rejestrowanie po można konfigurować czasu zmieniając opcję **Liczba dni przed użytkownikami musi potwierdzić swoje dane kontaktowe** , aby określić wartość różną od zera. Aby uzyskać więcej informacji, zobacz [Dostosowywanie zachowania zarządzania hasła użytkownika](active-directory-passwords-customize.md#password-management-behavior) .

  ![][002]

Po włączeniu tę opcję, gdy użytkownicy logować się do panel dostępu, zobaczą menu podręczne, informujące o tym, że administratora musi ich tak, aby sprawdzić swoje informacje kontaktowe. Ich go używać, aby zresetować swoje hasło, jeśli ich kiedykolwiek utracisz dostęp do swojego konta.

  ![][003]

Kliknięcie przycisku **Sprawdź teraz** przenosi je **do resetowania hasła portal rejestracji** u [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) i wymaga ich rejestrowania.  Rejestracja przy użyciu tej metody można odrzucone przez kliknięcie przycisku **Anuluj** lub zamykając okno, ale użytkownicy są przypomnienie każdorazowo ich Zaloguj się, jeśli nie zarejestrować.

  ![][004]

### <a name="uploading-data-yourself"></a>Samodzielnie przekazywania danych
Jeśli chcesz przekazać dane uwierzytelniania samodzielnie, użytkownicy nie muszą zarejestrować do resetowania zanim będzie mógł zresetować swoje hasło hasła.  Jak użytkownicy mają zasady, które zostały zdefiniowane Resetowanie danych uwierzytelniania zdefiniowane dla swojego konta, która spełnia hasło, a następnie Ci użytkownicy będą mogli zresetować swoje hasło.

Aby dowiedzieć się, jakie właściwości można ustawić za pośrednictwem połączenia AAD lub środowiska Windows PowerShell, zobacz, [jakie dane są używane przez resetowania hasła](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

Możesz przekazać dane uwierzytelniania za pomocą [Portalu zarządzania Azure](https://manage.windowsazure.com) , wykonując poniższe czynności:

1.  Przejdź do katalogu **rozszerzenie usługi Active Directory** w [Portalu zarządzania Azure](https://manage.windowsazure.com).
2.  Kliknij kartę **użytkowników** .
3.  Wybierz użytkownika, który Cię interesuje z listy.
4.  Na karcie pierwszego znajdziesz **Alternatywny adres E-mail**, które mogą być używane jako właściwości umożliwiające Resetowanie hasła.

    ![][005]

5.  Kliknij kartę **Informacji o pracy** .
6.  Na tej stronie znajdziesz **Telefon w biurze**, **telefonu komórkowego**, **Phone uwierzytelniania**i **Uwierzytelniania wiadomości E-mail**.  Te właściwości można także ustawić, aby umożliwić użytkownikowi zresetować swoje hasło.

    ![][006]

Zobacz, [jakie dane są używane przez resetowania hasła](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) , aby zobaczyć, jak każdy z tych właściwości można.

Zobacz, [jak uzyskać dostęp do hasła Resetuj dane dla użytkowników z programu PowerShell](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users) czytanie i ustawić te dane przy użyciu programu PowerShell.

## <a name="sample-training-materials"></a>Przykładowe materiały szkoleniowe
Pracujemy obecnie nad przykładowe materiały szkoleniowe, których można szybko organizacji IT i użytkowników doskonale o tym, jak wdrażać i używać w przypadku resetowania hasła.  Zamieścimy!


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Łącza do hasła Resetowanie dokumentacji
Poniżej znajdują się łącza do wszystkich stron dokumentacji Azure AD hasło:

* **Czy jesteś tutaj, ponieważ występują problemy z logowaniem?** Jeśli tak, [Oto jak można zmienić i zresetować swoje hasło](active-directory-passwords-update-your-own-password.md).
* [**Działania**](active-directory-passwords-how-it-works.md) — Dowiedz się więcej o sześć różnych składników usługi i każdej czy
* [**Wprowadzenie**](active-directory-passwords-getting-started.md) — Dowiedz się, jak użytkownicy mogą zresetować i zmieniać swoje hasła w chmurze lub lokalnych
* [**Dostosowywanie**](active-directory-passwords-customize.md) — Dowiedz się, jak dostosować wygląd i sposób działania i zachowanie usługi do potrzeb danej organizacji
* [**Uzyskaj więcej informacji**](active-directory-passwords-get-insights.md) — Dowiedz się więcej o naszych zintegrowane funkcje raportowania
* [**Często zadawane pytania**](active-directory-passwords-faq.md) — odpowiedzi na często zadawane pytania
* [**Rozwiązywanie problemów**](active-directory-passwords-troubleshoot.md) — Dowiedz się, jak szybko Rozwiązywanie problemów z usługą
* [**Aby uzyskać więcej informacji**](active-directory-passwords-learn-more.md) — głębokich przejdź do szczegółów technicznych opis działania usługi



[001]: ./media/active-directory-passwords-best-practices/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-best-practices/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-best-practices/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-best-practices/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-best-practices/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-best-practices/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-best-practices/007.jpg "Image_007.jpg"
