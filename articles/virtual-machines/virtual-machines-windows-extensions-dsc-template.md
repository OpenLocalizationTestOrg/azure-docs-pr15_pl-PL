<properties
   pageTitle="Potrzeby szablon Menedżera zasobów stanu konfiguracji | Microsoft Azure"
   description="Definiowanie szablonu Menedżera zasobów potrzeby konfiguracji stan w Azure wraz z przykładami i rozwiązywania problemów"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>VMSS systemu Windows i potrzeby konfiguracji stan z szablonami Azure Menedżera zasobów
Ten artykuł zawiera opis szablonu Menedżera zasobów na [potrzeby konfiguracji stan rozszerzenia obsługi](virtual-machines-windows-extensions-dsc-overview.md). 

## <a name="template-example-for-a-windows-vm"></a>Przykład szablonu dla maszyn wirtualnych systemu Windows

Poniższy fragment przechodzi do sekcji zasobów szablonu.

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }

```

## <a name="template-example-for-windows-vmss"></a>Przykład szablonu VMSS systemu Windows

Węzeł VMSS zawiera sekcja "właściwości" o "VirtualMachineProfile", "extensionProfile" atrybut. DSC zostanie dodany w obszarze "rozszerzenia". 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
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

## <a name="detailed-settings-information"></a>Informacje szczegółowe ustawienia

Następujące schemat jest ustawienia części z rozszerzeniem Azure DSC w szablonie Azure Menedżera zasobów.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Szczegóły
| Nazwa właściwości | Typ | Opis |
| --- | --- | --- |
| settings.wmfVersion | ciąg | Określa wersję programu Windows Management Framework, które powinny być zainstalowane na swojej maszyn wirtualnych. Ustawienie tej właściwości "najnowszą" instalacje najbardziej zaktualizowanej wersji WMF. Tylko bieżące możliwe wartości tej właściwości **"4.0", "5.0", "5.0PP', a"r"**. Te wartości są objęte aktualizacji. Wartość domyślna to "najnowszą".|
| Settings.Configuration.URL | ciąg | Umożliwia określenie lokalizacji adresu URL, z której do pobrania pliku zip konfiguracji DSC. Jeśli adres URL podany wymaga tokenu skojarzeń zabezpieczeń programu access, należy ustawić właściwość protectedSettings.configurationUrlSasToken na wartość token skojarzeń zabezpieczeń. Ta właściwość jest wymagane, jeśli zdefiniowano settings.configuration.script i/lub settings.configuration.function. |
| Settings.Configuration.Script | ciąg | Nazwa pliku skrypt, który zawiera definicję konfiguracji DSC. Ten skrypt musi być w folderze głównym pliku zip pobrane z adres URL określony przez właściwość configuration.url. Ta właściwość jest wymagane, jeśli zdefiniowano settings.configuration.url i/lub settings.configuration.script. |
| Settings.Configuration.Function | ciąg | Nazwa konfiguracji DSC. Konfigurację o nazwie muszą znajdować się w obszarze skrypt zdefiniowanych przez configuration.script. Ta właściwość jest wymagane, jeśli zdefiniowano settings.configuration.url i/lub settings.configuration.function. |
| settings.configurationArguments | Kolekcja | Określa parametry, które chcesz przekazać, konfiguracji DSC. Ta właściwość nie jest zaszyfrowany. |
| settings.configurationData.url | ciąg | Określa adres URL z których można pobrać pliku konfiguracji danych (.pds1), aby użyć jako danych wejściowych dla konfiguracji DSC. Jeśli adres URL podany wymaga tokenu skojarzeń zabezpieczeń programu access, należy ustawić właściwość protectedSettings.configurationDataUrlSasToken na wartość token skojarzeń zabezpieczeń.|
| settings.privacy.dataEnabled | ciąg | Włącza lub wyłącza telemetrycznego zbioru. Tylko możliwe wartości tej właściwości **"Włącz", "Wyłącz", ", lub $null**. Pozostawienie tej właściwości puste ani mieć wartości null umożliwia telemetrycznego. Wartość domyślna to ". [Więcej informacji](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings | Kolekcja | Określa alternatywnej lokalizacji, z których do pobrania WMF. [Więcej informacji](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments | Kolekcja | Określa parametry, które chcesz przekazać, konfiguracji DSC. Ta właściwość jest zaszyfrowany. |
| protectedSettings.configurationUrlSasToken | ciąg | Określa token skojarzeń zabezpieczeń dostępu do adresu URL zdefiniowanych przez configuration.url. Ta właściwość jest zaszyfrowany. |
| protectedSettings.configurationDataUrlSasToken | ciąg | Określa token skojarzeń zabezpieczeń dostępu do adresu URL zdefiniowanych przez configurationData.url. Ta właściwość jest zaszyfrowany. |

## <a name="settings-vs-protectedsettings"></a>Ustawienia a ProtectedSettings
Wszystkie ustawienia są zapisywane w pliku tekstowym ustawienia na maszyn wirtualnych.
Właściwości w obszarze "ustawienia" są właściwości publiczne, ponieważ nie są szyfrowane w pliku tekstowym ustawienia.
Właściwości w obszarze "protectedSettings" są szyfrowane za pomocą certyfikatu i nie są wyświetlane w formacie zwykłego tekstu w tym pliku na maszyn wirtualnych.

Jeśli konfiguracja wymaga poświadczeń, mogą one zostać dołączone w protectedSettings:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Przykład

Poniższy przykład pochodzi z sekcji "Wprowadzenie" na [stronie Omówienie obsługi rozszerzenia DSC](virtual-machines-windows-extensions-dsc-overview.md).
W tym przykładzie użyto szablony Menedżera zasobów zamiast polecenia cmdlet wdrożenia rozszerzenia. Zapisywanie konfiguracji "IisInstall.ps1", umieść ją w. Plik ZIP i przekaż plik adres URL dostępny. W tym przykładzie użyto magazynem obiektów blob Azure, ale istnieje możliwość pobierania. Pliki ZIP z dowolnego z dowolnego miejsca.

W szablonie Menedżera zasobów Azure poniższy kod powoduje, że maszyn wirtualnych pobrać właściwy plik i uruchomić odpowiedniej funkcji programu PowerShell:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-the-previous-format"></a>Aktualizowanie z poprzednim formacie
Wszystkie ustawienia w poprzednim formacie (zawierające właściwości publiczne ModulesUrl, ConfigurationFunction, SasToken lub właściwości) automatycznie dopasować na bieżący format i Uruchom tylko tak, jak przed.

Następującego schematu jest jakie poprzednie ustawienia schemat wyszukiwania, takie jak:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Poniżej opisano, jak poprzednim formacie przystosowuje do Twoich na bieżący format:

| Nazwa właściwości | Odpowiednik w poprzednim schematu |
| --- | --- |
| settings.wmfVersion | Ustawienia. WMFVersion |
| Settings.Configuration.URL | Ustawienia. ModulesUrl |
| Settings.Configuration.Script | Pierwsza część ustawienia. ConfigurationFunction (przed "\\\\") |
| Settings.Configuration.Function | Druga część ustawień. ConfigurationFunction (po "\\\\") |
| settings.configurationArguments | Ustawienia. Właściwości |
| settings.configurationData.url | protectedSettings.DataBlobUri (bez token skojarzenia zabezpieczeń) |
| settings.privacy.dataEnabled | Ustawienia. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings | Ustawienia. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments | protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken | Ustawienia. SasToken |
| protectedSettings.configurationDataUrlSasToken | Token skojarzenia zabezpieczeń z protectedSettings.DataBlobUri |


## <a name="troubleshooting---error-code-1100"></a>Rozwiązywanie problemów — kod błędu 1100
Kod błędu 1100 wskazuje, że występuje problem z użytkownikiem rozszerzenia DSC.
Tekst tych błędów jest zmienną i może ulec zmianie.
Poniżej przedstawiono kilka błędów, które mogą pojawić się i jak je poprawić.

### <a name="invalid-values"></a>Nieprawidłowe wartości
"Jest Privacy.dataCollection {0}. Tylko możliwe wartości ","Włącz"i"Wyłącz"" "jest WmfVersion {0}. Tylko możliwe wartości to... i "najnowszą" "

Problem: Podana wartość nie jest dozwolone.

Rozwiązanie: Zmień nieprawidłowej wartości na prawidłową wartość. Zobacz tabelę w sekcji szczegółów.

### <a name="invalid-url"></a>Nieprawidłowy adres URL
"Jest ConfigurationData.url {0}. To nie jest prawidłowy adres URL""jest DataBlobUri {0}. To nie jest prawidłowy adres URL""jest Configuration.url {0}. To nie jest prawidłowy adres URL"

Problem: A pod warunkiem, że adres URL nie jest prawidłowy.

Rozwiązanie: Sprawdź wszystkie podane adresy. Upewnij się, że wszystkie adresy URL rozpoznawać prawidłowych lokalizacji czy rozszerzenie mają dostęp na komputerze zdalnym.

### <a name="invalid-configurationargument-type"></a>Nieprawidłowy typ ConfigurationArgument
"Nieprawidłowe configurationArguments wpisz {0}"

Problem: Właściwości ConfigurationArguments nie może rozpoznać do obiektu skrótów. 

Rozwiązanie: Należy określić właściwość usługi ConfigurationArguments skrótów. Wykonaj opisane w tym przykładzie poprzedzających format. Uważaj na ofert, przecinki i nawiasy klamrowe.

### <a name="duplicate-configurationarguments"></a>Duplikowanie ConfigurationArguments
"Znaleziono zduplikowane argumenty"{0}"w publicznych i chronionych configurationArguments"

Problem: ConfigurationArguments w ustawieniach publicznej i ConfigurationArguments w ustawieniach chronionego zawierają właściwości o takiej samej nazwie.

Rozwiązanie: Usuń jedną z tych właściwości.

### <a name="missing-properties"></a>Brakujące właściwości
"Configuration.function wymaga, że jest określony configuration.url lub configuration.module"

"Configuration.url wymaga określonego tego configuration.script"

"Configuration.script wymaga określonego tego configuration.url"

"Configuration.url wymaga określonego tego configuration.function"

"ConfigurationUrlSasToken wymaga określonego tego configuration.url"

"ConfigurationDataUrlSasToken wymaga określonego tego configurationData.url"

Problem: Zdefiniowane właściwości musi inną właściwość, która nie jest widoczny.

Rozwiązania: 
- Podaj Brak właściwości.
- Usuń właściwość, który wymaga Brak właściwości.


## <a name="next-steps"></a>Następne kroki
Więcej informacji na temat zestawów skali DSC i maszyn wirtualnych w [Za pomocą zestawów skali maszyn wirtualnych z rozszerzeniem DSC Azure](../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)

Znajdź więcej szczegółowych informacji na [DSC i poświadczenia bezpiecznego zarządzania](virtual-machines-windows-extensions-dsc-credentials.md). 

Aby uzyskać więcej informacji na obsługi rozszerzenia Azure DSC zobacz [Wprowadzenie do obsługi rozszerzenia Azure potrzeby stan konfiguracji](virtual-machines-windows-extensions-dsc-overview.md). 

Aby uzyskać więcej informacji na temat programu PowerShell DSC, [odwiedź witrynę Centrum dokumentacji programu PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 
