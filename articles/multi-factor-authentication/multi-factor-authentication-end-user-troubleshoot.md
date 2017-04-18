<properties
    pageTitle="Rozwiązywanie problemów z Weryfikacja dwuetapowa | Microsoft Azure"
    description="Ten dokument zawiera użytkowników informacje dotyczące co należy zrobić, jeśli ich napotkasz problem z uwierzytelnianie wieloskładnikowe Azure."
    services="multi-factor-authentication"
    keywords = "uwierzytelnianie wieloskładnikowe klienta problem z uwierzytelnianiem, identyfikator korelacji"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="having-trouble-with-two-step-verification"></a>Masz problemy z Weryfikacja dwuetapowa

W tym artykule omówiono niektóre problemy, które mogą wystąpić przy Weryfikacja dwuetapowa. Jeśli masz problemy nie jest uwzględniona w tym miejscu, Podaj szczegółowy swoją opinię w sekcji komentarze tak, aby można było poprawić.

## <a name="i-lost-my-phone-or-it-was-stolen"></a>Utrata mojego telefonu lub został on kradzież

Istnieją dwa sposoby do niej wrócić do swojego konta. Pierwsza polega na Zaloguj się przy użyciu numeru telefonu alternatywny uwierzytelniania, jeśli masz skonfigurowane jedną. Druga należy skontaktować się z administratorem, aby wyczyścić ustawienia.

Jeśli Twój telefon zostało utracone lub kradzież, również zaleca się poproś administratora o Resetowanie hasła aplikacji i wyczyść pole wyboru podtrzymują dowolnego urządzenia. Jeśli administrator nie ma pewności, jak to zrobić, wskaż je w tym artykule: [Zarządzanie użytkownikami i urządzeń](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords).


### <a name="use-an-alternate-phone-number"></a>Alternatywny numer telefonu za pomocą

Jeśli skonfigurowano wiele opcji weryfikacji, łącznie z numerem telefonu pomocniczej lub aplikacji uwierzytelnienia na innym urządzeniu, służy jedną z następujących do zalogowania się.

Aby zalogować się przy użyciu alternatywnego numeru telefonu, wykonaj następujące czynności:

1. Zaloguj się w normalny sposób.
2. Po wyświetleniu monitu w celu dalszego zbadania konta, wybierz pozycję **Użyj opcji różnych weryfikacji**.

    ![Różne weryfikacji](./media/multi-factor-authentication-end-user-manage/differentverification.png)

3. Wybierz numer telefonu, których masz dostęp.

    ![Dodatkowego telefonu](./media/multi-factor-authentication-end-user-manage/altphone2.png)

4. Po jesteś w Twoim kontem, [zarządzać ustawieniami](multi-factor-authentication-end-user-manage-settings.md) zmienić swój numer telefonu uwierzytelniania.

>[AZURE.IMPORTANT]
>Należy skonfigurować numer telefonu pomocniczej uwierzytelniania. Jeśli numeru telefonu podstawowego i usługi aplikacji dla urządzeń przenośnych na telefonie z tym samym, możesz potrzebujesz trzecią możliwość Jeśli Twój telefon zostanie utracony lub kradzież.

### <a name="clear-your-settings"></a>Czyszczenie ustawień

Jeśli nie skonfigurowano pomocniczej uwierzytelniania numer telefonu, a następnie należy skontaktować się z administratorem, aby uzyskać pomoc. Zaleć im wyczyść ustawień, aby przy następnym zalogowaniu się, pojawi się monit o [Skonfiguruj swoje konto](multi-factor-authentication-end-user-first-time.md) ponownie.


## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>I nie odbiera tekstu lub połączenie na telefonie

Istnieje kilka powodów, dlaczego spróbuj zalogować się, ale nie odbierać tekstu lub rozmowy telefonicznej. Jeśli pomyślnie otrzymano teksty lub telefoniczne na Twój telefon w przeszłości, następnie jest prawdopodobnie problem z dostawcą telefonu, a nie konta. Upewnij się, że sygnał dobre komórek i jeśli chcesz otrzymywać wiadomości SMS upewnij się, że planu telefonu i usługi pomocy technicznej wiadomości SMS.

Jeśli zostały czas potrzebny kilka minut na tekst lub połączenie, jest wypróbować inną opcję najszybszy sposób, aby przejść do swojego konta.

1. Na stronie, która jest oczekujące na zatwierdzenie weryfikacji, zaznacz pole wyboru **Użyj opcji różnych weryfikacji** .

    ![Różne weryfikacji](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

2. Wybierz metodę telefonu numer lub dostarczenia, którego chcesz użyć.

    Odebranie wielu kodów weryfikacji działa tylko z najnowszych nich.

Jeśli nie masz użyć innej metody skonfigurowane, skontaktuj się z administratorem i poproś o wyczyścić ustawienia. Przy następnym zalogowaniu się, możesz zostanie wyświetlony monit o [Konfigurowanie uwierzytelniania wieloskładnikowego](multi-factor-authentication-end-user-first-time.md) ponownie.


Jeśli masz często opóźnienia z powodu sygnału nieprawidłowe komórki, zalecamy korzystanie z [aplikacji Microsoft uwierzytelnienia](multi-factor-authentication-microsoft-authenticator.md) na telefon smartphone. Aplikację można wygenerować kody losowe zabezpieczeń, które umożliwia logowanie się i kody te nie wymagają połączenia sygnału lub internet dowolną komórkę.


## <a name="app-passwords-are-not-working"></a>Nie działają hasła aplikacji

Najpierw upewnij się, że hasło aplikacji zostały wprowadzone poprawnie.  Jeśli nie jest nadal nie działa, spróbuj logowanie się i [Utwórz nowe hasło aplikacji](multi-factor-authentication-end-user-app-passwords.md).  Jeśli to nie działa, skontaktuj się z administratorem i ich [Usuwanie istniejących haseł aplikacji](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords) , a następnie można utworzyć nową.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Nie mogę znaleźć odpowiedzi problemu.

Jeśli po wypróbowaniu tych kroków, ale są nadal występowania problemów, skontaktuj się z administratorem lub osoby, która Konfigurowanie uwierzytelniania wieloskładnikowego dla Ciebie. Należy je mógł pomóc.

Ponadto można Zadaj pytanie na [forum Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) lub [kontakt z pomocą techniczną](https://support.microsoft.com/contactus) i firma Microsoft będzie odpowiedzieć problemu, jak możemy.

Jeśli kontakt z pomocą techniczną zawiera następujące informacje:

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
- [Zarządzanie ustawieniami na potrzeby weryfikacji dwuetapowej](multi-factor-authentication-end-user-manage-settings.md)  
- [Aplikacja Microsoft Authenticator — często zadawane pytania](multi-factor-authentication-app-faq.md)
