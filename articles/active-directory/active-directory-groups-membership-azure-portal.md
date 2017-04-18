<properties
    pageTitle="Zarządzanie grupami grupy jest członkiem w podglądzie usługi Azure Active Directory | Microsoft Azure"
    description="Grupy mogą zawierać innych grup w usłudze Azure Active Directory. Oto jak zarządzać te członkostwa."
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


# <a name="manage-the-groups-your-group-is-a-member-of-in-azure-active-directory-preview"></a>Zarządzanie grupami, które grupy jest członkiem w podglądzie usługi Azure Active Directory

Grupy mogą zawierać innych grup w podglądzie usługi Azure Active Directory. [Co to jest w podglądzie?](active-directory-preview-explainer.md) Oto jak zarządzać te członkostwa.

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a>Jak znaleźć grup, do których Moja grupa jest członkiem?

1.  Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta, który jest administratorem globalnym katalogu.

2.  Wybierz pozycję **więcej usług**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie naciśnij **klawisz Enter**.

  ![Otwieranie, zarządzanie użytkownikami](./media/active-directory-groups-membership-azure-portal/search-user-management.png)

3.  Na karta **Użytkownicy i grupy** zaznacz **wszystkie grupy**.

  ![Otwieranie karta grup](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)

4. Na karta **użytkowników i grup — wszystkie grupy** wybierz grupę.

5. Na *grupy *Grupa — ** ** Karta, wybierz pozycję **grupy członkostwa **.

  ![Otwieranie karta członkostwo w grupie](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. Aby dodać grupę jako członek innej grupy, na karta **Grupa — członkostwa w grupach** , wybierz polecenie **Dodaj** .

7. Wybierz grupę z karta **Wybieranie grupy** , a następnie wybierz przycisk **Wybierz** u dołu karta. Możesz dodać grupę do grupy tylko jeden naraz. Pole **użytkownika** filtruje Wyświetl według pasujących wpis część nazwy użytkownika lub urządzenia. Symbole wieloznaczne nie są akceptowane w tym polu.

  ![Dodawanie członkostwa w grupach](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)

8. Aby usunąć grupę jako członek innej grupy, na karta **Grupa — członkostwa w grupach** , wybierz grupę.

9. Na karta ***grupy*** wybierz polecenie **Usuń** i Potwierdź w wierszu.

  ![Usuwanie polecenia członkostwa](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)

9. Po zakończeniu zmieniania członkostwa w grupach grupy, wybierz przycisk **Zapisz**.


## <a name="additional-information"></a>Dodatkowe informacje

Te artykuły zawiera dodatkowe informacje dotyczące usługi Azure Active Directory.

* [Wyświetlanie istniejących grup](active-directory-groups-view-azure-portal.md)
* [Tworzenie nowej grupy i dodawanie członków](active-directory-groups-create-azure-portal.md)
* [Zarządzanie ustawieniami grupy](active-directory-groups-settings-azure-portal.md)
* [Zarządzanie członkami grupy](active-directory-groups-members-azure-portal.md)
* [Zarządzanie regułami dynamiczne dla użytkowników w grupie](active-directory-groups-dynamic-membership-azure-portal.md)
