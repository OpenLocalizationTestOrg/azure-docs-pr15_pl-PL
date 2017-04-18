<properties
   pageTitle="Za pomocą potrzeby stanu konfiguracji z zestawów skali maszyn wirtualnych | Microsoft Azure"
   description="Przy użyciu skali maszyn wirtualnych ustawia rozszerzeniem DSC Azure"
   services="virtual-machine-scale-sets"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machine-scale-sets"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Przy użyciu skali maszyn wirtualnych ustawia rozszerzeniem DSC Azure

[Zestawy skali maszyn wirtualnych (VMSS)](virtual-machine-scale-sets-overview.md) można używać z obsługi rozszerzenia [Azure potrzeby stan konfiguracji (DSC)](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md) . VMSS umożliwia wdrażanie i zarządzanie dużą liczbą maszyn wirtualnych i elastically można skalować i zmniejszanie w odpowiedzi, aby załadować. DSC służy do konfigurowania maszyny wirtualne ich trybu online, aby działają oprogramowania produkcji.

## <a name="differences-between-deploying-to-vm-and-vmss"></a>Różnice między wdrożeniem maszyn wirtualnych i VMSS

Podstawową strukturę szablonu dla VMSS nieznacznie różni się od jednego maszyn wirtualnych. W szczególności pojedynczy maszyn wirtualnych wdraża rozszerzenia węźle "virtualMachines". Istnieje wpis typu "rozszerzenia", w którym DSC zostanie dodany do szablonu

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

Węzeł VMSS zawiera sekcja "właściwości" o "VirtualMachineProfile", "extensionProfile" atrybut. DSC zostanie dodany w obszarze "rozszerzenia"

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-vmss"></a>Zachowanie VMSS

Zachowanie VMSS jest identyczny z zachowanie pojedynczego maszyn wirtualnych. Po utworzeniu nowego maszyn wirtualnych automatycznie jest przygotowana z rozszerzeniem DSC. Jeśli nowsza wersja WMF jest wymagane przez rozszerzenie, przed przełączeniem online ponowne uruchomienie maszyn wirtualnych. Gdy jest w trybie online, do pobrania zip konfiguracji DSC i świadczenie go na maszyn wirtualnych. Więcej informacji można znaleźć w [Omówienie rozszerzenia DSC Azure](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md).

## <a name="next-steps"></a>Następne kroki ##
Sprawdź [Menedżera zasobów Azure szablon z rozszerzeniem DSC](../virtual-machines/virtual-machines-windows-extensions-dsc-template.md).

Dowiedz się, jak [rozszerzenia DSC bezpieczną obsługę poświadczeń](../virtual-machines/virtual-machines-windows-extensions-dsc-credentials.md). 

Aby uzyskać więcej informacji na obsługi rozszerzenia Azure DSC zobacz [Wprowadzenie do obsługi rozszerzenia Azure potrzeby stan konfiguracji](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md). 

Aby uzyskać więcej informacji na temat programu PowerShell DSC, [odwiedź witrynę Centrum dokumentacji programu PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 


