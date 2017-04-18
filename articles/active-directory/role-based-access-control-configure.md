<properties
    pageTitle="Kontrola dostępu oparta na rolach za pomocą w portalu Azure | Microsoft Azure"
    description="Wprowadzenie zarządzanie dostępem oparta na rolach kontrola dostępu w portalu Azure. Aby przypisać uprawnienia do zasobów za pomocą przypisania roli."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="use-role-assignments-to-manage-access-to-your-azure-subscription-resources"></a>Zarządzanie dostępem do zasobów subskrypcji Azure za pomocą przypisania ról

> [AZURE.SELECTOR]
- [Zarządzanie dostępem przez użytkownika lub grupy](role-based-access-control-manage-assignments.md)
- [Zarządzanie dostępem według zasobów](role-based-access-control-configure.md)

Azure oparta na rolach programu Access Control (RBAC) umożliwia zarządzanie szerokiego dostępu dla Azure. Przy użyciu RBAC, można udzielić tylko ilość dostęp użytkownicy muszą wykonać swoje zadania. Ten artykuł ułatwia rozpoczęcie pracy z RBAC w portalu Azure. Jeśli potrzebujesz dodatkowych informacji o jak RBAC ułatwia zarządzanie dostępem, zobacz [Co to jest kontrola dostępu oparta na rolach](role-based-access-control-what-is.md).

## <a name="view-access"></a>Widok programu access
Aby zobaczyć, kto ma dostęp do zasobów, grupa zasobów lub subskrypcji z jego głównym karta w [Azure portal](https://portal.azure.com). Na przykład chcemy zobaczyć, kto ma dostęp do jednej z naszych grup zasobów:

1. Na pasku nawigacyjnym po lewej stronie wybierz pozycję **grupy zasobów** .  
    ![Grupy zasobów - ikony](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Wybierz nazwę grupy zasobów z karta **grup zasobów** .
3. **Kontrola dostępu (IAM)** wybierz z menu po lewej stronie.  
4. Karta kontrolki programu Access zawiera listę wszystkich użytkowników, grupy i aplikacje, które ma dostęp do tej grupy zasobów.  

    ![Karta użytkowników — w porównaniu z dziedziczone przypisane zrzut ekranu programu access](./media/role-based-access-control-configure/view-access.png)

Powiadomienie, że niektórzy użytkownicy zostały **przypisane** a inne **dziedziczone** do niego dostęp. Dostęp jest przypisane do grupy zasobów albo odziedziczone przydział do subskrypcji nadrzędnej.

> [AZURE.NOTE] Administratorzy klasyczny subskrypcji i administratorów współpracujących są traktowane jako właściciele subskrypcji w nowy model RBAC.


## <a name="add-access"></a>Dodawanie programu Access
Udzielanie dostępu z poziomu zasobów, grupa zasobów lub subskrypcji, która jest zakres przypisania roli.

1. Wybierz pozycję **Dodaj** na karta kontroli dostępu.  
2. Wybierz rolę, którą chcesz przypisać z karta **Wybierz rolę** .
3. Wybierz użytkownika, grupy lub aplikacji w katalogu, który chcesz udostępnić. Można przeszukiwać katalogu z wyświetlane nazwy, adresy e-mail i identyfikatory obiektów.  

    ![Dodawanie użytkowników karta — wyszukiwanie zrzut ekranu](./media/role-based-access-control-configure/grant-access2.png)

4. Wybierz **przycisk OK** , aby utworzyć przydziału. Menu podręczne **Dodawanie użytkownika** do śledzenia postępu.  
    ![Dodawanie paska postępu użytkownika — zrzut ekranu](./media/role-based-access-control-configure/addinguser_popup.png)

Po pomyślnym dodaniu przypisania roli, pojawi się na karta **użytkowników** .

## <a name="remove-access"></a>Usuwanie programu Access

1. Wybierz przypisanie roli na karta kontroli dostępu.
2. Kliknij przycisk **Usuń** Karta Szczegóły przydziału.  
3. Wybierz pozycję **Tak,** aby potwierdzić usunięcie.  
    ![Karta użytkowników - Usuń z roli zrzut ekranu](./media/role-based-access-control-configure/remove-access1.png)

Nie można usunąć dziedziczone przydziałów. Na poniższej ilustracji zauważyć, że jest wyszarzona, przycisk Usuń. Zamiast tego Obejrzyj szczegółów **Przypisane u** . Przejdź do zasobu wymienionego, aby usunąć przypisanie roli.

![Karta użytkowników — usuwanie dziedziczone access wyłącza przycisk zrzut ekranu](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a>Inne narzędzia do zarządzania programu access
Można przypisywać role i zarządzać dostępu za pomocą polecenia Azure RBAC w narzędziach niż Azure portal.  Skorzystaj z łączy, aby dowiedzieć się więcej o wymaganiach wstępnych i rozpocząć pracę z poleceń Azure RBAC.

- [Azure programu PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure interfejsu wiersza polecenia](role-based-access-control-manage-access-azure-cli.md)
- [INTERFEJSU API USŁUGI REST](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Następne kroki
- [Tworzenie raportu programu access Zmień historii](role-based-access-control-access-change-history-report.md)
- Zobacz [RBAC wbudowanych ról](role-based-access-built-in-roles.md)
- Definiowanie własnych [niestandardowych ról w Azure RBAC](role-based-access-control-custom-roles.md)
