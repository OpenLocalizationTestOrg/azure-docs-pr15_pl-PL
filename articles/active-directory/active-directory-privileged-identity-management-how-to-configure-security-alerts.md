<properties
   pageTitle="Jak skonfigurować alerty zabezpieczeń | Microsoft Azure"
   description="Dowiedz się, jak skonfigurować alertów zabezpieczeń dla rozszerzenia Azure uprzywilejowanych Zarządzanie tożsamościami."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/02/2016"
   ms.author="kgremban"/>

# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Jak skonfigurować alerty zabezpieczeń w Azure AD uprzywilejowanych Zarządzanie tożsamościami

## <a name="security-alerts"></a>Alerty zabezpieczeń
Azure uprzywilejowanych tożsamości zarządzania (PIM) generuje alerty w przypadku podejrzanych lub niebezpiecznych w środowisku usługi. Po uruchomieniu alertu go wyświetlane na pulpicie nawigacyjnym PIM. Zaznacz alert, aby wyświetlić raport, który zawiera listę użytkowników lub role, których dotyczy alert.

![Alerty zabezpieczeń PIM pulpitu nawigacyjnego — zrzut ekranu][1]



| Alert | Wyzwalacza | Zalecenia |
| ----- | ------- | -------------- |
| **Role są przypisywane poza PIM** | Administrator trwale został przypisany do roli, poza interfejsu PIM. | Przejrzyj nowe przypisanie roli. W związku z innymi usługami można przypisać tylko administratorzy trwały, go do odpowiedniej przypisania w razie potrzeby zmienić. |
| **Role są zbyt częste aktywowania** | Można było zbyt wiele reactivations tę samą rolę terminie w obszarze Ustawienia. | Skontaktuj się z użytkownikiem, aby sprawdzić, dlaczego ich została aktywowana roli tak wielokrotnie. Być może limit czasu jest zbyt krótka dla ich do wykonywania swoich zadań lub być może używasz skryptów automatycznie aktywować roli. |
| **Role nie wymagają uwierzytelnianie wieloskładnikowe aktywacji** | Istnieje ról bez MFA włączone w obszarze Ustawienia. | Firma Microsoft wymaga MFA najbardziej wysoce uprzywilejowanych ról, ale zdecydowanie zaleca się włączenie MFA aktywacji wszystkich ról. |
| **Administratorzy nie używasz ich uprzywilejowanych ról** | Istnieje odpowiedniej administratorów, które nie zostały aktywowane ostatnio ich role. | Rozpocznij przeglądu programu access, aby określić użytkowników, których nie potrzebujesz programu access. |
| **Zbyt wiele administratorów globalnych** | Istnieje administratorów globalnych niż zalecane. | Jeśli masz dużą liczbę globalny Administratorzy jest prawdopodobieństwo, że użytkownicy uzyskują uprawnienia więcej niż jest to wymagane. Przenoszenie użytkowników do roli mniej uprzywilejowanych lub upewnij niektóre z nich przyznania roli zamiast trwale przypisana. |

## <a name="configure-security-alert-settings"></a>Konfigurowanie ustawień alertów zabezpieczeń

Możesz dostosować niektóre alerty zabezpieczeń w PIM do pracy z środowiska i zabezpieczeniami. Wykonaj poniższe czynności, aby osiągnąć karta Ustawienia:

1. Zaloguj się do [portalu Azure](https://portal.azure.com/) i zaznacz opcję Podziel **Azure AD uprzywilejowanych Zarządzanie tożsamościami** z pulpitu nawigacyjnego.
2. Wybierz pozycję **zarządzane uprzywilejowanych role** > **Ustawienia** > **ustawień alertów**.

    ![Przejdź do ustawień alertów zabezpieczeń][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>Alert "Role są aktywowana zbyt częste"

Ten alert uaktywnia, jeśli użytkownik aktywuje tę samą rolę uprzywilejowanych wiele razy w określonym czasie. Można konfigurować okres i liczbę aktywacji.

- **Aktywacja, okres odnawiania**: Określ w dniach, godzin, minut, a drugi przedział czasu, którego chcesz użyć do śledzenia podejrzanych odnowienia.

- **Liczba aktywacji odnowienia**: Określ liczbę aktywacji od 2 do 100, można uznać warta alert w ramach czasowych wybierzesz. Można to zmienić ustawienie za pomocą suwaka lub wpisując liczbę w polu tekstowym.


### <a name="there-are-too-many-global-administrators-alert"></a>Alert "są zbyt wielu administratorów globalnego"

PIM uaktywnia ten alert, jeśli są spełnione dwa różne kryteria, a następnie można skonfigurować jedną z nich. Należy najpierw do osiągnięcia określonej wartości progowej administratorów globalnych. Drugi określonego procentu przypisania roli całkowita musi być administratorów globalnych. Jeśli tylko spełnia jednego z tych pomiarów, alert nie jest wyświetlana.  

- **Minimalna liczba administratorów globalnego**: Określ liczbę administratorów globalnego, od 2 do 100, należy rozważyć kwotę niebezpieczne.

- **Wartość procentową, administratorów globalnego**: określić stopień administratorów, którzy mają Administratorzy globalny, od 0% do 100%, czyli niebezpiecznych w środowisku.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>Alert "Administratorzy nie używasz ich uprzywilejowanych role"

Ten alert uaktywnia, jeśli użytkownik ma określoną ilość czasu bez aktywowania roli.

- **Liczba dni**: Określ liczbę dni od 0 do 100, przechodziły przez użytkownika bez aktywowania roli.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png
