<properties
    pageTitle="Wyłącz dodatki logowania użytkownika aplikacji przedsiębiorstwa w podglądzie usługi Azure Active Directory | Microsoft Azure"
    description="Jak wyłączyć aplikacji dla przedsiębiorstw, tak aby użytkownicy nie może logować się do niego w usłudze Active Directory platformy Azure"
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
    ms.date="10/17/2016"
    ms.author="curtand"/>


# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory-preview"></a>Wyłącz dodatki logowania użytkownika aplikacji przedsiębiorstwa w podglądzie usługi Azure Active Directory

Jest proste wyłączenia aplikacji dla przedsiębiorstw, tak aby użytkownicy nie mogą logować się do niego w podglądzie usługi Azure Active Directory (Azure AD). [Co to jest w podglądzie?](active-directory-preview-explainer.md) Musi mieć odpowiednie uprawnienia do zarządzania aplikacji przedsiębiorstwa. W bieżącym podglądzie, musisz być administratorem globalnym katalogu.

## <a name="how-do-i-disable-user-sign-ins"></a>Jak wyłączyć dodatki logowania użytkownika?

1. Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta, który jest administratorem globalnym katalogu.

2. Wybierz pozycję **więcej usług**, wprowadź **Usługi Azure Active Directory** w polu tekstowym, a następnie naciśnij **klawisz Enter**.

3. Na *directoryname *Usługi Azure Active Directory — ** ** karta (czyli Azure AD karta katalogu, w którym zarządzasz), wybierz pozycję **przedsiębiorstwo aplikacji **.

    ![Otwieranie aplikacji przedsiębiorstwa](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)

4. Na karta **aplikacji przedsiębiorstwa** zaznacz **wszystkie aplikacje**. Wyświetlić listę aplikacji, które można zarządzać.

5. Na karta **aplikacje dla przedsiębiorstw — wszystkie aplikacje** wybierz aplikację.

6. Na karta ***argumentu*** (oznacza to, że karta o nazwie wybranego aplikacji w tytule) wybierz pozycję **Właściwości**.

    ![Wybierając polecenie wszystkie aplikacje](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)

7. Na ***argumentu*** **-Właściwości** karta, wybierz pozycję **nie** dla **włączyć dla użytkowników zalogować się?**.

8. Wybierz polecenie **Zapisz** .

## <a name="next-steps"></a>Następne kroki

- [Zobacz wszystkie moje grupy](active-directory-groups-view-azure-portal.md)
- [Przypisywanie użytkownika lub grupy do aplikacji przedsiębiorstwa](active-directory-coreapps-assign-user-azure-portal.md)
- [Usuwanie użytkownika lub grupy przydział z aplikacji przedsiębiorstwa](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Zmienianie nazwy lub logo aplikacji przedsiębiorstwa](active-directory-coreapps-change-app-logo-user-azure-portal.md)
