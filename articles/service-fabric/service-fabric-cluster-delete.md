<properties
   pageTitle="Usuwanie Azure klaster i jego zasobów | Microsoft Azure"
   description="Dowiedz się, w jaki sposób całkowitego usunięcia tkaninie usługi klaster, usuwając grupę zasobów, zawierającą klaster lub selektywne usuwanie zasobów."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>

# <a name="delete-a-service-fabric-cluster-on-azure-and-the-resources-it-uses"></a>Usuwanie klastrze tkaninie usługi Azure i zasoby, które są używane

Klaster tkaninie usługi składa się z wielu innych zasobów Azure oprócz samej zasobu klaster. Tak, aby całkowicie usunąć klaster tkaninie usługi, należy usunąć wszystkie zasoby, które zostało wykonane z.
Dostępne są dwie opcje: albo usuń grupa zasobów, która znajduje się w grupie (który usuwa zasobu klaster i inne zasoby w grupie zasobów) lub specjalnie Usuwanie zasobu klaster i skojarzone zasobów (ale nie inne zasoby w grupie zasobów).

>[AZURE.NOTE] Usuwanie zasobu klaster **nie** usunąć wszystkie zasoby, które klaster tkaninie usługi składa się z.

## <a name="delete-the-entire-resource-group-rg-that-the-service-fabric-cluster-is-in"></a>Usuwanie grupy cały zasób (RG) klaster tkaninie usługi jest w

To jest najprostszym sposobem, aby upewnić się, że możesz usunąć wszystkie zasoby skojarzone z klastrem, w tym grupy zasobów. Możesz usunąć grup zasobów przy użyciu programu PowerShell lub za pośrednictwem portalu Azure. Jeśli grupy zasobów zawiera zasoby, które nie są związane z klastrem tkaninie usługi, można usunąć określonych zasobów.

### <a name="delete-the-resource-group-using-azure-powershell"></a>Usuwanie grupy zasobów przy użyciu programu PowerShell Azure

Możesz również usunąć grupa zasobów, uruchamiając następujące polecenia cmdlet programu PowerShell Azure. Upewnij się, że Azure PowerShell 1.0 lub większa jest zainstalowany na Twoim komputerze. Jeśli jeszcze tego nie tego wcześniej, postępuj zgodnie z instrukcjami podanymi w [jak zainstalować i skonfigurować Azure programu PowerShell.](../powershell-install-configure.md)

Otwórz okno programu PowerShell i uruchom następujące polecenia cmdlet PS:

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

Zostanie wyświetlony monit o potwierdzenie usunięcia, jeśli nie użyto *-życie* opcji. Na potwierdzenie RG i wszystkie zasoby, które zawiera są usuwane.

### <a name="delete-a-resource-group-in-the-azure-portal"></a>Usuwanie grupy zasobów w portalu Azure  

1. Zaloguj się do [portalu Azure](https://portal.azure.com).
2. Przejdź do klastrów tkaninie usługi, które chcesz usunąć.
3. Kliknij nazwę grupy zasobów na stronie essentials klaster.
4. Spowoduje to wyświetlenie na stronie **Essentials grupy zasobów** .
5. Kliknij przycisk **Usuń**.
6. Postępuj zgodnie z instrukcjami na stronie Kończenie procesu usuwania grupy zasobów.

![Usuwanie grupy zasobów][ResourceGroupDelete]


## <a name="delete-the-cluster-resource-and-the-resources-it-uses-but-not-other-resources-in-the-resource-group"></a>Usuwanie zasobu klaster i zasoby, które są używane, ale nie inne zasoby w grupie zasobów

Jeśli grupy zasobów zawiera tylko te zasoby, które są związane z klastrem tkaninie usługi, które chcesz usunąć, następnie łatwiej usunąć grupy cały zasób. Jeśli chcesz selektywne usuwanie zasobów jeden po drugim w grupie zasobów, następnie wykonaj poniższe czynności.

Jeśli wdrożono klaster za pomocą portalu lub przy użyciu jednego z szablonów Menedżera zasobów tkaninie usługi z galerii szablonów, wszystkie zasoby, które klaster używa są oznakowanie przy następujące dwa tagi. Można używać ich do określ zasoby, które chcesz usunąć.

***Znakowanie #1:*** Klucz = NazwaKlastra, wartość = "Nazwa klaster"

***Znakowanie #2:*** Klucz = resourceName, wartość = ServiceFabric

### <a name="delete-specific-resources-in-the-azure-portal"></a>Usuwanie określonych zasobów w portalu Azure

1. Zaloguj się do [portalu Azure](https://portal.azure.com).
2. Przejdź do klastrów tkaninie usługi, które chcesz usunąć.
3. Przejdź do **wszystkich ustawień** w karta essentials.
4. Kliknij na **znaczników** w obszarze **Zarządzanie zasobami** karta Ustawienia.
5. Kliknij jeden z **znaczników** w karta znaczników w celu uzyskania listy wszystkich zasobów zawierające ten znacznik.

    ![Znaczniki zasobów][ResourceTags]

6. Po umieszczeniu na liście oznakowane zasobów, kliknij każdy z zasobów, a następnie usuń je.

    ![Oznakowane zasobów][TaggedResources]

### <a name="delete-the-resources-using-azure-powershell"></a>Usuwanie zasobów za pomocą programu PowerShell Azure

Możesz usunąć zasoby jeden po drugim, uruchamiając następujące polecenia cmdlet programu PowerShell Azure. Upewnij się, że Azure PowerShell 1.0 lub większa jest zainstalowany na Twoim komputerze. Jeśli jeszcze tego nie tego wcześniej, postępuj zgodnie z instrukcjami podanymi w [jak zainstalować i skonfigurować Azure programu PowerShell.](../powershell-install-configure.md)

Otwórz okno programu PowerShell i uruchom następujące polecenia cmdlet PS:

```powershell
Login-AzureRmAccount
```
Dla każdej z nich zasobów, który chcesz usunąć, uruchom następujące polecenie:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of the resource group>" -Force
```

Aby usunąć zasobu klaster, uruchom następujące polecenie:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of the resource group>" -Force
```

## <a name="next-steps"></a>Następne kroki
Przeczytaj poniższe czynności, aby także informacje na temat uaktualniania klastrze i podziału usług:

- [Więcej informacji na temat uaktualniania klaster](service-fabric-cluster-upgrade.md)
- [Więcej informacji na temat podziału stanowe usługi związane z maksymalną skalę](service-fabric-concepts-partitioning.md)


<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
