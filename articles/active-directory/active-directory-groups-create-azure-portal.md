<properties
    pageTitle="Tworzenie nowej grupy w podglądzie usługi Azure Active Directory | Microsoft Azure"
    description="Jak utworzyć grupę w usługi Azure Active Directory i dodać do grupy użytkowników (członków)"
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


# <a name="create-a-new-group-in-azure-active-directory-preview"></a>Tworzenie nowej grupy w podglądzie usługi Azure Active Directory

> [AZURE.SELECTOR]
- [Azure portal](active-directory-groups-create-azure-portal.md)
- [Portal Azure klasyczny](active-directory-accessmanagement-manage-groups.md)
- [Programu PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

W tym artykule wyjaśniono, jak tworzyć i Wypełnij nową grupę w podglądzie usługi Azure Active Directory (Azure AD). [Co to jest w podglądzie?](active-directory-preview-explainer.md) Użyj grupy do wykonywania zadań zarządzania, takie jak przypisywanie licencji lub uprawnień do liczby użytkowników i urządzeń jednocześnie.

## <a name="how-do-i-create-a-group"></a>Jak utworzyć grupę?

1. Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta, który jest administratorem globalnym katalogu.

2. Wybierz pozycję **więcej usług**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie naciśnij **klawisz Enter**.

  ![Otwieranie, zarządzanie użytkownikami](./media/active-directory-groups-create-azure-portal/search-user-management.png)

3. Na karta **Użytkownicy i grupy** zaznacz **wszystkie grupy**.

  ![Otwieranie karta grup](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)

4. Na karta **użytkowników i grup — we wszystkich grupach** wybierz polecenie **Dodaj** .

  ![Wybranie polecenia Dodaj](./media/active-directory-groups-create-azure-portal/add-group-command.png)

5. Na karta **Grupa** Dodaj nazwę i opis grupy.

6. Aby wybrać członków, których chcesz dodać do grupy, zaznacz **przypisane** w polu **Typ członkostwa** , a następnie wybierz **członków**. Aby uzyskać więcej informacji na temat zarządzania członkostwo w grupie dynamiczne Zobacz [Używanie atrybutów tworzenie zaawansowanych reguł członkostwa w grupach](active-directory-groups-dynamic-membership-azure-portal.md).

  ![Wybieranie członków, których chcesz dodać](./media/active-directory-groups-create-azure-portal/select-members.png)

5. Na karta **członków** wybierz jedną lub więcej użytkowników lub urządzeń możesz dodawać do grupy i kliknij przycisk **Zaznacz** w dolnej części karta dodawanie ich do grupy. Pole **użytkownika** filtruje Wyświetl według pasujących wpis część nazwy użytkownika lub urządzenia. Symbole wieloznaczne nie są akceptowane w tym polu.

6. Po zakończeniu dodawania członków do grupy, wybierz opcję **Utwórz** na karta **grupy** .    

  ![Tworzenie grupy potwierdzenia](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)




## <a name="additional-information"></a>Dodatkowe informacje

Te artykuły zawiera dodatkowe informacje dotyczące usługi Azure Active Directory.

* [Wyświetlanie istniejących grup](active-directory-groups-view-azure-portal.md)
* [Zarządzanie ustawieniami grupy](active-directory-groups-settings-azure-portal.md)
* [Zarządzanie członkami grupy](active-directory-groups-members-azure-portal.md)
* [Zarządzanie członkostwami grupy](active-directory-groups-membership-azure-portal.md)
* [Zarządzanie regułami dynamiczne dla użytkowników w grupie](active-directory-groups-dynamic-membership-azure-portal.md)
