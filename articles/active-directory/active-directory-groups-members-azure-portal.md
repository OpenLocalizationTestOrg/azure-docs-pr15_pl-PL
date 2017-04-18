<properties
    pageTitle="Zarządzanie członkami grupy w podglądzie usługi Azure Active Directory | Microsoft Azure"
    description="Jak użytkowników i urządzeń, które należą do grupy w usłudze Active Directory platformy Azure"
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


# <a name="manage-the-members-for-a-group-in-azure-active-directory-preview"></a>Zarządzanie członkami grupy w podglądzie usługi Azure Active Directory

W tym artykule wyjaśniono, jak zarządzanie członkami grupy w podglądzie usługi Azure Active Directory (Azure AD). [Co to jest w podglądzie?](active-directory-preview-explainer.md)

## <a name="how-do-i-find-the-members-and-manage-them"></a>Jak znaleźć członków i zarządzać nimi?

1.  Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta, który jest administratorem globalnym katalogu.

2.  Wybierz pozycję **więcej usług**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie naciśnij **klawisz Enter**.

  ![Otwieranie, zarządzanie użytkownikami](./media/active-directory-groups-members-azure-portal/search-user-management.png)

3.  Na karta **Użytkownicy i grupy** zaznacz **wszystkie grupy**.

  ![Otwieranie karta grup](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)

4. Na karta **użytkowników i grup — wszystkie grupy** wybierz grupę.

5. Na *grupy *grupy — ** ** Karta Wybierz **elementy członkowskie **.

  ![Otwieranie karta członków](./media/active-directory-groups-members-azure-portal/view-group-members.png)

6. Aby dodać członków do grupy, karta **Grupa — członków** , wybierz pozycję **Dodaj członków**.

  ![Dodawanie polecenia członków](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)

7. Na karta **członków** wybierz jedną lub więcej użytkowników lub urządzeń możesz dodawać do grupy i kliknij przycisk **Zaznacz** w dolnej części karta dodawanie ich do grupy. Pole **użytkownika** filtruje Wyświetl według pasujących wpis część nazwy użytkownika lub urządzenia. Symbole wieloznaczne nie są akceptowane w tym polu.

8. Aby usunąć członków z grupy na karta **Grupa — członków** Wybierz członka.

9. Na karta ***członka*** wybierz polecenie **Usuń** i Potwierdź w wierszu.

  ![Usuwanie polecenia członkowie](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)

9. Po zakończeniu zmieniania członkowie grupy, wybierz przycisk **Zapisz**.


## <a name="additional-information"></a>Dodatkowe informacje

Te artykuły zawiera dodatkowe informacje dotyczące usługi Azure Active Directory.

* [Wyświetlanie istniejących grup](active-directory-groups-view-azure-portal.md)
* [Tworzenie nowej grupy i dodawanie członków](active-directory-groups-create-azure-portal.md)
* [Zarządzanie ustawieniami grupy](active-directory-groups-settings-azure-portal.md)
* [Zarządzanie członkostwami grupy](active-directory-groups-membership-azure-portal.md)
* [Zarządzanie regułami dynamiczne dla użytkowników w grupie](active-directory-groups-dynamic-membership-azure-portal.md)
