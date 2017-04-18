<properties
   pageTitle="Kreator zabezpieczeń Azure AD uprzywilejowanych Zarządzanie tożsamościami"
   description="Korzystanie z rozszerzeniem Azure Active Directory uprzywilejowanych Zarządzanie tożsamościami po raz pierwszy zostanie wyświetlona za pomocą Kreatora zabezpieczeń. W tym artykule opisano kroki korzystania z kreatora."
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
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="the-azure-ad-privileged-identity-management-security-wizard"></a>Kreator zabezpieczeń Azure AD uprzywilejowanych Zarządzanie tożsamościami

Jeśli jesteś pierwszej osoby, aby uruchomić Azure uprzywilejowanych tożsamości zarządzania (PIM) dla swojej organizacji, zostanie wyświetlona za pomocą kreatora. Kreator pomoże Ci zrozumienie zagrożeń uprzywilejowanych tożsamości i jak używać PIM w celu zmniejszenia tych czynników ryzyka. Nie musisz wprowadzić zmiany do istniejących przypisań ról w kreatorze, aby później.

## <a name="what-to-expect"></a>Czego można oczekiwać

Przed rozpoczęciem organizacji przy użyciu PIM wszystkie przypisania ról są trwałe: użytkowników są zawsze zadaniami o tych rolach nawet wtedy, gdy nie potrzebują one obecnie ich uprawnień.  Pierwszy krok kreatora zawiera listę ról wysokich uprawnieniach i ilu użytkowników znajdują się w tych ról. Rozwijanie określonym role, aby dowiedzieć się więcej informacji o użytkownikach, jeśli taka lub jeden z nich są nieznane.

Drugi krok kreatora umożliwia zmienić przypisania roli administratora.  

> [AZURE.WARNING]Należy pamiętać, że użytkownik ma co najmniej jednego administratora globalnego i więcej niż jeden administrator uprzywilejowanych roli za pomocą konta organizacji (nie konto Microsoft). Jeśli istnieje tylko jedna uprzywilejowanych roli administratora, organizacji nie będą mogli zarządzać PIM, usunięcie tego konta.
> Ponadto przypisania roli Zachowaj stałe, jeśli użytkownik ma konto Microsoft (konta używanego do zalogowania się do usług firmy Microsoft, takich jak Skype i Outlook.com). Jeśli planujesz wymagają MFA aktywacji dla danej roli, tego użytkownika zostaną zablokowane.


Po wprowadzeniu zmian kreatora będzie już widoczny. Przy następnym lub innego uprzywilejowanych roli administratora za pomocą PIM, zostanie wyświetlony PIM pulpitu nawigacyjnego.  

- Jeśli chcesz dodać lub usunąć użytkowników z ról lub zmienianie przydziałów z trwały do odpowiedniej, Dowiedz się więcej, jak [dodać lub usunąć rolę użytkownika](active-directory-privileged-identity-management-how-to-add-role-to-user.md).
- Jeśli chcesz udzielić dostępu do zarządzania PIM użytkownikom więcej, przeczytaj więcej w [jak udzielić dostępu do zarządzania w PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).



## <a name="next-steps"></a>Następne kroki
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
