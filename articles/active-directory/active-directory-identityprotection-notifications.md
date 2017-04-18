<properties
    pageTitle="Powiadomienia Azure Active Directory ochronę tożsamości | Microsoft Azure"
    description="Dowiedz się, obsługi działań dochodzenia powiadomienia."
    services="active-directory"
    keywords="ochrona tożsamości usługi Azure active directory chmury aplikacji odnajdowanie, zarządzanie aplikacji, zabezpieczeń, czynnik ryzyka, poziom ryzyka, luka w zabezpieczeniach, zasady zabezpieczeń"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-notifications"></a>Powiadomienia Azure Active Directory ochronę tożsamości 


Azure ochronę tożsamości AD wysyła dwa typy wiadomości e-mail z powiadomieniem automatyczną ułatwiające zarządzanie ryzykiem użytkownika i zdarzenia ryzyka:

- Alert e-mail zostało naruszone użytkownika

- Wiadomość e-mail z podsumowaniem tygodniowy

## <a name="user-compromised-alert-email"></a>Alert e-mail zostało naruszone użytkownika

Alert e-mail złamany użytkownika jest generowany podczas ochronę Azure AD tożsamości identyfikuje konto jako bezpieczne. Wiadomość e-mail zawiera łącze umożliwiające użytkownikom flagą raportu ryzyka na pulpicie nawigacyjnym ochronę tożsamości. Zaleca się natychmiast badanie powiadomienia o zostało naruszone.


## <a name="weekly-digest-email"></a>Wiadomość e-mail z podsumowaniem tygodniowy

Tygodniowy wiadomość e-mail z podsumowaniem zawiera podsumowanie nowego zdarzenia ryzyka.<br>
Zawiera:

- Użytkownicy na ryzyko
- Podejrzanych działań
- Wykryto luk
- Łącza do powiązanych raportów w ochronę tożsamości


<br>
![Rozwiązywanie problemu](./media/active-directory-identityprotection-notifications/400.png "Remediation")
<br> 

Możesz przełączać się wysyłanie tygodniowy wiadomość e-mail z podsumowaniem.
<br><br>
![Czynniki ryzyka użytkownika](./media/active-directory-identityprotection-notifications/62.png "User risks")
<br>
 

**Aby otworzyć okno dialogowe elementami konfiguracji**:

1. Na karta **ochronę Azure AD tożsamości** kliknij przycisk **Ustawienia**.
<br><br>
![Zasady użytkownika ryzyka](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
<br>

2. W sekcji **Ogólne** kliknij pozycję **powiadomienia**.
<br><br>
![Zasady użytkownika ryzyka](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
<br>




## <a name="see-also"></a>Zobacz też

- [Ochrona tożsamości Azure Active Directory](active-directory-identityprotection.md) 

