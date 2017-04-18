<properties
    pageTitle="Przypisywanie użytkownika lub grupy do aplikacji przedsiębiorstwa w podglądzie usługi Azure Active Directory | Microsoft Azure"
    description="Jak zaznaczyć aplikacji przedsiębiorstwa przypisać użytkownikowi lub grupie do niego w usłudze Active Directory platformy Azure"
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
    ms.date="10/03/2016"
    ms.author="curtand"/>

# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory-preview"></a>Przypisywanie użytkownika lub grupy do aplikacji przedsiębiorstwa w podglądzie usługi Azure Active Directory

Jest proste przypisać użytkownika lub grupę do aplikacji w przedsiębiorstwie w podglądzie usługi Azure Active Directory (Azure AD). [Co to jest w podglądzie?](active-directory-preview-explainer.md) Musi mieć odpowiednie uprawnienia do zarządzania aplikacji przedsiębiorstwa. W bieżącym podglądzie, musisz być administratorem globalnym katalogu.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>Jak przypisać użytkownikowi dostępu do aplikacji enterprise?

1. Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta, który jest administratorem globalnym katalogu.

2. Wybierz pozycję **więcej usług**, wprowadź usługi Azure Active Directory w polu tekstowym, a następnie naciśnij **klawisz Enter**.

3. Na *directoryname *Usługi Azure Active Directory — ** ** karta (czyli Azure AD karta katalogu, w którym zarządzasz), wybierz pozycję **przedsiębiorstwo aplikacji **.

    ![Otwieranie aplikacji przedsiębiorstwa](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)

4. Na karta **aplikacji przedsiębiorstwa** zaznacz **wszystkie aplikacje**. Zobaczysz listę aplikacji, które można zarządzać.

5. Na karta **aplikacje dla przedsiębiorstw — wszystkie aplikacje** wybierz aplikację.

6. Na karta ***argumentu*** (oznacza to, że karta o nazwie wybranego aplikacji w tytule) wybierz pozycję **Użytkownicy i grupy**.

    ![Wybierając polecenie wszystkie aplikacje](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)

7. Na ***argumentu*** **-użytkownika i przypisanie grupy** karta, wybierz polecenie **Dodaj** .

8. Na karta **Dodaj przydziału** wybierz **Użytkownicy i grupy**.

    ![Przypisywanie użytkownika lub grupy do aplikacji](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)

9. Na karta **Użytkownicy i grupy** wybierz jedną lub więcej użytkowników lub grup z listy, a następnie kliknij przycisk **Wybierz** , w dolnej części karta.

10. Na karta **Dodaj przydziału** wybierz **rolę**. Następnie na karta **Wybierz rolę** , wybierz rolę, aby zastosować do wybranych użytkowników lub grup, a następnie wybierz przycisk **OK** u dołu karta.

11. Na karta **Dodaj przydziału** kliknij przycisk **Przypisz** u dołu karta. Przypisane użytkownikom lub grupom ma uprawnienia zdefiniowane przez wybranej roli dla tej aplikacji przedsiębiorstwa.

## <a name="next-steps"></a>Następne kroki

- [Zobacz wszystkie moje grupy](active-directory-groups-view-azure-portal.md)
- [Usuwanie użytkownika lub grupy przydział z aplikacji przedsiębiorstwa](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Wyłącz dodatki logowania użytkownika aplikacji przedsiębiorstwa](active-directory-coreapps-disable-app-azure-portal.md)
- [Zmienianie nazwy lub logo aplikacji przedsiębiorstwa](active-directory-coreapps-change-app-logo-user-azure-portal.md)
