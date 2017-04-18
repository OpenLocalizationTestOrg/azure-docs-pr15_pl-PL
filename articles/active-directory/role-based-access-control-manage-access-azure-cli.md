<properties
    pageTitle="Zarządzanie kontrola dostępu oparta na rolach (RBAC) przy użyciu Azure polecenie | Microsoft Azure"
    description="Dowiedz się, jak zarządzać oparta na rolach kontrola dostępu (RBAC) przy użyciu Azure interfejs wiersza polecenia, wyszczególniając ról i roli akcji i przypisywanie ról do zakresów subskrypcji i aplikacji."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/22/2016"
    ms.author="kgremban"/>

# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Zarządzanie oparta na rolach kontrola dostępu przy użyciu Azure interfejsu wiersza polecenia

> [AZURE.SELECTOR]
- [Programu PowerShell](role-based-access-control-manage-access-powershell.md)
- [Polecenie Azure](role-based-access-control-manage-access-azure-cli.md)
- [INTERFEJSU API USŁUGI REST](role-based-access-control-manage-access-rest.md)

Kontrola dostępu oparta na rolach (RBAC) w portalu Azure i interfejsu API Menedżera zasobów Azure umożliwia zarządzanie dostępem do subskrypcji i zasobów na poziomie szerokiego. Za pomocą tej funkcji można przyznać dostęp dla użytkowników, grup lub podmiotów usługi Active Directory, przypisując do nich niektóre role w określonym zakresie.

Zanim będzie można używać Azure interfejs wiersza polecenia (polecenie), aby zarządzać RBAC, musisz mieć następujące czynności:

- Polecenie Azure wersji 0.8.8 lub nowszej. Aby zainstalować najnowszą wersję i skojarzyć subskrypcji Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../xplat-cli-install.md).
- Azure Menedżer zasobów w Azure interfejsu wiersza polecenia. Aby uzyskać więcej informacji, przejdź do [przy użyciu interfejsu Azure wiersza polecenia przy użyciu Menedżera zasobów](../xplat-cli-azure-resource-manager.md) .

## <a name="list-roles"></a>Lista ról

### <a name="list-all-available-roles"></a>Lista wszystkich dostępnych ról
Aby wyświetlić listę wszystkich dostępnych ról, należy użyć:

        azure role list

W poniższym przykładzie pokazano listę *wszystkich dostępnych ról*.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![Zrzut ekranu wiersza polecenia — Lista azure roli — RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Akcje listy roli
Aby wyświetlić listę działania roli, należy użyć:

    azure role show "<role name>"

W poniższym przykładzie pokazano akcje ról *współautora* i *Współautora maszyn wirtualnych* .

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![Zrzut ekranu wiersza polecenia - azure roli pokaz - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

##  <a name="list-access"></a>Lista programu access
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Lista przypisania roli skutecznych w grupie zasobów
Aby wyświetlić listę przypisania roli, znajdują się w grupie zasobów, należy użyć:

    azure role assignment list --resource-group <resource group name>

W poniższym przykładzie pokazano przypisania ról w grupie *pharma sprzedaż projecforcast* .

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Wiersz polecenia RBAC Azure - listy przypisania roli azure zrzut ekranu grupy](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Lista przypisania roli dla użytkownika
Aby wyświetlić przypisania roli dla określonego użytkownika i przydziałów, które są przypisane do grup użytkowników, należy użyć:

    azure role assignment list --signInName <user email>

Można też wyświetlić przypisania roli, które są dziedziczone z grupy, zmieniając polecenia:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

W poniższym przykładzie pokazano przypisania roli, które są przypisywane do *sameert@aaddemo.com* użytkownika. Ta opcja uwzględnia ról przypisanych bezpośrednio do użytkownika i które są dziedziczone z grupy.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Zrzut ekranu wiersza polecenia — Lista przypisania roli azure przez użytkownika — RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

##  <a name="grant-access"></a>Udzielanie dostępu
Aby udzielić dostępu po zidentyfikowaniu rolę, którą chcesz przypisać, należy użyć:

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>Przypisywanie roli do grupy w zakresie subskrypcji
W celu przypisania roli do grupy w zakresie subskrypcji, należy użyć:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Poniższy przykład przypisuje roli *czytnika* *Koch Aneta zespołu* w zakresie *subskrypcji* .


![Wiersz polecenia Azure RBAC — przypisanie roli azure utworzyć zrzut ekranu grupy](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Przypisywanie roli do aplikacji w zakresie subskrypcji
Aby przypisać rolę aplikacji w zakresie subskrypcji, należy użyć:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Poniższy przykład udziela roli *współautora* dla aplikacji *Azure AD* wybraną subskrypcję.

 ![Tworzenie przypisania roli azure wiersza polecenia Azure RBAC - przez aplikację](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Przypisywanie roli do użytkownika w zakresie grupa zasobów
Aby przypisać rolę do użytkownika w zakresie grupa zasobów, należy użyć:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

W przykładzie poniżej zezwolenie rola *Współautora maszyn wirtualnych* do *samert@aaddemo.com* użytkowników w zakresie *Pharma-sprzedaż-ProjectForcast* zasobów grupy.

![Wiersz polecenia Azure RBAC — przypisanie roli azure utworzyć zrzut ekranu użytkownika](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Przypisywanie roli do grupy w zakresie zasobów
W celu przypisania roli do grupy w zakresie zasobów, należy użyć:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

Poniższy przykład udziela rola *Współautora maszyn wirtualnych* do grupy w usłudze *Azure AD* w *podsieci*.

![Wiersz polecenia Azure RBAC — przypisanie roli azure utworzyć zrzut ekranu grupy](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

##  <a name="remove-access"></a>Usuwanie programu access
Aby usunąć przypisanie roli, należy użyć:

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

W poniższym przykładzie usuwane przypisanie roli *Współautora maszyn wirtualnych* z *sammert@aaddemo.com* użytkownika w grupie zasobów *Pharma-sprzedaż-ProjectForcast* .
Przykład następnie usuwa przypisanie roli z grupy dla tej subskrypcji.

![Zrzut ekranu wiersza polecenia - delete przypisania roli azure - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Tworzenie roli niestandardowych
Aby utworzyć niestandardową rolę, należy użyć:

    azure role create --inputfile <file path>

Poniższy przykład tworzy niestandardowe roli o nazwie *Operator maszyn wirtualnych*. Niestandardowe roli udziela dostępu do wszystkich operacji odczytu *Microsoft.Compute*, *Microsoft.Storage*i *Microsoft.Network* dostawców zasobów i udziela dostępu do uruchamiania, uruchom ponownie i monitorowanie maszyn wirtualnych. Roli niestandardowej można użyć w dwóch subskrypcji. W tym przykładzie użyto JSON pliku jako danych wejściowych.

![Zrzut ekranu JSON - roli niestandardowej definicji-](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![Tworzenie roli azure wiersza polecenia Azure RBAC - — zrzut ekranu](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Modyfikowanie rolę niestandardową

Aby zmodyfikować rolę niestandardową, najpierw użyj `azure role show` polecenie, aby pobrać definicji roli. Drugi wprowadź odpowiednie zmiany w pliku definicji roli. Ponadto za pomocą `azure role set` zapisać definicji roli zmienione.

    azure role set --inputfile <file path>

W poniższym przykładzie dodawane operację *Microsoft.Insights/diagnosticSettings/* **Akcje**i Azure subskrypcji **AssignableScopes** roli niestandardowych Operator maszyn wirtualnych.

![JSON - modyfikowanie roli niestandardowej definicji — zrzut ekranu](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Zrzut ekranu wiersza polecenia - azure roli set - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Usuwanie niestandardowego roli

Aby usunąć rolę niestandardową, najpierw za pomocą `azure role show` polecenie, aby określić **identyfikator** roli. Następnie należy użyć `azure role delete` polecenie, aby usunąć rolę, określając **identyfikator**.

W poniższym przykładzie usuwane roli niestandardowych *Operator maszyn wirtualnych* .

![Zrzut ekranu wiersza polecenia - azure roli delete - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Role niestandardowe listy

Aby wyświetlić listę ról, które są dostępne dla przydziałów w zakresie, użyj `azure role list` polecenia.

Poniższy przykład zawiera listę wszystkich ról, dostępnych dla przydziału w wybraną subskrypcję.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![Zrzut ekranu wiersza polecenia — Lista azure roli — RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

W poniższym przykładzie roli niestandardowych *Operator maszyn wirtualnych* nie jest dostępna w subskrypcji *Production4* , ponieważ tej subskrypcji nie znajduje się na **AssignableScopes** roli.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![Zrzut ekranu wiersza polecenia — Lista azure roli dla ról niestandardowych — RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)





## <a name="rbac-topics"></a>Tematy RBAC
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
