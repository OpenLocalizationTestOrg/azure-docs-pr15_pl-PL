<properties
    pageTitle="Azure Active Directory ochronę tożsamości — jak odblokować użytkowników | Microsoft Azure"
    description="Dowiedz się, jak odblokować użytkowników, które zostały zablokowane przez zasady Azure Active Directory ochronę tożsamości."
    services="active-directory"
    keywords="ochrona tożsamości usługi Azure active directory Odblokuj użytkownika"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory ochronę tożsamości — jak odblokować użytkowników

Zabezpieczonych Azure Active Directory tożsamości można skonfigurować zasady, aby zablokować użytkowników, jeśli skonfigurowane warunki są spełnione. Zazwyczaj użytkownika zablokowanych kontaktów pomocy technicznej zostać odblokowana. Ten tematy przedstawiono kroki, można wykonać, aby odblokować zablokowanych użytkownika.


## <a name="determine-the-reason-for-blocking"></a>Ustalanie przyczyny blokowania

Pierwszym krokiem do odblokowania użytkownika należy określić typ zasady, które zostało zablokowane użytkownika, ponieważ dalsze kroki zależą od go. Azure Active Directory ochronę tożsamości użytkownik może zostać albo zablokowana przez zasady logowania ryzyka lub zasady ryzyka użytkownika. 

Można uzyskać typu zasady, które zostało zablokowane użytkownikowi nagłówka w oknie dialogowym, które zostały przedstawione użytkownikowi podczas próby logowania:

|Zasady | Okno dialogowe użytkownika|
|--- | --- |
|Logowania ryzyka | ![Lista zablokowanych logowania](./media/active-directory-identityprotection-unblock-howto/02.png) |
|Użytkownik ryzyka | ![Zablokowanego konta](./media/active-directory-identityprotection-unblock-howto/104.png) |


Użytkownik, który jest zablokowany przez:

- Zasady logowania ryzyka jest nazywany podejrzanych logowania
- Zasady ryzyka użytkownika jest nazywany konta na ryzyko

 
## <a name="unblocking-suspicious-sign-ins"></a>Odblokowywanie podejrzanych rejestrowaniu

Aby odblokować podejrzanych logowania, masz następujące możliwości:

1. **Logowanie z lokalizacji znanych lub urządzenia** - typowe przyczyny zablokowanych podejrzanych rejestrowaniu są prób logowania od nieznanych lokalizacji lub urządzenia. Użytkownicy mogą szybko ustalić, czy jest przyczyny blokowania próbę logowania z lokalizacji znanych lub urządzenia.


3. **Wyklucz z zasad** — Jeśli uważasz, że bieżącej konfiguracji zasad logowania powoduje problemy dla określonych użytkowników, można wykluczyć użytkowników z niego. Zobacz [zasad logowania ryzyka](active-directory-identityprotection.md#sign-in-risk-policy) , aby uzyskać więcej informacji.
 
4. **Wyłączanie zasad** — Jeśli uważasz, że konfiguracji zasad powoduje problemy dla wszystkich użytkowników, możesz wyłączyć zasady. Zobacz [zasad logowania ryzyka](active-directory-identityprotection.md#sign-in-risk-policy) , aby uzyskać więcej informacji.


## <a name="unblocking-accounts-at-risk"></a>Odblokowywanie kont na ryzyko

Aby odblokować konto na ryzyko, masz następujące możliwości:

1. **Resetowanie hasła** — mogą resetować hasła użytkowników. Zobacz [Resetowanie ręcznego bezpieczne hasło](active-directory-identityprotection.md#manual-secure-password-reset) , aby uzyskać więcej informacji.

2. **Odrzuć wszystkie zdarzenia ryzyka** — zasady użytkownika ryzyka blokuje użytkownika, jeśli osiągnięto poziom ryzyka użytkownik skonfigurowany dla blokować dostęp. Możesz zmniejszyć użytkownika jest poziom ryzyka ręcznie zamykając zgłoszone zdarzenia ryzyka. Aby uzyskać więcej informacji zobacz [ręczne zamknięcie zdarzenia ryzyka](active-directory-identityprotection.md#closing-risk-events-manually).

3. **Wyklucz z zasad** — Jeśli uważasz, że bieżącej konfiguracji zasad logowania powoduje problemy dla określonych użytkowników, można wykluczyć użytkowników z niego. Zobacz [zasady ryzyka użytkownika](active-directory-identityprotection.md#user-risk-policy) , aby uzyskać więcej informacji.
 
4. **Wyłączanie zasad** — Jeśli uważasz, że konfiguracji zasad powoduje problemy dla wszystkich użytkowników, możesz wyłączyć zasady. Zobacz [zasady ryzyka użytkownika](active-directory-identityprotection.md#user-risk-policy) , aby uzyskać więcej informacji.




## <a name="next-steps"></a>Następne kroki

 Czy chcesz dowiedzieć się więcej na temat ochrony Azure AD tożsamości? Zapoznaj się z [Azure Active Directory ochronę tożsamości](active-directory-identityprotection.md).
 

