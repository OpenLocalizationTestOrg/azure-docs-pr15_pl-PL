<properties
    pageTitle="Usuwanie użytkownika lub grupy przydział z aplikacji przedsiębiorstwa w podglądzie usługi Azure Active Directory | Microsoft Azure"
    description="Jak usunąć przydział dostępu użytkownika lub grupy z aplikacji przedsiębiorstwa w usłudze Active Directory platformy Azure"
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
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory-preview"></a>Usuwanie użytkownika lub grupy przydziałów z aplikacji przedsiębiorstwa w podglądzie usługi Azure Active Directory

Jest proste usunąć użytkownika lub grupę z jest przypisany dostęp do jednej z aplikacji w przedsiębiorstwie w podglądzie usługi Azure Active Directory (Azure AD). [Co to jest w podglądzie?](active-directory-preview-explainer.md) Musi mieć odpowiednie uprawnienia do zarządzania aplikacji przedsiębiorstwa. W bieżącym podglądzie, musisz być administratorem globalnym katalogu.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Jak usunąć użytkownika lub grupę przydziału

1. Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta, który jest administratorem globalnym katalogu.

2. Wybierz pozycję **więcej usług**, wprowadź **Usługi Azure Active Directory** w polu tekstowym, a następnie naciśnij **klawisz Enter**.

3. Na *directoryname *Usługi Azure Active Directory — ** ** karta (czyli Azure AD karta katalogu, w którym zarządzasz), wybierz pozycję **przedsiębiorstwo aplikacji **.

    ![Otwieranie aplikacji przedsiębiorstwa](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)

4. Na karta **aplikacji przedsiębiorstwa** zaznacz **wszystkie aplikacje**. Zobaczysz listę aplikacji, które można zarządzać.

5. Na karta **aplikacje dla przedsiębiorstw — wszystkie aplikacje** wybierz aplikację.

6. Na karta ***argumentu*** (oznacza to, że karta o nazwie wybranego aplikacji w tytule) wybierz pozycję **Użytkownicy i grupy**.

    ![Wybieranie użytkowników lub grup](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)

7. Na ***argumentu*** **-użytkownika i przypisanie grupy** karta, wybierz jedną z więcej użytkowników lub grup, a następnie wybierz polecenie **Usuń** . Potwierdź decyzję w wierszu.

    ![Wybranie polecenia Usuń](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Następne kroki

- [Zobacz wszystkie moje grupy](active-directory-groups-view-azure-portal.md)
- [Przypisywanie użytkownika lub grupy do aplikacji przedsiębiorstwa](active-directory-coreapps-assign-user-azure-portal.md)
- [Wyłącz dodatki logowania użytkownika aplikacji przedsiębiorstwa](active-directory-coreapps-disable-app-azure-portal.md)
- [Zmienianie nazwy lub logo aplikacji przedsiębiorstwa](active-directory-coreapps-change-app-logo-user-azure-portal.md)
