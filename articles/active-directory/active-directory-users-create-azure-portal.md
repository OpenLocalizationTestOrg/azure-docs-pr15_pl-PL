<properties
    pageTitle="Dodawanie nowych użytkowników do usługi Azure Active Directory Podgląd | Microsoft Azure"
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
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="add-new-users-to-azure-active-directory-preview"></a>Dodawanie nowych użytkowników do usługi Azure Active Directory preview

> [AZURE.SELECTOR]
- [Azure portal](active-directory-users-create-azure-portal.md)
- [Portal Azure klasyczny](active-directory-create-users.md)

W tym artykule opisano sposób dodawania nowych użytkowników w organizacji w podglądzie Azure Active Direstory (Azure AD). [Co to jest w podglądzie?](active-directory-preview-explainer.md)

1.  Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta, który jest administratorem globalnym katalogu.

2.  Wybierz pozycję **więcej usług**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie naciśnij **klawisz Enter**.

    ![Otwieranie, zarządzanie użytkownikami](./media/active-directory-users-create-azure-portal/create-users-user-management.png)

3.  Na karta **Użytkownicy i grupy** zaznacz **wszystkich użytkowników**, a następnie wybierz pozycję **Dodaj**.

    ![Wybranie polecenia Dodaj](./media/active-directory-users-create-azure-portal/create-users-add-command.png)

4.  Wprowadź szczegółowe informacje dla użytkownika, takie jak **Nazwa** i **Nazwa użytkownika**. Fragment nazwy domeny, nazwy użytkownika musi być początkowej domyślnej nazwy domeny nazwy domeny "foo.onmicrosoft.com" lub nazwę domeny zweryfikowane, niefederacyjnej, takie jak "contoso.com".

5. Skopiuj lub w przeciwnym razie należy zwrócić uwagę wygenerowane hasła tak, aby można go udostępnić użytkownikowi po ukończeniu tego procesu.

6. Opcjonalnie można otwierać i Wypełnij informacje w karta **profilu** , karta **grupy** lub karta **roli katalogów** dla użytkownika. Aby uzyskać więcej informacji dotyczących ról użytkownik i administrator zobacz [Przypisywanie ról administratora w Azure AD](active-directory-assign-admin-roles.md).

7.  Na karta **użytkownika** wybierz pozycję **Utwórz**.

8. Bezpieczne rozpowszechnianie wygenerowane hasło do nowego użytkownika tak, aby się zalogować użytkownika.

## <a name="whats-next"></a>Co to jest dalej

- [Dodawanie użytkowników zewnętrznych](active-directory-users-create-external-azure-portal.md)
- [Resetowanie hasła użytkownika w nowy portal Azure](active-directory-users-reset-password-azure-portal.md)
- [Zmienianie informacji o pracy użytkownika](active-directory-users-work-info-azure-portal.md)
- [Zarządzanie profilami użytkowników](active-directory-users-profile-azure-portal.md)
- [Usuwanie użytkownika w swojej Azure AD](active-directory-users-delete-user-azure-portal.md)
- [Przypisywanie użytkowników do roli w usługi Azure AD](active-directory-users-assign-role-azure-portal.md)
