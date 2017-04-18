<properties
    pageTitle="Jak to działa: Zarządzanie hasłami w usłudze Azure AD | Microsoft Azure"
    description="Więcej informacji na temat poszczególnych składników Zarządzanie Azure AD hasła, w tym miejsce, w którym użytkownicy rejestracji, zresetuj i zmieniać swoje hasła i gdzie administratorzy skonfigurować, sprawozdanie, a Włącz zarządzanie hasłami usługi Active Directory w lokalnej."
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

# <a name="how-password-management-works"></a>Jak działa Zarządzanie hasłami

> [AZURE.IMPORTANT] **Czy jesteś tutaj, ponieważ występują problemy z logowaniem?** Jeśli tak, [Oto jak można zmienić i zresetować swoje hasło](active-directory-passwords-update-your-own-password.md).

Zarządzanie hasłami w usłudze Azure Active Directory składa się z kilku części logicznych, opisane poniżej.  Kliknij każde łącze, aby dowiedzieć się więcej na temat tego składnika.

- [**Portal konfiguracji zarządzania hasła**](#password-management-configuration-portal) — Administratorzy mogą kontrolować różnych aspektów sposobu zarządzania hasłami, przechodząc do ich katalogu Konfigurowanie karty w [Portalu zarządzania Azure](https://manage.windowsazure.com)ich dzierżaw.
- [**Portal rejestracji użytkownika**](#user-registration-portal) — użytkownicy będą mogli samodzielnie zarejestrować do resetowania za pośrednictwem portalu hasła.
- [**Portal Resetowanie hasła użytkownika**](#user-password-reset-portal) — użytkownicy mogą resetować swoje własne hasła przy użyciu wielu różnych problemów zgodnie z zasadami resetowania hasła kontrolowane przez administratora
- [**Portal zmienić hasło użytkownika**](#user-password-change-portal) — użytkownicy mogą zmieniać swoje własne hasła w dowolnym momencie wpisując ich stare hasło i wybierając nowe hasło, za pomocą portalu
- [**Raporty zarządzania hasła**](#password-management-reports) — Administratorzy mogą wyświetlania i analizowania hasło resetowania i rejestracji aktywność w swoich dzierżawy, przechodząc do sekcji "Raporty aktywności" Karta "Raporty" ich katalogu w [Portalu zarządzania Azure](https://manage.windowsazure.com)
- [**Hasło zapisu część Azure AD Connect**](#password-writeback-component-of-azure-ad-connect) — Administratorzy Opcjonalnie można włączyć funkcję zapisu hasła, podczas instalowania narzędzie Azure AD Connect służących do zarządzania federacyjnych lub hasło synchronizowane hasła użytkowników z chmury.

## <a name="password-management-configuration-portal"></a>Portal konfiguracji zarządzania hasła
Można skonfigurować zasady zarządzania hasłami dla konkretnego katalogu za pomocą [Portalu zarządzania Azure](https://manage.windowsazure.com) , przechodząc do sekcji **Resetowanie hasła użytkownika** na karcie **Konfigurowanie** katalogu.  Na tej stronie konfiguracji możesz sterować wielu aspektów sposobu zarządzania hasłami w organizacji, w tym:

- Włączanie i wyłączanie resetowania hasła dla wszystkich użytkowników w katalogu
- Ustawianie liczby problemów (jednej lub dwóch) użytkownika musi przejść, aby zresetować swoje hasło
- Ustawianie określonych typów problemów, które chcesz włączyć dla użytkowników w organizacji poniżej:
 - Telefon komórkowy (kod weryfikacyjny za pomocą tekstu lub połączeń głosowych)
 - Telefon w biurze (połączeń głosowych)
 - Alternatywny adres E-mail (kod weryfikacyjny za pośrednictwem poczty e-mail)
 - Pytania dotyczące zabezpieczeń (uwierzytelnianie oparte na wiedzy)
- Ustawianie liczby pytania użytkownika należy zarejestrować było użyć pytania metodę uwierzytelniania zabezpieczeń (widoczną tylko jeśli pytania dotyczące zabezpieczeń są włączone)
- Ustawianie liczby pytania użytkownika musi zostać podany podczas resetowania do używania metody uwierzytelniania pytania zabezpieczenia (widoczną tylko jeśli pytania dotyczące zabezpieczeń są włączone)
- Przy użyciu wstępnie puszkach, zlokalizowane, zabezpieczenia pytań, na które użytkownik może być użyta podczas rejestrowania dla hasła resetowanie (widoczną tylko jeśli pytania dotyczące zabezpieczeń są włączone)
- Definiowanie niestandardowych zabezpieczeń pytania, na które użytkownik może być użyta podczas rejestrowania dla hasła resetowanie (widoczną tylko jeśli pytania dotyczące zabezpieczeń są włączone)
- Wymaganie od użytkowników zarejestrować do resetowania, gdy znajdzie się aplikację Panel dostępu w [http://myapps.microsoft.com](http://myapps.microsoft.com)hasła.
- Wymaganie użytkownicy mogą ponownie potwierdzić swoje dane wcześniej zarejestrowanych po można konfigurować liczby dni upłynęło (widoczne tylko w przypadku włączenia wymuszoną rejestracji)
- Adres e-mail zgłoszeń niestandardowej lub adres URL, który ma być wyświetlany dla użytkowników, w przypadku, gdy mają problemu resetowanie haseł
- Włączanie lub wyłączanie możliwości zapisu hasła (w przypadku zapisu hasło został wdrożony przy użyciu funkcji AAD połączenie)
- Wyświetlanie stanu agenta hasło zapisu (w przypadku zapisu hasło został wdrożony przy użyciu funkcji AAD połączenie)
- Włączanie powiadomień e-mail dla użytkowników po zresetowaniu swoje hasła (znajdujący się w sekcji **powiadomienia** do [Portalu zarządzania Azure](https://manage.windowsazure.com))
- Włączanie powiadomień e-mail dla administratorów, gdy innych administratorów zresetować swoje własne hasła (znajdujący się w sekcji **powiadomienia** do [Portalu zarządzania Azure](https://manage.windowsazure.com)
- Znakowanie hasła użytkownika Resetowanie portalu i resetowania hasła wiadomości e-mail z logo i nazwa Twojej organizacji przy użyciu dzierżawy związane ze znakowaniem, funkcja dostosowywania (znajdujący się w sekcji **Właściwości katalogu** [Portalu zarządzania Azure](https://manage.windowsazure.com)

Aby uzyskać więcej informacji na temat konfigurowania zarządzania hasłami w Twojej organizacji, zobacz [Wprowadzenie: Zarządzanie hasłami AD Azure](active-directory-passwords-getting-started.md).

##<a name="user-registration-portal"></a>Portal rejestracji użytkownika
Zanim użytkownicy będą mogli używać w przypadku resetowania hasła, należy zaktualizować ich kontami użytkowników w chmurze z danymi poprawne uwierzytelnienie, aby upewnić się, że mogą przechodzić odpowiednią liczbę problemach resetowania hasła zdefiniowane przez administratora.  Administratorzy mogą również definiować tych informacji uwierzytelniania w imieniu ich użytkownika przy użyciu Azure lub Office web portali DirSync / Azure AD Connect lub środowiska Windows PowerShell.

Jednak jeśli wolisz, aby były zarejestrować własne dane użytkowników, oferujemy strony sieci web, które użytkownicy mogą przejdź do w celu udostępniania informacji.  Ta strona umożliwia użytkownikom określanie resetowania informacje uwierzytelniające zgodnie z hasła zasady, które zostały włączone w organizacji.  Po zweryfikowaniu te dane są przechowywane w ich chmury konto użytkownika ma być używany dla odzyskiwania konta w późniejszym czasie. Poniżej przedstawiono, jak wygląda portalu rejestracji:

  ![][001]

Aby uzyskać więcej informacji, zobacz [Wprowadzenie: Zarządzanie hasłami AD Azure](active-directory-passwords-getting-started.md) i [Najważniejsze wskazówki: Zarządzanie hasłami AD Azure](active-directory-passwords-best-practices.md).

##<a name="user-password-reset-portal"></a>Portal Resetowanie hasła użytkownika
Po włączeniu samodzielne hasła resetowanie, konfigurowanie organizacji Samodzielne resetowanie haseł i zapewnić, że użytkownicy mają odpowiednie dane kontaktowe w katalogu, użytkownicy w organizacji będą mogli resetowanie haseł automatycznie z dowolnej strony sieci web, który używa konto służbowe do logowania (na przykład [portal.microsoftonline.com](https://portal.microsoftonline.com)). Na stronach, takie jak te, użytkownicy zobaczą **nie może uzyskać dostępu do konta?** łącze.

  ![][002]

Klikając to łącze, spowoduje uruchomienie portalu resetowania hasła Sklep internetowy.

  ![][003]

Aby uzyskać więcej informacji na temat sposobu zresetować swoje własne hasła użytkowników, zobacz [Wprowadzenie: Zarządzanie hasłami AD Azure](active-directory-passwords-getting-started.md).

##<a name="user-password-change-portal"></a>Portal zmiana hasła użytkownika
Jeśli użytkownik chce zmienić swoje własne hasła, są to zrobić za pomocą portalu Zmienianie hasła w dowolnym momencie.  Użytkownicy mogą uzyskiwać dostęp do portalu Zmień hasło za pośrednictwem strony profilu Panel dostępu lub kliknięcie łącza "Zmień hasło" z poziomu innych aplikacji usługi Office 365.  W przypadku gdy wygasania haseł, użytkownicy również zapyta można je zmienić automatycznie podczas logowania.

  ![][004]

W obu przypadkach Jeśli włączono zapisu hasło i albo Federacji użytkownika lub czy synchronizacji haseł, te zmieniono hasła są zapisywane w usłudze Active Directory w lokalnej. Poniżej przedstawiono, jak wygląda portalu zmiana hasła:

  ![][005]

Aby dowiedzieć się, jak użytkownicy mogą zmieniać swoje własne hasła usługi Active Directory w lokalnej, zobacz [Wprowadzenie: Zarządzanie hasłami AD Azure](active-directory-passwords-getting-started.md).

##<a name="password-management-reports"></a>Zarządzanie hasłami raportów
Przechodzenie do karty **Raporty** i chcesz znaleźć w sekcji **Dzienniki aktywności** , zostanie wyświetlona dwa raporty zarządzania hasłami: **aktywności do resetowania hasła** i **rejestracji aktywności do resetowania hasła**.  Korzystanie z tych dwóch raportów, zostanie wyświetlony widok użytkownikom rejestrowania i przy użyciu hasła, resetowanie w Twojej organizacji. Poniżej przedstawiono wygląd tych raportów w [Portalu zarządzania Azure](https://manage.windowsazure.com):

  ![][006]

Aby uzyskać więcej informacji, zobacz [uzyskać więcej informacji: raporty zarządzania hasła Azure AD](active-directory-passwords-get-insights.md).

##<a name="password-writeback-component-of-azure-ad-connect"></a>Hasło zapisu część Azure AD Connect
Jeśli hasła użytkowników w organizacji pochodzą z lokalnym środowiskiem (albo za pośrednictwem federacji lub synchronizacji haseł), możesz zainstalować najnowszą wersję programu narzędzie Azure AD Connect umożliwiający aktualizowanie hasła bezpośrednio z chmury.  Oznacza to, że po użytkowników zapomnisz lub do zmodyfikowania swoje hasło AD, mogą je tak bezpośrednio z sieci web.  Oto, gdzie znaleźć zapisu hasła w Kreatorze instalacji narzędzie Azure AD Connect:

  ![][007]

Aby uzyskać więcej informacji na temat Azure AD Connect, zobacz [Wprowadzenie: narzędzie Azure AD Connect](active-directory-aadconnect.md). Aby uzyskać więcej informacji na temat zapisu hasła zobacz [Wprowadzenie: Zarządzanie hasłami AD Azure](active-directory-passwords-getting-started.md).


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Łącza do hasła Resetowanie dokumentacji
Poniżej znajdują się łącza do wszystkich stron dokumentacji Azure AD hasło:

* **Czy jesteś tutaj, ponieważ występują problemy z logowaniem?** Jeśli tak, [Oto jak można zmienić i zresetować swoje hasło](active-directory-passwords-update-your-own-password.md).
* [**Wprowadzenie**](active-directory-passwords-getting-started.md) — Dowiedz się, jak użytkownicy mogą zresetować i zmieniać swoje hasła w chmurze lub lokalnych
* [**Dostosuj**](active-directory-passwords-customize.md) — Dowiedz się, jak dostosować wygląd i sposób działania i zachowanie usługi do potrzeb danej organizacji
* [**Najważniejsze wskazówki**](active-directory-passwords-best-practices.md) — Dowiedz się, jak szybko wdrażanie i efektywne zarządzanie hasłami w organizacji
* [**Uzyskaj więcej informacji**](active-directory-passwords-get-insights.md) — Dowiedz się więcej o naszych zintegrowane funkcje raportowania
* [**Często zadawane pytania**](active-directory-passwords-faq.md) — odpowiedzi na często zadawane pytania
* [**Rozwiązywanie problemów**](active-directory-passwords-troubleshoot.md) — Dowiedz się, jak szybko Rozwiązywanie problemów z usługą
* [**Aby uzyskać więcej informacji**](active-directory-passwords-learn-more.md) — głębokich przejdź do szczegółów technicznych opis działania usługi



[001]: ./media/active-directory-passwords-how-it-works/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-how-it-works/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-how-it-works/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-how-it-works/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-how-it-works/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-how-it-works/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-how-it-works/007.jpg "Image_007.jpg"
