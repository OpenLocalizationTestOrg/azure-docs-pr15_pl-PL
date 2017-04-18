<properties
    pageTitle="Aplikacja Microsoft uwierzytelnienia — często zadawane pytania"
    description="Zawiera listę często zadawane pytania i odpowiedzi dotyczące aplikacji Microsoft Authentication i uwierzytelnianie wieloskładnikowe Azure."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="pblachar, librown"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="microsoft-authenticator-application-faq"></a>Aplikacja Microsoft Authenticator — często zadawane pytania

Aplikacja Microsoft Authenticator zastępowane aplikacji uwierzytelnienia Azure, a jest zalecane aplikacja jest stosowane uwierzytelnianie wieloskładnikowe Azure. Ta aplikacja jest dostępna dla Windows Phone, Android lub iOS.

## <a name="frequently-asked-questions"></a>Często zadawane pytania

- **Co się stało z uwierzytelnienia Azure, uwierzytelnianie wieloskładnikowe i aplikacji konto Microsoft?**

    Aplikacja Microsoft Authenticator zastępuje wzajemnie te aplikacje. Uaktualnienie do Authenticator Microsoft Azure uwierzytelnienia. Jeśli używasz uwierzytelnianie wieloskładnikowe lub aplikacji konta Microsoft, zainstaluj Authenticator firmy Microsoft i ponownie dodać konta. Upewnij się zakończyć dodawanie konta do nowej aplikacji przed usunięciem starych.

- **Już korzystam z aplikacji Microsoft Authenticator kodów weryfikacji. Jak przełączyć do powiadomienia wypychane jednym kliknięciem**  

    Zatwierdzanie logowania za pomocą powiadomień wypychanych jest dostępna tylko w przypadku kont Microsoft, a nie dla kont innych firm, na przykład Google lub Facebook. Służbowe konta Microsoft organizacji można wyłączyć tę opcję, ale.

    Jeśli dla Twojego konta osobistego za pomocą konta Microsoft, a chcesz przełączyć się do powiadomienia wypychane, musisz ponownie Dodaj swoje konto. Ponownie zarejestrować urządzenie za pomocą konta i Konfigurowanie powiadomień wypychanych.  

    Jeśli Twoje konto nie ma włączona weryfikacja dwuetapowa, zobacz [temat Weryfikacja dwuetapowa](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) Określ, czy jest dla Ciebie.  

- **Kiedy I będzie mógł korzystać z powiadomień wypychanych jednym kliknięciem na telefonie iPhone lub iPad?**  

    Ta funkcja jest w wersji beta do końca sierpnia, kiedy staje się ogólnie dostępne dla konta serwera Microsoft. Jeśli chcesz dołączyć do naszego programu beta, wysyłanie wiadomości e-mail do msauthenticator@microsoft.com. Uwzględnić imię, nazwisko oraz Apple ID w wiadomości.  

- **Powiadomienia wypychane jednym kliknięciem działają w przypadku kont firmy Microsoft?**  

    Nie, powiadomienia wypychane działa tylko przy użyciu konta Microsoft i konta usługi Azure Active Directory. Swojej pracy lub szkole korzysta z kont Azure AD, może wyłączyć tę funkcję.  

- **Urządzenie można przywrócić z kopii zapasowej, a kody Moje konto są brakujące lub nie działa. Co się stało?**  

    Ze względów bezpieczeństwa firma Microsoft nie Przywracanie konta z kopii zapasowych aplikacji. Jeśli aplikacji w systemie iOS Przywracanie z kopii zapasowej, konta nadal są wyświetlane, ale nie działają odbierać weryfikacji logowania lub wygenerować kody zabezpieczeń. Po przywróceniu aplikacji należy usunąć konta i dodać je ponownie.

- **Mam nowe urządzenie. Jak usunąć aplikację Microsoft Authenticator z urządzenie starych i przejście do nowego?**

    Dodawanie aplikacji Microsoft Authenticator do nowego urządzenia nie są automatycznie usuwane go z innych urządzeń. Aby zarządzać, które urządzenia zostały skonfigurowane dla Twojego konta, odwiedź witrynę samej umożliwia zarządzanie Weryfikacja dwuetapowa, i wybierz pozycję, aby usunąć stare aplikacje.

    Dla osobistego konta Microsoft Ta witryna sieci Web jest stronę [zabezpieczenia konta](https://account.microsoft.com/security) . Dla służbowe konta Microsoft Ta witryna sieci Web może być [Moje aplikacje](https://myapps.microsoft.com) lub niestandardowe portalu, który skonfigurowano organizacji.

## <a name="contact-us"></a>Kontakt z nami

Jeśli tutaj nie odpowiedzi na pytanie, pozostaw komentarz w dolnej części strony. Lub [kontakt z pomocą techniczną](https://support.microsoft.com/contactus) i firma Microsoft będzie odpowiadają problemu jak możemy.

Jeśli kontakt z pomocą techniczną obejmują możliwie największą część następujące informacje i można wykonywać następujące czynności:

- **Identyfikator użytkownika** — co to jest adres e-mail, który próbujesz Zaloguj się przy użyciu?
- **Ogólny opis błędu** — czy jakie komunikat o błędzie dokładnie zobacz?  Jeśli wystąpił żaden komunikat o błędzie, opisz nieoczekiwane działanie, które zauważysz, szczegóły.
- **Strony** — które strony zostały na kiedy został wyświetlony komunikat o błędzie (Dołącz adres URL)?
- **Kod błędu** - kod błędu, które otrzymujesz.
- **Identyfikator sesji** - identyfikator określonej sesji, które otrzymujesz.
- **Identyfikator korelacji** — jaki był kod identyfikator korelacji generowane, gdy zostanie wyświetlony komunikat o błędzie.
- **Sygnatura czasowa** — jaki był dokładną datę i godzinę został wyświetlony komunikat o błędzie (obejmuje strefy czasowej)?

Wiele z tych informacji można znaleźć na stronie logowania. Jeśli nie możesz sprawdzić do logowania się w czasie, wybierz pozycję **Wyświetl szczegóły**.

![Zaloguj się informacje o błędzie](./media/multi-factor-authentication-end-user-troubleshoot/view_details.png)

W tym te informacje pomagają nam rozwiązać problem tak szybko, jak to możliwe.

## <a name="related-topics"></a>Tematy pokrewne

- [Uwierzytelnianie wieloskładnikowe Azure — często zadawane pytania](multi-factor-authentication-faq.md)  
- [Informacje o Weryfikacja dwuetapowa](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) dla konta Microsoft
- [Masz problemy z Weryfikacja dwuetapowa?](multi-factor-authentication-end-user-troubleshoot.md)
