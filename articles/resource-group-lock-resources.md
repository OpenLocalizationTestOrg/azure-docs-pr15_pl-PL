<properties 
    pageTitle="Blokowanie zasobami za pomocą Menedżera zasobów | Microsoft Azure" 
    description="Uniemożliwić użytkownikom aktualizowanie lub usuwanie niektórych zasobów przez zastosowanie ograniczenie do wszystkich użytkowników i ról." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tomfitz"/>

# <a name="lock-resources-with-azure-resource-manager"></a>Blokowanie zasobami za pomocą Menedżera zasobów Azure

Jako administrator może być konieczne zablokować subskrypcji, grupa zasobów lub zasobu uniemożliwić innym użytkownikom w organizacji przypadkowemu usunięciu lub modyfikowanie krytycznych zasobów. Można ustawić poziom blokady **CanNotDelete** lub **tylko do odczytu**. 

- **CanNotDelete** oznacza autoryzowani użytkownicy nadal można odczytywać i modyfikować zasobu, ale nie można go usunąć. 
- **Tylko do odczytu** oznacza autoryzowani użytkownicy mogą odczytywać zasobu, ale nie można usunąć go lub na jej wykonywać żadnych akcji. Uprawnienia do zasobu są ograniczone do roli **czytnika** . 

Zastosowanie **tylko do odczytu** może prowadzić do nieoczekiwanych wyników, ponieważ niektórych działań wydawać przeczytaj operacje rzeczywiście wymagają dodatkowe akcje. Na przykład umieszczania blokada **tylko do odczytu** na koncie miejsca do magazynowania zapobiega wszystkim użytkownikom wyświetlanie kluczy. Na liście, którą operacji klawiszy jest wykonywane za pomocą żądania POST, ponieważ zwracane klawiszy są dostępne dla operacji zapisu. Na przykład innego wprowadzania blokada **tylko do odczytu** na zasób aplikacji usługi uniemożliwia Visual Studio Server Explorer wyświetlania plików dla zasobu, ponieważ interakcji wymaga zapisu.

W odróżnieniu od kontrola dostępu oparta na rolach służy do stosowania ograniczenia we wszystkich użytkowników i ról zarządzania blokady. Aby uzyskać informacje o ustawianiu uprawnień dla użytkowników i ról, zobacz [Kontrola dostępu oparta na rolach Azure](./active-directory/role-based-access-control-configure.md).

Po zastosowaniu blokada w zakresie nadrzędnej wszystkie zasoby podrzędne dziedziczą samej blokady. Nawet zasoby, które zostaną dodane później dziedziczą blokady z witryny nadrzędnej. Największymi blokady w dziedziczenie ma pierwszeństwo.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Osób, które można utworzyć lub usunąć blokady w organizacji

Aby utworzyć lub usunąć blokady zarządzania, musi mieć dostęp do **Microsoft.Authorization/\* ** lub **Microsoft.Authorization/locks/\* ** akcje. Wbudowane ról tylko **właściciel** i **Administrator dostępu użytkowników** udzielono działaniami.

## <a name="creating-a-lock-through-the-portal"></a>Tworzenie blokady za pośrednictwem portalu

[AZURE.INCLUDE [resource-manager-lock-resources](../includes/resource-manager-lock-resources.md)]

## <a name="creating-a-lock-in-a-template"></a>Tworzenie blokady w szablonie

W poniższym przykładzie pokazano szablonu, który tworzy blokada na koncie miejsca do magazynowania. Konto miejsca do magazynowania, do którego jest stosowana blokada jest dostępna jako parametru. Nazwa blokady jest tworzona przez połączenie z **/Microsoft.Authorization/** Nazwa zasobu i nazwę blokady, w tym przypadku **myLock**.

Dotyczy typu określonego typu zasobu. Do przechowywania ustawienia typu "Microsoft.Storage/storageaccounts/providers/locks".

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="creating-a-lock-with-rest-api"></a>Tworzenie blokady za pomocą interfejsu API usługi REST

Można zablokować wdrożonym zasobów za pomocą [Interfejsu API usługi REST blokad zarządzania](https://msdn.microsoft.com/library/azure/mt204563.aspx). Interfejsu API usługi REST umożliwia tworzenie i usuwanie blokad i pobieranie informacji o istniejącej blokady.

Aby utworzyć blokada, uruchom polecenie:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Zakres może być subskrypcji, grupa zasobów lub zasobu. Nazwa lock jest dowolnego elementu, który chcesz zadzwonić blokady. Wersja api korzystać **2015-01-01**.

W wezwaniu na zawiera obiekt JSON, który umożliwia określenie właściwości blokady.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

Przykłady można znaleźć w temacie [Interfejsu API usługi REST blokad zarządzania](https://msdn.microsoft.com/library/azure/mt204563.aspx).

## <a name="creating-a-lock-with-azure-powershell"></a>Tworzenie blokady za pomocą programu PowerShell Azure

Można zablokować wdrożonym zasobów za pomocą programu PowerShell Azure za pomocą **Nowej AzureRmResourceLock** , jak pokazano w poniższym przykładzie.

    New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup

Azure programu PowerShell zawiera innych poleceń dotyczących pracy blokad, takich jak **Zestaw AzureRmResourceLock** zaktualizować blokada i **AzureRmResourceLock Usuń** , aby usunąć blokadę.

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać więcej informacji na temat pracy z blokowania zasobów zobacz [Blokowanie w dół i zasoby Azure](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
- Aby uzyskać informacje na temat organizowania logicznie zasobów, zobacz [Używanie znaczników w celu organizowania zasobów](resource-group-using-tags.md)
- Aby zmienić grupę zasobów zasób znajduje się w, zobacz [Przenoszenie zasobów do nowej grupy zasobów](resource-group-move-resources.md)
- Ograniczenia i konwencje można stosować w subskrypcji z niestandardowych zasad. Aby uzyskać więcej informacji zobacz [Używanie zasad zarządzania zasobami i kontrolować dostęp](resource-manager-policy.md).
