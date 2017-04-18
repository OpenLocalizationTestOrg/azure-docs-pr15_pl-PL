<properties
    pageTitle="Przypisywanie użytkownika do ról administratora w wersji preview usługi Azure Active Directory | Microsoft Azure"
    description="Wyjaśniono, jak zmienić informacje administracyjne użytkownika w usłudze Active Directory platformy Azure"
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

# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory-preview"></a>Przypisywanie użytkownika do ról administratora w wersji preview usługi Azure Active Directory

W tym artykule wyjaśniono, jak przypisać użytkownikowi w podglądzie usługi Azure Active Directory (Azure AD) roli administracyjnej. [Co to jest w podglądzie?](active-directory-preview-explainer.md) Aby uzyskać informacji na temat dodawania nowych użytkowników w organizacji zobacz [Dodawanie nowych użytkowników do usługi Azure Active Directory](active-directory-users-create-azure-portal.md). Dodano użytkowników nie masz uprawnień administratora domyślnie, ale można przypisywać role do nich w dowolnym momencie.

## <a name="assign-a-role-to-a-user"></a>Przypisywanie roli do użytkownika

1.  Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta, który jest administratorem globalnym katalogu.

2.  Wybierz pozycję **więcej usług**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie naciśnij **klawisz Enter**.

    ![Zarządzanie użytkownikami otwierający](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)

3.  Na karta **Użytkownicy i grupy** zaznacz **wszystkich użytkowników**.

    ![Otwieranie wszystkich karta użytkowników](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)

4. Na karta **użytkowników i grup — wszystkich użytkowników** wybierz użytkownika z listy.

5. Na karta dla wybranego użytkownika wybierz **rolę katalogu**, a następnie przypisać do roli użytkownika z listy **roli katalogu** . Aby uzyskać więcej informacji dotyczących ról użytkownik i administrator zobacz [Przypisywanie ról administratora w Azure AD](active-directory-assign-admin-roles.md).

      ![Przypisywanie użytkowników do roli](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)

6. Wybierz przycisk **Zapisz**.


## <a name="whats-next"></a>Co to jest dalej

- [Dodawanie użytkownika](active-directory-users-create-azure-portal.md)
- [Resetowanie hasła użytkownika w nowy portal Azure](active-directory-users-reset-password-azure-portal.md)
- [Zmienianie informacji o pracy użytkownika](active-directory-users-work-info-azure-portal.md)
- [Zarządzanie profilami użytkowników](active-directory-users-profile-azure-portal.md)
- [Usuwanie użytkownika w swojej Azure AD](active-directory-users-delete-user-azure-portal.md)
