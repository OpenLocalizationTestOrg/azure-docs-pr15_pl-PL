<properties
    pageTitle="Azure doświadczenia rejestrowanie MFA uwierzytelnianie wieloskładnikowe Azure"
    description="Ta strona umożliwi wskazówki na miejsce docelowe, aby wyświetlić różne metody logowania przy użyciu Azure MFA."
    keywords="Uwierzytelnianie użytkownika, logowanie, zaloguj się przy użyciu telefonu komórkowego, zaloguj się przy użyciu Telefon w biurze"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>Zaloguj się pracę w programie uwierzytelnianie wieloskładnikowe Azure
> [AZURE.NOTE]  Poniższą dokumentację dostępnych na tej stronie zawiera typowe środowisko logowania.  Aby uzyskać pomoc dotyczącą logowania się zobacz [problemy z uwierzytelnianie wieloskładnikowe Azure](multi-factor-authentication-end-user-manage-settings.md)



## <a name="what-will-your-sign-in-experience-be"></a>Co będzie informacje logowania użytkownika?
W zależności od tego, jak zalogować się i używanie uwierzytelnianie wieloskładnikowe będą różne dla środowiska pracy użytkownika.  W tej sekcji możemy zawiera informacje dotyczące czego mogą się spodziewać podczas logowania się.  Wybierz ten, który najlepiej opisuje, co robią:


Co robisz?|Opis
:------------- | :------------- |
[Logowanie się przy użyciu telefonu komórkowego lub pakietu office](#signing-in-with-mobile-or-office-phone) | Jest to, czego można oczekiwać z zalogowaniem się za pomocą telefonu komórkowego lub pakietu office.
[Logowanie się przy użyciu aplikacji Microsoft Authenticator za pomocą powiadomień](#signing-in-with-the-microsoft-authenticator-app-using-notification) | Jest to, czego można oczekiwać przy użyciu aplikacji Microsoft Authenticator z powiadomienia.
[Logowanie się przy użyciu aplikacji Microsoft Authenticator, używając kod weryfikacyjny](#signing-in-with-the-microsoft-authenticator-app-using-verification-code)|Jest to, czego można oczekiwać thapp Authenticator firmy Microsoft przy użyciu kodu weryfikacyjnego.
[Logowanie się przy użyciu alternatywna metoda](#signing-in-with-an-alternate-method)|Spowoduje to wyświetlenie czego mogą się spodziewać, jeśli chcesz użyć alternatywna metoda.

## <a name="signing-in-with-mobile-or-office-phone"></a>Logowanie się przy użyciu telefonu komórkowego lub pakietu office

Następujące informacje będą opisują występowania uwierzytelnianie wieloskładnikowe za pomocą telefonu komórkowego lub pakietu office.

### <a name="to-sign-in-with-a-call-to-your-office-or-mobile-phone"></a>Zaloguj się przy użyciu połączenia do pakietu office lub telefon komórkowy

- Zaloguj się do aplikacji lub usługi, takie jak przy użyciu nazwy użytkownika i hasła usługi Office 365.
- Microsoft nawiąże połączenie.

![Połączenia firmy Microsoft](./media/multi-factor-authentication-end-user-signin-phone/call.png)

- Odpowiedz na telefon, a następnie naciśnij klawisz #.

![Odpowiedzi](./media/multi-factor-authentication-end-user-signin-phone/phone.png)

- Czy można teraz zalogować się.</li>

## <a name="signing-in-with-the-microsoft-authenticator-app-using-notification"></a>Logowanie się przy użyciu aplikacji Microsoft Authenticator za pomocą powiadomień

Następujące informacje będą opisano możliwości używania uwierzytelnianie wieloskładnikowe z aplikacją Microsoft Authenticator, gdy są wysyłane powiadomienia.

### <a name="to-sign-in-with-a-notification-sent-the-microsoft-authenticator-app"></a>Zaloguj się przy użyciu powiadomienie wysyłane aplikacji Authenticator firmy Microsoft

- Zaloguj się do aplikacji lub usługi, takie jak przy użyciu nazwy użytkownika i hasła usługi Office 365.
- Microsoft wyśle powiadomienia.

![Firma Microsoft wysyła powiadomienie](./media/multi-factor-authentication-end-user-signin-app-notify/notify.png)


- Odpowiedz telefonu i naciśnij klawisz Weryfikuj.  Jeśli firma wymaga numeru PIN zostanie wyświetlony monit dla niego tutaj.

![Sprawdź](./media/multi-factor-authentication-end-user-signin-app-notify/phone.png)

![Konfiguracja](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

- Czy można teraz zalogować się.


## <a name="signing-in-with-the-microsoft-authenticator-app-using-verification-code"></a>Logowanie się przy użyciu aplikacji Microsoft Authenticator, używając kod weryfikacyjny

Następujące informacje będą opisano możliwości używania uwierzytelnianie wieloskładnikowe z aplikacją Microsoft Authenticator, gdy za pomocą kodu weryfikacyjnego.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Aby zalogować się przy użyciu kodu weryfikacyjnego z aplikacją Microsoft Authenticator

- Zaloguj się do aplikacji lub usługi, takie jak przy użyciu nazwy użytkownika i hasła usługi Office 365.
- Microsoft wyświetli monit o kod weryfikacyjny.

![Wprowadź kod weryfikacyjny](./media/multi-factor-authentication-end-user-signin-app-verify/verify.png)

- Otwórz aplikację Microsoft Authenticator na telefonie i wprowadź kod w polu miejsce, w którym rejestrujesz się.

![Pobieranie kodu](./media/multi-factor-authentication-end-user-signin-app-verify/phone.png)



- Czy można teraz zalogować się.


## <a name="signing-in-with-an-alternate-method"></a>Logowanie się przy użyciu alternatywna metoda


Następną sekcję procedurach pokazano, jak Zaloguj się przy użyciu alternatywna metoda po metodę podstawowego może nie być dostępna.

### <a name="to-sign-in-with-an-alternate-method"></a>Zaloguj się przy użyciu alternatywna metoda

- Zaloguj się do aplikacji lub usługi, takie jak przy użyciu nazwy użytkownika i hasła usługi Office 365.
- Zaznacz pole wyboru Użyj opcji różnych weryfikacji.  Będzie Prezentuj do wyboru różne opcje. Numer widoczny będzie podstawą ile masz konfiguracji.

![Użyj metody alternatywne](./media/multi-factor-authentication-end-user-signin-alt/alt.png)

- Wybierz pozycję alternatywna metoda i zaloguj się.
