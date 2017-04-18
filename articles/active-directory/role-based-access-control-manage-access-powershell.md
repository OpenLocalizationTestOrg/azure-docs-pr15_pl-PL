<properties
    pageTitle="Zarządzanie kontrola dostępu oparta na rolach (RBAC) przy użyciu programu PowerShell Azure | Microsoft Azure"
    description="Jak zarządzać RBAC przy użyciu programu PowerShell Azure, w tym role, przypisywanie ról i usuwanie przypisania roli."
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

# <a name="manage-role-based-access-control-with-azure-powershell"></a>Zarządzanie kontrola dostępu oparta na rolach przy użyciu programu PowerShell Azure

> [AZURE.SELECTOR]
- [Programu PowerShell](role-based-access-control-manage-access-powershell.md)
- [Polecenie Azure](role-based-access-control-manage-access-azure-cli.md)
- [INTERFEJSU API USŁUGI REST](role-based-access-control-manage-access-rest.md)


Kontrola dostępu oparta na rolach (RBAC) w portalu Azure i interfejsu API zarządzania zasobów Azure umożliwia zarządzanie dostępem do subskrypcji usługi na poziomie szerokiego. Za pomocą tej funkcji można przyznać dostęp dla użytkowników, grup lub podmiotów usługi Active Directory, przypisując do nich niektóre role w określonym zakresie.

Zanim będzie można używać programu PowerShell do zarządzania RBAC, musisz mieć następujące czynności:

- Azure programu PowerShell wersji 0.8.8 lub nowszej. Aby zainstalować najnowszą wersję i skojarzyć subskrypcji Azure, zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

- Azure poleceń cmdlet Menedżera zasobów. Zainstaluj [poleceń cmdlet Menedżera zasobów Azure](https://msdn.microsoft.com/library/mt125356.aspx) w programie PowerShell.

## <a name="list-roles"></a>Lista ról

### <a name="list-all-available-roles"></a>Lista wszystkich dostępnych ról
Do ról RBAC listy, które są dostępne dla przydziałów i do sprawdzenia operacje, na którym przyznano w przypadku uzyskiwania dostępu za pomocą `Get-AzureRmRoleDefinition`.

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![Zrzut ekranu programu PowerShell-Get AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>Akcje listy roli
Aby wyświetlić listę akcji dla konkretnej roli, należy użyć `Get-AzureRmRoleDefinition <role name>`.

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![Zrzut ekranu programu PowerShell — dla konkretnej roli Get-AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>Kto ma dostęp
Aby wyświetlić przydziały dostępu RBAC, za pomocą `Get-AzureRmRoleAssignment`.

### <a name="list-role-assignments-at-a-specific-scope"></a>Lista przypisania ról w określonym zakresie
Można zobaczyć wszystkie przydziały dostępu dla określonej subskrypcji, zasobu lub grupy zasobów. Na przykład, aby wyświetlić wszystkie aktywne przydziałów dla grupy zasobów, należy użyć `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![Zrzut ekranu programu PowerShell — dla grupy zasobów Get-AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a>Lista role przypisane do użytkownika
Aby wyświetlić wszystkie role przypisane do określonego użytkownika i role, które są przypisane do grupy, do której należy użytkownik, za pomocą `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![Zrzut ekranu programu PowerShell — dla użytkownika Get-AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Administrator usługi klasyczny listy i przypisania roli coadmin
Do listy przypisań dostępu dla administratora klasyczny subskrypcji i coadministrators, użyj:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>Udzielanie dostępu
### <a name="search-for-object-ids"></a>Wyszukiwanie identyfikatory obiektów
W celu przypisania roli, należy zidentyfikować obiekt (użytkownika, grupy lub aplikacji) i zakresu.

Jeśli nie wiesz, identyfikator subskrypcji, możesz można znaleźć karta **subskrypcji** w portalu Azure. Aby dowiedzieć się, jak wyszukiwać identyfikator subskrypcji, zobacz [Get-AzureSubscription](https://msdn.microsoft.com/library/dn495302.aspx) w witrynie MSDN.

Aby uzyskać identyfikator obiektu grupy usługi Azure AD, należy użyć:

    Get-AzureRmADGroup -SearchString <group name in quotes>

Aby uzyskać identyfikator obiektu wystawcy usługi Azure AD lub aplikacji, należy użyć:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Przypisywanie roli do aplikacji w zakresie subskrypcji
Aby udzielić dostępu do aplikacji w zakresie subskrypcji, należy użyć:

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![Zrzut ekranu programu PowerShell — nowe AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Przypisywanie roli do użytkownika w zakresie grupa zasobów
Aby udzielić dostępu do użytkownika w zakresie grupa zasobów, należy użyć:

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![Zrzut ekranu programu PowerShell — nowe AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Przypisywanie roli do grupy w zakresie zasobów
Aby udzielić dostępu do grupy w zakresie zasobów, należy użyć:

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![Zrzut ekranu programu PowerShell — nowe AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>Usuwanie programu access
Aby usunąć dostępu dla użytkowników, grupy i aplikacje, należy użyć:

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![Zrzut ekranu programu PowerShell Usuń AzureRmRoleAssignment - RBAC](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>Tworzenie roli niestandardowych
Aby utworzyć niestandardową rolę, użyj `New-AzureRmRoleDefinition` polecenia.

Po utworzeniu roli niestandardowych przy użyciu programu PowerShell, należy zacząć od jednego z [wbudowanych role](role-based-access-built-in-roles.md). Atrybuty Dodaj *Akcje*, *notActions*lub *zakresy* , które chcesz edytować, a następnie zapisz zmiany jako nowej roli.

W poniższym przykładzie zaczyna się od roli *Współautora maszyn wirtualnych* , a które używane w celu utworzenia niestandardowych roli o nazwie *Operator maszyn wirtualnych*. Nowej roli udziela dostępu do wszystkich operacji odczytu *Microsoft.Compute*, *Microsoft.Storage*i *Microsoft.Network* dostawców zasobów i udziela dostępu do uruchamiania, uruchom ponownie i monitorowanie maszyn wirtualnych. Roli niestandardowej można użyć w dwóch subskrypcji.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![Zrzut ekranu programu PowerShell-Get AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

## <a name="modify-a-custom-role"></a>Modyfikowanie rolę niestandardową
Aby zmodyfikować rolę niestandardową, najpierw użyj `Get-AzureRmRoleDefinition` polecenie, aby pobrać definicji roli. Drugi wprowadź odpowiednie zmiany do definicji roli. Ponadto za pomocą `Set-AzureRmRoleDefinition` polecenie, aby zapisać definicji roli zmienione.

W poniższym przykładzie dodawane `Microsoft.Insights/diagnosticSettings/*` operacji do roli niestandardowych *Operator maszyn wirtualnych* .

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![Zrzut ekranu programu PowerShell-Set AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

W poniższym przykładzie dodawane Azure subskrypcji do zakresów można przypisać roli niestandardowych *Operator maszyn wirtualnych* .

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2"
Set-AzureRmRoleDefinition -Role $role)
```

![Zrzut ekranu programu PowerShell-Set AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

## <a name="delete-a-custom-role"></a>Usuwanie niestandardowego roli

Aby usunąć rolę niestandardową, użyj `Remove-AzureRmRoleDefinition` polecenia.

W poniższym przykładzie usuwane roli niestandardowych *Operator maszyn wirtualnych* .

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![Zrzut ekranu programu PowerShell Usuń AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>Role niestandardowe listy
Aby wyświetlić listę ról, które są dostępne dla przydziałów w zakresie, użyj `Get-AzureRmRoleDefinition` polecenia.

Poniższy przykład zawiera listę wszystkich ról, które są dostępne dla przydziału w wybraną subskrypcję.

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![Zrzut ekranu programu PowerShell-Get AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

W poniższym przykładzie roli niestandardowych *Operator maszyn wirtualnych* nie jest dostępna w subskrypcji *Production4* , ponieważ tej subskrypcji nie znajduje się na **AssignableScopes** roli.

![Zrzut ekranu programu PowerShell-Get AzureRmRoleDefinition - RBAC](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>Zobacz też
- [Przy użyciu programu PowerShell Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md)
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
