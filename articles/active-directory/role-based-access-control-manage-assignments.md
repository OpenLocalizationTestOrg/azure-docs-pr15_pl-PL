<properties
    pageTitle="Wyświetlanie Azure przydziały dostępu | Microsoft Azure"
    description="Wyświetlanie i zarządzanie nimi w przypisaniach kontrola dostępu oparta na rolach dla dowolnego użytkownika lub grupę w portalu Azure"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="jeffsta"/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal---public-preview"></a>Widok programu access przydziałów dla użytkowników i grup w portalu Azure - publicznej podglądu

> [AZURE.SELECTOR]
- [Zarządzanie dostępem przez użytkownika lub grupy](role-based-access-control-manage-assignments.md)
- [Zarządzanie dostępem według zasobów](role-based-access-control-configure.md)

Z oparta na rolach programu Access Control (RBAC) w podglądzie usługi Azure Active Directory, możesz zarządzać dostępem do Azure zasobów. [Co to jest w podglądzie?](active-directory-preview-explainer.md)

Dostęp przypisana RBAC jest szerokiego, ponieważ mogą ograniczyć uprawnienia na dwa sposoby:

- **Zakres:** Przypisania roli RBAC są ograniczone do określonych subskrypcji, grupa zasobów lub zasobu. Przyznany dostęp do zasobu w pojedynczy użytkownik nie może uzyskać dostępu inne zasoby w tej samej subskrypcji.
- **Roli:** W zakresie przydziału, jest zawężony dostęp nawet później przypisywanie roli. Role mogą być wysokiego poziomu, takich jak właściciel lub szczegółowe, takie jak czytnik maszyn wirtualnych.

Tylko można przypisywać role z poziomu subskrypcji, grupa zasobów lub zasób, który jest zakresem dla przydziału. Ale wszystkich przydziałów dostępu dla danego użytkownika lub grupy można wyświetlać w jednym miejscu.

Uzyskaj więcej informacji na temat sposobu [użycia przypisania roli zarządzać dostępem do zasobów Azure subskrypcji](role-based-access-control-configure.md).

##  <a name="view-access-assignments"></a>Widok programu access przydziałów

Aby wyszukać przydziały dostęp dla jednego użytkownika lub grupy, uruchom w usługi Azure Active Directory w [Azure portal](http://portal.azure.com).

1. Wybierz pozycję **Azure Active Directory**. Jeśli ta opcja nie jest widoczna na liście nawigacji, wybierz pozycję **Więcej usług** , a następnie przewiń w dół do znajdowania **Usługi Azure Active Directory**.
2. Wybierz pozycję **Użytkownicy i grupy**, a następnie albo **wszystkich użytkowników** lub **wszystkie grupy**. Na przykład możemy skupić się na poszczególnych użytkowników.
    ![Zarządzanie użytkownikami i grupami w usługi Azure Active Directory — zrzut ekranu](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. Wyszukaj użytkownika według nazwiska lub nazwy użytkownika.
4. Wybierz pozycję **zasoby Azure** na karta użytkownika. Zostaną wyświetlone wszystkie przypisania dostępu dla tego użytkownika.

### <a name="read-permissions-to-view-assignments"></a>Aby wyświetlić przydziały uprawnienia do odczytu

Ta strona zawiera tylko przydziałów programu access, do których masz uprawnienia do odczytu. Na przykład masz dostęp do odczytu do subskrypcji A i przejdź do karta Azure zasobów, aby sprawdzić przydziałów użytkownika. Można zobaczyć jej przypisania dostępu dla subskrypcji odpowiedzi, ale nie będzie widoczna, Anna również ma dostęp przydziały w subskrypcji B.

## <a name="delete-access-assignments"></a>Usuwanie przydziałów programu access

Z tym karta możesz usunąć przydziały programu access, które zostały przypisane bezpośrednio do użytkownika lub grupy. Przydział programu access jest dziedziczona grupy nadrzędnej, należy przejść do zasobu lub innej subskrypcji i zarządzanie nimi przydziału.

1. Z listy wszystkich przydziałów dostępu dla użytkownika lub grupy wybierz ten, który chcesz usunąć.
2. Wybierz pozycję **Usuń** , a następnie **Tak,** aby potwierdzić.
    ![Usuwanie przypisanie programu access — zrzut ekranu](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="related-topics"></a>Tematy pokrewne

- Rozpoczynanie pracy z oparta na rolach kontrola dostępu do [użycia przypisania roli zarządzać dostępem do zasobów Azure subskrypcji](role-based-access-control-configure.md)
- Zobacz [RBAC wbudowanych ról](role-based-access-built-in-roles.md)
