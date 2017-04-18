<properties
    pageTitle="Wdrażanie aplikacji na zestawach skali maszyn wirtualnych | Microsoft Azure"
    description="Wdrażanie aplikacji na zestawach skali maszyn wirtualnych"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>


# <a name="upgrade-a-virtual-machine-scale-set"></a>Uaktualnianie zestawu skali maszyn wirtualnych

W tym artykule opisano, jak można wdrożeniem aktualizacji systemu operacyjnego skalę Azure maszyn wirtualnych bez dowolnego przestoje. W tym kontekście aktualizacji systemu operacyjnego wymaga zmiana wersji lub wersji systemu operacyjnego lub zmiana identyfikator URI obraz niestandardowy. Aktualizowanie bez przestoje podczas aktualizowania maszyn wirtualnych z nich pojedynczo lub w grupach (na przykład jedną domenę błędów naraz) zamiast w całym dokumencie. W ten sposób zachować uruchomiony dowolny maszyn wirtualnych, które nie są są uaktualniane.

W celu uniknięcia niejednoznaczności Przejdźmy odróżnić trzy typy aktualizacji systemu operacyjnego, które można wykonać:

- Zmienianie wersji SKU obrazu platformy. Na przykład zmiana Ubuntu wersji 14.04.2-LTS z 14.04.201506100 14.04.201507060, lub zmienić Ubuntu 15.10-r SKU na 16.04.0-LTS/latest. W tym artykule omówiono w tym scenariuszu.

- Zmienianie identyfikatora URI, która kieruje do nowej wersji obraz niestandardowy wbudowany (**Właściwości** > **virtualMachineProfile** > **storageProfile** > **osDisk** > **obraz** > **Identifier**). W tym artykule omówiono w tym scenariuszu.

- Poprawianie OS z poziomu maszyny wirtualnej (to przykłady instalowanie poprawki zabezpieczeń i uruchamianie usługi Windows Update). W tym scenariuszu jest obsługiwane, ale nie zostały omówione w tym artykule.

Dwóch pierwszych opcji są obsługiwane wymagania określone w niniejszym artykule. Musisz utworzyć nową skalę ustawiona na wykonywanie trzecia opcja.

Zestawy skali maszyn wirtualnych wdrożonych w ramach klaster [Tkaninie usługi Azure](https://azure.microsoft.com/services/service-fabric/) nie są objęte tutaj.

Podstawowe sekwencji do zmiany wersji systemu operacyjnego i SKU obrazu platformy lub identyfikator URI obraz niestandardowy wygląda następująco:

1. Uzyskaj modelu zestawu skali maszyn wirtualnych.

2. Zmiana wersji, SKU lub identyfikator URI wartości w modelu.

3. Aktualizowanie modelu.

4. Czy połączenia *manualUpgrade* w przypadku maszyn wirtualnych w zestawie Skala. Ten krok ma zastosowanie tylko jeśli *upgradePolicy* jest ustawiona na **ręcznie** w zestawie Skala. Jeśli jest ustawiony na **Automatyczne**, maszyn wirtualnych są uaktualniane jednocześnie, powodując przestoje.


Z tych informacji tła pamiętać Zobaczmy, jak można zaktualizować wersję skalę Ustaw w programie PowerShell i za pomocą interfejsu API usługi REST. W tych przykładach obejmuje obraz platformy, ale w tym artykule znajdują się za mało informacji do dostosowania tego procesu obraz niestandardowy.

## <a name="powershell"></a>Programu PowerShell##

W tym przykładzie aktualizacji Ustawianie do nowej wersji 4.0.20160229 skalę maszyn wirtualnych systemu Windows. Po zaktualizowaniu modelu, czy aktualizacji jednego wystąpienia maszyn wirtualnych jednocześnie.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Aktualizowania identyfikatora URI dla obraz niestandardowy, zamiast zmieniać platformy wersję obrazu, należy zastąpić linii "Ustaw nową wersję" podobną do następujących czynności:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```


## <a name="the-rest-api"></a>Interfejsu API usługi REST

Oto kilka przykładów Python, które wdrożeniem aktualizacji wersji systemu operacyjnego za pomocą interfejsu API usługi REST Azure. Umożliwia zarówno wykonaj GET w modelu zestawu skali następuje położenie z modelem zaktualizowane biblioteki lightweight [azurerm](https://pypi.python.org/pypi/azurerm) interfejsu API usługi REST Azure opakowanie funkcji. Przyjrzyj są również widoki wystąpienia maszyn wirtualnych do identyfikowania maszyn wirtualnych przez domenę aktualizacji.

### <a name="vmssupgrade"></a>Vmssupgrade

 [Vmssupgrade](https://github.com/gbowerman/vmsstools) czy skrypt Python, który jest używany do wdrożenia uaktualnieniu systemu operacyjnego uruchomionego skali maszyn wirtualnych jedną domenę aktualizacji naraz.

![Skrypt Vmssupgrade dotyczące wybierania maszyn wirtualnych lub domena aktualizacji](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Ten skrypt umożliwia wybranie określonych maszyn wirtualnych zaktualizować lub określić domenę aktualizacji. Obsługuje zmieniania wersję platformy obraz lub zmieniania identyfikator URI obraz niestandardowy.

### <a name="vmsseditor"></a>Vmsseditor

[Vmsseditor](https://github.com/gbowerman/vmssdashboard) to ogólnego przeznaczenia edytor dla zestawów skali maszyn wirtualnych przedstawiający stan maszyn wirtualnych heatmap gdzie jeden wiersz reprezentuje jedną domenę aktualizacji. Między innymi można zaktualizować model zestawu skali z nowej wersji, SKU lub obraz niestandardowy identyfikator URI, a następnie wybierz domen błędów przeprowadzić uaktualnienie. Po wykonaniu tej czynności maszyn wirtualnych w tej domenie aktualizacji są uaktualniane do nowego modelu. Alternatywnie możesz zrobić uaktualnienia stopniowego na podstawie rozmiaru partii wybranych przez użytkownika.  

Następujące zrzucie ekranu pokazano modelu skalę ustawieniem Ubuntu 14.04-2LTS wersji 14.04.201507060. Wiele więcej opcji zostały dodane do tego narzędzia, ponieważ pochodzi ten zrzut ekranu.

![Model Vmsseditor skalę ustawieniem Ubuntu 14.04-2LTS](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Po kliknięciu, **Uaktualnianie** i **Uzyskać szczegółowe informacje**, Rozpocznij maszyn wirtualnych w UD 0, aby zaktualizować.

![Aktualizacja przedstawiający Vmsseditor w toku](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)
