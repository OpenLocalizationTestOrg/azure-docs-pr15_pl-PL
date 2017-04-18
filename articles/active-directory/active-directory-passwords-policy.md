<properties
    pageTitle="Zasady haseł i ograniczenia usługi Azure Active Directory | Microsoft Azure"
    description="W tym artykule opisano zasady, które dotyczą w usługi Azure Active Directory, takich jak dozwolonych znaków, długość i wygasania hasła"
  services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Zasady haseł i ograniczenia usługi Azure Active Directory

Ten artykuł zawiera opis zasady haseł i złożoności skojarzone z kont użytkowników przechowywanych w katalogu Azure AD.

> [AZURE.IMPORTANT] **Czy jesteś tutaj, ponieważ występują problemy z logowaniem?** Jeśli tak, [Oto jak można zmienić i zresetować swoje hasło](active-directory-passwords-update-your-own-password.md).

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>Zasady UserPrincipalName, które dotyczą wszystkich kont użytkowników

Co konto użytkownika, które należy zalogować się do systemu uwierzytelniania Azure AD musi zawierać wartość atrybutu główna nazwa (UPN) unikatowego użytkownika związane z tym kontem. W poniższej tabeli konspektów zasady, które dotyczą obu lokalnych kont użytkowników powierzając jej ich konserwację usługi Active Directory (synchronizowane w chmurze) oraz do kont użytkowników tylko do chmury.

|   Właściwość           |     Wymagania dotyczące UserPrincipalName  |
|   ----------------------- |   ----------------------- |
|  Znaki są dozwolone    |  <ul> <li>A – Z</li> <li>-z </li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
|  Znaki nie są dozwolone  | <ul> <li>Dowolny '@' znak, który nie jest oddzielając nazwy użytkownika z domeny.</li> <li>Nie może zawierać znak kropki ".' poprzedzającego '@' symbol</li></ul> |
| Ograniczenia długości  |       <ul> <li>Całkowity czas trwania nie może przekraczać 113 znaków</li><li>64 znaki przed ‘@’ symbol</li><li>48 znaków po ‘@’ symbol</li></ul>

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Zasady haseł, które mają zastosowanie jedynie do kont użytkowników w chmurze

W poniższej tabeli opisano ustawienia zasad dostępne haseł, które mogą być stosowane do kont użytkowników, które są tworzone i zarządzane w Azure AD.

|  Właściwość       |    Wymagania dotyczące          |
|   ----------------------- |   ----------------------- |
|  Znaki są dozwolone   |   <ul><li>A – Z</li><li>-z </li><li>0 – 9</li> <li>@# $ % ^ & * - _ ! + = {} [] & #124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
|  Znaki nie są dozwolone   |       <ul><li>Znaki Unicode</li><li>Spacje</li><li> **Tylko silnych haseł**: nie może zawierać znak kropki ".' poprzedzającego '@' symbol</li></ul> |
|   Ograniczenia haseł | <ul><li>8 znaków minimalną i maksymalną 16 znaków</li><li>**Tylko silnych haseł**: wymaga 3 na 4 z następujących czynności:<ul><li>Małe litery</li><li>Wielkie litery</li><li>Cyfry (0 – 9)</li><li>Symbole (patrz powyżej ograniczeń hasła)</li></ul></li></ul> |
| Czas trwania wygasania haseł      | <ul><li>Wartość domyślna: **90** dni </li><li>Wartość jest można konfigurować za pomocą polecenia cmdlet Set-MsolPasswordPolicy z Azure Active Directory modułu dla programu Windows PowerShell.</li></ul> |
| Powiadomienie o wygaśnięciu hasła |  <ul><li>Wartość domyślna: **14** dni (przed wygaśnięciem hasła)</li><li>Wartość jest można konfigurować za pomocą polecenia cmdlet Set-MsolPasswordPolicy.</li></ul> |
| Wygasanie haseł |  <ul><li>Wartość domyślna: **FAŁSZ** dni (oznacza, że wygaśnięcie hasła jest włączona) </li><li>Wartość można skonfigurować dla poszczególnych kont użytkowników przy użyciu polecenia cmdlet Set-MsolUser. </li></ul> |
|  Historii haseł  | Nie można ponownie użyć ostatnie hasło. |
|  Czas trwania historii hasła | Zawsze |
|  Blokada konta | Po 10 niepomyślnego logowania próbach (niepoprawnego hasła) użytkownik będzie zablokowane przez minutę. Dalsze niepoprawnych próbach logowania zostanie zablokowane użytkownika na zwiększenie czasu trwania. |


## <a name="next-steps"></a>Następne kroki

* **Czy jesteś tutaj, ponieważ występują problemy z logowaniem?** Jeśli tak, [Oto jak można zmienić i zresetować swoje hasło](active-directory-passwords-update-your-own-password.md).
* [Zarządzanie hasła z dowolnego miejsca](active-directory-passwords.md)
* [Jak działa Zarządzanie hasłami](active-directory-passwords-how-it-works.md)
* [Wprowadzenie do zarządzania usługą hasła](active-directory-passwords-getting-started.md)
* [Dostosowywanie zarządzania hasłami](active-directory-passwords-customize.md)
* [Najważniejsze wskazówki dotyczące zarządzania hasła](active-directory-passwords-best-practices.md)
* [Jak uzyskać operacyjne wniosków z raportów zarządzania hasła](active-directory-passwords-get-insights.md)
* [Zarządzanie hasłami — często zadawane pytania](active-directory-passwords-faq.md)
* [Rozwiązywanie problemów z zarządzaniem hasła](active-directory-passwords-troubleshoot.md)
* [Dowiedz się więcej](active-directory-passwords-learn-more.md)
