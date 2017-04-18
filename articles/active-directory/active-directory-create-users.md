<properties
    pageTitle="Dodawanie nowych użytkowników do usługi Azure Active Directory | Microsoft Azure"
    description="Omówiono sposób dodawania nowych użytkowników lub zmienianie informacji o użytkownikach w usługi Azure Active Directory."
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
    ms.topic="get-started-article"
    ms.date="09/22/2016"
    ms.author="curtand"/>

# <a name="add-new-users--or-users-with-microsoft-accounts-to-azure-active-directory"></a>Dodawanie nowych lub użytkowników z kontami Microsoft usługi Azure Active Directory

Dodawanie użytkowników do zapełnienia katalogu. W tym artykule wyjaśniono sposób dodawania nowych użytkowników w organizacji oraz jak dodawać użytkowników, którzy mają konta serwera Microsoft. Aby uzyskać więcej informacji na temat dodawania użytkowników z innych katalogów w usłudze Azure Active Directory lub dodawanie użytkowników z partnerem firmy zobacz [Dodawanie użytkowników z innych katalogów lub partnera firmy w usłudze Azure Active Directory](active-directory-create-users-external.md). Dodano użytkowników nie masz uprawnień administratora domyślnie, ale można przypisywać role do nich w dowolnym momencie.

## <a name="add-a-user"></a>Dodawanie użytkownika

1. Zaloguj się do [portal Azure klasyczny](https://manage.windowsazure.com) przy użyciu konta administratora globalnego katalogu.
2. Zaznacz **Usługi Active Directory**, a następnie wybierz nazwę katalogu organizacji.
3. Wybierz kartę **użytkowników** , a następnie na pasku poleceń wybierz pozycję **Dodaj użytkownika**.
4. Na stronie **Przekaż nam o tym użytkowniku** w polu **Typ użytkownika**wybierz jedną z opcji:

    - **Nowy użytkownik w organizacji** — doda nowe konto użytkownika w katalogu.
    - **Użytkownik z istniejącego konta Microsoft** — dodaje istniejącego konta dla klientów indywidualnych Microsoft do katalogu (na przykład konto programu Outlook)

5. W zależności od **typu użytkownika**wprowadź nazwę użytkownika (w przypadku nowego użytkownika) lub adres e-mail (dla użytkowników za pomocą konta Microsoft).
6. Na stronie **profilu** użytkownika podać imię i nazwisko, nazwę przyjazne dla użytkownika i roli użytkownika z listy **ról** . Aby uzyskać więcej informacji dotyczących ról użytkownik i administrator zobacz [Przypisywanie ról administratora w Azure AD](active-directory-assign-admin-roles.md). Określ, czy **Włączyć uwierzytelnianie wieloskładnikowe** dla użytkownika.
7. Na stronie **Pobierz hasło tymczasowe** wybierz pozycję **Utwórz**.

> [AZURE.IMPORTANT] Jeśli organizacja korzysta z więcej niż jedną domenę, po dodaniu konta użytkownika należy wiedzieć o następujących problemów:
>
> - **Aby dodać konta użytkowników z tym samym głównej nazwy użytkownika (UPN) domen, dodać, na przykład** geoffgrisso@contoso.onmicrosoft.com, **następuje** geoffgrisso@contoso.com.
> - **Nie** dodawaj geoffgrisso@contoso.com przed dodaniem geoffgrisso@contoso.onmicrosoft.com. To zamówienie jest ważne, a może być kłopotliwe, aby cofnąć.

## <a name="change-user-information"></a>Zmień informacje o użytkowniku

Możesz zmienić dowolny atrybut użytkownika oprócz identyfikatora obiektu.

1. Otwórz katalog.
2. Wybierz kartę **użytkowników** , a następnie wybierz nazwę wyświetlaną użytkownika, którego chcesz zmienić.
3. Wprowadź zmiany, a następnie kliknij przycisk **Zapisz**.

Jeśli użytkownik, który chcesz zmienić jest synchronizowane z usługą Active Directory w lokalnej, nie można zmienić informacje o użytkowniku, za pomocą tej procedury. Aby zmienić użytkownika, użyj narzędzia do zarządzania z lokalnej usługi Active Directory.

## <a name="guest-user-management-and-limitations"></a>Zarządzanie użytkownikami gościa i ograniczenia

Konta gości korzystają z innych katalogów, które zostały zaproszone do katalogu, aby uzyskać dostęp do dokumentów programu SharePoint, aplikacji lub innych Azure zasobów. Konta gościa w katalogu ma jego źródłowych atrybut UserType ustawiony "Gość". Zwykli użytkownicy (w szczególności członkowie katalogu) mają atrybut UserType "Element".

Goście mają ograniczony zestaw uprawnień w katalogu. Tych praw ogranicza możliwość tworzenia gości do znalezienia informacji na temat innych użytkowników w katalogu. Jednak gościa użytkownicy nadal mogą korzystać z użytkowników i grup skojarzonych z zasobami, nad którymi pracuje. Użytkowników gości wykonywać następujące czynności:

- Zobacz inni użytkownicy i grupy skojarzony z subskrypcją usługi Azure, do którego są przydzielone
- Zobacz członkowie grup, do których należą
- Znajdowanie innych użytkowników w katalogu, jeśli wiedzą, pełny adres e-mail użytkownika
- Zobacz tylko ograniczony zestaw atrybutów użytkowników, dla których wyglądają w górę — ograniczone do wyświetlania nazwę, adres e-mail, głównej nazwy użytkownika (UPN) i miniaturę fotografii
- Uzyskiwanie listy zweryfikowanych domen w katalogu
- Wyraża zgodę na aplikacje, udzielanie im samej dostęp członkowie w katalogu

## <a name="set-guest-user-access-policies"></a>Ustawianie zasad dostępu użytkownika Gość

Karta **Konfigurowanie** katalogu zawiera opcji w celu kontrolowania dostępu użytkowników gości. Te opcje można zmienić tylko w portal Azure klasyczny przez administratora globalnego katalogu. Obecnie nie istnieje metoda programu PowerShell lub interfejsu API.

Aby otworzyć kartę **Konfiguruj** w portalu klasyczny Azure, wybierz **Usługi Active Directory**, a następnie wybierz nazwę katalogu.

![Konfigurowanie karty w usługi Azure Active Directory][1]

Opcje do kontrolowania dostępu użytkowników gości można edytować.

![Opcje kontroli dostępu dla gości][2]


## <a name="whats-next"></a>Co to jest dalej

- [Dodawanie użytkowników z innych katalogów lub partnera firmy w usłudze Active Directory platformy Azure](active-directory-create-users-external.md)
- [Administrowanie Azure AD](active-directory-administer.md)
- [Zarządzanie hasłami w Azure AD](active-directory-manage-passwords.md)
- [Zarządzanie grupami w Azure AD](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
