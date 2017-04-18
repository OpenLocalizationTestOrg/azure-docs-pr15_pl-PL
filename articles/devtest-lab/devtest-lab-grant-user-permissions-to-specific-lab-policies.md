<properties
    pageTitle="Udzielanie użytkownikom uprawnień do zasad określonych ćwiczenia | Microsoft Azure"
    description="Dowiedz się, jak udzielanie użytkownikom uprawnień do określonych ćwiczenia zasad Labs DevTest na podstawie potrzeb każdego użytkownika"
    services="devtest-lab,virtual-machines,visual-studio-online"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="tarcher"/>

# <a name="grant-user-permissions-to-specific-lab-policies"></a>Udzielanie użytkownikom uprawnień do zasad określonych ćwiczenia

## <a name="overview"></a>Omówienie

W tym artykule pokazano, jak za pomocą programu PowerShell udzielanie użytkownikom uprawnień do zasad określonego ćwiczenia. W ten sposób uprawnień można stosować oparte na potrzeby każdego użytkownika. Na przykład może chcesz udzielić określonego użytkownika możliwość wprowadzania zmian w ustawieniach zasad maszyn wirtualnych, ale nie zasady kosztów.

## <a name="policies-as-resources"></a>Zasady jako zasoby

Opisane w artykule [Kontrola dostępu oparta na rolach Azure](../active-directory/role-based-access-control-configure.md) RBAC umożliwia zarządzanie dostępem szerokiego zasobów dla Azure. Przy użyciu RBAC, można oddzielnie różnic obowiązków w obrębie organizacji DevOps i udzielić tylko ilość dostępu użytkownikom, którzy potrzebują do wykonywania zadań.

W laboratoriach DevTest zasady jest typ zasobu, który umożliwia akcji RBAC **Microsoft.DevTestLab/labs/policySets/policies/**. Każdy zasady ćwiczenia zasobu w polu Typ zasobu zasad i można przypisywać zakres do roli RBAC.

Na przykład w celu udzielenia użytkownikom odczytu/zapisu uprawnień do zasad **Dozwolone rozmiary maszyn wirtualnych** , należy utworzyć niestandardową rolę działającego w programie **Microsoft.DevTestLab/labs/policySets/policies/** * akcji, a następnie przypisać odpowiednich użytkowników do tej roli niestandardowych w zakresie * *Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

Aby dowiedzieć się więcej o niestandardowych ról w RBAC, zobacz [Kontrola dostępu role niestandardowe](../active-directory/role-based-access-control-custom-roles.md).

##<a name="creating-a-lab-custom-role-using-powershell"></a>Tworzenie roli niestandardowych ćwiczenia przy użyciu programu PowerShell
Aby rozpocząć pracę, musisz przeczytaj następujący artykuł będzie wyjaśniono, jak zainstalować i skonfigurować polecenia cmdlet programu PowerShell Azure: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

Po skonfigurowaniu polecenia cmdlet programu PowerShell Azure można wykonywać następujące zadania:

- Wyświetl wszystkie operacje akcje dla dostawcy zasobów
- Akcje listy w określonej roli:
- Tworzenie roli niestandardowych

Poniższy skrypt programu PowerShell przedstawiono przykłady sposobów wykonywania następujących zadań:

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

##<a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Przypisywanie uprawnień do użytkownika w celu określone zasady przy użyciu ról niestandardowych
Po zdefiniowany przez użytkownika do ról niestandardowych, możesz je przypisać do użytkowników. Aby przypisać rolę niestandardową do użytkownika, należy najpierw uzyskać **identyfikator obiektu** reprezentującą tego użytkownika. Aby to zrobić, należy użyć polecenia cmdlet **Get-AzureRmADUser** .

W poniższym przykładzie **identyfikator obiektu** użytkownika *SomeUser* jest 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Po umieszczeniu **identyfikator obiektu** dla użytkownika i nazwa roli niestandardowej, możesz przypisać tej roli użytkownika przy użyciu polecenia cmdlet **New-AzureRmRoleAssignment** :

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

W poprzednim przykładzie zasady **AllowedVmSizesInLab** są używane. Możesz użyć dowolnej z następujących zasad:

- MaxVmsAllowedPerUser
- MaxVmsAllowedPerLab
- AllowedVmSizesInLab
- LabVmsShutdown

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Następne kroki

Raz możesz zostały uprawnienia do określonych ćwiczenia zasad, Oto niektóre następne kroki:

- [Bezpieczny dostęp do ćwiczenia](devtest-lab-add-devtest-user.md).

- [Ustawianie zasad ćwiczenia](devtest-lab-set-lab-policy.md).

- [Tworzenie szablonu ćwiczenia](devtest-lab-create-template.md).

- [Tworzenie niestandardowego artefaktów dla swojego maszyny wirtualne](devtest-lab-artifact-author.md).

- [Dodawanie maszyny z artefaktów na ćwiczenia](devtest-lab-add-vm-with-artifacts.md).
