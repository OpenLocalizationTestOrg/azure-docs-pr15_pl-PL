<properties
    pageTitle="Logowania napotkania Azure AD tożsamości chroniona za pomocą | Microsoft Azure"
    description="Omówienie interfejsu użytkownika po ochronę tożsamości ma nieco ograniczane lub wyeliminować użytkownika lub gdy uwierzytelnianie wieloskładnikowe jest wymagane przez zasady."
    services="active-directory"
    keywords="ochrona tożsamości usługi Azure active directory chmury aplikacji odnajdowanie, zarządzanie aplikacji, zabezpieczeń, czynnik ryzyka, poziom ryzyka, luka w zabezpieczeniach, zasady zabezpieczeń"
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
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Środowiska logowania z ochroną Azure AD tożsamości

Za pomocą Azure Active Directory ochronę tożsamości można:

- wymaganie od użytkowników zarejestrować uwierzytelniania wieloskładnikowego

- ryzykowna rejestrowaniu i złamany użytkowników

Odpowiedź systemu tych problemów ma wpływ na użytkownika logowanie, ponieważ tylko bezpośrednio rejestrowanie się w przez podanie nazwy użytkownika i hasła nie będzie można już. Dodatkowe czynności są wymagane uzyskanie bezpiecznie użytkownika do firm.

W tym temacie przedstawiono omówienie interfejsu logowania użytkownika we wszystkich przypadkach, które mogą wystąpić.

**Uwierzytelnianie wieloskładnikowe**

- Uwierzytelnianie wieloskładnikowe rejestracji



**Logowania na ryzyko**

- Ryzykowna odzyskiwania logowania

- Ryzykowna logowania zablokowane

- Uwierzytelnianie wieloskładnikowe rejestracji podczas ryzykowna logowania
 

**Użytkownik ryzyka**

- Odzyskiwanie skierowanego na to konto

- Skierowanego na to konto zablokowane




## <a name="multi-factor-authentication-registration"></a>Uwierzytelnianie wieloskładnikowe rejestracji

Najlepszego środowiska pracy użytkownika dla obu przepływu odzyskiwania skierowanego na to konto i ryzykowna przepływu logowania, to gdy użytkownik może samodzielnie odzyskać. Jeśli użytkownicy są zarejestrowane uwierzytelniania wieloskładnikowego, mają numer telefonu skojarzony z kont, który może być używany w celu przekazania problemach zabezpieczeń. Zaangażowanie nie pomocy technicznej lub administrator jest potrzebny do odzyskiwanie z złamania konta. W związku z tym ma zalecane uzyskanie użytkowników zarejestrowanych uwierzytelniania wieloskładnikowego. 

Administratorzy mogą:

- Ustawianie zasad, która wymaga użytkownikom skonfigurowanie kont w celu zwiększenia bezpieczeństwa weryfikacji. 
- w przypadku, gdy chce przekazać użytkownikom okres prolongaty przed zarejestrowaniem umożliwia pomijanie rejestracji uwierzytelnianie wieloskładnikowe do 30 dni.

**Rejestracja uwierzytelnianie wieloskładnikowe składa się z trzech kroków:**

1. W pierwszym kroku użytkownik otrzymuje powiadomienie o wymaganiach w celu konfigurowania konta dla uwierzytelnianie wieloskładnikowe. 

    ![Rozwiązywanie problemu] (./media/active-directory-identityprotection-flows/140.png "Rozwiązywanie problemu")


2. Aby skonfigurować uwierzytelnianie wieloskładnikowe, należy pozostawić wiedzieć, jak chcesz się skontaktować.

    ![Rozwiązywanie problemu] (./media/active-directory-identityprotection-flows/141.png "Rozwiązywanie problemu")
 
3. System przesyła trudne do Ciebie musisz odpowiedzieć.

    ![Rozwiązywanie problemu] (./media/active-directory-identityprotection-flows/142.png "Rozwiązywanie problemu")

 



## <a name="risky-sign-in-recovery"></a>Ryzykowna odzyskiwania logowania

Gdy administrator skonfigurował zasady logowania ryzyka, odpowiednich użytkowników są odpowiednie powiadomienie podczas próby logowania. 

**Ryzykowna przepływ logowania występują dwie czynności:** 

1. Użytkownik zostanie poinformowany, że wykryto coś nietypowego o ich logowania, takich jak logowanie się w nowej lokalizacji, urządzenie lub aplikacji. 

    ![Rozwiązywanie problemu] (./media/active-directory-identityprotection-flows/120.png "Rozwiązywanie problemu")

2. Użytkownik musi potwierdzić swoją tożsamość przy rozwiązywaniu wezwanie zabezpieczeń. Jeśli użytkownik jest zarejestrowany uwierzytelniania wieloskładnikowego muszą przesyłania danych Kod zabezpieczeń, aby jej numer telefonu. Ponieważ jest to tylko ryzykowna Zaloguj się i nie skierowanego na to konto użytkownika nie trzeba zmienić hasło w tego przepływu. 

    ![Rozwiązywanie problemu] (./media/active-directory-identityprotection-flows/121.png "Rozwiązywanie problemu")



 
## <a name="risky-sign-in-blocked"></a>Ryzykowna logowania zablokowane
Administratorzy można także ustawić zasady logowania ryzyka Blokowanie użytkowników po logowania w zależności od poziomu ryzyka. Aby uzyskać odblokowane, użytkownicy końcowi musi skontaktuj się administratora lub pomocy technicznej lub mogą one spróbuj się zalogować z znanych lokalizacji lub urządzenia. Własny odzyskiwanie przy rozwiązywaniu uwierzytelnianie wieloskładnikowe nie jest w tym przypadku.

![Rozwiązywanie problemu] (./media/active-directory-identityprotection-flows/200.png "Rozwiązywanie problemu")




## <a name="compromised-account-recovery"></a>Odzyskiwanie skierowanego na to konto

Jeśli skonfigurowano zasady zabezpieczeń ryzyka użytkownika, użytkownicy, którzy spełniają użytkownika ryzyka poziom określony w zasadach (a więc są uznawane za zostało naruszone) musi przejść przepływu odzyskiwania złamania użytkownika, zanim oni mogli logować się. 

**Przepływ odzyskiwania złamania użytkownika występują trzy kroki:**

1. Użytkownik jest informowany, że ich zabezpieczenia konta na ryzyko ze względu na podejrzane działania lub przecieku poświadczeń.

    ![Rozwiązywanie problemu] (./media/active-directory-identityprotection-flows/101.png "Rozwiązywanie problemu")

2.  Użytkownik musi potwierdzić swoją tożsamość przy rozwiązywaniu wezwanie zabezpieczeń. Użytkownik jest zarejestrowany uwierzytelniania wieloskładnikowego można samodzielnie odzyskać przed ich wykorzystaniem. Musisz przesłanie kod zabezpieczeń, aby jej numer telefonu. 

    ![Rozwiązywanie problemu] (./media/active-directory-identityprotection-flows/110.png "Rozwiązywanie problemu")


3.  Ponadto użytkownik jest musieli zmieniać swoje hasła, ponieważ innej osoby może mieć dostęp do swojego konta. Zrzuty ekranu tego doświadczenia są poniżej.
 
    ![Rozwiązywanie problemu] (./media/active-directory-identityprotection-flows/111.png "Rozwiązywanie problemu")



## <a name="compromised-account-blocked"></a>Skierowanego na to konto zablokowane 

Aby uzyskać użytkownika, którego została zablokowana przez zasady zabezpieczeń użytkownika ryzyka odblokowana, użytkownik musi skontaktować się z administratorem lub pomocy technicznej. Własny odzyskiwanie przy rozwiązywaniu uwierzytelnianie wieloskładnikowe nie jest w tym przypadku.


![Rozwiązywanie problemu] (./media/active-directory-identityprotection-flows/104.png "Rozwiązywanie problemu")



 
## <a name="reset-password"></a>Resetowanie hasła

Zablokowanie złamany użytkowników z zalogowaniem się administrator mogą powodować zwrócenie hasło tymczasowe dla nich. Użytkownicy będą musieli zmieniać swoje hasła podczas następnego logowania.

![Rozwiązywanie problemu] (./media/active-directory-identityprotection-flows/160.png "Rozwiązywanie problemu")


 




 

## <a name="see-also"></a>Zobacz też

- [Ochrona tożsamości Azure Active Directory](active-directory-identityprotection.md) 