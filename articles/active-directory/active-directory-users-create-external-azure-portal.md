<properties
    pageTitle="Dodawanie użytkowników z innych katalogów lub partnera firmy w podglądzie usługi Azure Active Directory | Microsoft Azure"
    description="Wyjaśniono, jak dodawać użytkowników i zmienianie informacji o użytkownikach w usługi Azure Active Directory, łącznie z użytkownikami zewnętrznymi i Gość."
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

# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory-preview"></a>Dodawanie użytkowników z innych katalogów lub partnera firmy w podglądzie usługi Azure Active Directory

> [AZURE.SELECTOR]
- [Azure portal](active-directory-users-create-external-azure-portal.md)
- [Portal Azure klasyczny](active-directory-create-users-external.md)

W tym artykule wyjaśniono, jak dodawać użytkowników z innych katalogów w podglądzie usługi Azure Active Directory (Azure AD) lub z partnerem firmy. [Co to jest w podglądzie?](active-directory-preview-explainer.md) Aby uzyskać informacje na temat dodawania nowych użytkowników w organizacji oraz dodawania użytkowników, którzy mają konta serwera Microsoft zobacz [Dodawanie nowych użytkowników do usługi Azure Active Directory](active-directory-users-create-azure-portal.md). Dodano użytkowników nie masz uprawnień administratora domyślnie, ale można przypisywać role do nich w dowolnym momencie.

## <a name="add-a-user"></a>Dodawanie użytkownika

1.  Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta, który jest administratorem globalnym katalogu.

2.  Wybierz pozycję **więcej usług**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie naciśnij **klawisz Enter**.

    ![Otwieranie, zarządzanie użytkownikami](./media/active-directory-users-create-external-azure-portal/create-users-user-management.png)

3.  Na karta **Użytkownicy i grupy** zaznacz **użytkowników**, a następnie wybierz pozycję **Dodaj**.

    ![Wybranie polecenia Dodaj](./media/active-directory-users-create-external-azure-portal/create-users-add-command.png)

4. Na karta **użytkownika** Podaj nazwę wyświetlaną w polu **Nazwa** i logowania nazwę użytkownika w polu **Nazwa użytkownika**.

5. Skopiuj lub w przeciwnym razie należy zwrócić uwagę wygenerowane hasła tak, aby można go udostępnić użytkownikowi po ukończeniu tego procesu.

6. Opcjonalnie wybierz **profil** , aby najpierw dodać użytkowników i nazwisko, stanowisko i nazwa działu.
    
    ![Otwieranie profilu użytkownika](./media/active-directory-users-create-external-azure-portal/create-users-user-profile.png)

    - Wybierz pozycję **grupy** , aby dodać użytkownika do jednej lub kilku grup.

        ![Dodawanie użytkownika do grupy](./media/active-directory-users-create-external-azure-portal/create-users-user-groups.png)

    - Wybierz **rolę organizacji** , aby przypisać użytkownikowi rolę z listy **ról** . Aby uzyskać więcej informacji dotyczących ról użytkownik i administrator zobacz [Przypisywanie ról administratora w Azure AD](active-directory-assign-admin-roles.md).

        ![Przypisywanie użytkowników do roli](./media/active-directory-users-create-external-azure-portal/create-users-assign-role.png)

7. Wybierz polecenie **Utwórz**.

8. Bezpieczne rozpowszechnianie wygenerowane hasło do nowego użytkownika tak, aby się zalogować użytkownika.

> [AZURE.IMPORTANT] Jeśli organizacja korzysta z więcej niż jedną domenę, po dodaniu konta użytkownika należy wiedzieć o następujących problemów:
>
> - **Aby dodać konta użytkowników z tym samym głównej nazwy użytkownika (UPN) domen, dodać, na przykład** geoffgrisso@contoso.onmicrosoft.com, **następuje** geoffgrisso@contoso.com.
> - **Nie** dodawaj geoffgrisso@contoso.com przed dodaniem geoffgrisso@contoso.onmicrosoft.com. To zamówienie jest ważne, a może być kłopotliwe, aby cofnąć.

Jeśli zmienisz informacje dla użytkownika, którego tożsamości są synchronizowane z usługą Active Directory w lokalnej, nie można zmienić informacje o użytkowniku w portalu klasyczny Azure. Aby zmienić informacje o użytkowniku, użyj narzędzia do zarządzania z lokalnej usługi Active Directory.


## <a name="whats-next"></a>Co to jest dalej

- [Dodawanie użytkownika](active-directory-users-create-azure-portal.md)
- [Resetowanie hasła użytkownika w nowy portal Azure](active-directory-users-reset-password-azure-portal.md)
- [Przypisywanie użytkowników do roli w usługi Azure AD](active-directory-users-assign-role-azure-portal.md)
- [Zmienianie informacji o pracy użytkownika](active-directory-users-work-info-azure-portal.md)
- [Zarządzanie profilami użytkowników](active-directory-users-profile-azure-portal.md)
- [Usuwanie użytkownika w swojej Azure AD](active-directory-users-delete-user-azure-portal.md)
